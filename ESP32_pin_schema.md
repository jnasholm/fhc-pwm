# ESP32 pin schema

The controller IO pins are assigned according to the table below[^1][^2][^3].

## Prototype 0.1

|Function|Pin|Comment|
|------------------------------|:--:|--------|
|Zone 1 actuator|16|
|Zone 2 actuator|17|Used for ethernet port if enabled
|Zone 3 actuator|18|Used for ethernet port if enabled
|Zone 4 actuator|19|
|Zone 5 actuator|22|
|Zone 6 actuator|23|Used for ethernet port if enabled
|Zone 7 actuator|25|
|Zone 8 actuator|26|
|Room temperature sensors (DS18B20)|13|
|Room climate sensors (I2C SDA)|13|Alternative use
|Outdoor temperature sensor (DS18B20)|14|
|Room climate sensors (I2C SCL)|14|Alternative use
|Distributor line temperature sensors (DS18B20)|27|
|System status LED|32|
|Controller status LED|33|  
|Battery voltage|35|May require soldering on the ESP32 board to enable|
|Charging voltage|39|May require soldering on the ESP32 board to enable|

## Prototype 0.2

|Function|Pin|Comment|
|------------------------------|:--:|--------|
|Zone 1 actuator|04|
|Zone 2 actuator|16|
|Zone 3 actuator|17|Used for ethernet port if enabled
|Zone 4 actuator|18|Used for ethernet port if enabled
|Zone 5 actuator|19|
|Zone 6 actuator|21|
|Zone 7 actuator|22|
|Zone 8 actuator|23|Used for ethernet port if enabled
|Temperature sensors bus 0 (DS18B20)|25|
|Temperature sensors bus 1 (DS18B20)|26|
|Room climate sensors (I2C SDA)|14|
|Room climate sensors (I2C SCL)|27|
|Outdoor temperature sensor (NTC)|34|Downstream configuration with 5.66kOhm resistance at 5°C
|Outdoor temperature sensor power (NTC)|13|Supply +3.3V over a 10kOhm resistor
|System status LED|32|
|Controller status LED|33|  
|Battery voltage|35|May require soldering on the ESP32 board to enable|
|Charging voltage|39|May require soldering on the ESP32 board to enable|

[^1]: [ESP32-Devkit-Lipo GPIOs map](https://www.olimex.com/Products/IoT/ESP32/ESP32-DevKit-LiPo/resources/ESP32-DevKit-Lipo-GPIOs.png)
[^2]: [ESP32-POE-ISO GPIO map](https://www.olimex.com/Products/IoT/ESP32/ESP32-POE-ISO/resources/ESP32-POE-ISO-GPIO.png)
[^3]: [ESP32 Pinout Reference: Which GPIO pins should you use?](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
