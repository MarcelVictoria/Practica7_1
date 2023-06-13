# PRACTICA 7
En esta práctica reproduciremos música tanto desde la memoria interna como desde una tarjeta SD externa.

## EJERCICIO 1: Reproducción desde la memoria interna
Para este ejercicio sólo necesitamos un altavoz y cables. Para el montaje hemos seguido la siguiente asignación de pines:
- LRC --> G25
- BCLK --> G26
- DIN --> G22
- GND --> GND
- Vino --> 3.3V


El funcionamiento del programa consiste en: los datos de sonido se almacenan como una matriz en la RAM interna del ESP32. 
Usamos la placa de conexión audio MAX98357 I2S para decodificar la señal digital en una señal analógica. Encontramos los datos del audio en el archivo *sampleaac.h*. Accedemos a este archivo en el setup:
```cpp
in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
```
Ésta es la señal de entrada. A continuación deben convertirse los datos en un audio:
```cpp
aac = new AudioGeneratorAAC();
```
Y por último que salgan por el puerto de salida, en este caso un altavoz.
```cpp
out = new AudioOutputI2S();
out -> SetGain(0.125);
out -> SetPinout(26,25,22);
aac->begin(in, out);
```
Cuando se termina el audio, por pantalla se muestra en bucle el mensaje *Sound Generator*. Esto es lo que tenemos programado en el loop:
```cpp
void loop(){
if (aac->isRunning()) {
aac->loop();
} else {
aac -> stop();
Serial.printf("Sound Generator\n");
delay(1000);
}
}
```
## EJERCICIO 2: Reproducción de un archivo WAV desde una tarjeta SD externa
El código de este ejercicio y su correspondiente informe lo podemos encontrar en el repositorio Practica7_ex2.