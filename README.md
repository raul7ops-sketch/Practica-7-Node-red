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

11. Nos vamos al Wokwi y armamos el circuito de la siguiente manera:
![](https://github.com/raul7ops-sketch/Practica-7-Node-red/blob/main/Reporte%207.8.png?raw=true)

12. Copiamos el siguiente codigo y lo agregamos al wokwi.
