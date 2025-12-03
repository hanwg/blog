---
title: "Side Projects"
layout: "side projects"
summary: "Collection of my personal projects"
---
A collection of my side projects.
It's a little bare at the moment since I'm still in the midst of filling up this section.

*PS: Click on the images to view its gallery.*

---

## <img src="n8n-icon.png" alt="N8N logo" class="side-projects-icon" /> Agentic Expense Tracking Workflow

<code>JavaScript</code>

A low-code solution based on workflow automation and AI agent for processing, classifying and importing financial transactions (fund transfers, credit card charges, and other forms of cashless payments) into an expense tracker.

[✒️ Blog Post](../posts/2025/09/agentic-expense-tracking-workflow)

---

<a href="ha-telegram-bot-integration/logo.png" class="glightbox-telegram" data-title="Home Assistant Telegram bot integration">
  <img src="ha-telegram-bot-integration/logo.png" alt="Telegram logo" style="background-color: rgb(29, 30, 32); padding: 20px;" />
</a>
<a href="ha-telegram-bot-integration/setup.png" class="glightbox-telegram" data-title="Connecting to a Telegram bot"></a>
<a href="ha-telegram-bot-integration/service-details.png" class="glightbox-telegram" data-title="Service details" data-description="This page shows details about the service such as:<br>- <strong>Service info</strong>: configuration details of the bot<br>- <strong>Automations, scenes, scripts</strong>: shows where the bot is being used<br>- <strong>Notifiers</strong>: users or groups that the bot is interacting with<br>- <strong>Events</strong>: messages sent/received by the bot<br>- <strong>Activity</strong>: chronological log of events"></a>

<script type="text/javascript">
    GLightbox({
        selector: ".glightbox-telegram"
    });
</script>

## Home Assistant Telegram Bot Integration

<code>HomeAutomation</code> <code>Python</code> <code>JavaScript</code>

Current code owner and active maintainer of the [Home Assistant (HA)](https://www.home-assistant.io/) Telegram bot integration,
the most popular HA notification service used in over **21k** active installations.

The Telegram bot integration is used for sending and receiving messages from a [Telegram bot account](https://core.telegram.org/bots).
This enables users to receive notifications and control their smart home via Telegram.

### Milestones and Achievements

[2025 December Release](https://www.home-assistant.io/blog/2025/07/02/release-20257/#integration-quality-scale-achievements):
Achieved 🥈 Silver on the [integration quality scale](https://www.home-assistant.io/docs/quality_scale/)

[2025 November Release](https://www.home-assistant.io/blog/2025/11/05/release-202511/#noteworthy-improvements-to-existing-integrations): 
Added event entities to simplify automations

2025 October: Hacktoberfest 2025 Super Contributor

[2025 July Release](https://www.home-assistant.io/blog/2025/07/02/release-20257/#integration-quality-scale-achievements):
Achieved 🥉 Bronze on the integration quality scale

*Disclaimer: I am not affiliated with Home Assistant.*

[🌐 Website](https://www.home-assistant.io/integrations/telegram_bot/) | [📄 GitHub Repo](https://github.com/hanwg/core) | [✒️ Blog Post](../posts/2025/07/open-source-journey)

---

<a href="sg-bus-arrivals/logo.png" class="glightbox-sg-bus-arrivals" data-title="SG Bus Arrivals">
  <img src="sg-bus-arrivals/logo.png" alt="SG Bus Arrivals logo" style="background-color: white; padding: 20px;" />
</a>
<a href="sg-bus-arrivals/search-bus-service.png" class="glightbox-sg-bus-arrivals" data-title="Search for bus service" data-description="Searches for bus services by bus stop"></a>
<a href="sg-bus-arrivals/add-bus-service.png" class="glightbox-sg-bus-arrivals" data-title="Add bus service" data-description="Queries the bus stop information and lists the bus services to allow the user to select 1 or more bus services to be tracked"></a>
<a href="sg-bus-arrivals/config-entries.png" class="glightbox-sg-bus-arrivals" data-title="Configuration entries" data-description="Shows the configured bus services"></a>
<a href="sg-bus-arrivals/dashboard.png" class="glightbox-sg-bus-arrivals" data-title="Dashboard" data-description="Dashboard shows the bus arrivals"></a>
<a href="sg-bus-arrivals/service-details.png" class="glightbox-sg-bus-arrivals" data-title="Service details"></a>
<a href="sg-bus-arrivals/train-service-alerts.png" class="glightbox-sg-bus-arrivals" data-title="Train service alerts" data-description="Alerts for train service disruptions"></a>

<script type="text/javascript">
    GLightbox({
        selector: ".glightbox-sg-bus-arrivals"
    });
</script>

## SG Bus Arrivals

<code>HomeAutomation</code> <code>Python</code>

SG Bus Arrivals is a custom integration for [Home Assistant](https://www.home-assistant.io/).
It uses the [LTA DataMall](https://datamall.lta.gov.sg/content/datamall/en.html) APIs to fetch data on public transport services.
The data can then be used in automations to display bus arrival times or trigger notifications based on geo-proximity or presence.

[📄 GitHub Repo](https://github.com/hanwg/sg-bus-arrivals) | [✒️ Blog Post](../posts/2025/05/sg-bus-arrivals)

---

## <img src="pdf-datatable-icon.png" alt="PDF DataTable logo" class="side-projects-icon" /> PDF DataTable

<code>React</code> <code>HTML</code> <code>CSS</code> <code>JavaScript</code>

PDF DataTable is a productivity tool that I built to export PDFs to CSV files.
I used the React-PDF library to render PDFs to allow me to select elements so that I can quickly filter out relevant records to be exported. 

[🌐 Website](https://pdf-datatable.hanwg.top) | [📄 GitHub Repo](https://github.com/hanwg/pdf-datatable) | [✒️ Blog Post](../posts/2025/04/pdf-datatable) 

---

## <img src="blog-icon.png" alt="My Tech Blog logo" class="side-projects-icon" /> My Tech Blog

<code>HTML</code> <code>CSS</code> <code>JavaScript</code>

In early 2025, I took the plunge and started this blog.
I already had a personal wiki based on [BookStack](https://www.bookstackapp.com/) which I created a long time back, but I wanted to create a different space to share my passions, expertise and thoughts.
A place where I can also track my journey and growth.
It isn't my first side project, but this one feels particularly meaningful for me.

[🌐 Website](..) | [📄 GitHub Repo](https://github.com/hanwg/blog) | [✒️️ Blog Post](../posts/2025/03/new-website)
