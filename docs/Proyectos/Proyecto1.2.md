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

## Lista de materiales (BOM)

| Componente | Precio | Obtención |
| :--- | :--- | :--- |
| Portapilas | $70 | Comprado |
| Power Bank | $350 | Comprado |
| Baterías | $440 | Comprado |
| Servo | $100 | Comprado |
| ESP32 | $180 | Reutilizado |
| Puente H TB6612 | $102 | Prestado |
| Motores TT con caja reductora | $140 | Prestado |
| Llantas de plástico | $49 | Comprado |
| Rueda loca | $20 | Comprado |
| Tornillos y tuercas | $10 | Prestado |
| Power Bank adicional | $350 | Comprado |
| Impresión del chasis | $16 | Prestado |
| Impresión de la carrocería | $60 | Prestado |
| Jumpers | $60 | Prestado |
| Mini switch | $20 | Comprado |

> **Nota:** Se conservan los dos registros de Power Bank de la lista original.

---

## Análisis de riesgos

| Riesgo | Solución propuesta |
| :--- | :--- |
| Sobreesfuerzo del servo | Reemplazarlo por otro servo. |
| Desprendimiento de una soldadura | Volver a soldar la conexión. |
| Descarga de las baterías | Utilizar baterías de repuesto cargadas. |
| Motores con poca velocidad | Revisar la alimentación y la capacidad de corriente. |
| El carro no se mueve | Verificar conexiones, alimentación y código. |

---

## Bitácora de fallas

| ¿Qué falló? | ¿Cómo lo encontraron? | ¿Cómo lo resolvieron? |
| :--- | :--- | :--- |
| El servo hacía movimientos destinados a los motores. | Se utilizó la aplicación prototipo para controlar el servo y los motores. | Se corrigieron conexiones sueltas. |
| El servo nunca se detenía. | Se probó el carro con el control de Xbox. | Se ajustó el código para detener el servo. |
| El servo dejó de funcionar por sobreesfuerzo. | El servo externo dejó de girar después de corregir el código. | Se consiguió un reemplazo, pero no hubo tiempo para instalarlo y se descartó su uso. |
| El carro no se movía. | Se encendió varias veces y ninguno de los motores respondió. | Se revisaron conexiones, se cambiaron las baterías y se volvió a cargar el código. |

---

## Video de funcionamiento

<p>En el siguiente video se puede observar el funcionamiento del carro.</p>

<video controls width="100%" style="max-width: 750px; display: block; margin: 15px auto; border-radius: 8px;">
    <source src="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/videos/VideoCarroBT.mp4" type="video/mp4">
    Tu navegador no puede reproducir este video.
</video>

[Abrir video en nueva pestaña](https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/videos/VideoCarroBT.mp4){ .md-button target="_blank" }

---

## Requisitos vs. resultados

| # | Requisito | Resultado |
| :-: | :--- | :---: |
| 1 | Movimiento mediante Bluetooth | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 2 | Control de velocidad PWM | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 3 | Protocolo documentado | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 4 | Sistema failsafe | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 5 | Interruptor físico | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 6 | Alimentación separada y GND común | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 7 | Cableado ordenado | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 8 | Superficie para marcador ArUco | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 9 | Control mediante Xbox | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 10 | Pinzas rígidas | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 11 | Dos ruedas traseras y una frontal | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 12 | Cables de motores atravesando el chasis | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 13 | Carrocería protectora | <span style="color: #167647; font-weight: bold;">Sí</span> |
| 14 | Servo para patear la pelota | <span style="color: #b42318; font-weight: bold;">No</span> |

---

## Lecciones aprendidas

1. **Hacer el trabajo con anticipación:** Avanzar desde que se asignó el proyecto permitió contar con tiempo para corregir los errores que aparecieron.
2. **Verificar las conexiones:** En varias ocasiones se encontró que una conexión se había soltado y, al corregirla, el sistema volvió a funcionar.
3. **Comprar materiales adecuados:** El portapilas resultó demasiado pequeño para las pilas que se utilizarían, lo que generó un gasto adicional.

---

## Créditos

<p>
    Se utilizó Claude AI para generar la estructura del diagrama
    del código, el esquemático de conexiones y la estructura
    del README del carro.
</p>