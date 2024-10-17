# Bonificación unidad 1
## Diagramas de estado:
![image](https://github.com/user-attachments/assets/8fa2b440-dd64-4c44-8d04-cb32e35c834c)
## Proceso de concepción y construcción de la experiencia:
La experiencia del juego se concibió como una metáfora de la creciente crisis climática, enfocada en las decisiones difíciles que los seres humanos deben tomar en condiciones extremas. Queríamos crear una situación de presión constante, donde el jugador, como padre de familia, tuviera que balancear recursos limitados con la supervivencia de sus seres queridos. Este concepto se cristalizó a través de un ciclo repetitivo de decisiones que simulan la urgencia y la ansiedad de enfrentar una crisis de temperatura extrema.

El proceso de construcción comenzó con el diseño del sistema de decisiones. Se establecieron los diferentes estados de hidratación de cada miembro de la familia y cómo estos cambiarían en función de las condiciones ambientales y las decisiones del jugador. A continuación, se trabajó en la integración del sensor de temperatura para hacer que la experiencia fuera dinámica y que la dificultad aumentara de forma realista según los datos de temperatura del mundo real. Finalmente, se definió un temporizador que regiría cada ciclo de 30 segundos, permitiendo una experiencia rítmica pero tensa.

El juego fue testeado con múltiples variaciones de la temperatura para ajustar la dificultad y hacer que las decisiones fueran impactantes pero no abrumadoras. Se consideraron las respuestas emocionales de los jugadores para ajustar la narrativa y los gráficos, creando un entorno que reflejara la gravedad de las decisiones tomadas bajo presión.

Este proceso colaborativo y iterativo fue clave para crear una experiencia inmersiva donde el jugador se sintiera tanto en control como atrapado por las circunstancias, enfrentando un dilema moral constante.

## Manual del desarrollador: programas micro y Unity:
Introducción
El juego de supervivencia bajo condiciones de calor extremo está diseñado como una experiencia interactiva, conectando un entorno virtual desarrollado en Unity con datos del mundo real obtenidos a través de un sensor de temperatura conectado a una Raspberry Pi. Este manual cubre tanto la estructura general del juego como los aspectos técnicos detallados de la integración entre Unity y la Raspberry Pi. Además, se describen las funciones clave, las interacciones entre las diferentes capas del sistema y las mejores prácticas para depurar y optimizar el rendimiento.

Arquitectura del sistema
El sistema se divide en dos partes principales: el juego en Unity y la lectura de datos de temperatura a través de la Raspberry Pi. La comunicación entre ambos sistemas se realiza a través de un protocolo de comunicación serial o vía red local, dependiendo de la configuración implementada.

Unity: Se encarga de la lógica del juego, la interfaz de usuario y el renderizado. En este entorno, se desarrollan las mecánicas de decisiones, los ciclos de hidratación y el control del temporizador.
Raspberry Pi: Lee los datos del sensor de temperatura (por ejemplo, un sensor DHT22 o DS18B20) y envía esta información a Unity en intervalos regulares para ajustar la dificultad del juego en tiempo real.
Diagrama de flujo general
El juego sigue un flujo cíclico de 30 segundos, donde el jugador debe tomar decisiones sobre cómo gestionar la hidratación de su familia. Mientras tanto, el sensor de temperatura mide constantemente el ambiente y ajusta el ritmo de deshidratación de los personajes.

Inicio del ciclo:
Unity recibe los datos del sensor de temperatura.
Se ajustan los parámetros de deshidratación de los personajes en función de la temperatura.
Toma de decisiones del jugador:
El jugador tiene 30 segundos para decidir si gastar dinero en agua o sacrificar hidratación.
Ejecución de las decisiones:
Según las decisiones del jugador, los niveles de hidratación de los personajes se ajustan.
Se actualizan los recursos disponibles.
Fin del ciclo:
Se verifica si algún personaje ha alcanzado un nivel crítico de deshidratación. Si es así, se termina el juego.
Si no, comienza un nuevo ciclo con posibles variaciones de temperatura.


Programas micro: Raspberry Pi y sensor de temperatura
El código en la Raspberry Pi está diseñado para leer datos de un sensor de temperatura y enviarlos a Unity mediante un puerto serial o una conexión TCP/UDP. A continuación, se describen los componentes clave del código en Raspberry Pi:

1. Lectura del sensor
El programa en la Raspberry Pi incluye una librería para interactuar con el sensor de temperatura. Dependiendo del sensor que se utilice, la configuración del hardware puede variar. En general, el programa sigue los siguientes pasos:

Inicializa el sensor.
Realiza lecturas periódicas de la temperatura cada segundo.
Envía los datos a Unity en un formato predefinido, como una cadena JSON que contiene los datos de la temperatura actual.
2. Envío de datos a Unity
La comunicación con Unity puede realizarse mediante dos métodos:

Serial: Se utiliza un puerto serial para enviar los datos de temperatura directamente a Unity.
Red local (Socket TCP/UDP): Se utiliza un socket de red para establecer comunicación entre la Raspberry Pi y Unity, lo que permite una mayor flexibilidad en la comunicación, especialmente si los dispositivos no están directamente conectados.
Programas Unity: Lógica del juego
En Unity, el desarrollo del juego se basa en una serie de scripts organizados en torno a la mecánica de decisiones, la actualización de hidratación y el control del temporizador. A continuación, se detallan los aspectos clave del código en Unity:

1. Controlador de temperatura
Este script es responsable de recibir los datos de la Raspberry Pi, interpretarlos y ajustarlos dentro del contexto del juego.

Recepción de datos: Este componente escucha los datos enviados por la Raspberry Pi (ya sea a través del puerto serial o red) y los procesa.
Ajuste de dificultad: Dependiendo de la temperatura recibida, se ajustan los parámetros del juego, como la velocidad de deshidratación de los personajes o el costo del agua.
2. Gestión del temporizador y ciclos de decisión
El ciclo de 30 segundos está gestionado por un temporizador que controla la interfaz de usuario y el momento en que el jugador debe tomar decisiones.

Temporizador: Un script controla el temporizador y emite eventos cuando se agota el tiempo, lo que fuerza al jugador a tomar una decisión.
Ciclo de juego: Una vez que se toma una decisión, se ajustan las variables del juego (recursos y niveles de hidratación) y se inicia el siguiente ciclo.
3. Mecánica de decisiones
Las decisiones del jugador están centradas en la gestión de recursos y la supervivencia de la familia. El jugador puede elegir entre gastar dinero en agua o sacrificar hidratación. La lógica de estas decisiones está gestionada por un script dedicado a la actualización de las variables correspondientes:

Actualizar hidratación: Basado en la decisión del jugador, se reduce o incrementa la hidratación de cada personaje.
Control de recursos: Si se gasta dinero en agua, los recursos disponibles del jugador se actualizan.
4. Manejo de eventos críticos
Si la hidratación de algún personaje cae a cero, el juego termina. Esto es gestionado por un evento crítico que evalúa constantemente las variables de hidratación:

Verificación continua: Cada ciclo, el juego verifica si alguno de los personajes ha llegado a un nivel crítico de hidratación. Si esto ocurre, se dispara un evento de "derrota" y se detiene el juego.
5. Interfaz de usuario
La interfaz gráfica muestra el estado actual de la familia: niveles de hidratación, temperatura ambiental, tiempo restante en el ciclo y recursos disponibles. Un script de interfaz de usuario actualiza estos elementos en tiempo real para mantener al jugador informado.

Pruebas y depuración
Testeo del sensor: Antes de integrar completamente el sensor con Unity, se realizan pruebas aisladas para garantizar que los datos de temperatura se leen y envían correctamente.
Pruebas de ciclos: Se simulan diferentes valores de temperatura para comprobar cómo varía la dificultad del juego y ajustar los parámetros de deshidratación en consecuencia.

## Manual de usuario:
Introducción
Bienvenido al juego de supervivencia en un mundo afectado por altas temperaturas. En este juego, asumirás el papel de un padre de familia que debe tomar decisiones cruciales para mantener a tu esposa y dos hijos hidratados, mientras las temperaturas extremas aumentan constantemente. Utilizarás tus recursos limitados de dinero para comprar agua o, en casos más difíciles, decidirás sacrificar la hidratación de un miembro de la familia para que los demás sobrevivan. El reto es mantener a todos con vida mientras luchas contra un entorno hostil, donde las decisiones rápidas y calculadas son clave para la supervivencia.

Este manual te guiará en el funcionamiento básico y avanzado del juego.

Inicio del juego
Pantalla principal: Al iniciar el juego, te recibirán con la pantalla de inicio, donde puedes ver el estado de tu familia: sus niveles de hidratación, la temperatura ambiental y los recursos disponibles (dinero).
Condiciones iniciales: Todos los miembros de la familia comenzarán con niveles de hidratación moderados, pero la temperatura aumentará progresivamente con cada ciclo de 30 segundos.
Controles del jugador
Botones de decisión:

Comprar agua: Gasta una parte de tus recursos (dinero) para adquirir una botella de agua. Podrás asignar esta agua a cualquiera de los miembros de la familia.
Sacrificar hidratación: En lugar de comprar agua, puedes decidir reducir el nivel de hidratación de uno de los miembros de la familia para ahorrar recursos.
Asignación de agua: Si decides comprar agua, el juego te dará la opción de asignar esa agua a uno de los miembros de tu familia. Podrás elegir entre:

Padre
Madre
Hijo 1
Hijo 2
El objetivo es mantener a la familia hidratada, ya que si el nivel de hidratación de algún miembro cae a cero, ese personaje morirá y perderás el juego.

Ciclos del juego
El juego se divide en ciclos de 30 segundos, durante los cuales tendrás que tomar decisiones para mantener a tu familia con vida:

Impacto de la temperatura: La temperatura del entorno, medida en tiempo real a través del sensor de la Raspberry Pi, influirá directamente en la velocidad a la que los niveles de hidratación disminuyen. A mayor temperatura, más rápidamente se deshidratarán los personajes.

Niveles de hidratación: Cada miembro de la familia tiene una barra de hidratación que se irá vaciando gradualmente según las condiciones ambientales. Si no tomas decisiones a tiempo, los niveles de hidratación caerán, y si algún personaje llega a cero, perderás el juego.

Estados y variables importantes
Estrategias de supervivencia
Equilibrio de recursos: Es crucial mantener un balance entre gastar dinero y sacrificar hidratación. Gasta solo cuando sea necesario y asegúrate de priorizar a los miembros que se encuentran en mayor peligro.
Monitoreo constante: Mantén un ojo en los niveles de temperatura y en las barras de hidratación. Un ligero descuido puede ser la diferencia entre la vida y la muerte de un miembro de la familia.
Decisiones rápidas: El ciclo de 30 segundos te da poco tiempo para pensar, por lo que debes familiarizarte rápidamente con los controles y la dinámica del juego.
Final del juego
El juego puede terminar de dos maneras:

Victoria: Si logras mantener a tu familia hidratada durante un número determinado de ciclos (generalmente 10 ciclos o más, dependiendo de la dificultad), se te mostrará un mensaje de victoria, indicando que has sobrevivido al calor extremo.
Derrota: Si alguno de los miembros de la familia pierde completamente su hidratación, el juego termina. Un mensaje en pantalla te informará de la muerte de ese personaje y te dará la opción de reiniciar el juego.
