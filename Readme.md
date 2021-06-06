
# Practica 6.1: LECTURA/ESCRITURA DE MEMORIA SD


## Codigo de la practica

```
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
File myFile;
void setup(){
Serial.begin(9600);
Serial.print("Iniciando SD ...");
SPI.begin(18,19,23,4);
if(! SD.begin(4)){
Serial.println("No se pudo inicializar");
return;
}
Serial.println("inicializacion exitosa");
// Escribimos en el fichero
myFile = SD.open("/archivo.txt",FILE_WRITE);
myFile.println("Hola mundo!!");
myFile.close();
myFile=SD.open("/archivo.txt"); //Abrimos, mostramos y leemos
if (myFile) {
Serial.println("archivo.txt:");
while (myFile.available()) {
Serial.write(myFile.read());
}
myFile.close();
}
else {
Serial.println("Error al abrir el archivo");
}
}
void loop(){}
```

# Funcionamiento 

Antes de empezar con el codigo tenemos que cargar las tres librerias, con la segunda libreria podremos comunicarnos con los dispositivos SPI y con la libreria SD podremos leer y escribir en tarjetas SD:

```
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
```
Definimos el `void setup()`, dentro de el inicializamos una comunicación en serie a una velocidad de 9600 bauds y una impresió por pantalla:

```
Serial.begin(9600);
Serial.print("Iniciando SD ...");
```

Indicamos los pines de conexión para el bus SPI:

`SPI.begin(18,19,23,4);`

Seguidamente creamos un if, si este if no se inicia en el pin 4 , se imprimira por pantalla "No se pudo inicializar".

```
if (!SD.begin(4)) {
Serial.println("No se pudo inicializar");
return;
}
```

Si la inicialización ha sido exitosa, se iprimira este mensaje:

`Serial.println("inicializacion exitosa");`

Abrimos un archivo con `SD.open()`. En el caso de que el fichero no exista, tenemos la opción FILE_WRITE que nos permitira poder crear un archivo y asi abrirlo.

`myFile = SD.open("/archivo.txt",FILE_WRITE);`

Escribimos cualquier cosa en este archivo con `myFile.println()` y despues lo cerramos.

```
myFile.println("Hola mundo!!");
myFile.close();
```

Seguidamente creamos otro if, su funcionamiento es que si el archivo mencionado se ha abierto, imprimiremos por pantalla los que este escrito, para hacer esto utilizaremos 
`while (myFile.available())`, y por ultimo cerramos el if con `myFile.close()`.

```
if (myFile) {
Serial.println("archivo.txt:");
while (myFile.available()) {
Serial.write(myFile.read());
}
myFile.close();
```
Para finalizar haremos un else que cuando no se habra el archivo, imprima por pantalla:

```
else {
Serial.println("Error al abrir el archivo");
}
```

# Salida de esta practica por el terminal

```
Iniciando SD ...sd init ok
inicializacion exitosa
Hola mundo!!
```