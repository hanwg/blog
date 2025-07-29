---
title: "Home Assistant custom integration tutorial"
summary: "How to create a new Home Assistant custom integration"
date: 2025-05-02
tags:
  - SoftwareEngineering
---
I'm developing a custom Home Assistant integration, and I hope this post will help new developers create their own by explaining what I had to learn.
The custom integration that I'm building is [SG Bus Arrivals](https://github.com/hanwg/sg-bus-arrivals) and you can reference code from this repository as an example. 

## Setting up your development environment

The [dev container setup](https://developers.home-assistant.io/docs/development_environment#developing-with-visual-studio-code--devcontainer) is the simplest way to set up your development environment.

In this approach, you will be creating a container which contains your source code along with the latest release version of Home Assistant.
All development will be done within this container and not directly on your local machine.

This approach streamlines the development process since code changes are always in-sync with the development environment and you will also have the added benefit of the debugger.

## Custom integration folder structure
Your custom integration should minimally have the following folder structure:
```
config/custom_components/<domain>
├── __init__.py
├── manifest.json
├── config_flow.py
├── <platform>.py
└── translations
    └── en.json
```

## Elements of a custom integration

### Manifest

The [manifest](https://developers.home-assistant.io/docs/creating_integration_manifest/) `config/custom_components/<domain>/manifest.json` is a file describing your integration.

Tips:
- for custom integrations, you MUST specify a value for `version`
- You MUST specify `"config_flow": true` if your integration uses config flow for UI-based configuration
- Entries in the manifest SHOULD be sorted
- If your configuration can only have 1 entry, specify `"single_config_entry": true` and Home Assistant will handle the validation for you

### Config Flow

Config flow (`config/custom_components/<domain>/config_flow.py`) is the user interface for setting up your integration.
All data is stored as `config_entries` objects which are saved in the `config/.storage/core.config_entries` json file of your Home Assistant instance.

The config flow consists of **steps** with the first step having a `step_id` of `user`.
The function naming convention for a **step** is `async_step_<step_id` hence, the function name for the first step is `async_step_user`. 

Each step has its own form fields which uses the python [voluptuous](https://pypi.org/project/voluptuous/) data validation library to define the schema and perform validation.

Typically, each step needs to do the following:

1) **Show the form** - User has not submitted the form or the submitted form has validation errors.
   In this case we need to use the `async_show_form` function to display the form to the user. 
   ```python
   async def async_step_user(
        self, user_input: dict[str, Any] | None = None
   ) -> ConfigFlowResult:
        
        errors: dict[str, str] = {}
        errors = your_validation(user_input, errors)
   
        if not user_input or errors:
            return self.async_show_form(
                step_id="user", // step_id must match your function name
                data_schema=vol.Schema({vol.Required("field1"): str})
            )
   ```

2) **Chain more steps** - You can chain steps to provide a configuration wizard user experience.
   To do this, you can define arbitrary step_id and its corresponding `async_step_<step_id>` function.
   ```python
   async def async_step_user(
        self, user_input: dict[str, Any] | None = None
   ) -> ConfigFlowResult:
        
        // ... handle other scenarios     
   
        return await self.async_step_next()  // invoke the next step (e.g. step_id=next)
   
   async def async_step_next(
       self, user_input: dict[str, Any] | None = None
   ) -> ConfigFlowResult:
   
       // ... handle other scenarios     
   
       return self.async_show_form(
            step_id="next",
            data_schema=vol.Schema({vol.Required("field2"): str})
       )
   ```

3) **Create the configuration entry**. Once you are done with the user inputs, invoke `async_create_entry` to create the configuration entry which will be passed to the `async_setup_entry` function in `__init__.py`.
   ```python
        return self.async_create_entry(
            title="Config entry display", data=user_input
        )
    ```

### Init
`async_setup_entry` function in the `<platform>.py` file.

Platform

### Translations

The Translation topic is already well covered by the [official documentation](https://developers.home-assistant.io/docs/config_entries_config_flow_handler#translations) so I'll skip this section. 