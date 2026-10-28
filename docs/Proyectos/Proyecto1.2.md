# Proyecto 1: Carro por Bluetooth

* **Universidad:** Universidad Iberoamericana Puebla
* **Materia:** Introducción a la Mecatrónica

---

## Integrantes del equipo

<div class="integrantes">
    <div class="integrante">Daniel Alejandro Trejo Macías</div>
    <div class="integrante">Carlos Mario Diaz Moscoso</div>
    <div class="integrante">Johan Mauricio Cerqueda Rojas</div>
    <div class="integrante">Jose Pablo Sanchez Guerrero</div>
    <div class="integrante">Abraham Ramirez Garcia</div>
</div>

---

## Objetivo

<p>
    Construir en equipo un carro controlado por Bluetooth,
    integrando los conocimientos del curso de electrónica,
    microcontroladores, actuadores, comunicación y mecanismos.
    El carro será utilizado en un torneo de fútbol RC.
</p>

---

## Requisitos funcionales

* Movimiento hacia adelante, atrás, izquierda, derecha y alto mediante Bluetooth.
* Control de velocidad por PWM, con al menos tres niveles.
* Documentación del protocolo de comandos.
* Sistema failsafe: detener el carro si no recibe comandos durante un segundo.
* Interruptor físico de encendido y apagado accesible.
* Alimentación de motores separada del ESP32, con tierra común (GND).
* Cableado ordenado y sujeto al chasis.
* Superficie superior despejada de al menos 6 × 6 cm para el marcador ArUco.

### Requisitos propios del equipo

* Control mediante un mando de Xbox.
* Pinzas rígidas en la parte delantera para maniobrar la pelota.
* Dos ruedas traseras y una rueda frontal.
* Cables de los motores atravesando el chasis para conectarlos al puente H.
* Carrocería para mantener protegidos los componentes.
* Uso de un servo en las pinzas para patear la pelota.

---

## Arquitectura del sistema

<p>
    El ESP32 recibe las instrucciones del control y administra
    los movimientos del carro. El puente H TB6612FNG permite
    controlar los motores, mientras que la alimentación de los
    motores se mantiene separada de la del microcontrolador.
</p>

<img src="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/imgs/DiagramaBloques.png" alt="Diagrama de bloques del carro por Bluetooth">

[Ver diagrama completo](https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/imgs/DiagramaBloques.png){ .md-button target="_blank" }

---

## Plan de trabajo por semanas

### Semana 1
* Programación del código inicial.
* Diseño del chasis.
* Diseño y programación de la aplicación de prueba.

### Semana 2
* Cableado del ESP32, puente H y motores.
* Diseño de la carrocería.
* Soldadura de cables.
* Impresión del chasis y la carrocería.
* Ajustes al código para manejar el carro con el control.

### Boceto del chasis
<img src="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/imgs/BocetoChasis.png" alt="Boceto del chasis del carro">

---

## Código de control (ESP32)

<p>Código definitivo implementado en el ESP32 para el control mediante mando de Xbox a través de Bluetooth:</p>

```cpp
#include <Arduino.h>
#include <BLEGamepadClient.h>
#include <ESP32Servo.h>

XboxController controller;
Servo brazoServo;

// Pines TB6612FNG
const int pinPWMA = 14;
const int pinAIN1 = 27;
const int pinAIN2 = 26;
const int pinBIN1 = 25;
const int pinBIN2 = 33;
const int pinPWMB = 32;
const int pinSTBY = 4;

// LED integrado: parpadea buscando control, fijo si está conectado
const int pinLED = 2;

// ================== AJUSTES ==================
const bool USAR_SERVO = false;       // Pon en true si vuelves a utilizar el servo de tiro

// Servo SG90 (mecanismo de tiro - opcional)
const int pinSERVO = 13;
const int POS_REPOSO = 0;
const int POS_GOLPE  = 90;

const bool INVERTIR_Y = false;       // si empujas arriba y va hacia atrás, pon true
const bool INVERTIR_X = false;       // si gira al lado contrario, pon true
const bool INVERTIR_MOTOR_A = true;  // motor izquierdo (si gira al revés, pon true)
const bool INVERTIR_MOTOR_B = false; // motor derecho

const float ZONA_MUERTA = 0.12;
const int   PWM_MINIMO  = 60;
const int   NIVELES[3]  = {150, 210, 255};  // 3 niveles (baja, media, alta)
int nivel = 1;                              // arranca en media

const int PASO_ACEL = 25;                   // aceleración limitada por tick
const unsigned long PERIODO_CONTROL = 10;

// Tiro (solo aplica si USAR_SERVO es true)
const unsigned long TIEMPO_GOLPE    = 120;
const unsigned long TIEMPO_RECARGA = 300;

// Failsafe opcional
const bool USAR_WATCHDOG = false;
const unsigned long TIEMPO_FAILSAFE = 1000;

const bool DEBUG = true;
// =============================================

const int PWM_FREQ = 20000;
const int PWM_RES  = 8;

// Estado de motores (con signo: positivo adelante, negativo atrás)
int actualA = 0;
int actualB = 0;

// Estado del tiro
enum EstadoTiro { TIRO_LIBRE, TIRO_EXTENDIDO, TIRO_RECARGA };
EstadoTiro estadoTiro = TIRO_LIBRE;
unsigned long inicioTiro = 0;

// Estado de botones y reportes
bool prevLB = false, prevRB = false, prevTiro = false;
float ultX = 0, ultY = 0;
unsigned long ultimoCambio = 0;
unsigned long ultimoControl = 0;
unsigned long ultimoDebug = 0;
unsigned long reportes = 0;

// ---------- Motores ----------
void direccionMotor(int in1, int in2, int valor) {
  if (valor > 0) {
    digitalWrite(in1, HIGH); digitalWrite(in2, LOW);
  } else if (valor < 0) {
    digitalWrite(in1, LOW);  digitalWrite(in2, HIGH);
  } else {
    digitalWrite(in1, HIGH); digitalWrite(in2, HIGH);  // freno corto
  }
}

void escribirMotores(int a, int b) {
  if (INVERTIR_MOTOR_A) a = -a;
  if (INVERTIR_MOTOR_B) b = -b;
  direccionMotor(pinAIN1, pinAIN2, a);
  direccionMotor(pinBIN1, pinBIN2, b);
  ledcWrite(pinPWMA, abs(a));
  ledcWrite(pinPWMB, abs(b));
}

void detenerTodo() {
  actualA = 0;
  actualB = 0;
  escribirMotores(0, 0);
}

// Aceleración limitada; frenar o bajar velocidad es inmediato
int rampa(int actual, int objetivo) {
  if ((actual > 0 && objetivo < 0) || (actual < 0 && objetivo > 0)) actual = 0;
  if (abs(objetivo) <= abs(actual)) return objetivo;
  if (objetivo > 0) return min(actual + PASO_ACEL, objetivo);
  return max(actual - PASO_ACEL, objetivo);
}

int valorPWM(float v) {
  float a = fabsf(v);
  if (a < 0.03) return 0;
  int pwm = PWM_MINIMO + (int)(a * (NIVELES[nivel] - PWM_MINIMO));
  return v > 0 ? pwm : -pwm;
}

// Mezcla tipo "arcade": Y = avance, X = giro
void calcularObjetivo(float x, float y, int &objA, int &objB) {
  float mag = sqrtf(x * x + y * y);
  if (mag < ZONA_MUERTA) { objA = 0; objB = 0; return; }
  if (mag > 1.0f) { x /= mag; y /= mag; mag = 1.0f; }
  float escala = (mag - ZONA_MUERTA) / (1.0f - ZONA_MUERTA);
  x = x / mag * escala;
  y = y / mag * escala;

  float izq = y + x;  // motor A = izquierdo
  float der = y - x;  // motor B = derecho
  float mx = fmaxf(fabsf(izq), fabsf(der));
  if (mx > 1.0f) { izq /= mx; der /= mx; }

  objA = valorPWM(izq);
  objB = valorPWM(der);
}

// ---------- Tiro (servo) ----------
void iniciarTiro() {
  if (!USAR_SERVO) return;
  if (estadoTiro != TIRO_LIBRE) return;
  if (!brazoServo.attached()) brazoServo.attach(pinSERVO, 500, 2400);
  brazoServo.write(POS_GOLPE);
  inicioTiro = millis();
  estadoTiro = TIRO_EXTENDIDO;
}

void actualizarTiro(unsigned long ahora) {
  if (!USAR_SERVO) return;
  switch (estadoTiro) {
    case TIRO_EXTENDIDO:
      if (ahora - inicioTiro >= TIEMPO_GOLPE) {
        brazoServo.write(POS_REPOSO);
        estadoTiro = TIRO_RECARGA;
      }
      break;
    case TIRO_RECARGA:
      if (ahora - inicioTiro >= TIEMPO_GOLPE + TIEMPO_RECARGA) {
        brazoServo.detach();
        estadoTiro = TIRO_LIBRE;
      }
      break;
    default:
      break;
  }
}

void setup() {
  Serial.begin(115200);
  delay(500);

  Serial.println();
  Serial.println("========================================");
  Serial.println("    CARRITO FUTBOL - CONTROL XBOX