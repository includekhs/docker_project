# 2nd KHU (Kyung Hee University) College of Electronics and Information Engineering Creative Innovation Project

### Team Name: My Life is NG(New Generation)
### Contest Introduction: Container-Based Embedded System Virtualization<br><br>

<p align="center"><img width="542" alt="KakaoTalk_20231009_164827855" src="https://github.com/includekhs/docker_project/assets/117493461/fe82280e-bfbc-455c-b1e9-6bfd7c23a9f9"></p>

We are building embedded system virtualization using Docker containers.

We aim to implement a single Dockerfile script that is compatible with various hardware platforms, allowing applications to run in containers.

Ultimately, we plan to present a prototype for open-source and embedded system virtualization.<br><br>

We already pushed our own Docker Image on Docker Hub using ```docker buildx build``` command. You can pull the image:

```
docker pull includekhs/final:6.0
```
<br><br>
Used IoT Devices (Orange Pi Zero, Raspberry Pi 4, Orange Pi Zero 3):

- Raspberry Pi 4 (Broadcom BCM2711) $~$ : Sensor(DHT11) application, publisher
- Orange Pi Zero (Allwinner H2+) $~$ : Sensor(DHT22) application, publisher
- Orange Pi Zero 3 (Allwinner H618) $~$ : MQTT broker and subscriber<br><br>

Used Libraries:
```
pyA20==0.2.1
paho-mqtt==1.6.1
Adafruit_DHT==1.4.0
sqlite3=3.40.1
```
<br>

## Alert
After you entered into container, you should set the environment variable which specify the mqtt broker ip address(choose between 2 solutions): 

- Solution 1. Setting ```export BROKER={ip_address}```
- Solution 2. Add some layer ```ENV BROKER={ip_address}``` to Dockerfile

<br><br>

## Raspberry pi 4
To use the DHT sensor on Raspberry Pi 4, GPIO control is required. We used the Adafruit_DHT library for this purpose.

- ```rpi_sensor.py``` reads temperature and humidity data from the sensor.
- ```rpi_mqtt.py``` sends the temperature and humidity data read from the sensor to the MQTT broker (Orange Pi Zero 3) using the paho-mqtt library.

Source code related to this can be easily found and implemented as there are various resources available.<br><br>

## Orangepi zero
To use the DHT sensor on Orange Pi Zero, GPIO control is required. We used open-source pyA20 resources from GitHub.

1. DHT22 sensor reference: https://github.com/jingl3s/DHT11-DHT22-Python-library-Orange-PI.git
2. pyA20 reference: https://github.com/LinhDNguyen/orangepi_zero_gpio.git

- ```opi_sensor.py``` reads temperature and humidity data from the sensor.
  
- ```opi_mqtt.py``` sends the temperature and humidity data read from the sensor to the MQTT broker (Orange Pi Zero 3) using the paho-mqtt library.
  
- ```/DHT11-DHT22-Python-library-Orange-PI/dht.py``` reads temperature and humidity data from the DHT22 sensor and contains helper functions for data validation.

- ```/orangepi_zero_gpio``` is an installation file for the pyA20 library.

<br>

## Orangepi zero3 (UPDATED: 23/10/06)
Orange Pi Zero 3 operates as an MQTT broker and receives temperature/humidity data from Orange Pi Zero and Raspberry Pi 4.<br>

- ```mqtt_broker.py``` receives the temperature and humidity data from Orange pi Zero and Raspberry pi 4. <br><br>

**UPDATED**: We have used ```sqlite3``` database library for using ```docker volume```. And we added ```init_db.py``` file which role is initializing the database configuration.

**UPDATED**: Also, ```mqtt_broker.py``` has ```INSERT``` sql command to operate database

