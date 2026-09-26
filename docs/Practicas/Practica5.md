---
titulo: "Sesión 5 — Comunicación Bluetooth"
fecha: 2026-09-25
autor: "Andrea Paola Carmona Casiano, Johan Mauricio Cerqueda Rojas, Francisco Javier Pérez Hernández"
estado: completa   # borrador | completa
---

# Sesión 5 — Comunicación Bluetooth

## Objetivos
- **Enlace Bluetooth:** ✔️ <br>
    ◦ SerialBT.begin() funcionando, comandos recibidos y mostrados en el Monitor Serial. <br>
    ◦ Verificar que el emparejamiento con el celular es estable antes de avanzar.
- **LED con Bluetooth:** ✔️ <br>
    ◦ Control de un LED con los comando ON/OFF desde el celular. <br>
    ◦ Uso correcto de mensaje.trimn(), sin esto la comparación falla.
- **Protocolo de comando:** ✔️ <br>
    ◦ Documentar la tabla comando ➔ acción (mínimo ON/OFF).
- **Experimento de latencia:** ❌ <br>
    ◦ Probar el control con delay(100) en el loop y sin él, sentir la diferencia a proposito <br>
    (No realizada porque no lo vi jeje)

 
## Materiales
- (1×) ESP32 DevKit V1 <br>
- (1×) Cable USB de datos <br>
- (1×) LED + (1×) resistor 220 Ω <br>
- Breadboard y jumpers <br>
- Celular Android (o PC con Bluetooth) + app "Serial Bluetooth Terminal"


## Desarrollo
**Protocolo y Código:** <br>
![Código en el IDE de Arduino y prueba del comando ON en el Monitor Serial](./img_practica_5/ON.png){ width=50% } <br>
*Código en el IDE de Arduino y prueba del comando ON en el Monitor Serial* <br>

![Código en el IDE de Arduino y prueba del comando OFF en el Monitor Serial](./img_practica_5/OFF.png){ width=50% } <br>
*Código en el IDE de Arduino y prueba del comando OFF en el Monitor Serial* <br>


| Comando (Bluetooth) | Acción en el ESP32 | Estado del LED (Pin 23) |
| :---: | :--- | :---: |
| **`ON`** | Pone el pin 23 en nivel alto (`HIGH`) | Encendido |
| **`OFF`** | Pone el pin 23 en nivel bajo (`LOW`) | Apagado |


**Circuito** <br>
![Armado del circuito en protoboard](./img_practica_5/armado.png){ width=50% } <br>
*Armado del circuito en protoboard* <br>

**Demostraciones en Video** <br>
[*Emparejamiento y envío de comandos vía Bluetooth*](./img_practica_5/funcion.mp4) <br>

**Explicación:**<br>

    - **Envío de la señal:** Las instrucciones de texto (ON/OFF) se ingresan en el Monitor Serial de la computadora y se transmiten por aire vía Bluetooth hacia el ESP32.

- **Lectura y limpieza:** Al recibir la señal, el código usa readStringUntil('\n') para capturar el texto y la función mensaje.trim() para eliminar espacios o caracteres invisibles que puedan interrumpir la lectura.   

- **Evaluación lógica:** Mediante una estructura condicional if, el sistema verifica el mensaje si recibe "ON" asigna nivel alto (HIGH) al pin 23 para encender el LED, si recibe "OFF" asigna nivel bajo (LOW) para apagarlo. 

- **Gestión de puertos COM:** Para lograr la comunicación inalámbrica seleccionamos en el IDE el puerto virtual Bluetooth (COM8), reservando el puerto USB físico (COM5) únicamente para la carga del programa.

## Fallas
- **Síntoma:** El programa compilaba sin errores pero el ESP32 no respondía a los comandos enviados por Bluetooth, además de que la tarjeta no estaba vinculada al sistema.
- **Cómo lo encontré:** Se pidió apoyo a la profesora para revisar la falla y se identificó que la transmisión se estaba enviando al puerto USB (COM5) en lugar del canal Bluetooth.
- **Solución:** Se vinculó el ESP32 por Bluetooth a la computadora y se cambió en el IDE de Arduino la salida de puerto al COM8 para enviar la señal inalámbricamente.

## Aprendizajes
En esta práctica se aprendió a enviar y procesar comandos de texto vía Bluetooth para controlar un componente físico, utilizando el Monitor Serial como una interfaz activa de control y aplicando la función trim() para la limpieza de datos. Asimismo, se comprendió la diferencia técnica entre el puerto USB físico asignado a la carga de código (COM5) y el puerto virtual utilizado para la transmisión por bluetooth (COM8).

## Siguiente paso
Como siguiente paso, se buscará aplicar esta transmisión inalámbrica a un proyecto de mayor complejidad, como el control de un carrito robótico.