# Escáner I2C con Arduino

Este programa escanea todos los dispositivos conectados al bus I2C de un Arduino y muestra sus direcciones en el monitor serial.

## Código

```cpp
#include <Arduino.h>
#include <Wire.h>

void setup() {
    Wire.begin();
    Serial.begin(115200);
    while (!Serial); // Leonardo: espera a que se inicie el monitor serial
    Serial.println("\nI2C Scanner");
}

void loop() {
    byte error, address;
    int nDevices;
    Serial.println("Scanning...");
    nDevices = 0;

    for (address = 1; address < 127; address++) {
        // El escáner I2C utiliza el valor de retorno de Wire.endTransmission()
        // para ver si un dispositivo respondió a la dirección.
        Wire.beginTransmission(address);
        error = Wire.endTransmission();

        if (error == 0) {
            Serial.print("I2C device found at address 0x");
            if (address < 16)
                Serial.print("0");
            Serial.print(address, HEX);
            Serial.println(" !");
            nDevices++;
        } else if (error == 4) {
            Serial.print("Unknown error at address 0x");
            if (address < 16)
                Serial.print("0");
            Serial.println(address, HEX);
        }
    }

    if (nDevices == 0)
        Serial.println("No I2C devices found\n");
    else
        Serial.println("done\n");

    delay(5000); // espera 5 segundos para el próximo escaneo
}


## Objetivo
El objetivo de este programa es escanear el bus I2C en busca de dispositivos y mostrar sus direcciones en el monitor serial. Es útil para identificar las direcciones I2C de los dispositivos conectados y asegurarse de que están funcionando correctamente.

### Funcionamiento
***Inicialización en setup(): ***

- Wire.begin(): Inicia la comunicación I2C.
- Serial.begin(115200): Inicia la comunicación serial a una velocidad de 115200 baudios.
- Espera a que el monitor serial esté listo (útil para placas como el Arduino Leonardo).
- Imprime "I2C Scanner" en el monitor serial.

***Escaneo en loop():***

- Imprime "Scanning..." en el monitor serial.
- Recorre todas las posibles direcciones I2C (de 1 a 126).
- Para cada dirección, intenta iniciar una transmisión I2C.
- Si un dispositivo responde (sin error), imprime la dirección del dispositivo en el monitor serial.
- Si ocurre un error desconocido (código de error 4), lo informa.
- Después de escanear todas las direcciones, informa si se encontraron dispositivos o no.
- Espera 5 segundos antes de iniciar un nuevo escaneo.
- Explicación de las Funciones Clave
- Wire.begin(): Inicializa el bus I2C.
- Serial.begin(115200): Configura la velocidad de la comunicación serial.
- Wire.beginTransmission(address): Inicia una transmisión I2C hacia la dirección especificada.
- Wire.endTransmission(): Finaliza una transmisión I2C y devuelve un valor que indica si hubo éxito o un error.
- Serial.print() y Serial.println(): Imprimen texto en el monitor serial.

###  Uso Práctico
Este programa es extremadamente útil para desarrolladores que trabajan con múltiples dispositivos I2C y necesitan identificar sus direcciones. Al ejecutar este escáner, se puede verificar rápidamente la conexión y el funcionamiento de los dispositivos I2C conectados al Arduino.

Este programa es una herramienta esencial para cualquier proyecto que utilice comunicación I2C, proporcionando una manera fácil y rápida de detectar dispositivos y solucionar problemas de conexión.