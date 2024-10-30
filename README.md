# Unidad 4:
Aprendizaje Obtenido
Estos ejercicios permitieron aplicar la comunicación en tiempo real entre Unity y Arduino usando el plugin Ardity, y profundizar en la sincronización de ambos sistemas a través de la comunicación serial.

Ejercicio 1: Introducción a Ardity
Se exploró el funcionamiento básico del plugin y sus configuraciones. Las escenas de ejemplo ayudaron a comprender cómo Unity envía y recibe datos de Arduino a través de una conexión serial.

Ejercicio 2: Concepto de Hilos
Se analizó la importancia de los hilos en Unity para realizar la comunicación en tiempo real sin bloquear el hilo principal del juego. Aprender cómo Ardity utiliza un hilo secundario para la transmisión continua de datos fue clave para entender la multitarea en Unity.

Ejercicio 3: Análisis de Escena DemoScene_UserPoll_ReadWrite
Aquí se revisó en detalle la comunicación bidireccional entre Unity y Arduino:

Código en Arduino: Envía un mensaje cada dos segundos y responde a comandos específicos, sin bloqueos.
Código en Unity: El script envía y recibe datos en cada frame, y se aprendió cómo asociar el SerialController en Unity para lograr una conexión estable.
Conclusión
La práctica mostró cómo integrar Unity con Arduino usando hilos y comunicación serial, permitiendo una interacción fluida en tiempo real.
