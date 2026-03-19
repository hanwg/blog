---
title: "Self-hosted: Local Telegram Bot API"
summary: "Bringing the Telegram Bot API to Your Smart Home"
date: 2026-03-02
tags:
  - SoftwareEngineering
  - HomeAutomation
cover:
  image: "telegram-bot-api-server.png"
  alt: "Self-hosting"
---
Did you know that you can self-host the Telegram Bot API?

Today, I’m excited to share a project I’ve been working on: a fully containerized version of the official Telegram Bot API server, specifically optimized for the [Home Assistant](https://www.home-assistant.io/) platform.

You can find the project on my GitHub repo here: [hanwg/hassio-apps](https://github.com/hanwg/hassio-apps)

## What's Telegram Bot API?

Before diving into the project details, it's helpful to understand what is [Telegram Bot API](https://core.telegram.org/bots/api).

Telegram bots are accounts operated by software rather than people.
They are very versatile with a wide range of uses ranging from commercial services, interactive gaming to personal automations.

By default, bots communicate via the official Telegram cloud servers at `api.telegram.org`.
However, Telegram didn't just provide this service, they also open-sourced the code: [tdlib/telegram-bot-api](https://github.com/tdlib/telegram-bot-api).
This enables anyone to build and host their own local Telegram Bot API server.

## Why Go Local?

While the official Telegram cloud servers are adequate for most use cases, there are some key limitations.

**Small file size limits.**
If your bot handles high resolution media, you are likely to encounter the `Request Entity Too Large` error.
Local hosting pushes the file size limits to 2GB.

**Complex webhook requirements.**
For your bot to have 2-way communications with users, you typically need a [webhook](https://core.telegram.org/bots/webhooks) to be able to receive messages.
This means that you need a publicly routable IP address and an endpoint protected with an SSL certificate which would involve setting up a domain and managing its DNS entry.
With a local server, you can use the `127.0.0.1` loopback address (or any internal IP addresses) over plain HTTP connections without compromising security.

**Network latency.**
While network latencies with the cloud isn't a dealbreaker, it can sometimes be a noticeable issue.
Using a local server eliminates connection round-trips between your bot and the Telegram's cloud servers.
This reduces latency, making interactions with your bot feel snappier and provides a better user experience.

Here's an illustration for you to visualize how the local setup compares to using the Telegram cloud servers:
{{< figure src="architecture.svg" caption="The green components are managed by you while those in blue are managed by Telegram">}}

## The Project: Telegram Bot API for Home Assistant

Normally, building from source is a technical hurdle and manual process involving managing dependencies, compilation and long build times due to the numerous I/O-intensive operations.
My goal was to streamline the entire workflow so that users can install, configure, deploy (with auto-updates!) the Telegram Bot API server locally with a few clicks through the Home Assistant UI.

### Developing the Solution

The project focuses on containerization of the Telegram bot API and consists of 3 core components.

**Multi-stage Docker build.**
I developed a custom `Dockerfile` utilizing a multi-stage build approach.
In the "build" stage, I downloaded the codebase, installed the dependencies and build tools and then compiled the application binaries.
The final stage then copies only the required artifacts to a much smaller base image.
The result is a lightweight, production-ready image.

**Automation and multi-platform support.**
To ensure broad compatibility, I implemented <abbr title="Continuous Integration / Continuous Delivery">CI/CD</abbr> pipelines using GitHub Actions to build images for both the AMD and ARM architectures.
This ensures that the app is supported for users regardless of whether they are using traditional desktops/servers or lightweight hardware such as the Raspberry Pi.
Furthermore, I can easily build new images whenever there's new upstream releases from the official Telegram bot API by triggering the GitHub Actions. 

**Polished user experience.**
To make the app accessible, I designed the repository layout and YAML configurations specifically for Home Assistant.
This generates native UI configuration screens and enables the app to be easily discovered, installed, configured and updated through the Home Assistant's built-in App Store.

## The First Milestone

After the first beta release, I switched hats from "Developer" to "User".
I launched the App Store and walked through the entire setup process exactly as a new user would.
This "User #0" phase was essential for smoothing out rough edges and catching bugs that would only appear outside the development environment.
After a few final minor refinements, I finally published the app.

It's been a few weeks now since the official launch.
Although the user base is small and growing at its own pace, it has been a rewarding experience to see strangers discover and engage with something that I've built.

## Final Remarks

In the open source community, there is a common misconception that "real" impact comes from releasing software by writing tons of code from scratch.

That's not the case.
Bridging the gap between raw source code and a user-friendly app by taking a complex tool and making it accessible and easy for everyone to deploy is just as valuable as contributing code.
