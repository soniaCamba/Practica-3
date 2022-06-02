# Practica-3

# Practica 3A

## CÓDIGO
```
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>
// SSID & Password
const char* ssid = "****"; // Enter your SSID here
const char* password = "****"; //Enter your Password here
WebServer server(80); // Object of WebServer(HTTP port, 80 is defult)

void handle_root(void);

void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);

// Connect to your wi-fi modem
WiFi.begin(ssid, password);

// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
  delay(1000);
  Serial.print(".");
}

Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}

void loop() {
  server.handleClient();
}

// HTML & CSS contents which display on web server
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
  <h1>TOP 10 RESTAURANTES BARCELONA SEGUN TIMEOUT;</h1>\
  <li> 1.Lasarte </li>\
  <li> 2.Gresca </li>\
  <li> 3.disfrutar </li>\
  <li> 4.Alkimia </li>\
  <li> 5.Cocina hermanos Torres </li>\
  <li> 6.Cinc sentits </li>\
  <li> 7.Direkte Boqueria</li>\
  <li> 8.Casa Xica</li>\
  <li> 9.Hofmann </li>\
  <li> 10.La Gormanda</li>\
</body>\
</html>";

// Handle root url (/)
void handle_root() {
  server.send(200, "text/html", HTML);
}
```

## FUNCIONAMIENTO

Una vez lo tengamos conectado al wifi iniciamos el loop. Dentro llamanos a una función que será la que ejecute el codigo en html.


# Practica 3B

## CODIGO
```
#include <Arduino.h>
#include "BluetoothSerial.h"
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
BluetoothSerial SerialBT;
void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32test"); //Bluetooth device name
  Serial.println("The device started, now you can pair it with bluetooth!");
}

void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read());
  }
  delay(20);
  }
```

## FUNCIONAMIENTO

> Libreria necesaria para que funcione el Bluetooth
```
#include "BluetoothSerial.h"
```
> Habilitamos el Bluetooth
```
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
```
Luego el loop se va a encargar de enviar y recibir los datos a traves del BluetoothSerial que hemos inicializado antes (BluetoothSerial SerialBT;).
