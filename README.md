!! NOTE this sensor is still in development !!

# P2000 Sensor

This is a simple p2000 sensor for home assistant

### Installation

Copy this folder to `<config_dir>/custom_components/p2000/`.

Add the following to your `configuration.yaml` file:


```yaml
# Example configuration.yaml entry
sensor:
  - platform: p2000
    name: p2000_Zwolle
    gemeenten:
      - zwolle
    capcodes:
      - 1234567
```

** Either or 'gemeenten' or 'capcodes' should be filled. **

Be aware that if you use `gemeenten` and `capcodes` in the same config entry both one of the `gemeenten` and one of the `capcodes` should be in the `melding`


You should get a sensor like te following with a lot of attributes.

The id is unique and changes with every new p2000 message.


![Tux, the Linux mascot](./assets/screenshot01.png)

Extracting data can be done with a template like:

`{{ state_attr('sensor.p2000_zwolle', 'melding') }}`

----------------------------------------------------------------------------------
### lovelace Dashboard

You need markdown and logbook for the following settings

More information on your dashboard.<br> tekst in Dutch.<br>
![afbeelding](./assets/dashboard1.png)

#### markdown card
```
type: markdown
content: >
  Datum : {{ state_attr('sensor.p2000', 'datum' ) }}   Tijd: {{
  state_attr('sensor.p2000', 'tijd') }}


  Melding : {{ state_attr('sensor.p2000', 'melding') }}<br>


  {{ state_attr('sensor.p2000', 'tekstmelding') }}<br>

  -

  Plaats: {{ state_attr('sensor.p2000', 'plaats') }}

  Straat : {{ state_attr('sensor.p2000', 'straat') }} 

  Regio : {{ state_attr('sensor.p2000', 'regio') }}

  capcode : {{ state_attr('sensor.p2000', 'capstring') }}

  lat : {{ state_attr('sensor.p2000', 'latitude') }}  long: {{
  state_attr('sensor.p2000', 'longitude') }}

  <br>

  Dienst: {{ state_attr('sensor.p2000', 'dienst') }}

  info :  {{ state_attr('sensor.p2000', 'brandinfo') }}<br>

  Regio id : {{ state_attr('sensor.p2000', 'regioid') }}       Dienst id : {{
  state_attr('sensor.p2000', 'dienstid') }}

  Prio : {{ state_attr('sensor.p2000', 'prio1') }}

  Grip : {{ state_attr('sensor.p2000', 'grip') }}


  Id nr : {{ state_attr('sensor.p2000', 'id') }}<br>
```
----------------------------------------------------------------------------------
Also <br>
![afbeelding](./assets/dashboard.png)

#### logbook card
```
type: logbook
entities:
  - sensor.p2000
hours_to_show: 24
title: '112'
```

#### automation for HA
```
alias: p2000 notificatie
description: ''
trigger:
  - platform: state
    entity_id: sensor.p2000
condition: []
action:
  - service: telegram_bot.send_message
    data:
      title: '"P2000 Melding"'
      message: >-
        Datum : {{ state_attr('sensor.p2000', 'datum' ) }}   Tijd: {{
        state_attr('sensor.p2000', 'tijd') }}

        "{{ states.sensor.p2000.attributes.melding}}   
         {{states.sensor.p2000.attributes.capstring }}   
         Id nr : {{ state_attr('sensor.p2000', 'id') }}"
  - service: telegram_bot.send_location
    data:
      latitude: '{{ states.sensor.p2000.attributes.latitude }}'
      longitude: '{{ states.sensor.p2000.attributes.longitude }}'
mode: single

```
