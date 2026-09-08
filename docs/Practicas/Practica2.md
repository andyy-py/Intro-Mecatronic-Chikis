---
titulo: "Sesión 2 — ESP32: Salida, Entrada & Antirrebote"
fecha: 2026-09-04
autor: "Andrea Paola Carmona Casiano, Johan Mauricio Cerqueda Rojas, Francisco Javier Pérez Hernández"
estado: completa   # borrador | completa
---

# Sesión 2 — ESP32: Salida, Entrada & Antirrebote

## Objetivos
- **BLINK (salida digital):** ✅ <br>
    ◦ LED externo en GPIO23 parpadeando a 1 Hz.
- **BLINK con botón (entrada digital):** ✅ <br>
    ◦ El LED enciende mientras el botón está presionado (INPUT_PULLUP).
- **TOGGLE con antirrebote:** ✅ <br>
    ◦ Cada presión del botón alterna el LED. <br>
    ◦ Sin delay().

 
## Materiales
- (1×) ESP32 DevKit V1 (WROOM-32) <br>
- (1×) Cable USB de datos <br>
- (1×) LED + (1×) resistor 220 Ω <br>
- (1×) Push button <br>
- (1×) Resistor 10 kΩ (opcional) <br>
- Protoboard y jumpers <br>


## Desarrollo
**Esquemáticos** <br>
![Esquemático básico para parpadeo de LED](./img_practica_2/blink_esquema.png){ width=50% } <br>
*Esquemático de conexión BLINK* <br>

![Esquemático para control mediante botón y antirrebote](./img_practica_2/boton_rebote_esquema.png){ width=50% } <br>
*Esquemático de conexión BLINK CON BOTÓN Y TOGGLE* <br>


**Códigos** <br>
![Código fuente del programa Blink](./img_practica_2/blink_codigo.png){ width=50% } <br>
*Código implementado de BLINK* <br>

![Código fuente del programa Blink con botón](./img_practica_2/blink_boton.png){ width=50% } <br>
*Código implementado de BLINK CON BOTÓN* <br>

![Código fuente del programa Antirrebote](./img_practica_2/rebote_codigo.png){ width=50% } <br>
*Código implementado de TOGGLE* <br>


**Demostraciones en Video** <br>
[*Demostración de funcionamiento: BLINK*](./img_practica_2/blink.mp4) <br>

[*Demostración de funcionamiento: BLINK CON BOTÓN*](./img_practica_2/blink_boton.mp4) <br>

[*Demostración de funcionamiento: TOGGLE*](./img_practica_2/rebote.mp4) <br>


**Explicación:** <br>
*¿Qué es el rebote de un botón?*<br>
Es el "chispazo" mecánico que ocurre al presionar un botón. Sus placas metálicas internas rebotan microsegundos antes de asentarse, se siente un solo clic pero la ESP32 es tan rápida que lee 10 clics seguidos. <br>
*¿Por qué con INPUT_PULLUP la lógica queda invertida?*<br>
La ESP32 alimenta el pin internamente todo el tiempo, por lo que mientras el botón está suelto el pin detecta energía y marca HIGH. Al presionarlo mandas esa corriente a tierra y la apagas marcando LOW. <br>

## Fallas
- **Síntoma:** Al intentar subir el código a la ESP32, aparecía un error indicando que el puerto estaba ocupado o no existía. El cable USB actuaba simplemente como un cargador, pero no transmitía datos.
- **Cómo lo encontré:** Revisamos el Administrador de Dispositivos en la computadora dentro de la sección de puertos COM y LPT. Notamos que no detectaba ninguna placa conectada por USB y únicamente aparecían los puertos del Bluetooth.
- **Solución:** Identificamos el chip de la tarjeta ESP32 el cual es el CP2102 e instalamos su driver correspondiente en la laptop. Tras la instalación, la computadora reconoció el puerto bajo el nombre Silicon Labs CP210x USB to UART Bridge, logrando así que los datos se transmitieran correctamente a la tarjeta.

## Aprendizajes
En esta práctica aprendimos la importancia de configurar correctamente los controladores de comunicación entre la computadora y la tarjeta ESP32 antes de intentar subir cualquier código. También comprendimos cómo funciona la lógica de programación para salidas digitales, cómo manejar entradas con el modo INPUT_PULLUP y cómo solucionar los problemas de rebote en botones tanto por hardware como por software.

## Siguiente paso
Para las siguientes prácticas buscamos aprender más sobre programación para poder hacer sistemas un poco más complejos, entender bien su funcionamiento y lograr armarlos correctamente antes de ponerlos en práctica.