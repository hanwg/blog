---
title: "Top Smart Home Automations That Make Life Easier 🏖️"
summary: "Simplify daily tasks with smart home automations"
date: 2025-12-06
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
> Automations typically have the triggers-conditions-actions pattern so even if you are using other platforms (Apple Homekit, Google Home, etc.), you can probably create similar automations although the configuration will be different.  

---

## 💡 Motion Activated Lighting

Let's start off with the most basic and most commonly used automation - motion activated lighting.

This automation is particularly a life-saver for me as I have a toddler at home.
I have strategically placed a motion sensor near her bedroom to ensure that if she needs a nighttime trip to the toilet, the lights will turn on automatically which eliminates the need to fumble for distant light switches.

> [!TASK] What you'll need
> 1. Motion sensor
> 2. Smart bulb, switch or light

**How the automation works**:
Most motion sensors are operated with a button battery and provide 2 attributes: a binary on-off state for motion and a numerical value for illuminance.
These 2 values are used to activate the automation.

**Trigger**: Motion is detected.

**Condition**: The illuminance is below a threshold (we only want to execute the automation if it's too dark).

**Actions**:
1. Turn on the light. 
2. Wait for the motion sensor to clear (i.e. no movement detected).
3. Turn off the light.

> [!CODE] Automation: Motion activated lights 
> ```yaml
> triggers:
>   - trigger: state
>     entity_id:
>       - binary_sensor.motion_sensor_motion
>     to:
>       - "on"
> conditions:
>   - condition: numeric_state
>     entity_id: sensor.motion_sensor_illuminance
>     below: 20
> actions:
>   - action: light.turn_on
>     target:
>       entity_id: light.ceiling
>   - wait_for_trigger:
>       - trigger: state
>         entity_id:
>           - binary_sensor.motion_sensor_motion
>         to:
>           - "off"
>   - action: light.turn_off
>     target:
>       entity_id: light.ceiling
>```

> [!TIP] Tip: Improving the accuracy of your motion sensor
> 1. Shadows, reflections, and movement caused by wind (e.g. fluttering curtains) may trigger motion sensors.
> 2. Motion sensors have a cone-shaped detection zone and typically have an optimal range where it can detect movement reliably.
>   Too close, you will be in its blind spot; too far, you will be out of the detection zone.
> 3. Continue to fine-tune by re-positioning your motion sensor or adjusting its sensitivity and detection interval for the best results.

## 🛋️ Adaptive Lighting

This quality-of-life improvement can be used in conjunction with other lighting automations such as the motion activated lighting mentioned above.
Its main purpose is to set up warm lighting and dimming the light during nighttime to reduce eye strain and for a cozy ambience as I wind down for the day.

> [!TASK] What you'll need
> 1. Smart bulb or light

**How the automation works**:

**Trigger**: By scheduled time.

**Condition**: Light is on.

**Actions**:
1. Set brightness of the light.
2. Set color temperature of the light.

> [!NOTE]
> I don't have any automation code to share here since I am using HA's [Adaptive Lighting](https://github.com/basnijholt/adaptive-lighting) custom integration to set up this.

## ❄️ Turn Off The Air Conditioner When Doors/Windows Are Open

## 🧹 Vacuum The House When I'm Away, Dock When I'm Coming Home

## 🔊 Reminders When Washer Is Finished



## 🚨 NVR Notifications
