# Motion Light (Weather + Sunset + Time + Seasonal Weather Logic)

A Home Assistant automation blueprint that turns on a light when motion is detected and decides whether to activate based on sunset, a fallback time, weather conditions, and seasonal logic.

## Overview

This blueprint is intended for motion-activated lighting that should behave differently during the day and at night.

- After sunset, the light can turn on regardless of season or weather.
- After a chosen fallback time, the light can also turn on regardless of season or weather.
- Before sunset and before the fallback time, the light turns on only if the current weather matches the allowed weather list and the current season is allowed.
- The light can optionally turn on at a chosen brightness level.
- The automation can skip turning the light on again if it is already on.
- The light turns off after motion has stayed clear for the configured duration.

Home Assistant blueprints are reusable automation templates that can be imported and then used to create automations from a shared YAML definition.

## Features

- Motion-triggered light activation
- Sunset override
- Late-evening fallback time override
- Weather-based daytime activation
- Seasonal control for pre-sunset weather logic
- Optional brightness percentage
- Skip if the light is already on
- Automatic turn-off after motion clears

## Inputs

| Input | Description |
|---|---|
| `motion_entity` | Motion sensor that triggers the automation. |
| `light_entity` | Light entity to turn on and off. |
| `weather_entity` | Weather entity, such as `weather.openweathermap`. |
| `season_entity` | Season sensor, usually `sensor.season`. |
| `allowed_seasons` | Seasons in which weather-based pre-sunset activation is allowed. |
| `allowed_weather_conditions` | Weather states that allow pre-sunset activation. |
| `force_on_after_time` | Time after which the light can turn on regardless of weather or season. |
| `light_duration` | How long the light remains on after motion goes clear. |
| `skip_if_light_on` | If enabled, does not re-run when the light is already on. |
| `brightness_pct` | Optional brightness percentage; `0` uses the light's default behavior. |

## Weather states

The blueprint uses Home Assistant weather entity states instead of provider-specific raw text values.

Supported values:

- `clear-night`
- `cloudy`
- `fog`
- `hail`
- `lightning`
- `lightning-rainy`
- `partlycloudy`
- `pouring`
- `rainy`
- `snowy`
- `snowy-rainy`
- `sunny`
- `windy`
- `windy-variant`
- `exceptional`

These are standard Home Assistant weather states exposed through weather entities and used consistently across supported integrations.

## Logic

The blueprint allows the light to turn on when any of these conditions is true:

1. It is after sunset.
2. It is after the configured fallback time.
3. It is before sunset and before the fallback time, and both of these are true:
   - the current season is in the allowed season list,
   - the current weather is in the allowed weather list.

This approach works well across the year because Home Assistant tracks sunset dynamically, while the fallback time still provides a reliable late-evening activation point during long summer days.

## Installation

### Import from a URL

Home Assistant supports importing a blueprint by URL through the Blueprints page.

1. Copy the URL of the blueprint YAML file.
2. In Home Assistant, go to **Settings** -> **Automations & scenes** -> **Blueprints**.
3. Click **Import Blueprint**.
4. Paste the URL and import it.
5. Create an automation from the blueprint and fill in the required inputs.

### Manual installation

A blueprint can also be installed manually by placing the YAML file in Home Assistant's blueprint folder.

1. Save the YAML file to:

```text
/config/blueprints/automation/
```

2. Reload blueprints or restart Home Assistant if needed.
3. Create a new automation from the blueprint.

## Repository contents

```text
.
├── README.md
└── motion-light-weather-sunset-season.yaml
```

## License

This project is licensed under the MIT License.
