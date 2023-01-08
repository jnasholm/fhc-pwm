# fhc-pwm
ESP32 SSR Floor Heating Controller with zone thermostats, modulated actuators and mixing valve control

Project to create a smart floor heating controller for private homes with a hydronic underfloor system and optionally combined with a radiator system mixing valve. Brief project description:
<ul>
 <li>The controller is an ESP32 programmed with ESPHome.</li>
 <li>The climate thermostats are based on ESPHome PID control.</li>
 <li>The zone valve actuators are operated with 24 VAC (wax motor type without direct position feedback).</li>
 <li>The mixing valve actuator is a stepper motor also operated with 24 VAC.</li>
 <li>The zone valve actuators are controlled with ESPHome PWM.</li>
 <li>The switching modules are equipped with solid state relays.</li>
 <li>The controller can operate either 8 zone valve actuators or 6 zone valve actuators and 1 mixing valve actuator.</li>
</ul>

Room temperature data can be supplied either through sensors connected directly to the controller or through sensors registered in Home Assistant. This is also the case for outdoor temperature data. The mixing valve temperature feedback from the hydronic manifold is done with sensors connected directly to the controller. The preferred sensor for line temperature data is the DS18B20 which also works perfectly fine for room temperature and outdoor temperature data. Optionally the RHT03 sensor can be used for room temperature data which would give relative humidity data as well.

Modulation of the valve actuators is done with solid state relay modules which have galvanic isolation between control signal pins and switching terminals. The modules are equipped with one solid state relay for each valve actuator which usually equals the number of zones to control. Each solid state relay can switch a current up to 2 A. Reasonably modern valve actuators draw a current of 0.3 A during initial warm-up, which can last for about 2 minutes. After warm-up the power consumption for each actuator is usually 1 W. Connecting two actuators to each solid state relay is possible in case a zone is controlled by more than one actuator.

The controller hardware is theoretically capable of operating 110-230 VAC zone valve and mixing valve actuators but this will not be tested.

My home is a two floor detached building built in 2010. Insulation is quite good and the thermal inertia is very high. Large windows to the south add a considerable amount of heat on sunny days even during wintertime. The ground floor is heated with hydronic underfloor heating and the first floor is heated by radiators. The heat source is as gas boiler with a modulating controller and a climate compensated outdoor temperature sensor. Each room on the ground floor is a separate heating control zone with a room temperature sensor. The living room is one zone heated with two separate regions of heating tube and therefore has two valve actuators, controlled by a single room temperature sensor. In total 4 heating zones and 5 valve actuators. The supply line temperature to the hydronic heating distributor is controlled by a 3-way mixing valve operated by a stepper motor actuator. A rudimentary (not smart) controller continuously adjust the mixing valve setting based on measured outdoor temperature and supply line temperature feedback.
