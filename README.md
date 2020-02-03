# homeassistant
**AsWaM's** home assistant configuration
This is a work in progress, not a tutorial. Happy if it helps.

Using Hass.io (tried Jeedom, Hass OS, and dockered installation)

## Expected improvements
- Add motors to the Rolle Shutters and integrate them
- <del>Add a Tado smart TRV in the bathroom</del> --> Done

## Live demo (floorplan)
 - Overview is created with Picture elements and image superpositions
[Youtube Live demo](https://www.youtube.com/watch?v=4EGnCFBxhZg)

- Livingroom is created with Picture elements and image superpositions
[Youtube Live demo](https://www.youtube.com/watch?v=QuAtu_bE5hE)

## Used integrations & Hardware
### Heating / Climate
- Tado smart TRVs (x5) + Tado Gateway
### Custom Sensors
- Template (for sensor batteries, average temperature, turned on lights etc...) 
- History (Already cleaned today, Sonnzed notification, ect) 
### Calendar
- Google Calendar
### Notification
- Telegram
### Cameras integration
- Synology, custom python script to trigger home/away mode
### Cameras Models
- Foscam FI9821P
- Xiaomi dafang with [EliasKotlyar Xiaomi-Dafang-Hacks custom Firmware](https://github.com/EliasKotlyar/Xiaomi-Dafang-Hacks)
- Eufy Security cam V2 (not integrated yet) 
### Presence Detection
- bluetooth device tracker
- <del>Tado GeoFence</del> Desactivated that, prevents you to remotely turn the heating on
- Could not get the NUT ble tracker to work (ble_device_tracker)
### Sensors
- Xiaomi Gateway V2
- Xiaomi Temperature and Humidity Sensor (x9) both v1 and v2
- Xiaomi Window/Door Sensor (x6) both v1 and v2
- Xiaomi Motion Sensor (x5) both v1 and v2
- Xiaomi Bed Activity Sensor (currently unused)
### Actionners
- Xiaomi Smart Button (x3) both v1 and v2
- Xiaomi Magic Cube (x2)
- Xiaomi Smart Plug (x3)
- Sonoff T2 EU 1CH (x4) with [arendst Tasmota Custom Firmware](https://github.com/arendst/Tasmota)
- Sonoff T2 EU 2CH (x3) with [arendst Tasmota Custom Firmware](https://github.com/arendst/Tasmota)
### Media Player
- LG TV
### Plants
- Mi flora (x2)
### Meteo
- YR.no
- DarkSky
### Lights
- Yeelight RGB Bulb (x6) both v1 and v2
- YeeLight RGB Led Strip (x3) both v1 and v2
- Yeelight BedSide Lamp (v2)
- Xiaomi Philips BedSide Lamp
### Infrared / Radio Frequency
- Broadlink RM Plus
### Vaccuum cleaner
- Roborock S50 Vacuum cleaner
### Garage
- Garadget
### Hass.IO Extensions
- Google cloud backup
- Tasmota Admin
- Mosquito Broker
### Hardware
- FritzBox 6190 Cable
- Xiaomi Mi Router 3G with [OpenWRT LuCi](https://openwrt.org/docs/guide-user/luci/start)
- Synology NAS DS218+
- Raspberry Pi 4B

## Groups
Originally, (pre-lovelace) this was used for both display and use of multiple sensors/automations/lights etc together.
Now, with lovelace, the display is useless, but still here for automations, sums, etc

### Device tracker

The magic about that, is that the group is 'home' if any of the trackers is 'home' and is 'not_home' if nobody is in the house. So the group is actually 'anyone home?' it switches to 'home' when the first person comes, and switches to 'not_home' when the last departs, which is just perfect for automations

```
  tracked_devices:
    name: Tracked devices
    entities:
       - device_tracker.qcombtd
       - device_tracker.samsungsmg920f
       - device_tracker.galaxy_s8
       - device_tracker.lia
```

### Sensors
The magic about that, is that if any goes from 'off' to 'on' the the whole group goes from 'off' to 'on', so you can trigger an automation if any of them is triggered, and if you add a new one, just add it to the group and it is intergrated to automations automatically

The list here is not exhaustive, have a look a the groups.yaml file

Motion sensors inside the house 
```
  motion_sensors:
    name: Mouvement intérieur
    entities:
       - binary_sensor.motion_sensor_158d0001e47f52
       - binary_sensor.motion_sensor_158d0001e47d34
       - binary_sensor.motion_sensor_158d0001ddca38
       - binary_sensor.motion_sensor_158d0001b7542d
       - binary_sensor.motion_sensor_158d0001d6675f
```
Open closed sensor on doors
```
  door_sensors:
    name: Ouvertures Portes
    entities:
       - binary_sensor.door_window_sensor_158d0001ab1b67
       - binary_sensor.door_window_sensor_158d0001ab5aaa
       - binary_sensor.door_window_sensor_158d0001d8526a
       - binary_sensor.door_window_sensor_158d000272f13c
```

These groups help for the averages
```
  room_humidity:
    name: Humidité Maison
    entities:
       - sensor.humidity_158d0001b96127
       - sensor.humidity_158d0001b92bcc
       - sensor.humidity_158d0001b8f1b1
       - sensor.humidity_158d00022734f8
  room_temperature:
    name: Température Maison
    entities:
       - sensor.temperature_158d0001b96127
       - sensor.temperature_158d0001b92bcc
       - sensor.temperature_158d0001b8f1b1
       - sensor.temperature_158d00022734f8
```
### Lights

These groups allow to control several lights simultaneously
```

  light_salon:
    name: Salon
    entities:
       - light.yeelight_color1_34ce008fcea8
       - light.yeelight_color1_34ce00900013
       - light.yeelight_color1_7811dc6aaca4
       - light.yeelight_strip1_7811dca22953
```

### Automations

Allows to deactivate the automations not "supported" by the wife when she is at home, and reactivate them the rest of the time

```
  non_waf:
    name: Non WAF scenarios
    entities:
       - automation.allumage_sdb_auto
       - automation.extinction_sdb_auto
       - automation.allumage_couloir_auto
       - automation.extinction_couloir_auto
       - automation.changement_lumieres_a_6h_et_20
       - automation.changement_lumieres_a_7h_et_19h
       - automation.changement_lumieres_a_9h_et_17h
       - automation.changement_lumieres_a_22h
       - automation.changement_lumieres_a_23
```


## Automations
Currently 66
Soon...
## Custom Sensors
Currently many
Soon...

## How it works
### lovelace dynamic
It is based on the moster card. You can find it on [ciotlosm Github](https://github.com/ciotlosm/custom-lovelace/tree/master/monster-card). Copy it to your /js folder

Add code at the beigining of the lovelace file
```
resources:
  - type: js
   url: /local/js/monster-card.js?v=1.1
```
You for example can the get all the turned on lights with the following code
```
      - card:
          title: Lumières Allumées
          type: entities
        filter:
          include:
            - entity_id: light.*
              state: 'on'
        type: 'custom:monster-card'
```
Or the present device trackers
```
      - card:
          title: Personnes présentes
          type: glance
        filter:
          include:
            - entity_id: device_tracker.*
              state: home
        type: 'custom:monster-card'
```
Or all the low batteries
```
      - card:
          title: Batteries inférieures a 33%
          type: glance
        filter:
          include:
            - entity_id: sensor.bat*
              state: < 30
        type: 'custom:monster-card'
```
Last example with exclusions, all the turned on switches except the ones created by the dafang cam
```
     - card:
          title: Interrupteurs Allumées
          type: entities
        filter:
          exclude:
            - entity_id: switch.dafang*
          include:
            - entity_id: switch.*
              state: 'on'
        type: 'custom:monster-card'
```
### Picture elements
You need a background image, and if you like a different one for the switched (on or off) then another one.
I used [Paint.Net](https://www.getpaint.net/download.html) to darken/lighten the images
then you just need to switch the image according to the status of a sensor (here a switch). You could also do the same with css transforms on the image.
```
          - entity: switch.chambre
            image: /local/images/chambre.png
            state_image:
              'off': /local/images/chambre_off.png
              'on': /local/images/chambre.png
            style:
              left: 30%
              top: 64.5%
              width: 24.5%
            tap_action:
              action: none
            type: image
 ```
You can also add a tap action on the image
 ```
- entity: input_boolean.mode_waf
            state_image:
              'off': /local/images/waf_off.png
              'on': /local/images/waf.png
            style:
              left: 6%
              top: 28%
              width: 9%
            tap_action:
              action: toggle
            title: mode waf
            type: image
 ```
