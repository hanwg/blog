---
title: "My Open Source Development Journey: From User to Code Owner"
summary: "How I evolve from a casual user to a code owner for a open source project"
date: 2025-07-30
tags:
  - SoftwareEngineering
  - HomeAutomation
cover:
  image: "open-source-journey.png"
  alt: "Journey"
  caption: "My long journey of contributing to open source"
---
This month, I am excited to share that I've officially become a code owner for the [Home Assistant Telegram integration](https://www.home-assistant.io/integrations/telegram_bot/).
In this post, I will be sharing my journey on how I went from being a user to an active maintainer.

For those who are unfamiliar, [Home Assistant (HA)](https://www.home-assistant.io/) is a free open source platform for managing and controlling smart home devices such as switches and lights.
The Telegram integration is one of Home Assistant's most widely used integration under the Notifications category.

*Side note: I am not affiliated with Home Assistant; I am just one of the many open-source developers who contribute to the project.*

## A User with a Problem

I started my smart home transformation in 2023 when I moved into my new home.
I wanted a single control plane for all my devices and this led me to Home Assistant.
As I delved deeper, I started to explore various integrations and built complex automations.

In some of my automations, I configured notifications to send me alerts via Telegram when things go wrong or require my attention.
Like many complex systems, there are sometimes bugs or areas for improvement.
Although the Telegram integration has a significant number of users, there wasn't much development activity in recent months.
Bugs lingered and requests for enhancements remain unanswered.

With my background in software engineering and growing curiosity on how the Home Assistant works, I decided it's time to take a peek under the hood.

## The First Step

For a new contributor, diving into an open source project can be a daunting task.
There's a massive codebase, copious amount of documentation and sprawling discussions spanning forums, Discord channels and GitHub issues.
Furthermore, there's rarely anyone available to provide guidance.

I was undeterred and began my research:
- I scoured the developer documentation to understand how to set up my development environment, build and execute tests.
  I also made sure to adhere to coding styles and guidelines for a smoother review process.
- I examined new and old pull requests to narrow down my search to the relevant source files that I need to modify for my planned changes.
- I referenced other integrations of similar nature to get a better grasp on the flow of the integration.

Armed with these newfound knowledge, I began working on my first pull request.

## The First Pull Request (PR)

My first PR was driven by a desire to simplify the Telegram integration's configuration.
Back then, the integration could only be configured using YAML, and every change I made required a reboot.
This was disruptive, especially when I was building and testing automations with different configurations.

Hence, I decided to create a User Interface (UI) for configuring the integration.
This not only allowed on-the-fly reload of configuration changes but also eliminated configuration errors, which was common when editing YAML files.
After a week of hard work, I finally completed my changes and submitted my PR.

A week later, someone from the Home Assistant core team reviewed my PR.
What followed was an eye-opening albeit long and laborious process - I had unknowingly picked a complex issue for my first PR.
I received extensive feedback which required a ton of rework and new changes such as:
- Documentation for the end user
- Refactoring to maintain code quality
- Handling migration and backwards compatibility
- Registering an issue to inform user of the YAML deprecation
- General code improvements (The development was done in python, which wasn't my main programming language)

Despite the challenges, I soldiered on and incorporated all the feedback I've received to refine the PR.
Weeks later, my PR passed the bar was finally approved!

*A big thank-you to my PR reviewer Martin Hjelmare, who has been supportive and provided much of the valuable feedback.
I would also like to extend my thanks to the members of the HA community who have contributed to the PR via other aspects such as testing and translations.*

## Building on Progress

After my first PR, I had acquired a firm grasp of the development process, a strong familiarity with python and a deeper understanding of the Telegram functionalities.
Responses from the Home Assistant community was encouraging which motivated me to continue to contribute in various ways, such as:
- Implementing new features
- Bug fixes
- Improving code quality
- Contributing test cases
- Writing/Improving documentation
- Resolving issues

Eventually, a few members of the community suggested that I should appoint myself as code owner for the Telegram integration.
I submitted my request and sailed through the review.
This has been a huge honor for me as it signifies trust and acknowledges my contributions and expertise for the integration.

## Final Thoughts

My acceptance as code owner hasn't fundamentally changed my involvement;
I'm still actively maintaining the Telegram integration and involved with the community.
It's been a rewarding journey where I honed my skills and learned about the platform that I use daily.

If you are a long time lurker of an open source project, I highly encourage you to contribute!
Your first pull request can be something as simple as improving documentation or adding test cases. 
As you gain deeper understanding, you can progress to bug fixes or enhancements.
