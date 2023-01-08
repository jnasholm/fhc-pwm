# fhc-pwm
ESP32 SSR Floor Heating Controller with zone thermostats, modulated actuators and mixing valve control

Project to create a smart floor heating controller for private homes with a hydronic underfloor system and optionally combined with a radiator system mixing valve. The controller is an ESP32 programmed with ESPHome, the zone valve actuators are operated of 24 VAC (wax motor type without direct position feedback) and the mixing valve actuator is a stepper motor also operated of 24 VAC.

Room temperature data can be supplied either through sensors connected directly to the controller or through sensors registered in Home Assistant. This is also the case for outdoor temperature data. The mixing valve temperature feedback from the hydronic manifold is done with sensors connected directly to the controller. The preferred sensor for line temperature data is the DS18B20 which also works perfectly fine for room temperatures and outdoor temperature data. Optionally the RHT03 sensor can be used for room temperature data which would give relative humidity data as well.

The controller hardware is theoretically capable of operating 110-230 VAC zone valve and mixing valve actuators but this will not be tested.
