---
title: "Top Smart Home Automations That Make Life Easier 🏖️"
summary: "Simplify daily tasks with smart home automations"
date: 2025-11-06
draft: true
tags:
  - HomeAutomation
cover:
   image: "home-automations.png"
   alt: "Home automations"
   caption: "Simplify daily tasks with smart home automations"
---
Having a smart home has been a game changer for me.
It's not just about cool gadgets; it's about making your life more convenient, more efficient (think lower utility bills) and secure (notifications when I'm away from home).
While the sky is the limit with automating your smart home, I've found a number of core automations that have become indispensable in my smart home.

Here are some of my favourite home automations that I use and love.

> [!NOTE]
> My smart home is powered by [Home Assistant](https://www.home-assistant.io/) (HA) so any blueprint, automation or script shared here are based on that platform.
> Automations typically have the triggers-conditions-actions pattern so even if you are using other platforms (e.g. Apple Homekit, Google Home), you can probably create similar automations although the configuration will be different.  

## Motion Activated Lights

Let's start off with the most basic and most commonly used automation - motion activated lights.

This automation is particularly a life-saver for me as I have a toddler at home.
I have strategically placed a motion sensor near her bedroom to ensure that if she needs a nighttime trip to the toilet, the lights will turn on automatically upon her exit.
This eliminates the need to fumble for distant light switches.

What you'll need:
- Motion sensor
- Smart bulb, switch or light

Most motion sensors are typically operated with a button battery and provide 2 attributes: motion and illuminance.
These attributes are used in the automation setup:
1. Trigger when motion is detected.
2. Check illuminance is below a threshold (we only want to turn on the lights if it's too dark).
3. Turn on the light.
4. Wait for the motion sensor to clear (no movement detected).
5. Turn off the light.

The automation:
```yaml
triggers:
  - trigger: state
    entity_id:
      - binary_sensor.motion_sensor_motion
    to:
      - "on"
conditions:
  - condition: numeric_state
    entity_id: sensor.motion_sensor_illuminance
    below: 20
actions:
  - action: light.turn_on
    target:
      entity_id: light.ceiling
  - wait_for_trigger:
      - trigger: state
        entity_id:
          - binary_sensor.motion_sensor_motion
        to:
          - "off"
  - action: light.turn_off
    target:
      entity_id: light.ceiling
```

> [!TIP] Tip 1: Avoid false positives (detecting "ghosts")
> Shadows, reflections, and movement caused by wind (e.g. fluttering curtains) may trigger motion sensors.
> Avoid false positives by re-positioning your motion sensor or fine-tuning its sensitivity and detection interval.

> [!TIP] Tip 2: Avoid false negatives (movement not detected)
> Motion sensors have a cone-shaped detection zone and typically have an optimal range where it can detect movement reliably.
> Too close, you will be in its blind spot; too far, you will be out of the detection zone.
