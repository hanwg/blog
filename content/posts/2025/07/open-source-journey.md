---
title: "My Open Source Development Journey: From User to Code Owner"
summary: "How I evolve from a casual user to a code owner for a open source project"
date: 2025-07-01
draft: true
tags:
  - SoftwareEngineering
---
This month, I am excited to share that I have been accepted as a new code owner for the [Home Assistant Telegram integration](https://www.home-assistant.io/integrations/telegram_bot/).
In this post, I will be sharing my journey on how I went from being a user to an active maintainer.

For those who are unfamiliar, [Home Assistant (HA)](https://www.home-assistant.io/) is a free open source platform for managing and controlling smart home devices such as switches and lights.
The Telegram integration is one of HA's most widely used integration under the Notifications category.

*Side note: I am not affiliated with Home Assistant; I am just one of the many volunteers who contribute to the project.*

## A User with a Problem

I started my smart home transformation in 2023 when I moved into my new home.
I wanted a single control plane for all my devices and this led me to Home Assistant.
As I delved deeper, I started to explore various integrations and built complex automations.

In some of my automations, I configured notifications to send me alerts via Telegram when things go wrong or require my attention.
Like many complex systems, there are sometimes bugs or areas for improvement.
Despite having a significant number of users, the Telegram integration hasn't had much activity for a number of years and issues remained unresolved.

Given my background in software engineering and having issues impacting my automations, I decided it's time to take a peek under the hood.

## The First Pull Request

For a new contributor, diving into an open source project can be a daunting task.
There was a massive codebase, copious documentation and sprawling discussions spanning forums, discord channels and GitHub issues.



Current and only (at the time of writing) active maintainer, I am responsible for:
- Implementing new features
- Bug fixes
- Improving code quality
- Contributing test cases
- Writing documentation
- Resolving issues
