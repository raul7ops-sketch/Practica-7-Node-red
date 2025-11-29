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
2. Una vez instalada nos vamos al "CMD" de nuestro dispositivo y le ponemos "Node-Red" y se haabilitara el servidor.
![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20.1.png?raw=true)
3. Despues nos vamos al navegado y ponemos la siguiente direccion: 127.0.0.1:1880
4. Armamos nuestros bloques de la siguiente manera.
![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207%20.2.png?raw=true)
5. En el bloque "mqtt in" se configura de la siguiente manera:
