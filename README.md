# fhc-pwm
<!-- [![GitHub release (latest by date)](https://img.shields.io/github/v/release/jnasholm/fhc-pwm)](https://github.com/jnasholm/fhc-pwm/releases) -->
<!-- ![GitHub last commit](https://img.shields.io/github/last-commit/jnasholm/fhc-pwm) -->

# ESP32 SSR Floor Heating Controller with zone thermostats and modulated actuators

Project to create a smart floor heating controller for private homes with a hydronic underfloor system and optionally combined with a radiator system mixing valve. Brief project description:

- The controller is an [ESP32](https://www.olimex.com/Products/IoT/ESP32/ESP32-DevKit-LiPo/open-source-hardware) programmed with [ESPHome](https://esphome.io/).
- The climate thermostats are based on ESPHome PID control[^1][^2].
- The [zone valve actuators](https://www.roth-sverige.se/fileadmin/user_upload/Roth_North_Europe/Images_for_Roth_North_Europe/Sverige/PDF_files_for_produkter/Touchline_SL/Datasheet_Roth_reglermotor_24V_och_230V_SE_20200312.pdf) are operated with 24 VAC (wax motor[^3] type without direct position feedback).
- The zone valve actuators are controlled with ESPHome PWM.
- The switching modules are equipped with [solid state relays](https://www.velleman.eu/products/view?id=461416).
- The controller can operate up to 8 zone valve actuators.
- Integration with [Home Assistant](https://www.home-assistant.io/) is recommended. Sensor data and configuration parameters can be imported to operate the controller.
- Direct communication between ESP nodes through the network broadcast [UDP Component](https://esphome.io/components/udp) in ESPHome from version 2024.9.

> [!TIP]
> In case the supply line controller needs the same upgrade: [ESP32 SSR Mixing valve actuator controller](https://github.com/jnasholm/mvc-pwm)

> [!NOTE]
> The controller hardware is theoretically capable of operating 110-230 VAC zone valve  actuators. This will however not be tested in the project.

If you preferr to replace the zone valve actuators with [proportinal servo actuators](https://homematic-ip.com/en/product/valve-drive-motorised), have a look at [this project](https://github.com/nliaudat/floor-heating-controller).

## Description
The controller is designed to be a drop-in replacement for legacy hydronic floor heating controllers. Existing hydronic manifold will be used and may be upgraded with modern valve actuators with lower power consumption. Room temperature data can be supplied either through sensors connected directly to the controller or through sensors registered in Home Assistant. This is also the case for outdoor temperature data.

Room temperature data can be provided by digital sensors like the [DS18B20](https://www.electrokit.com/en/product/ds18b20-to-92-temperature-sensor/) which also works perfectly fine for outdoor temperature data.

Room climate data can be provided either by the Bosch [BMP280](https://www.bosch-sensortec.com/products/environmental-sensors/pressure-sensors/bmp280/) temperature and pressure sensor, or the Bosch [BME280](https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/) temperature, humidity and pressure sensor. These sensors are connected to the controllers I<sup>2</sup>C bus with a configurable adress (0x76 or 0x77). Connect a [multiplexer](https://www.electrokit.com/produkt/tca9548a-i2c-multiplexer-8ch) to the controllers I<sup>2</sup>C bus in order to extend the possible number of connected sensors. To increase the total wire length of the I<sup>2</sup>C bus, also connect an active [extender](https://www.electrokit.com/produkt/adafruit-i2c-extender-ltc4311).

Alternatively the [RHT03](https://www.electrokit.com/en/product/temp-moist-sensor-rht03/) sensor can be used for room temperature data which would give relative humidity data as well.

> [!TIP]
> Upgrade for legacy floor heating room thermostats: [ESP Room Climate Sensor](https://github.com/jnasholm/rcs)

Modulation of the valve actuators is done with solid state relay modules which have galvanic isolation between control signal pins and switching terminals. The modules are equipped with one solid state relay for each valve actuator which usually equals the number of zones to control. Each solid state relay can switch a current up to 2 A. Reasonably modern valve actuators draw a current of 0.3 A during initial warm-up, which can last for about 2 minutes. After warm-up the power consumption for each actuator is usually 1 W. Connecting two actuators to each solid state relay is possible in case a zone is controlled by more than one actuator.

### Development building
My home is a two floor detached building built in 2010. Insulation is quite good and the thermal inertia is very high. Large windows to the south add a considerable amount of heat on sunny days even during wintertime. The ground floor is heated with hydronic underfloor heating and the first floor is heated by radiators. The heat source is a gas boiler with a modulating controller and a climate compensated outdoor temperature sensor. Each room on the ground floor is a separate heating control zone with a room temperature sensor. The living room is one zone heated with two separate regions of heating tube and therefore has two valve actuators, controlled by a single room temperature sensor. In total 4 heating zones and 5 valve actuators. The supply line temperature to the hydronic heating distributor is controlled by a 3-way mixing valve operated by an electric motor actuator. A rudimentary (not smart) controller continuously adjusts the mixing valve setting based on measured outdoor temperature and supply line temperature feedback.

## Controller function

The smart floor heating controller has three principal modes of operation, automatic, summer and manual. Chose the mode of operation from the front-end or through automations in Home Assistant.

### Automatic mode
This is the active state (1) for the actuators. The thermostat function and modulation is in effect for each zone individually. Each actuator is operated completely based on parameter settings.

When the first actuator is switched on, the indicator to run the circulation pump is also switched on. When the last actuator is switched off, the indicator to run the circulation pump is also switched off. This can be used to start and stop the circulation pump if needed.

### Summer mode
This is the active state (2) for the actuators. No thermostat function or modulation is in effect. All actuators are switched on and the indicator to run the circulation pump is also switched on.

The controller enters summer mode when the outdoor temperature is above the configured temperature. This mode will maintain circulation in all zones regardless of current room temperature in each zone. The controller resumes automatic mode when the outdoor temperature is 6°C below the configured temperature.

### Manual mode
This is the idle state (3) for the actuators. No thermostat function or modulation is in effect. The actuators are operated in the front-end through manual switches, one for each zone. Each actuator remains energised until the corresponding switch in the front-end is turned off.

### Controller block diagram

```mermaid
flowchart TD
    A[Zone Temperature 1-8]==>B[Zone Actuator Controller]
    C[Outdoor Temperature]==>B
    B==>D[Zone Actuator 1-8]
```

## Front-end configuration and control

|Configuration and control|Description|Default|
|----------------------------|--------------------------------------|:------:|
|**Controller mode**|Switch on for automatic mode, switch off for manual mode.|on|
|**Zone 1, 2, 3, ... activate**|Switch on to open the corresponding zone valve, switch off to close the corresponding zone valve. Functioning in manual mode only.|off|
|**Summer mode**|Switch on for summer mode, switch off for normal mode.|off|
|**Summer mode temperature**|Enable summer mode above this outdoor temperature.|20°C|

## Static controller configuration parameters

The following parameters are configurable in the source code of ```control-actuators.yaml``` before compilation and upload to the controller. The same control parameters can be used for zones that are similar in area and thermal characteristics. The control parameters will have to be adjusted for zones that differ in area and thermal characteristics.

|Parameter|Value|Description|
|--------------------|:----:|--------------------------------------------------|
|```kp```|4.8|PID climate proportional control parameter|
|```ki```|0.00208333|PID climate integral control parameter|
|```kd```|12.0|PID climate derivative control parameter|
|```output_averaging_samples```|3|PID climate output averaging|
|```derivative_averaging_samples```|5|PID climate derivative averaging|
|```threshold_high```|0.16°C|PID climate deadband upper limit|
|```threshold_low```|0°C|PID climate deadband lower limit|
|```kp_multiplier```|0.04|PID climate deadband proportional scaling|
|```ki_multiplier```|0.06|PID climate deadband integral scaling|
|```kd_multiplier```|0|PID climate deadband derivative scaling|
|```min_temperature```|16.0°C|PID climate thermostat lower limit|
|```max_temperature```|26.0°C|PID climate thermostat upper limit|
|```temperature_step```|0.1°C|PID climate thermostat minimum change|

> [!NOTE]
> Configuration parameters are entered for each zone individually.

## PID controller parameters

Finding and fine tuning the PID controller parameters can be a bit easier with a structured approach. I have had the best results with the [Ziegler-Nichols method](https://en.wikipedia.org/wiki/Ziegler%E2%80%93Nichols_method) (Åström and Hägglund). Suggested reading on this topic is the [Principles of PID](https://blog.opticontrols.com/archives/344).

This is how the parameters for my current controller are calculated. The same control parameters are used for all zones. Work in progress and probably far from optimized.

### Winter season 2024-2025

|Parameter|Value|Description or calculation|
|--------------------|:----:|--------------------------------------------------|
|Sampling time, Ts|10 s|Update interval of the zone temperature sensor|
|Critical gain, Kc|8.0|Proportional constant causing oscillation|
|Critical period, Pc|960 s|Cycle time at oscillating condition|
|Integral time, Ti|480 s|0.50 x Pc|
|Derivative time, Td|120 s|0.25 x Ti|
|Proportional constant, kp|4.8|0.60 x Kc|
|Integral constant, ki|0.00208333|1/Ti|
|Derivative constant, kd|12.0|Td/Ts|

### Winter season 2023-2024

|Parameter|Value|Description or calculation|
|--------------------|:----:|--------------------------------------------------|
|Sampling time, Ts|10 s|Update interval of the zone temperature sensor|
|Critical gain, Kc|8.0|Proportional constant causing oscillation|
|Critical period, Pc|720 s|Cycle time at oscillating condition|
|Integral time, Ti|360 s|0.50 x Pc|
|Derivative time, Td|90 s|0.25 x Ti|
|Proportional constant, kp|4.8|0.60 x Kc|
|Integral constant, ki|0.00277778|1/Ti|
|Derivative constant, kd|9.0|Td/Ts|

> [!NOTE]
> Control parameters are entered for each zone individually.

## Prototype

![Prototype on breadboard](/images/prototype-breadboard_1.png)
<br>
*Prototype wired on breadboard and connected to solid state relays.*

![Prototype on experiment board](/images/prototype-experimentboard_1.png)
<br>
*Prototype wired on experiment board and assembled with solid state relays.*

![Prototype in enclosure](/images/prototype-enclosure_1.png)
<br>
*Prototype in enclosure connected to status LEDs on breadboard.*

![Prototype in enclosure](/images/prototype-enclosure_2.png)
<br>
*Prototype in enclosure with status LEDs.*

![Prototype installed](/images/prototype-installed_1.png)
<br>
*Prototype installed in DIN rail connection cabinet. Floor heating controller on the left and [mixing valve controller](https://github.com/jnasholm/mvc-pwm) on the right.*

## Future

The goal is to mature the hardware design enough to get proper printed circuit boards fabricated. Next step is to make a second prototype with all components positioned as realistically as possible. For this purpose only through hole components are used. Using surface mount components may be the final step of prototyping.

![Prototype PCB](/images/esp32_ssr_fhc_pcb.png)
<br>
*Prototype printed circuit board for a 9 unit 159.2 mm DIN enclosure.*

## References

[^1]: [PI Parameter Influence on Underfloor Heating Energy Consumption and Setpoint Tracking in nZEBs](https://www.mdpi.com/1996-1073/13/8/2068)
[^2]: [Reglering, om P-, I-, D-bidraget](https://www.bastec.se/anvandarmanual/reglering-p-i-d-bidraget/)
[^3]: [Thermo-Electric Actuator](https://waxmotor.com/products/thermo-electric-actuator)
