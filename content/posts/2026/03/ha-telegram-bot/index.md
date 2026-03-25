---
title: "Navigating the Telegram Bot Migration in Home Assistant"
summary: "The why's and how's of Telegram bot migrations for Home Assistant"
date: 2026-03-19
tags:
  - SoftwareEngineering
  - HomeAutomation
cover:
  image: "ha-telegram-bot.png"
  alt: "Telegram"
---

If you have been using the [Home Assistant Telegram bot integration](https://www.home-assistant.io/integrations/telegram_bot/), you probably have seen a series of persistent migration alerts recently.
This post breaks down why these changes are happening and the steps that you can take to complete the migration smoothly.

---

## 2025.7: The Shift to UI Configuration

The shift from YAML-based to UI managed configuration has been a platform-wide goal for Home Assistant since 2020.

### Why the Change?

The core reasoning behind this transition was documented in Architecture Decision Record (ADR) [0010-integration-configuration](https://github.com/home-assistant/architecture/blob/master/adr/0010-integration-configuration.md).
It's a long read but the <abbr title="Too long, didn't read">TL;DR</abbr> can be summarized as follows:
- **Improved user experience:** Home Assistant is prioritizing a more accessible, UI-driven approach for integration configuration. Moving away from YAML-based configurations eliminates the risk of syntax errors or input validation issues and also allows for instant configuration changes without the need for a full system restart. 
- **Maintenance overhead.** Supporting both YAML-based and UI managed configurations creates a significant maintenance and support overhead for maintainers. Dropping support for YAML configuration allows maintainers to focus on stability and feature enhancements. 

This change was also discussed in the Home Assistant blog: [The future of YAML](https://www.home-assistant.io/blog/2020/04/14/the-future-of-yaml/).

### Migrating to UI configuration

With the release of [2025.7](https://www.home-assistant.io/blog/2025/07/02/release-20257#now-available-to-set-up-from-the-ui), the Telegram bot integration deprecated YAML configuration and introduced a new UI setup.
While Home Assistant will automatically import your existing YAML configuration from `configuration.yaml` into the UI, you will still need to manually delete the old entries.

For those interested in the technical details of the implementation, you can learn more in the Pull Request (PR): [PR #144617](https://github.com/home-assistant/core/pull/144617).

Note: As of the [2025.12](https://www.home-assistant.io/blog/2025/12/03/release-202512/#backward-incompatible-changes) release, YAML support for Telegram bot has been removed entirely.
Any existing YAML configuration will simply fail and there will be no automatic import or migration warnings.

If you missed the migration deadline and still have Telegram bot entries in your `configuration.yaml`, you will need to do the following:
1. Remove the deprecated entries from your `configuration.yaml`.
2. Restart your Home Assistant instance.
3. Refer to the [Telegram bot configuration documentation](https://www.home-assistant.io/integrations/telegram_bot/#configuration) to set up the integration using the UI.
4. Remember to [allowlist chat IDs](https://www.home-assistant.io/integrations/telegram_bot/#allowlisting-chat-ids-via-subentries) to enable your bot to send and receive messages with the chats defined in the list.

---

## 2025.11: Deprecation of the Legacy Telegram Integration

In the [2025.11](https://www.home-assistant.io/changelogs/core-2025.11) release, support for [notify entities](https://developers.home-assistant.io/docs/core/entity/notify/) was added for the **Telegram bot** integration.
Simultaneously, a migration alert was added for the **Telegram** integration via [PR #150720](https://github.com/home-assistant/core/pull/150720).

### Why is the Telegram Integration Being Deprecated?

There's 2 reasons behind the deprecation.

**The 2-Telegram Problem**

Historically, Home Assistant has maintained 2 separate Telegram integrations which has been a constant source of confusion for users:
- **Telegram bot**: This is the core engine that performs the actual sending and receiving of messages.
- **Telegram** (legacy): This is a wrapper for Telegram bot. It provides actions (formerly known as services) for sending messages to a specific chat.

To streamline the experience and reduce complexity, these 2 integrations are being merged into the Telegram bot integration.

**The Legacy Notify Platform is Being Retired**

The Telegram integration was built on the legacy [notify platform](https://www.home-assistant.io/integrations/notify/#action).
This platform creates actions known as **notifiers** which can then be used for sending notifications.

For example, the `configuration.yaml` below would create a `notify.sarah` action.
This enabled other integrations and modules such as the (legacy) [Alert](https://www.home-assistant.io/integrations/alert/) integration and the [Multi-Factor Authentication (MFA)](https://www.home-assistant.io/docs/authentication/multi-factor-auth/) module to send notifications by referencing the `sarah` notifier.

> [!EXAMPLE]
> **Legacy Telegram Notifier**
> ```yaml
> notify:
>   - platform: telegram
>     name: "sarah"  # notifier
>     chat_id: 1234567890
> ```

As explained in [architecture discussion #1041](https://github.com/home-assistant/architecture/discussions/1041), the notify platform is being retired due to the following issues:
- **No UI support.** The notify platform can only be set up via `configuration.yaml` which conflicts with the goal of making Home Assistant more user-friendly.
- **No schema validation.** Because the notifiers are generated dynamically, they do not have any associated schema. This results in limited input validation and often cause challenges when creating automations or scripts.
- **No translation support.** Notifiers are also missing translation keys required for localization, making them less accessible to the global audience.

The legacy notify platform is making way for the modern [notify action](https://www.home-assistant.io/integrations/notify/#notify-action) which allows you to send notifications to multiple targets using **notify entities**.

### Migrating Telegram to Telegram Bot

If you have received the migration alert, it means that you are still using the legacy Telegram integration in your `configuration.yaml`. A sample of the migration alert is as follows:

> **Migration of Telegram notify service**
>
> The Telegram `notify` service has been migrated.
> A new `notify` entity per chat ID is available now.
> Update all affected automations to use the new `notify.send_message` action exposed by these new entities and then restart Home Assistant.

To complete the migration, you will need to perform the following steps:
1. Update all automations and scripts to use modern notify actions (examples below).
2. Remove all Telegram notifiers from your `configuration.yaml`.
3. Restart your Home Assistant instance.

> [!IMPORTANT]
> If you are using integrations or modules that rely on Telegram notifiers (for example, Alert and MFA), there is currently **no direct migration path** at the moment.
> Those integrations and modules must first be updated by their respective maintainers to support the modern notify action.
> Because of this dependency, the removal of the legacy Telegram integration has been **postponed from 2026.5 to 2026.8 (tentative)**.

Here are some examples on how you can update your automations and scripts:

> [!TIP]
> **Use Telegram bot notification actions**
>
> The modern `notify.send_message` notify action only support the `title` and `message` parameters.
> If you are using integration specific features, you must use the integration's actions.
> You can refer to the available [notification actions](https://www.home-assistant.io/integrations/telegram_bot#notification-actions) provided by Telegram bot.

> [!TIP]
> **Notifiers vs Notify Entities**
>
> Notifiers are **action names** (not actions!) while notify entities are **entities** representing a notification target.
A common mistake is specifying notify entities instead of notifiers in the `configuration.yaml` when using the legacy notify platform.

> [!EXAMPLE]
> **Simple notification with text message only**
> ```yaml
> # Old
> action: notify.sarah
> data:
>   message: "Yay! A message from Home Assistant."
> ```
>
> ```yaml
> # New
> action: notify.send_message  # replaced
> data:
>   message: "Yay! A message from Home Assistant."
> ```

> [!EXAMPLE]
> **Notification with integration specific features (for example, inline keyboard)**
> Reminder: You cannot use the generic `notify.send_message` action for unsupported features.
> ```yaml
> # Old
> action: notify.sarah
> data:
>   message: "Yay! A message from Home Assistant."
>   data:
>     inline_keyboard:
>       - 'Task 1:/command1, Task 2:/command2'
> ```
> 
> ```yaml
> # New
> action: telegram_bot.send_message  # replaced
> data:
>   message: "Yay! A message from Home Assistant."
>   inline_keyboard:  # note: no nesting of `data
>     - 'Task 1:/command1, Task 2:/command2'
> ```

> [!EXAMPLE]
> **Another example notification using integration specific feature: Photos**
> ```yaml
> # Old
> action: notify.sarah
> data:
>   message: "Yay! A message from Home Assistant."
>   data:
>     photo:
>       - url: "http://example.com/photo.jpg"
> ```
> 
> ```yaml
> # New
> action: telegram_bot.send_photo  # replaced
> data:
>   message: "Yay! A message from Home Assistant."
>   url: "http://example.com/photo.jpg"  # note: no nesting of `data`
> ```
> Side note: Multiple photos are not supported at the moment but there's an open PR ([PR #160939](https://github.com/home-assistant/core/pull/160939)) for this feature.

---

## 2026.1: Strict Action Parameters and the `timeout` Deprecation

The [2026.1](https://www.home-assistant.io/blog/2026/01/07/release-20261/#backward-incompatible-changes) release introduced 2 changes which were implemented by [PR #155198](https://github.com/home-assistant/core/pull/155198).

### The Move to Strict Action Schemas

Prior to 2026.1, the schema for Telegram bot actions used a `ALLOW_EXTRA` setting which ignored any unsupported parameters you included in the action's `data` field.
While this seemed convenient, it wasn't ideal as this setting allowed typos and obsolete configurations to go unnoticed.

To catch configuration errors early, the schema is now strictly enforced.
As a result, you may encounter the `extra keys not allowed @ data` error when running any Telegram bot action.

> [!NOTE]
> This deprecation does not trigger any migration alerts.
> This is because the change involves dropping an undocumented feature which is not officially supported.
> 
> Your only heads-up is the notice in the "Backward-incompatible changes" section of the release notes.

**The Migration**

To complete the migration, you must update all your automations and scripts to remove all unsupported parameters in the `data` field.
In the following example, `telegram_bot.send_message` was the action involved and `no_such_parameter` is the offending field to be removed.

> [!EXAMPLE]
> **Action with unsupported parameter**
> ```yaml
> action: telegram_bot.send_message
> data:
>   message: "Yay! A message from Home Assistant."
>   no_such_parameter: "Oops"  # remove this line!
> ```
> Error: Failed to perform the action telegram_bot.send_message. extra keys not allowed @ data['no_such_parameter']. Got None

### Deprecation of the `timeout` Action Parameter

Besides strict schema validation, the 2026.1 release also deprecated the `timeout` action parameter.
The migration alert is triggered when a Telegram bot action which uses the `timeout` parameter is executed:

> [!WARNING]
> **The timeout parameter for Telegram bot is being removed**
> 
> Update all affected automations and scripts to remove the `timeout` parameter and then click SUBMIT to fix this issue.
> The deprecated parameter was last seen in the `{action}` action originating from `{action_origin}`.

**Don't we need timeouts?**

The `timeout` parameter was originally intended for actions handling large files such as `telegram_bot.send_videos`.
However, there was a bug and the setting was incorrectly applied to **read timeouts** (the time allowed for receiving response) instead of **write timeouts** (the time allowed for uploads).

The community offered several suggestions, including:
- Fixing the bug while retaining the `timeout` parameter.
- Introducing a new option to configure a global `timeout` that would apply for all actions.

Ultimately, the Home Assistant core team declined the suggestions.
The consensus is that timeouts are technical implementation details which the user shouldn't have to manage manually and it conflicts with the goal of a user-friendly experience.

To address the timeout issue for large file transfers (and also slow networks), a separate update ([PR #162978](https://github.com/home-assistant/core/pull/162978)) was included in the [2026.3](https://www.home-assistant.io/changelogs/core-2026.3) release.
In this update, the default write timeout of 20 seconds was increased to **30 minutes** across all Telegram bot actions.
The new timeout was chosen based on typical bandwidth speeds of most users and also to accommodate the **2GB maximum file size limit** supported by Telegram.

**Handling the `timeout` migration**

To resolve the migration alert, identify the automation/script and action flagged in the alert.
Then, update the action to remove the `timeout` parameter from the `data` field.

> [!EXAMPLE]
> ```yaml
> action: telegram_bot.send_video
> data:
>   url: "https://example.com/video.mpeg"
>   timeout: 30  # remove this line!
> ```

---

## 2026.3: Deprecation of the `target` Action Parameter

### Why `target` is Going Away

The `target` action parameter originated from the legacy notify platform for specifying recipients that will receive the notification. 
However, as Home Assistant evolved, `target` is also used for specifying devices, entities and areas (or a combination of them) where an action is performed (see: [Targeting areas and devices](https://www.home-assistant.io/docs/scripts/perform-actions/#targeting-areas-and-devices)).

To better distinguish action targets and target parameter, the `target` parameter is being deprecated in the [2026.3](https://www.home-assistant.io/changelogs/core-2026.3) release via [PR #159745](https://github.com/home-assistant/core/pull/159745).

Here's an example to illustrate the differences: 

> [!EXAMPLE]
> ```yaml
> # Action target
> action: light.turn_on
> target:  # not nested
>   area_id: living_room
>   device_id:
>   - ff22a1889a6149c5ab6327a8236ae704
>   - 52c050ca1a744e238ad94d170651f96b
>   entity_id:
>     - light.hallway
> ```
>
> ```yaml
> # Target parameter
> action: telegram_bot.send_message
> data:
>   message: "Yay! A message from Home Assistant."
>   target:  # nested in `data`
>     - 1234567890
> ```

### Migrating `target` to `entity_id` or `chat_id`

You will need to perform migration if you have received the following migration alert:

> [!WARNING]
> **The `target` parameter for Telegram bot is being removed**
> 
> The `target` parameter is being deprecated.
> You should update any automations and scripts that use that parameter in Telegram bot actions.
> Use `entity_id` to specify Telegram bot notify entities or `chat_id` to replace `target`.

There are 2 options for migration:
1. Replace `target` with `entity_id` by specifying notify entities. 
2. Rename `target` to `chat_id`.

> [!EXAMPLE]
> ```yaml
> # Old
> action: telegram_bot.send_message
> data:
>   message: "Yay! A message from Home Assistant."
>   target:  # deprecated
>     - 1234567890
> ```
>
> ```yaml
> # Option 1: Replace with `entity_id`
> action: telegram_bot.send_message
> data:
>   message: "Yay! A message from Home Assistant."
>   entity_id: notify.telegram  # use your notify entity
> ```
>
> ```yaml
> # Option 2: Rename `target` to `chat_id`
> action: telegram_bot.send_message
> data:
>   message: "Yay! A message from Home Assistant."
>   chat_id:  # rename `target` to `chat_id`
>     - 1234567890  # replace with your chat_id
> ```

**Why Telegram bot doesn't support action targets yet**

If you have opted for the first migration option, you might find it strange that the `entity_id` is nested in the `data` field (as an action parameter) instead of `target` (as an action target):
> ```yaml
> action: telegram_bot.send_message
> #target:
> #  entity_id:  <-- shouldn't it be here?
> #    - notify.telegram
> data:
>   message: "Yay! A message from Home Assistant."
>   entity_id:  # why is it under `data`?
>     - notify.telegram
> ```

This behavior, reported in [issue #164503](https://github.com/home-assistant/core/issues/164503), is due to a platform-wide validation bug.
Since fixing this bug could impact other integrations, the safest and easiest course of action was to wait for the removal of `target` parameter in 2026.9 before proceeding with the implementation of action targets for Telegram bot.

The good news?
Once action targets have been implemented, you don't need to perform another migration; your setup will continue to work seamlessly:

> [!Example]
> These 2 actions are identical: action targets are automatically formatted into action parameters.
> ```yaml
> # Action target
> action: telegram_bot.send_message
> target:
>   entity_id:
>     - notify.telegram
> data:
>   message: "Yay! A message from Home Assistant."
> ```
> ```yaml
> # Action parameter
> action: telegram_bot.send_message
> data:
>   message: "Yay! A message from Home Assistant."
>   entity_id:
>     - notify.telegram
> ```

**Troubleshooting the `call_service` Migration Alert**

If your migration alert points to `call_service` rather than a specific automation or script, it means the deprecated parameter is being triggered by an underlying service.
Currently, Home Assistant is not able to pinpoint which specific integration or service is making this call.

In this scenario, there's no possible migration path.
You will need to wait for the relevant integration or module developer to release an update that supports the new schema.

> [!NOTE]
> In [PR #165299](https://github.com/home-assistant/core/pull/165299), the legacy Telegram integration has been updated to use the new schema.
> If this was the source of your migration alert, it should be resolved starting from the **2026.4** release.

---

## Closing Remarks

While this series of deprecations seem like a migration hurdle, each step is an effort to make the Telegram bot integration more stable and user-friendly.

Here's a summary of the deprecations:

| Feature                              | Deprecation | Removal            |
|--------------------------------------|-------------|--------------------|
| YAML configuration                   | 2025.7      | 2025.12            |
| ⚠️ Telegram integration              | 2025.11     | 2026.8 (Tentative) |
| Strict schema validation for actions | 2026.1      | 2026.1             |
| `timeout` action parameter           | 2026.1      | 2026.7             |
| `target` action parameter            | 2026.3      | 2026.9             |

> [!IMPORTANT]
> ⚠️ Telegram support is NOT leaving Home Assistant.
> The legacy **Telegram** integration is simply being merged into the **Telegram bot** integration.
