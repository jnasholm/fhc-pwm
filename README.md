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

In case the supply line controller needs the same upgrade: [ESP32 SSR Mixing valve actuator controller](https://github.com/jnasholm/mvc-pwm/tree/main)

**Note:** The controller hardware is theoretically capable of operating 110-230 VAC zone valve and mixing valve actuators. This will however not be tested in the project.

If you preferr to replace the zone valve actuators with [proportinal servo actuators](https://homematic-ip.com/en/product/valve-drive-motorised), have a look at [this project](https://github.com/nliaudat/floor-heating-controller).

## Description
Room temperature data can be supplied either through sensors connected directly to the controller or through sensors registered in Home Assistant. This is also the case for outdoor temperature data. The mixing valve temperature feedback from the hydronic manifold is done with sensors connected directly to the controller. The preferred sensor for line temperature data is the [DS18B20](https://www.electrokit.com/en/product/ds18b20-to-92-temperature-sensor/) which also works perfectly fine for room temperature and outdoor temperature data. Optionally the [RHT03](https://www.electrokit.com/en/product/temp-moist-sensor-rht03/) sensor can be used for room temperature data which would give relative humidity data as well.

Modulation of the valve actuators is done with solid state relay modules which have galvanic isolation between control signal pins and switching terminals. The modules are equipped with one solid state relay for each valve actuator which usually equals the number of zones to control. Each solid state relay can switch a current up to 2 A. Reasonably modern valve actuators draw a current of 0.3 A during initial warm-up, which can last for about 2 minutes. After warm-up the power consumption for each actuator is usually 1 W. Connecting two actuators to each solid state relay is possible in case a zone is controlled by more than one actuator.

## Development building
My home is a two floor detached building built in 2010. Insulation is quite good and the thermal inertia is very high. Large windows to the south add a considerable amount of heat on sunny days even during wintertime. The ground floor is heated with hydronic underfloor heating and the first floor is heated by radiators. The heat source is as gas boiler with a modulating controller and a climate compensated outdoor temperature sensor. Each room on the ground floor is a separate heating control zone with a room temperature sensor. The living room is one zone heated with two separate regions of heating tube and therefore has two valve actuators, controlled by a single room temperature sensor. In total 4 heating zones and 5 valve actuators. The supply line temperature to the hydronic heating distributor is controlled by a 3-way mixing valve operated by a stepper motor actuator. A rudimentary (not smart) controller continuously adjusts the mixing valve setting based on measured outdoor temperature and supply line temperature feedback.

[^1]: [PI Parameter Influence on Underfloor Heating Energy Consumption and Setpoint Tracking in nZEBs](https://www.mdpi.com/1996-1073/13/8/2068)
[^2]: [Reglering, om P-, I-, D-bidraget](https://www.bastec.se/anvandarmanual/reglering-p-i-d-bidraget/)
[^3]: [Thermo-Electric Actuator](https://waxmotor.com/products/thermo-electric-actuator)
