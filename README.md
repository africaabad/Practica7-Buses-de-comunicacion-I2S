# PRACTICA 7 :  Buses de comunicación III (I2S)  

El objetivo de la practica actual es describir el funcionamiento del bus I2S y realizar una practica para comprender su  funcionamiento 

Alumna: **Africa Abad**

![Homer](https://s3.abcstatics.com/media/play/2018/08/22/homer-simpson-kJU--1248x698@abc.JPG)

***PROGMEM:*** 
modificador de almacenamiento en C y C++ que se utiliza en microcontroladores para indicar que los datos deben ser almacenados en la memoria de programa (flash) en lugar de en la memoria RAM. La memoria de programa (PROGMEM) es una sección de la memoria del microcontrolador reservada para almacenar datos que no necesitan ser modificados durante la ejecución del programa, como por ejemplo, datos constantes, tablas, configuraciones, y en este caso, archivos de audio.



## Ejercicio Practico 1  reproducción desde memoria interna

1. Descibir la salida por el puerto serie 

La salida por el puerto serie es

    AAC done

cada vez que el archivo AAC haya terminado de reproduirse. Esto nos proporciona una indicación a tiempo real de cuándo ha finalizado la reproducción del archivo AAC.


2. Explicar el funcionamiento 

El código `.cpp`está diseñado para reproducir un archivo de audio AAC almacenado en memoria de programa (PROGMEM) utilizando un generador de audio AAC y una salida de audio I2S. 

El archivo AAC era la voz de Homer Simpson.

1. Inclusión de bibliotecas

- `Arduino.h`
- `AudioGeneratorAAC.h`: biblioteca para generar audio a partir de archivos AAC.
- `AudioOutputI2S.h`: biblioteca para la salida de audio utilizando el protocolo I2S.
- `AudioFileSourcePROGMEM.h`: proporciona una forma de leer archivos de audio almacenados en la memoria de programa (PROGMEM).
- `sampleaac.h`: archivo de audio AAC que se reproducirá. Está almacenado en la memoria de programa (PROGMEM).



2. Declaración de variables globales:

    AudioFileSourcePROGMEM *in;

`in`:  puntero al objeto `AudioFileSourcePROGMEM`, que se utiliza para acceder al archivo de audio almacenado en PROGMEM.

    AudioGeneratorAAC *aac;

`aac`: puntero al objeto `AudioGeneratorAAC`, que se encarga de decodificar y generar audio a partir del archivo AAC.

    AudioOutputI2S *out;

`out`: puntero al objeto `AudioOutputI2S`, que se utiliza para la salida de audio mediante el protocolo I2S.

3. Función `setup()`:

    ```cpp
    void setup() {
    Serial.begin(115200);
    audioLogger = &Serial;
    in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
    aac = new AudioGeneratorAAC();
    out = new AudioOutputI2S();
    aac->begin(in, out);
    }
    ```

Se inicia la comunicación serial a una velocidad de 115200 baudios para la salida de mensajes de depuración.

Se establece `audioLogger` para dirigir la salida de mensajes de audio al puerto serie.

Se crea el objeto `AudioFileSourcePROGMEM` llamado `in`, `AudioGeneratorAAC` llamado `aac` y `AudioOutputI2S` llamado `out`.

Se llama al metodo `begin()` de `aac`, pasando como argumentos el objeto `in` y `out`, para iniciar la reproducción del audio AAC.

4. Función `loop()`:

    ```cpp
    void loop() {
      if (aac->isRunning()) {
        aac->loop();
      } else {
        Serial.printf("AAC done\n");
        delay(1000);
      }
    }
    ```

Se verifica si el generador de audio AAC está en funcionamiento.
- Si está en funcionamiento, se llama al método `loop()` de aac para procesar el audio.
- Si la reproducción ha finalizado, se imprime "AAC done" por el puerto serie y se espera un segundo antes de repetir el ciclo.
