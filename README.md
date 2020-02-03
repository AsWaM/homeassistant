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
- Tado smart TRVs (x3) + Tado Gateway
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
- Tado GeoFence
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
