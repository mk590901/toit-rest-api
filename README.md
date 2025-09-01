# Access WEB services in TOIT

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

# Movie II

https://github.com/user-attachments/assets/6144d44d-dfe1-4c26-993e-8ff17d6361f1

