# Realistic Button Card Templates

Neumorphic, glow-reactive button card templates for [Home Assistant](https://www.home-assistant.io/) dashboards, built for [custom:button-card](https://github.com/custom-cards/button-card).

## Templates Included

| Template | Description |
|---|---|
| `realistic_button` | Base template — neumorphic card with dynamic color glow, Twinkly effect palettes, badge, state dot, and icon color matching |
| `realistic_button_weather` | Weather variant with animated rain drops, puddle, ripple effects, and automatic day/night icon switching |
| `realistic_button_climate` | Thermostat/climate variant with large temperature readout, setpoint display, and HVAC-action color tinting |
| `realistic_button_fireplace` | Fireplace variant with animated flame glow, flicker, and high/low/no-heat state detection |
| `realistic_button_fan` | Fan variant with speed-based spin animation, percentage labels, and glow |

## Prerequisites

- [Home Assistant](https://www.home-assistant.io/) 2024.1+
- [custom:button-card](https://github.com/custom-cards/button-card) installed via HACS
- [HACS](https://hacs.xyz/) (Home Assistant Community Store)

## Installation

### HACS (Recommended)

1. Open HACS in your Home Assistant instance
2. Click the **⋮** menu (top right) → **Custom repositories**
3. Add this repository URL:
   ```
   https://github.com/YOUR_USERNAME/realistic-button-card
   ```
4. Set category to **Plugin**
5. Click **Add**
6. Find **Realistic Button Card Templates** in the HACS Frontend store and click **Download**
7. **Restart Home Assistant** (or do a hard-refresh of the browser)

The JS resource is automatically added to your Lovelace configuration. The templates register themselves into `custom:button-card` on page load — no `!include` or YAML changes needed.

### Manual Installation

1. Download `dist/realistic-button-card.js` from this repository
2. Place it in `config/www/realistic-button-card/realistic-button-card.js`
3. Add the resource in **Settings → Dashboards → ⋮ → Resources**:
   - URL: `/local/realistic-button-card/realistic-button-card.js`
   - Type: **JavaScript Module**
4. Restart Home Assistant

### How It Works

The JS file auto-injects all five templates into `button_card_templates` when the Lovelace UI loads. You do **not** need to add `button_card_templates: !include ...` to your dashboard YAML — just use `template:` directly on any card.

> **Note:** If you also define templates with the same name in your dashboard YAML, the YAML version takes precedence (the JS will not overwrite existing templates).

## Quick Start

### Basic Light Button

```yaml
type: custom:button-card
entity: light.living_room
template: realistic_button
name: Living Room
```

### Weather Button

```yaml
type: custom:button-card
entity: weather.home
template: realistic_button_weather
name: Weather
variables:
  weather_condition_entity: weather.home
  weather_rain_entity: sensor.rain_gauge
```

### Climate / Thermostat Button

```yaml
type: custom:button-card
entity: climate.living_room
template: realistic_button_climate
name: Thermostat
variables:
  show_setpoint: true
  show_humidity: true
```

### Fireplace Button

```yaml
type: custom:button-card
entity: switch.fireplace
template: realistic_button_fireplace
name: Fireplace
```

### Fan Button

```yaml
type: custom:button-card
entity: fan.bedroom
template: realistic_button_fan
name: Bedroom Fan
```

## Configuration Reference

### `realistic_button` (Base Template)

| Variable | Default | Description |
|---|---|---|
| `accent` | `var(--accent-color)` | Accent color for glow and active states |
| `icon_name` | `null` | Force a specific icon (overrides entity icon) |
| `icon_on` / `icon_off` | `null` | Icons for active/inactive states |
| `subtitle` | `''` | Static label text (overrides auto-generated label) |
| `badge` | `''` | Badge text shown in top-right corner |
| `show_state_dot` | `false` | Show a colored status indicator dot |
| `picture` | `null` | Entity picture URL |
| `icon_color_matches_light` | `true` | Match icon color to light's RGB/HS/CT color |
| `glow_for_non_lights` | `true` | Show glow for non-light entities when active |
| `glow_non_light_color` | `var(--accent-color)` | Glow color for non-light entities |
| `glow_non_light_opacity` | `0.35` | Glow opacity for non-light entities |
| `glow_active_states` | `on,open,opening,...` | Comma-separated states that activate the glow |
| `glow_origin_x` | `50` | Glow radial gradient X origin (%) |
| `glow_origin_y` | `88` | Glow radial gradient Y origin (%) |
| `glow_radius_x` | `120` | Glow radial gradient X radius (%) |
| `glow_radius_y` | `90` | Glow radial gradient Y radius (%) |
| `glow_falloff` | `65` | Glow fade-out distance (%) |
| `twinkly_members` | `[]` | List of entity IDs for Twinkly group color detection |
| `twinkly_effect_map` | *(see YAML)* | Map of effect names → glow colors |
| `twinkly_effect_palettes` | *(see YAML)* | Map of effect names → multi-color palette arrays |
| `twinkly_multi_mode` | `blend` | How to render multi-color palettes (`blend`) |

### `realistic_button_weather`

| Variable | Default | Description |
|---|---|---|
| `weather_icon` | `''` | Force a specific weather icon |
| `weather_condition_entity` | `''` | Entity ID to read weather condition from |
| `weather_precip_entity` | `''` | Precipitation sensor entity ID |
| `weather_fallback_icon` | `mdi:weather-partly-cloudy` | Fallback icon |
| `weather_use_daynight` | `true` | Switch icons for day/night based on `sun.sun` |
| `weather_rain_enabled` | `false` | Force rain animation on |
| `weather_rain_entity` | `''` | Entity that triggers rain animation when active/numeric > 0 |
| `weather_rain_color` | `#4fc3f7` | Rain drop and puddle color |
| `weather_puddle_height` | `6` | Puddle height (%) |
| `weather_rain_drops` | `16` | Number of rain drops |
| `weather_rain_speed` | `1.2` | Rain animation speed multiplier |
| `weather_disable_base_glow` | `true` | Hide the base glow layer |

### `realistic_button_climate`

| Variable | Default | Description |
|---|---|---|
| `ambient_sensor` | `null` | Separate temperature sensor entity ID |
| `show_setpoint` | `true` | Show target temperature in footer |
| `show_humidity` | `false` | Show humidity in footer |
| `show_mode` | `false` | Show HVAC mode/action in footer |
| `heat_color` | `#ff7a3d` | Color for heating |
| `cool_color` | `#3b82f6` | Color for cooling |
| `auto_color` | `#a855f7` | Color for auto mode |
| `fan_color` | `#22c55e` | Color for fan mode |
| `dry_color` | `#f59e0b` | Color for dry mode |
| `bg_tint_alpha` | `0.18` | Background tint opacity |

### `realistic_button_fireplace`

| Variable | Default | Description |
|---|---|---|
| `fireplace_attr` | `null` | Attribute to read flame level from |
| `fireplace_high_states` | `[high, hi, max, ...]` | States considered "High" |
| `fireplace_low_states` | `[low, lo, min, ...]` | States considered "Low" |
| `fireplace_noheat_states` | `[on, 1]` | States considered "No Heat" |
| `fireplace_high_color` | `#ff3b3b` | Glow color for high |
| `fireplace_low_color` | `#ff3b3b` | Glow color for low |
| `fireplace_noheat_color` | `#ff9900` | Glow color for no-heat |
| `flame_enabled` | `true` | Enable flame animation |
| `flame_speed` | `2.8s` | Flame movement animation duration |
| `flame_flicker_speed` | `1.3s` | Flame flicker animation duration |

### `realistic_button_fan`

| Variable | Default | Description |
|---|---|---|
| `fan_control_entity` | `false` | Override entity for toggle action |
| `fan_on_color` | `rgb(34,197,94)` | Icon color when on |
| `fan_off_color` | `var(--secondary-text-color)` | Icon color when off |
| `fan_speed_labels` | `{33: Low, 66: Medium, 100: High}` | Map of percentage → label |
| `fan_spin_low` | `2.3s` | Spin duration at low speed |
| `fan_spin_med` | `1.2s` | Spin duration at medium speed |
| `fan_spin_high` | `.5s` | Spin duration at high speed |
| `fan_low_threshold` | `33` | Percentage threshold for low speed |
| `fan_med_threshold` | `66` | Percentage threshold for medium speed |
| `fan_value_attr` | `auto` | Attribute to read speed from (`auto`, `percentage`, `speed`, `preset_mode`) |
| `fan_glow_color` | `#22c55e` | Glow color |
| `fan_glow_alpha` | `0.3` | Glow opacity |

## Layout Tips

These buttons look best in a grid layout:

```yaml
type: grid
columns: 3
square: true
cards:
  - type: custom:button-card
    entity: light.kitchen
    template: realistic_button
    name: Kitchen
  - type: custom:button-card
    entity: light.bedroom
    template: realistic_button
    name: Bedroom
  - type: custom:button-card
    entity: switch.tv
    template: realistic_button
    name: TV
    variables:
      icon_name: mdi:television
```

## Development

### Editing Templates

The source of truth is `dist/realistic_button_templates.yaml`. After editing:

```bash
npm install      # first time only
npm run build    # regenerates dist/realistic-button-card.js
```

Commit both the YAML and the generated JS. The CI will verify they stay in sync.

### Project Structure

```
├── dist/
│   ├── realistic_button_templates.yaml   ← source of truth
│   └── realistic-button-card.js          ← generated (do not hand-edit)
├── build.js                              ← YAML → JS converter
├── hacs.json                             ← HACS plugin manifest
├── info.md                               ← HACS store description
└── package.json
```

## Troubleshooting

**Templates not loading?**
- Open browser DevTools → Console and look for the `realistic-button-card` registration message
- Make sure `custom:button-card` is installed and working
- Try a hard refresh (Ctrl+Shift+R / Cmd+Shift+R)
- Check that the resource is listed in **Settings → Dashboards → Resources**

**Templates in YAML override JS?**
- By design. If you define `realistic_button` in your dashboard YAML, that version wins. Remove it from YAML to use the JS-injected version.

## License

MIT — see [LICENSE](LICENSE).
