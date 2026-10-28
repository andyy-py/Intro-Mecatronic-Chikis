<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Proyecto 1 | Carro por Bluetooth</title>

    <style>
        * {
            box-sizing: border-box;
            scroll-behavior: smooth;
        }

        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #f2f5f9;
            color: #243044;
            line-height: 1.7;
        }

        header {
            background: #102a43;
            color: white;
            text-align: center;
            padding: 45px 20px;
        }

        header h1 {
            font-size: 2.3rem;
            margin-bottom: 10px;
        }

        nav {
            background: #176b87;
            padding: 14px;
            text-align: center;
        }

        nav a {
            color: white;
            text-decoration: none;
            margin: 8px 12px;
            display: inline-block;
        }

        nav a:hover {
            text-decoration: underline;
        }

        main {
            max-width: 1050px;
            margin: 25px auto;
            padding: 0 18px;
        }

        section {
            background: white;
            padding: 25px;
            margin-bottom: 25px;
            border-radius: 12px;
            box-shadow: 0 3px 12px #0000000d;
        }

        h2 {
            color: #176b87;
            border-bottom: 2px solid #dce8ef;
            padding-bottom: 8px;
        }

        h3 {
            color: #102a43;
        }

        img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
            display: block;
            margin: 15px auto;
        }

        video {
            width: 100%;
            max-width: 750px;
            display: block;
            margin: 15px auto;
            border-radius: 8px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
        }

        th, td {
            border: 1px solid #d5dce5;
            padding: 10px;
            text-align: left;
        }

        th {
            background: #176b87;
            color: white;
        }

        tr:nth-child(even) {
            background: #f2f7fa;
        }

        .tabla-contenedor {
            overflow-x: auto;
        }

        .integrantes {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 12px;
        }

        .integrante {
            background: #edf5f8;
            padding: 15px;
            border-radius: 8px;
        }

        .cumplido {
            color: #167647;
            font-weight: bold;
        }

        .no-cumplido {
            color: #b42318;
            font-weight: bold;
        }

        .boton {
            display: inline-block;
            background: #176b87;
            color: white;
            text-decoration: none;
            padding: 12px 18px;
            border-radius: 7px;
            margin: 8px 0;
        }

        footer {
            background: #102a43;
            color: white;
            text-align: center;
            padding: 22px;
        }

        @media (max-width: 600px) {
            header h1 {
                font-size: 1.7rem;
            }

            section {
                padding: 17px;
            }
        }
    </style>
</head>

<body>

<header>
    <h1>Proyecto 1: Carro por Bluetooth</h1>
    <p>Universidad Iberoamericana Puebla</p>
    <p>Materia: Introducción a la Mecatrónica</p>
</header>

<nav>
    <a href="#integrantes">Integrantes</a>
    <a href="#objetivo">Objetivo</a>
    <a href="#requisitos">Requisitos</a>
    <a href="#arquitectura">Arquitectura</a>
    <a href="#plan">Plan de trabajo</a>
    <a href="#materiales">Materiales</a>
    <a href="#riesgos">Riesgos</a>
    <a href="#fallas">Fallas</a>
    <a href="#resultados">Resultados</a>
    <a href="#video">Video</a>
    <a href="#lecciones">Lecciones</a>
</nav>

<main>

    <section id="integrantes">
        <h2>Integrantes del equipo</h2>
        <div class="integrantes">
            <div class="integrante">Daniel Alejandro Trejo Macías</div>
            <div class="integrante">Carlos Mario Diaz Moscoso</div>
            <div class="integrante">Johan Mauricio Cerqueda Rojas</div>
            <div class="integrante">Jose Pablo Sanchez Guerrero</div>
            <div class="integrante">Abraham Ramirez Garcia</div>
        </div>
    </section>

    <section id="objetivo">
        <h2>Objetivo</h2>
        <p>
            Construir en equipo un carro controlado por Bluetooth,
            integrando los conocimientos del curso de electrónica,
            microcontroladores, actuadores, comunicación y mecanismos.
            El carro será utilizado en un torneo de fútbol RC.
        </p>
    </section>

    <section id="requisitos">
        <h2>Requisitos funcionales</h2>
        <ul>
            <li>Movimiento hacia adelante, atrás, izquierda, derecha y alto mediante Bluetooth.</li>
            <li>Control de velocidad por PWM, con al menos tres niveles.</li>
            <li>Documentación del protocolo de comandos.</li>
            <li>Sistema failsafe: detener el carro si no recibe comandos durante un segundo.</li>
            <li>Interruptor físico de encendido y apagado accesible.</li>
            <li>Alimentación de motores separada del ESP32, con tierra común (GND).</li>
            <li>Cableado ordenado y sujeto al chasis.</li>
            <li>Superficie superior despejada de al menos 6 × 6 cm para el marcador ArUco.</li>
        </ul>

        <h3>Requisitos propios del equipo</h3>
        <ul>
            <li>Control mediante un mando de Xbox.</li>
            <li>Pinzas rígidas en la parte delantera para maniobrar la pelota.</li>
            <li>Dos ruedas traseras y una rueda frontal.</li>
            <li>Cables de los motores atravesando el chasis para conectarlos al puente H.</li>
            <li>Carrocería para mantener protegidos los componentes.</li>
            <li>Uso de un servo en las pinzas para patear la pelota.</li>
        </ul>
    </section>

    <section id="arquitectura">
        <h2>Arquitectura del sistema</h2>
        <p>
            El ESP32 recibe las instrucciones del control y administra
            los movimientos del carro. El puente H TB6612FNG permite
            controlar los motores, mientras que la alimentación de los
            motores se mantiene separada de la del microcontrolador.
        </p>
        <img
            src="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/imgs/DiagramaBloques.png"
            alt="Diagrama de bloques del carro por Bluetooth">
        <a class="boton"
           href="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/imgs/DiagramaBloques.png"
           target="_blank">Ver diagrama completo</a>
    </section>

    <section id="plan">
        <h2>Plan de trabajo por semanas</h2>

        <h3>Semana 1</h3>
        <ul>
            <li>Programación del código inicial.</li>
            <li>Diseño del chasis.</li>
            <li>Diseño y programación de la aplicación de prueba.</li>
        </ul>

        <h3>Semana 2</h3>
        <ul>
            <li>Cableado del ESP32, puente H y motores.</li>
            <li>Diseño de la carrocería.</li>
            <li>Soldadura de cables.</li>
            <li>Impresión del chasis y la carrocería.</li>
            <li>Ajustes al código para manejar el carro con el control.</li>
        </ul>

        <h3>Boceto del chasis</h3>
        <img
            src="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/imgs/BocetoChasis.png"
            alt="Boceto del chasis del carro">
    </section>

    <section id="materiales">
        <h2>Lista de materiales (BOM)</h2>
        <div class="tabla-contenedor">
            <table>
                <thead>
                    <tr>
                        <th>Componente</th>
                        <th>Precio</th>
                        <th>Obtención</th>
                    </tr>
                </thead>
                <tbody>
                    <tr><td>Portapilas</td><td>$70</td><td>Comprado</td></tr>
                    <tr><td>Power Bank</td><td>$350</td><td>Comprado</td></tr>
                    <tr><td>Baterías</td><td>$440</td><td>Comprado</td></tr>
                    <tr><td>Servo</td><td>$100</td><td>Comprado</td></tr>
                    <tr><td>ESP32</td><td>$180</td><td>Reutilizado</td></tr>
                    <tr><td>Puente H TB6612</td><td>$102</td><td>Prestado</td></tr>
                    <tr><td>Motores TT con caja reductora</td><td>$140</td><td>Prestado</td></tr>
                    <tr><td>Llantas de plástico</td><td>$49</td><td>Comprado</td></tr>
                    <tr><td>Rueda loca</td><td>$20</td><td>Comprado</td></tr>
                    <tr><td>Tornillos y tuercas</td><td>$10</td><td>Prestado</td></tr>
                    <tr><td>Power Bank adicional</td><td>$350</td><td>Comprado</td></tr>
                    <tr><td>Impresión del chasis</td><td>$16</td><td>Prestado</td></tr>
                    <tr><td>Impresión de la carrocería</td><td>$60</td><td>Prestado</td></tr>
                    <tr><td>Jumpers</td><td>$60</td><td>Prestado</td></tr>
                    <tr><td>Mini switch</td><td>$20</td><td>Comprado</td></tr>
                </tbody>
            </table>
        </div>
        <p><strong>Nota:</strong> Se conservan los dos registros de Power Bank de la lista original.</p>
    </section>

    <section id="riesgos">
        <h2>Análisis de riesgos</h2>
        <div class="tabla-contenedor">
            <table>
                <thead>
                    <tr>
                        <th>Riesgo</th>
                        <th>Solución propuesta</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Sobreesfuerzo del servo</td>
                        <td>Reemplazarlo por otro servo.</td>
                    </tr>
                    <tr>
                        <td>Desprendimiento de una soldadura</td>
                        <td>Volver a soldar la conexión.</td>
                    </tr>
                    <tr>
                        <td>Descarga de las baterías</td>
                        <td>Utilizar baterías de repuesto cargadas.</td>
                    </tr>
                    <tr>
                        <td>Motores con poca velocidad</td>
                        <td>Revisar la alimentación y la capacidad de corriente.</td>
                    </tr>
                    <tr>
                        <td>El carro no se mueve</td>
                        <td>Verificar conexiones, alimentación y código.</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </section>

    <section id="fallas">
        <h2>Bitácora de fallas</h2>
        <div class="tabla-contenedor">
            <table>
                <thead>
                    <tr>
                        <th>¿Qué falló?</th>
                        <th>¿Cómo lo encontraron?</th>
                        <th>¿Cómo lo resolvieron?</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>El servo hacía movimientos destinados a los motores.</td>
                        <td>Se utilizó la aplicación prototipo para controlar el servo y los motores.</td>
                        <td>Se corrigieron conexiones sueltas.</td>
                    </tr>
                    <tr>
                        <td>El servo nunca se detenía.</td>
                        <td>Se probó el carro con el control de Xbox.</td>
                        <td>Se ajustó el código para detener el servo.</td>
                    </tr>
                    <tr>
                        <td>El servo dejó de funcionar por sobreesfuerzo.</td>
                        <td>El servo externo dejó de girar después de corregir el código.</td>
                        <td>Se consiguió un reemplazo, pero no hubo tiempo para instalarlo y se descartó su uso.</td>
                    </tr>
                    <tr>
                        <td>El carro no se movía.</td>
                        <td>Se encendió varias veces y ninguno de los motores respondió.</td>
                        <td>Se revisaron conexiones, se cambiaron las baterías y se volvió a cargar el código.</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </section>

    <section id="video">
        <h2>Video de funcionamiento</h2>
        <p>En el siguiente video se puede observar el funcionamiento del carro.</p>
        <video controls>
            <source
                src="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/videos/VideoCarroBT.mp4"
                type="video/mp4">
            Tu navegador no puede reproducir este video.
            <a href="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/videos/VideoCarroBT.mp4">
                Ver video
            </a>
        </video>
        <a class="boton"
           href="https://danielalejandrotmgh.github.io/IntroMecatronicaEquipo7/recursos/videos/VideoCarroBT.mp4"
           target="_blank">Abrir video</a>
    </section>

    <section id="resultados">
        <h2>Requisitos vs. resultados</h2>
        <div class="tabla-contenedor">
            <table>
                <thead>
                    <tr>
                        <th>#</th>
                        <th>Requisito</th>
                        <th>Resultado</th>
                    </tr>
                </thead>
                <tbody>
                    <tr><td>1</td><td>Movimiento mediante Bluetooth</td><td class="cumplido">Sí</td></tr>
                    <tr><td>2</td><td>Control de velocidad PWM</td><td class="cumplido">Sí</td></tr>
                    <tr><td>3</td><td>Protocolo documentado</td><td class="cumplido">Sí</td></tr>
                    <tr><td>4</td><td>Sistema failsafe</td><td class="cumplido">Sí</td></tr>
                    <tr><td>5</td><td>Interruptor físico</td><td class="cumplido">Sí</td></tr>
                    <tr><td>6</td><td>Alimentación separada y GND común</td><td class="cumplido">Sí</td></tr>
                    <tr><td>7</td><td>Cableado ordenado</td><td class="cumplido">Sí</td></tr>
                    <tr><td>8</td><td>Superficie para marcador ArUco</td><td class="cumplido">Sí</td></tr>
                    <tr><td>9</td><td>Control mediante Xbox</td><td class="cumplido">Sí</td></tr>
                    <tr><td>10</td><td>Pinzas rígidas</td><td class="cumplido">Sí</td></tr>
                    <tr><td>11</td><td>Dos ruedas traseras y una frontal</td><td class="cumplido">Sí</td></tr>
                    <tr><td>12</td><td>Cables de motores atravesando el chasis</td><td class="cumplido">Sí</td></tr>
                    <tr><td>13</td><td>Carrocería protectora</td><td class="cumplido">Sí</td></tr>
                    <tr><td>14</td><td>Servo para patear la pelota</td><td class="no-cumplido">No</td></tr>
                </tbody>
            </table>
        </div>
    </section>

    <section id="lecciones">
        <h2>Lecciones aprendidas</h2>
        <ol>
            <li>
                <strong>Hacer el trabajo con anticipación.</strong>
                Avanzar desde que se asignó el proyecto permitió contar
                con tiempo para corregir los errores que aparecieron.
            </li>
            <li>
                <strong>Verificar las conexiones.</strong>
                En varias ocasiones se encontró que una conexión se
                había soltado y, al corregirla, el sistema volvió a funcionar.
            </li>
            <li>
                <strong>Comprar materiales adecuados.</strong>
                El portapilas resultó demasiado pequeño para las pilas
                que se utilizarían, lo que generó un gasto adicional.
            </li>
        </ol>
    </section>

    <section id="creditos">
        <h2>Créditos</h2>
        <p>
            Se utilizó Claude AI para generar la estructura del diagrama
            del código, el esquemático de conexiones y la estructura
            del README del carro.
        </p>
    </section>

</main>

<footer>
    <p>Universidad Iberoamericana Puebla</p>
    <p>Introducción a la Mecatrónica | Proyecto 1</p>
</footer>

</body>
</html>