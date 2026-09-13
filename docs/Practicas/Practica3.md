---
titulo: "Sesión 3 — Motor DC, Puente H & Servo"
fecha: 2026-09-11
autor: "Andrea Paola Carmona Casiano, Johan Mauricio Cerqueda Rojas, Francisco Javier Pérez Hernández"
estado: completa   # borrador | completa
---

# Sesión 3 — Motor DC, Puente H & Servo

## Objetivos
- **Dirección y velocidad (motor DC):** ✅ <br>
    ◦ Motor gira en ambos sentidos controlado por in1/in2. <br>
    ◦ Control de velocidad por PWM (mínimo 3 velocidades distintas). <br>
    ◦ Identifica el PWM mínimo de arranque del motor. <br>
- **Prueba de carga:** ✅ <br>
    ◦ Mide corriente en arranque y en giro libre (multímetro en serie).
    ◦ Compara ambos valores — ¿cuál es mayor y por qué?
- **Servo:** ✅ <br>
    ◦ Demuestra 3 posiciones (0°, 90°, 180°).<br>
    ◦ Muestra el cálculo de duty para cada una.

 
## Materiales
- (1×) ESP32 DevKit V1 <br>
- (1×) Driver TB6612 (puente H) <br>
- (1–2×) Motor DC TT con caja reductora <br>
- (1×) Servo SG90 (o similar) <br>
- (1×) Potenciómetro 10 kΩ <br>
- Fuente/batería para motores (separada del ESP32, GND común) <br>
- Multímetro <br>
- Protoboard y jumpers <br>

## Desarrollo
**Esquemáticos** <br>
![--](./img_practica_3/esquema_1.png){ width=50% } <br>
*--* <br>

![--](./img_practica_3/esquema_2.png){ width=50% } <br>
*--* <br>


**Códigos** <br>
![--](./img_practica_3/codigo_1.png){ width=50% } <br>
*--* <br>

![--](./img_practica_3/codigo_2.png){ width=50% } <br>
*--* <br>

**Demostraciones en Video** <br>
[*--*](./img_practica_3/esquematico_1.mp4) <br>

[*--*](./img_practica_3/esquematico_2.mp4) <br>


**Explicación:** <br>
En esta práctica se llevó a cabo la simulación y el control de dos motores DC mediante una tarjeta Arduino UNO y un módulo de puente H (L293D) en la plataforma Tinkercad, donde el circuito y su programación permitieron gestionar de manera precisa el funcionamiento de los motores al enviar señales digitales para activar el movimiento en un sentido, ejecutar una parada temporal y posteriormente invertir la dirección del giro.

## Fallas
- **Síntoma:** Al iniciar la simulación en Tinkercad, ninguno de los dos motores DC presentaba movimiento , a pesar de que el código parecía estar ejecutando las funciones de movimiento.
- **Cómo lo encontré:** Con el apoyo de herramientas de inteligencia artificial para auditar la estructura del programa y la lógica de conexiones, se identificó y corrigió el código fuente mediante la declaración de las variables de habilitación (enable1 y enable2) en los pines PWM del Arduino.
- **Solución:** Tras cargar el código corregido y ejecutar la simulación, el motor respondio inmediatamente, logrando realizar la secuencia de avance, paro y reversa de manera automatizada y continua.

## Aprendizajes
--

## Siguiente paso
--