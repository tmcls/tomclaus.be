---
title: "Integrating Skoda Enyaq with Home Assistant and A Better Routeplanner (ABRP)"
date: 2024-11-25
categories: 
  - "home-automation"
tags: 
  - "a-better-routeplanner"
  - "abrp"
  - "api"
  - "hacs"
  - "home-assistant"
  - "skoda"
coverImage: "be64c41a9e38042d8b739adae8bf21db6cbdac6e8ed191cd759a964ebc958e54-1.jpg"
---

  
I own a Škoda Enyaq electric vehicle (EV), which I’ve integrated with my Home Assistant setup. This allows me to easily monitor essential metrics like battery status, estimated range, and more. As I plan an upcoming ski trip, I’m considering using ABRP for route planning again — this time without [relying on my OBD dongle](https://tomclaus.be/reizen/skien-in-val-thorens-met-de-elektrische-auto/).

## **Connecting the Škoda Enyaq to Home Assistant**

To retrieve data, I use the new [MySkoda Integration](https://github.com/skodaconnect/homeassistant-myskoda), as the older ŠKODA Connect Home Assistant integration has been deprecated. You can easily add this new repository to Home Assistant via [HACS](https://www.hacs.xyz/) (Home Assistant Community Store).

![Tom's personal Home Assistant integration with a view on his Skoda Enyaq](/images/Screenshot-2024-11-26-at-08.52.49-2-1024x713.png)

  
Once the integration is set up, you can access all the statistics and settings available in the official Škoda app directly within your Home Assistant dashboard.

## **A Better Routeplanner (ABRP) Integration**

If you’re familiar with A Better Routeplanner (ABRP), you know how essential it is to have accurate, live data from your car for efficient route planning. While you could use an OBD dongle to send this data, there’s also the option to connect ABRP directly to your car via an API.

#### Generating an ABRP Token

In the ABRP app, you can create a Generic Token to link your ABRP account. This token is required for sending data from Home Assistant to ABRP. Once you have generated the token, you’ll need to configure Home Assistant to send data.

![ABRP Live Connections overview](/images/Screenshot-2024-11-26-at-08.58.49.png)

#### Setting Up Home Assistant to Send Data to ABRP

**1\.** **Edit the `configuration.yaml` file** in Home Assistant to include the following code:

```
rest_command:
    update_abrp:
      method: POST
      content_type: "charset=utf-8; application/x-www-form-urlencoded"
      url: >      
         {% raw %} {% set tlm = {
        "utc":float(as_timestamp(states('sensor.skoda_last_changed'))),
        "soc":states('sensor.skoda_enyaq_soc'),
        "est_battery_range":float(states('sensor.skoda_range')),
        "is_charging":states('sensor.skoda_chargestate') == "charging",
        "is_dcfc":states('sensor.skoda_power') > "22",
        "hvac_setpoint":state_attr('climate.skoda_hvac','temperature'),
        "lat": state_attr('device_tracker.skoda_positie','latitude'),
        "lon": state_attr('device_tracker.skoda_positie','longitude'),
        "odometer": states('sensor.skoda_odometer'),
        "capacity": 77,
        "soe": states('sensor.skoda_range') | float * 77 / 100,
        } -%}
        https://api.iternio.com/1/tlm/send?api_key=32b2162f-9599-4647-8139-66e9f9528370&token=TOKEN&tlm={{tlm|to_json|urlencode}}{% endraw %}
```

Replace **`TOKEN`** with your ABRP personal token. The API key provided is the public key for ABRP.  
You probably need to replace the sensor data to your own car sensors.

**2\. Customize Parameters**

If you have another car or integration that supports additional or different parameters, you can modify the payload accordingly. For a complete list of supported parameters, refer to the [official ABRP API documentation](https://documenter.getpostman.com/view/7396339/SWTK5a8w#fdb20525-51da-4195-8138-54deabe907d5).

**3\. Automation to Push Data**

To send live car data to ABRP every 10 seconds, set up the following automation in Home Assistant:

```
alias: ABRP Send Data
description: "Send Skoda Enyaq data from Home Assistant to ABRP"
triggers:
  - trigger: time_pattern
    seconds: "10"
conditions: []
actions:
  - action: rest_command.update_abrp
    metadata: {}
    data: {}
    response_variable: status
mode: single
```

![Home Assistant Automation view](/images/Screenshot-2024-11-26-at-09.12.20-1024x501.png)

#### Results

This setup ensures that Home Assistant sends the latest available data from your Škoda Enyaq to ABRP every 10 seconds, providing live vehicle information for accurate and efficient route planning.

![ABRP live connection data](/images/Screenshot-2024-11-26-at-09.13.36.png)
