# P3.B. Bluetooth

## Code: 

    //This example code is in the Public Domain (or CC0 licensed, at your option.)
    //By Evandro Copercini - 2018
    //This example creates a bridge between Serial and Classical Bluetooth (SPP)
    //and also demonstrate that SerialBT have the same functionalities of a normal Serial
    #include "BluetoothSerial.h"
    #if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
    #error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
    #endif
    BluetoothSerial SerialBT; // Creamos la instancia
    void setup() {
        Serial.begin(115200); // Se inicializa la comunicacion a esta velocidad
        SerialBT.begin("ESP32test"); //Se inicia la comunicacion con el nombre del dispositivo
        Serial.println("The device started, now you can pair it with bluetooth!");
    }
    void loop() {
        // Verificar si existen datos en la comunicacion serial del USB, si se encuentran se leen por la misma y se transmiten/envian escribiendose hacia la comunicacion Bluetooth, la que estará connectada a otro dispositivo mediante Bluetooth.
        if (Serial.available())
        { 
            SerialBT.write(Serial.read()); 
        }
        // Verificar si existen datos en el otro dispositivo via Bluetooth conectado a serialBT, si los hay se leen con la misma comunicacion y se escriben a traves del puerto USB, la comunicacion Serial.
        if (SerialBT.available()) {
            Serial.write(SerialBT.read()); 
        }
        delay(20);
    }


## Descripcion: 
Este codigo crea una conexion entre la comunicacion Serial (USB)  y una conexion Bluetooth. Esto permite que los datos enviados por una comunicacion se puedan leer correctamente y sean escritos en la otra. <br><br> Cada funcion se encarga de un trabajo: <br>
<ul>
    <li> setup () --> Se inicializa la conexión junto a la velocidad y nombre, una vez conectado, se reflejara con un mensaje. </li>
    <li> loop () --> Se encarga de comprobar los datos de cada comunicacion a demás de enviar y escribir a la otra comunicacion en caso de encontrar datos. </li>
</ul><br>

### SALIDAS:
Las salidas que se obtienen a través de la impresión serie (puerto USB) serán los datos transmitidos desde la comunicación Bluetooth y los datos recibidos desde la comunicación Serial, dependiendo de la dirección de la comunicación en ese momento.

Si envías datos desde otro dispositivo a través de Bluetooth al ESP32:
<ul>
    <li>Los datos serán recibidos y leídos por el ESP32 a través de la comunicación Bluetooth (SerialBT). Luego, esos datos se enviarán a través de la comunicación Serial (puerto USB) <br>
    Por lo tanto, verás los datos recibidos desde Bluetooth impresos en el monitor serie del puerto USB. </li>
</ul>


Si envías datos desde el ESP32 a través de la comunicación serial tradicional (puerto USB):
<ul>
    <li>Los datos serán leídos por el ESP32 desde la comunicación Serial (USB).
    Luego, esos datos se enviarán a través de la comunicación Bluetooth.<br>
    Por lo tanto, verás los datos enviados desde el ESP32 a través de Bluetooth impresos en el otro dispositivo.</li>
</ul>

### Conclusión: 
Se escribiran los datos que se han leido en otro dispositivo en el monitor serie y viceversa utilizando el Bluetooth.