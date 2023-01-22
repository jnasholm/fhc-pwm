# ESP32 pin schema

The controller IO pins are assigned according to the table below[^1][^2].

|Function|Pin|Comment|
|------------------------------|:--:|--------|
|Zone 1 actuator|16|
|Zone 2 actuator|17|Used for ethernet port if enabled
|Zone 3 actuator|18|Used for ethernet port if enabled
|Zone 4 actuator|19|
|Zone 5 actuator|22|
|Zone 6 actuator|23|Used for ethernet port if enabled
|Zone 7 actuator or Mixing valve actuator decrease|25|
|Zone 8 actuator or Mixing valve actuator increase|26|
|Room temperature sensors (DS18B20)|13|
|Outdoor temperature sensor (DS18B20)|14|
|Distributor line temperature sensors (DS18B20)|27|
|System status LED|32|
|Controller status LED|33|  
|Battery voltage|35|May require soldering on the ESP32 board to enable|
|Charging voltage|39|May require soldering on the ESP32 board to enable|

[^1]: [ESP32-Devkit-Lipo GPIOs map](https://www.olimex.com/Products/IoT/ESP32/ESP32-DevKit-LiPo/resources/ESP32-DevKit-Lipo-GPIOs.png)
[^2]: [ESP32 Pinout Reference: Which GPIO pins should you use?](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
