# ESP_Thermostat

This is the first iteration of my ESP8266-based thermostat replacement.  The project consists of a NodeMCU and relay shields that will be located at the boiler/zone valves of my house along with multiple "sensors" around the house that will report temperature, pressure, and humidity over MQTT to Home Assistant.  Home Assistant will then call for heating or cooling accordingly.  This will eventually be replaced by a Python program on my server that will integrate with Home Assistant and Tensor Flow to provide the most comfort to home occupants.  The idea is to use the heat loss, sensor readings, and occupancy to determine best time to run HVAC system.  

Parts List
------------------------------------

Zone Controller - NodeMCU, relay boards - I have four zones in my house currently and will be adding two more with the addition of radiant heating at each end of the house.

TPH Sensors - Wemos D1 Mini, DHT11, and SSD1306 LCD.  DHT11 to be replaced by BME280 eventually, so code and case will be designed with that in consideration.

On the backend hosted locally, I am using Home Assistant and Mosquito.  This is my first project sharing on Github and I am not a programmer, so much of the code is hacked together.  I will be printing the housings for everything and will post models on Thingverse.


Roadmap
-------------------------------------

TPH Sensors - finish coding, deploy and publish readings to MQTT topic for Home Assistant tracking.  The benefit of getting these rolled out first will be that they can already be dropped ina nd integrated into my current setup, and also to start accumulating good data to train the system with later.

Zone Controller - basically just going to use this to subscribe to MQTT topics and then react by using the relay board to complete circuit to call for heat.  Still working out the details for redundancy, but will leave current thermostats in place for a little while and adjust to use as fail safe.

Incorporate Tensor Flow - I haven't completely made up my mind on how to tackle this yet.  Since every house is different and the inputs will vary, there won't be a sufficient training set that I can use.  I have already started recording data with some Ecobee sensors I have around the house, and with the addition of these three TPH sensors, it should be decent enough to start with.  pyfann is the neural network library I am using and inputs will be the following:

Inside temp
Outside temp
Schedule
Heat loss - average and by zone
Occupancy

Layout
----------------------------------------
```
  TPH Sensor 1  ------------  
                            | 
  TPH Sensor 2  ------------ ---- MQTT -->  Home Assistant ----> Zone Controller
                           |
  TPH Sensor 3  ------------    




  Zone Controller -----> Zone 1 - Basement (Hydronic Baseboard)
           |
           |-----------> Zone 2 - 1st Floor (Hydronic Baseboard) 
           |
           |-----------> Zone 3 - 2nd Floor (Hydronic Baseboard)
           |
           |-----------> Zone 4 - Hot Water
         
             (loops to be added)
           |
           |-----------> Zone 5 - Master Bedroom/Bathroom, Main Bathroom (Hydronic Radiant)
           |
           |-----------> Zone 6 - Living Room (Hydronic Radiant)
         
``` 
