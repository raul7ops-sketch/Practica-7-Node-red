# Practica-7-Node-red
## Introduccion
### En este reporte utilizamos un software llamado "Nodo-RED" donde se hace programacion con bloques para poder hacer un servidor donde recibe los datos arrojados por el ESP22 de Wokwi y en el cual estos pueden ser graficados y ponerle indicadores, ya que esto nos permite monitorear a distancia nuestros dispositivos.

## Material y equipo a ejecutar 
- Software Node RED
- ESP22 (WOKWI)
- DHT11
- Sensor ultrasonico
- Un servidor Online
## Pasos a realizar Para la instalacion de Node-RED
1. Entramos a la siguiente pagina y le damos en "get Node" y se descargara el archivo solo debemos darle a instalar.
![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207.png?raw=true)


3. Una vez instalada nos vamos al "CMD" de nuestro dispositivo y le ponemos "Node-Red" y se haabilitara el servidor.

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20.1.png?raw=true)


4. Despues nos vamos al navegado y ponemos la siguiente direccion: 127.0.0.1:1880

5. Armamos nuestros bloques de la siguiente manera.


![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20.2.png?raw=true)




6. En el bloque "mqtt in" se configura de la siguiente manera:

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207.3.png?raw=true)


6.En el bloque "Json" se modifica de la manera siguiente.


![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207.4.png?raw=true)


7. Se agregan tres bloques de funcion, para la temperatura, la humedad y la distancia.

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20temperatura.png?raw=true)

![](https://raw.githubusercontent.com/raul7ops-sketch/Practica-7-Node-red/adc8c7137459043ba22f147e59915d943ab66a70/Reporte%207%20Humedad.png)

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20Distancia.png?raw=true)

8. Agregamos dos bloques, uno es para las graficas y el otro para poner un indicador.

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20.5.png?raw=true)

9. En los bloques de graficas se le pondra lo siguiente con la respectiva funcion que debe llevar:

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207.6.png?raw=true)

10. En los bloques de indicadores se pondra lo siguiente con su respectiva funcion

![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20.7.png?raw=true)

12. Nos vamos al Wokwi y armamos el circuito de la siguiente manera:

    
![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207.8.png?raw=true)

14. Copiamos el siguiente codigo y lo agregamos al wokwi.
``` #include <ArduinoJson.h>
#include <WiFi.h>
#include <PubSubClient.h>
#define BUILTIN_LED 2
#include "DHTesp.h"
const int DHT_PIN = 15;
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 2; 
DHTesp dhtSensor;
long duration;
int distance;
int safetyDistance;
// Update these with values suitable for your network.

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqtt_server = "3.121.19.141";
String username_mqtt="educatronicosiot";
String password_mqtt="12345678";

WiFiClient espClient;
PubSubClient client(espClient);
unsigned long lastMsg = 0;
#define MSG_BUFFER_SIZE  (50)
char msg[MSG_BUFFER_SIZE];
int value = 0;

void setup_wifi() {

  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  randomSeed(micros());

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  // Switch on the LED if an 1 was received as first character
  if ((char)payload[0] == '1') {
    digitalWrite(BUILTIN_LED, LOW);   
    // Turn the LED on (Note that LOW is the voltage level
    // but actually the LED is on; this is because
    // it is active low on the ESP-01)
  } else {
    digitalWrite(BUILTIN_LED, HIGH);  
    // Turn the LED off by making the voltage HIGH
  }

}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str(), username_mqtt.c_str() , password_mqtt.c_str())) {
      Serial.println("connected");
      // Once connected, publish an announcement...
      client.publish("outTopic", "hello world");
      // ... and resubscribe
      client.subscribe("inTopic");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void setup() {
  pinMode(BUILTIN_LED, OUTPUT);     // Initialize the BUILTIN_LED pin as an output
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
}

void loop() {

// Reads the echoPin, returns the sound wave travel time in microseconds


// Calculating the distance
distance= duration*0.034/2;

safetyDistance = distance;
long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(2000);     

delay(1000);
TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  unsigned long now = millis();
  if (now - lastMsg > 2000) {
    lastMsg = now;
    //++value;
    //snprintf (msg, MSG_BUFFER_SIZE, "hello world #%ld", value);

    StaticJsonDocument<128> doc;

    doc["DEVICE"] = "Raul Aguilar";
    //doc["Anho"] = 2022;
    //doc["Empresa"] = "Educatronicos";
    doc["TEMPERATURA"] = String(data.temperature, 1);
    doc["HUMEDAD"] = String(data.humidity, 1);
   doc["DISTANCIA"] = String(d);

    String output;
    
    serializeJson(doc, output);

    Serial.print("Publish message: ");
    Serial.println(output);
    Serial.println(output.c_str());
    client.publish("RaulA", output.c_str());
  }
}
```

## Evidencias y resultados:
