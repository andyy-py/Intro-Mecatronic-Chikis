---
titulo: "Sesión 2 — ESP32: Salida, Entrada & Antirrebote"
fecha: 2026-09-04
autor: "Andrea Paola Carmona Casiano, Johan Mauricio Cerqueda Rojas"
estado: completa   # borrador | completa
---

# Sesión 2 — ESP32: Salida, Entrada & Antirrebote

## Objetivos
- **BLINK (salida digital):** ✅ 
    ◦ LED externo en GPIO23 parpadeando a 1 Hz.
- **BLINK con botón (entrada digital):** ✅
    ◦ El LED enciende mientras el botón está presionado (INPUT_PULLUP).
- **TOGGLE con antirrebote:** ✅
    ◦ Cada presión del botón alterna el LED.
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
-- <br>
*¿Por qué con INPUT_PULLUP la lógica queda invertida?*<br>
-- <br>

## Fallas
- **Síntoma:** --
- **Cómo lo encontré:** --
- **Solución:** --

## Aprendizajes
--

## Siguiente paso
--