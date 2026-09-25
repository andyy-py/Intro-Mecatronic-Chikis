---
titulo: "Sesión 4 — Sensores 101"
fecha: 2026-08-28
autor: "Andrea Paola Carmona Casiano, Johan Mauricio Cerqueda Rojas, Francisco Javier Pérez Hernández"
estado: completa   # borrador | completa
---

# Sesión 4 — Sensores 101

## Objetivos
- **Potenciómetro:** ✔️ <br>
    ◦ Motor gira en ambos sentidos controlado por in1/in2. <br>
    ◦ Control de velocidad por PWM (mínimo 3 velocidades distintas). <br>
    ◦ Identifica el PWM mínimo de arranque del motor. <br>
- **LM35 (calibración):** ✔️ <br>
    ◦ Mide corriente en arranque y en giro libre (multímetro en serie). <br>
    ◦ Compara ambos valores, ¿cuál es mayor y por qué? <br>
- **Filtro:** ✔️ <br>
    ◦ Demuestra 3 posiciones (0°, 90°, 180°).<br>
    ◦ Muestra el cálculo de duty para cada una. 
- **MPU6050:** ❌
    ◦ Lectura de roll y pitch en al menos 3 inclinaciones distintas.<br>
    ◦ Detección de al menos un impacto con umbral ajustado. <br>
    (No realizada por falta del material)


## Materiales
- (1×) ESP32 DevKit V1 <br>
- (1×) Potenciómetro 10 kΩ <br>
- (1×) LM35 <br>
- (1×) Acelerómetro MPU6050 (módulo I2C) <br>
- Termómetro de referencia (ambiental o infrarrojo del laboratorio) <br>
- Protoboard y jumpers <br>


## Desarrollo
# 1. Curvas de calibración del potenciómetro (ADC vs. ángulo/posición) y del LM35 (T referencia vs. ADC), con la recta ajustada.

Con la recta ajustada: La práctica pide leer el potenciómetro mediante el ADC del ESP32 y relacionar la lectura con posición o ángulo. El ESP32 utiliza valores ADC de 0 a 4095 y la presentación recomienda usar pines ADC1.

![Eje X: lectura ADC, Eje Y: ángulo de referencia.](./img_practica_4/Imagen1.png){ width=50% } ![Eje X: lectura ADC, Eje Y: ángulo de referencia.](./img_practica_4/Imagen2.png){ width=50% } <br>
*Eje X: lectura ADC, Eje Y: ángulo de referencia.* <br>


# 2.Tablas de datos

| Punto | Ángulo de referencia (°) | Lectura ADC | Ángulo calculado (°) | Error (°) |
| :--- | :---: | :---: | :---: | :---: |
| **Mínimo** | 0° | 25 | 1.6° | +1.6° |
| **25%** | 67.5° | 1015 | 66.9° | -0.6° |
| **50%** | 135° | 2055 | 135.5° | +0.5° |
| **75%** | 202.5° | 3060 | 201.8° | -0.7° |
| **Máximo** | 270° | 4080 | 269° | -1° |

<br>
Recorrido de 0° a 270°

| Punto | T<sub>referencia</sub> (°C) | Lectura ADC | T<sub>calculada</sub> (°C) | Error (°C) |
| :--- | :---: | :---: | :---: | :---: |
| **Ambiente** | 24.0 °C | 296 | 23.85 °C | -0.15 °C |
| **Entre dedos** | 30.0 °C | 374 | 30.14 °C | +.14 °C |
| **Lámpara** | 42.0 °C | 518 | 41.74 °C | -0.26 °C |
| **Lata fría** | 18.0 °C | 220 | 17.73 °C | -0.27°C |
| **Otro** | 68.3 | 848 | 68.33 | mismo |

<br>
Temperatura detectada
<br>
Para tener los datos de las tablas, fue necesario realizar el circuito físico e instalar diferentes programas, para que el Arduino detectara el Esp35, una vez lo detectó, fue necesario realizar el código para que este funcionara y mostrara los datos necesarios

![Ángulo cambia a 67.5 y marca diferente ADC](./img_practica_4/Captura1.png){ width=50% } ![Ángulo cambia a 67.5 y marca diferente ADC](./img_practica_4/Imagen3.png){ width=50% } <br>
*Ángulo cambia a 67.5 y marca diferente ADC* <br>

![Ángulo de 135.5, ADC 2055](./img_practica_4/Imagen4.png){ width=50% } <br>
*Ángulo de 135.5, ADC 2055* <br>

![Ángulo: 202.5](./img_practica_4/Imagen5.png){ width=50% } <br>
*Ángulo: 202.5* <br>

![Ángulo: 270](./img_practica_4/Imagen6.png){ width=50% } <br>
*Ángulo: 270* <br>


**Tabla del LM35** <br>
El LM35 entrega aproximadamente $10\text{ mV}$ por cada $\text{°C}$: a $25\text{ °C}$, su salida es $0.25\text{ V}$.

$$
T\,[\text{°C}] = V_{out} \times 100
$$

Además:

$$
V = \frac{ADC}{4095} \times 3.3
$$

Por tanto:

$$
T = \frac{ADC}{4095} \times 3.3 \times 100
$$

Por ejemplo, para $\text{ADC} = 374$:

$$
V = \frac{374}{4095}(3.3)
$$

$$
V \approx 0.3014\text{ V}
$$

Entonces:

$$
T = 0.3014(100)
$$

$$
T \approx 30.14\text{ °C}
$$


![--](./img_practica_4/--){ width=50% }
*--* <br>