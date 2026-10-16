---
titulo: "Proyecto — Robot Móvil Diferencial con ESP32 y Nintendo Switch Joy-Con"
fecha: 2026-10-15
autor: "Andrea Paola Carmona Casiano, Hernández Cruz Tania, Hernández Domínguez Jesús Emiliano, Francisco Javier Pérez Hernández"
estado: completa   # borrador | completa
---

# Propuesta y Desarrollo — Robot Móvil Diferencial con ESP32

## Objetivos

Fabricar, diseñar y programar un carro utilizando un ESP32 controlado por Bluetooth desde un dispositivo (celular o computadora). Al final, se demostrará su funcionamiento en un torneo de fútbol 1v1 contra los demás proyectos.

### Requisitos

#### Funcionales
* **R1:** El carro se mueve adelante, atrás, izquierda, derecha y alto por comandos Bluetooth.
* **R2:** Control de velocidad por PWM (mínimo 3 niveles o velocidad proporcional).
* **R3:** El protocolo de comandos está documentado en el portafolio (qué comando hace qué).
* **R4:** Failsafe: si se pierde el enlace o no llegan comandos en $\le 1\text{ s}$ (o $\le 500\text{ ms}$ en la implementación con control), el carro se detiene solo.
* **R5:** Interruptor físico de encendido/apagado accesible sin desarmar nada.
* **R6:** Alimentación de motores separada del ESP32 (con GND común), como se vio en el tema de Actuadores.
* **R7:** Cableado ordenado y sujeto: nada de cables sueltos arrastrando.
* **R8:** Superficie plana horizontal de al menos $6 \times 6\text{ cm}$ en la parte superior, despejada: ahí vivirá el marcador ArUco del Proyecto Final.

#### Propios
* Que tenga 4 llantas *(ajustado posteriormente en la bitácora a tracción diferencial con rueda loca)*.
* Que tenga un capote completo.
* Utilizar un puente H (TB6612FNG / L289N).
* Que tenga unas pinzas para empujar la pelota *(ajustado a placa plana empujadora)*.
* Controlarlo con un control de Xbox / Joy-Con de Nintendo Switch vía *Bluepad32*.

---

## Plan por Semanas

* **Semana 1:** Modelado del chasis en SolidWorks, a partir de un kit para visualizar mejor los componentes y estructura. Soldado de los cables a los motores, y montaje de las ruedas y switch para asegurar su funcionamiento.
* **Semana 2:** Impresión 3D del chasis y capote para agregar el cuadrado de $6 \times 6\text{ cm}$. Programación del control por Bluetooth para validar su funcionamiento por medio de un joystick.

---

## Criterios de Éxito

* Cumplir los 8 requisitos funcionales.
* Tener un failsafe donde el carro se detenga al no recibir indicaciones.
* Entregar el repositorio en tiempo y forma.
* Lograr controlar el carro por medio de un control inalámbrico (Xbox / Joy-Con).

---

## Materiales y BOM (Presupuesto Documentado)

| Componente | Cantidad | Costo |
| :--- | :---: | :---: |
| Batería portátil | 1 pza | \$300.00 |
| Termofit 1/16 | 1 pza | \$10.00 |
| Jumper hembra H | 1 pza | \$38.00 |
| Motorreductor de plástico AMA | 2 pza | \$70.00 |
| Cautín de Lápiz | 1 pza | \$105.00 |
| Mini Switch 3 patas | 1 pza | \$15.00 |
| ESP32 DevKit V1 | 1 pza | \$171.55 |
| Cinchos negros | 1 paquete | \$34.00 |
| Kit de chasis | 1 pza | \$363.00 |
| Cable negro y rojo | 3 m | \$30.00 |
| Tornillos | Varios | \$25.00 |

---

## Análisis de Riesgos

!!! warning "Riesgos Identificados"
    * Que la soldadura se rompa.
    * No se conecte el Bluetooth al ESP32.
    * Existan cables sueltos que se atoren con las ruedas.
    * Descarga de las pilas por exceso de consumo.
    * Fallar al intentar conectar 4 llantas a 2 motores.

---

## Arquitectura del Sistema

!!! failure "Pendiente"
    **Falta arquitectura:**

![Diagrama en bloques de la arquitectura del robot móvil](./IMG 7/arquitectura_bloques.png){ align=center width=600 }
*Diagrama en bloques del sistema indicando la distribución de energía (fuentes separadas con GND común), el interruptor, el ESP32 DevKit V1, el driver TB6612FNG y los actuadores.*

---

## Diseño

### Boceto del Chasis y Pinout

El diseño fue elaborado en SolidWorks partiendo de las dimensiones del kit físico para garantizar que los componentes encajaran dentro de la huella máxima de $20 \times 20\text{ cm}$.

| Pin ESP32 | Pin / Señal TB6612FNG | Descripción |
| :---: | :---: | :--- |
| **GPIO 23** | STBY | Standby / Habilitación general del driver |
| **GPIO 13** | PWMA | Control de velocidad PWM (Motor Izquierdo) |
| **GPIO 12** | AIN1 | Dirección Motor Izquierdo A |
| **GPIO 14** | AIN2 | Dirección Motor Izquierdo B |
| **GPIO 25** | PWMB | Control de velocidad PWM (Motor Derecho) |
| **GPIO 27** | BIN1 | Dirección Motor Derecho A |
| **GPIO 26** | BIN2 | Dirección Motor Derecho B |

![Boceto y modelado 3D del chasis y capote superior en SolidWorks](./IMG 7/chasis_cad.png){ align=center width=500 }
*Vista en CAD del chasis con la plataforma plana de $6 \times 6\text{ cm}$ para el marcador ArUco.*

---

## Desarrollo y Configuración

### 1. Configuración de Bluepad32 para Control Inalámbrico

Para usar el control de Nintendo Switch Joy-Con / Xbox con el ESP32, se configuraron las siguientes URLs adicionales en las preferencias del IDE de Arduino:

[https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json](https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json)
[https://raw.githubusercontent.com/ricardoquesada/esp32-arduino-lib-builder/master/bluepad32_files/package_esp32_bluepad32_index.json](https://raw.githubusercontent.com/ricardoquesada/esp32-arduino-lib-builder/master/bluepad32_files/package_esp32_bluepad32_index.json)

![Configuración del entorno de Arduino IDE para Bluepad32](./IMG 7/configuracion_bluepad32.png){ width=50% } <br>
*Verificación de la instalación del paquete para el control inalámbrico.*

### 2. Protocolo de Comandos
- **Joystick Analógico (Eje Y):** Controla avance y retroceso con una zona muerta digital de 35 unidades.
- **Joystick Analógico (Eje X):** Aplica mezcla de tracción diferencial dividida entre 2 (`velocidadBase / 2`).
- **Botones L1 / R1 (SL / SR):** Disminuyen o aumentan la velocidad base en pasos de 15 unidades (Rango: 60 a 255).

### 3. Código Fuente y Failsafe

!!! failure "Pendiente"
    **Falta el código fuente completo e integrado en el reporte:**

// Código base desarrollado con ejemplos de Arduino y asistencia de IA para Bluepad32 y TB6612FNG

## 4. Simulación en Wokwi
Se simuló el comportamiento del driver TB6612FNG mediante un chip personalizado y pares de LEDs (verde para avance, rojo para retroceso).

![Simulación del driver TB6612FNG en Wokwi](./IMG 7/simulacion_wokwi.png){ width=50% }

*Prueba lógica de los motores en Wokwi.*

### Demostración en Video de la Simulación
- [Ver video de la simulación del circuito en Wokwi](./IMG 7/simulacion_video.mp4)

---

## Bitácora de Fallas

| ¿Qué falló? | ¿Cómo lo encontraron? | ¿Cómo lo resolvieron? |
| :--- | :--- | :--- |
| **Las medidas del chasis** | Midiendo para corroborar que se cumplían las especificaciones. | Rediseñando la pieza y volviendo a imprimir. |
| **Idea principal de 4 llantas** | Descubrimos que era un motor por llanta, por lo que se descartó la idea de las 4 llantas. | Agregamos una rueda loca, para que el carro tenga soporte. |
| **Descarga de las pilas** | Los motores consumían más corriente que el ESP32, por lo que las pilas se agotaban muy rápido. | Utilizando dos fuentes de alimentación diferentes (con GND común). |
| **Diseño de las pinzas (empujar pelota)** | Nos comentaron que no era posible nuestra idea porque retenía la pelota. | Lo cambiamos por un cuadrado plano para avanzar con la pelota. |
| **Error en la librería de control (Arduino IDE)** | Al compilar no se descargaba correctamente la librería compatible con el ESP32. | Se agregaron las URLs específicas de *ricardoquesada* para instalar *Bluepad32*. |

---

## Requisitos vs. Resultados

| # | Requisito | ¿Se cumple? |
| :-: | :--- | :---: |
| **1** | Movimiento adelante, atrás, izquierda, derecha y alto por Bluetooth | Sí |
| **2** | Control de velocidad por PWM, mínimo 3 niveles | Sí |
| **3** | Protocolo de comandos documentado | Sí |
| **4** | Failsafe: el carro se detiene si no llegan comandos en 1 s o menos | Sí |
| **5** | Interruptor físico accesible sin desarmar | Sí |
| **6** | Alimentación de motores separada del ESP32, con GND común | Sí |
| **7** | Cableado ordenado y sujeto | Sí |
| **8** | Superficie plana horizontal de al menos $6 \times 6\text{ cm}$ en la parte superior | Sí |

---

## Lecciones Aprendidas

1. **La alimentación de los motores y la del ESP32 deben ser independientes:** Al conectar los motores y el ESP32 a las mismas pilas, la batería se agotaba muy rápido. Aprendimos que los motores consumen mucha más corriente que el microcontrolador y que cada subsistema necesita su propia fuente manteniendo una tierra común.
2. **Partir de algo físico facilita y acelera el modelado en CAD:** Compramos un kit de carrito para visualizar la estructura, y con ese modelo tangible como referencia modelamos nuestro chasis en SolidWorks. Aprendimos que tener las medidas y la forma de los componentes a la mano reduce errores y ahorra tiempo en el diseño.
3. **Planear y respetar los tiempos deja margen para corregir:** Tuvimos fallas al imprimir, y logramos arreglarlas porque aún había tiempo para reimprimir y ajustar. Por eso, realizar el proyecto en tiempo ayuda a que se puedan resolver estos errores sin prisa. Aprendimos que los proyectos requieren tiempo y que esto solo se logra trabajando con anticipación.
4. **Soldadura y componentes físicos:** Manejar con cuidado la soldadura, aislar con termofit y entender el funcionamiento físico de los interruptores mecánicos previene fallas eléctricas y cortocircuitos.

---

## Siguiente Paso y Pruebas

!!! failure "Pendiente"
    **Falta demostración del carro real:**

- [Video de demostración del carro recorriendo la pista y activando el Failsafe ($\le 2\text{ min}$)](./IMG 7/demostracion_carro_failsafe.mp4)