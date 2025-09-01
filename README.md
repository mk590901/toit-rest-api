# TOIT and REST API

Below is an application that allows you to turn the __ESP32S3__ chip into an __MQTT__ client of weather service.

## Introduction

Toit supports network protocols, including __HTTP__, which makes it suitable for interacting with __REST APIs__ implemented on third-party servers as a client, sending __HTTP__ requests. In this case, you can use this feature to create an application that provides __weather data__ upon request. The __ESP32 chip__ for instance like to given below, can easily be turned into an independense __MQTT client__ that provides, for example, an android application with the ability to request weather information.

![weather_esp32s3](https://github.com/user-attachments/assets/4c45358e-fa72-47c9-a2bb-ef897690c2ec)

## Weather services used in app

The application interacts with two cloud services:
* https://openweathermap.org/api, which allows you to get geo-location in the form [latitude, longitude] based on the request [city, country code]. The request must also contain an __API key__, which can be obtained by registering on the site.
* https://api.open-meteo.com/, which provides a free __REST API__ for obtaining current weather data based on [latitude, longitude] and a list of weather variables, details on the website open-meteo.com

## Implementation

The application consists of two parts:
> MQTT client in the _mqtt_bridge.toit_ file, which allows
* Receive data for a request pair [city, country code]. For example, "Tokyo, JP", "Jerusalem, IL" or "Rovaniemi, FI", 
* Create and execute a request using the __doneRequest__ function and send the received weather data to the desired topic via the MQTT bridge.
> The __doneRequest__ function, in the _weather.toit_ file, which receives data for a request and returns a json string with current weather data.

NB! Details on using __HTTP__ requests in __TOIT__ can be found at https://docs.toit.io/tutorials/network/http.

[weather.webm](https://github.com/user-attachments/assets/07172905-7f2b-4f62-ae97-89b6d4c27dc2)

## Application management

> Installing packages:

* __mqtt__
```
$ jag pkg install github.com/toitware/mqtt@v2
```
* __http__
```
$ jag pkg install github.com/toitlang/pkg-http@v2
```
* __certificate-roots__
```
$ jag pkg install github.com/toitware/toit-cert-roots@v1
```

> Loading the application:

```
micrcx@micrcx-desktop:~/toit/rest$ jag run -d midi mqtt_bridge.toit
Scanning for device with name: 'midi'
Running 'mqtt_bridge.toit' on 'midi' ...
Success: Sent 144KB code to 'midi' in 3.58s
micrcx@micrcx-desktop:~/toit/rest$
```

> Monitoring
```
micrcx@micrcx-desktop:~/toit/rest$ jag monitor -p /dev/ttyACM1
Starting serial monitor of port '/dev/ttyACM1' ...
ESP-ROM:esp32s3-20210327
Build:Mar 27 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0x8 (SPI_FAST_FLASH_BOOT)
Saved PC:0x40385b12
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fce2810,len:0xdc
load:0x403c8700,len:0x4
load:0x403c8704,len:0xa08
load:0x403cb700,len:0x257c
entry 0x403c8854
[toit] INFO: starting <v2.0.0-alpha.184>
[toit] DEBUG: clearing RTC memory: powered on by hardware source
[toit] INFO: running on ESP32S3 - revision 0.2
[toit] INFO: using SPIRAM for heap metadata and heap
[wifi] DEBUG: connecting
E (4027) wifi:Association refused too many times, max allowed 1
[wifi] WARN: connect failed {reason: unknown reason (208)}
[wifi] DEBUG: closing
[jaguar] WARN: running Jaguar failed due to 'CONNECT_FAILED: unknown reason (208)' (1/3)
[wifi] DEBUG: connecting
E (6777) wifi:Association refused too many times, max allowed 1
[wifi] WARN: connect failed {reason: unknown reason (208)}
[wifi] DEBUG: closing
[jaguar] WARN: running Jaguar failed due to 'CONNECT_FAILED: unknown reason (208)' (2/3)
[wifi] DEBUG: connecting
[wifi] DEBUG: connected
[wifi] INFO: network address dynamically assigned through dhcp {ip: 192.168.1.147}
[wifi] INFO: dns server address dynamically assigned through dhcp {ip: [192.168.1.1]}
[jaguar.http] INFO: running Jaguar device 'midi' (id: '1429cbbc-ca45-4c11-88ee-9500a1c318dc') on 'http://192.168.1.147:9000'
[jaguar] INFO: program 640bb80f-b6f2-b084-5981-e8acd22b8c07 started
DEBUG: connected to broker
DEBUG: connection established
Connected to MQTT broker broker.hivemq.com
Received: weather_out/topic: {"request":"Nuuk,GL"}
Received message on 'weather_out/topic': {"request":"Nuuk,GL"}
hasRequest->true hasCmd->false
request->Nuuk,GL
processing->[Nuuk,GL]
answer->[{"Location":"Nuuk,GL, (64.18, -51.74)","Temperature":"6.10 °C","Wind Speed":"28.60 km/h","Surface Pressure":"1002.20 (hPa)","Relative Humidity":"92.00 %","Precipitation":"0.20 mm"}]
Received: weather_out/topic: {"request":"Calgary,CA"}
Received message on 'weather_out/topic': {"request":"Calgary,CA"}
hasRequest->true hasCmd->false
request->Calgary,CA
processing->[Calgary,CA]
answer->[{"Location":"Calgary,CA, (51.05, -114.07)","Temperature":"13.00 °C","Wind Speed":"8.80 km/h","Surface Pressure":"896.30 (hPa)","Relative Humidity":"73.00 %","Precipitation":"0.00 mm"}]
Received: weather_out/topic: {"request":"Beijing,CN"}
Received message on 'weather_out/topic': {"request":"Beijing,CN"}
hasRequest->true hasCmd->false
request->Beijing,CN
processing->[Beijing,CN]
answer->[{"Location":"Beijing,CN, (39.91, 116.39)","Temperature":"30.60 °C","Wind Speed":"12.80 km/h","Surface Pressure":"1000.00 (hPa)","Relative Humidity":"54.00 %","Precipitation":"0.00 mm"}]
Received: weather_out/topic: {"request":"Moscow,RU"}
Received message on 'weather_out/topic': {"request":"Moscow,RU"}
hasRequest->true hasCmd->false
request->Moscow,RU
processing->[Moscow,RU]
answer->[{"Location":"Moscow,RU, (55.75, 37.62)","Temperature":"23.90 °C","Wind Speed":"5.50 km/h","Surface Pressure":"996.20 (hPa)","Relative Humidity":"55.00 %","Precipitation":"0.00 mm"}]
Received: weather_out/topic: {"request":"Rovaniemi,FI"}
Received message on 'weather_out/topic': {"request":"Rovaniemi,FI"}
hasRequest->true hasCmd->false
request->Rovaniemi,FI
processing->[Rovaniemi,FI]
answer->[{"Location":"Rovaniemi,FI, (66.50, 25.72)","Temperature":"10.00 °C","Wind Speed":"4.30 km/h","Surface Pressure":"1002.10 (hPa)","Relative Humidity":"70.00 %","Precipitation":"0.00 mm"}]
Received: weather_out/topic: {"request":"Helsinki,FI"}
Received message on 'weather_out/topic': {"request":"Helsinki,FI"}
hasRequest->true hasCmd->false
request->Helsinki,FI
processing->[Helsinki,FI]
answer->[{"Location":"Helsinki,FI, (60.17, 24.94)","Temperature":"15.90 °C","Wind Speed":"11.50 km/h","Surface Pressure":"1008.50 (hPa)","Relative Humidity":"93.00 %","Precipitation":"0.00 mm"}]
Received: weather_out/topic: {"request":"Reykjavik,IS"}
Received message on 'weather_out/topic': {"request":"Reykjavik,IS"}
hasRequest->true hasCmd->false
request->Reykjavik,IS
processing->[Reykjavik,IS]
answer->[{"Location":"Reykjavik,IS, (64.15, -21.94)","Temperature":"10.10 °C","Wind Speed":"7.20 km/h","Surface Pressure":"1000.70 (hPa)","Relative Humidity":"83.00 %","Precipitation":"0.00 mm"}]
^C
micrcx@micrcx-desktop:~/toit/rest$  

```

# Third-party flutter MQTT client app allows send requests to and receive results to/from ESP32S3 chip

https://github.com/user-attachments/assets/6144d44d-dfe1-4c26-993e-8ff17d6361f1

