# Fox ESS Energy Card

An animated solar/battery/grid energy flow dashboard for Home Assistant, based on FoxESS inverter sensors. Supports any inverter integration — all sensors are configurable via the visual editor.

<img src="docs/Demo.gif" width="50%"/>

## Features

- Animated photon flow on solar, battery, grid and home wires
- Day/night sky gradient with sun rise/set, stars, and cloud system
- Live weather clouds and rain overlay (Rainy/Cloudy states)
- Detail overlay: PV string data, temperatures, fault codes
- Force Charge / Force Discharge work mode indicators
- Fully configurable — map any sensor to any field in the visual editor

---

## Installation

### Via HACS (recommended)

1. In HACS → **Frontend** → **+ Explore & Download Repositories**
2. Search for **Fox ESS Energy Card** and install
3. Add the resource in **Settings → Dashboards → Resources** (HACS does this automatically)

### Manual

1. Copy `dist/energy-flow-card.js` to `/config/www/energy-flow-card.js`
2. In HA: **Settings → Dashboards → Resources → Add Resource**
   - URL: `/local/energy-flow-card.js`
   - Type: `JavaScript module`

---

## Background Image

Upload your house background image to `/config/www/energy-house.png`  
(or set a custom path in the card editor under **Appearance → Background Image URL**).

The SVG canvas is **600 × 400** px. The image is scaled to fit with `preserveAspectRatio="xMidYMid meet"`.

---

## Card Configuration

Add the card to a dashboard and use the **visual editor** to map each sensor. All fields are optional — unmapped sensors default to `0` / `'N/A'`.

### Manual YAML example (FoxESS defaults)

```yaml
type: custom:energy-flow-card
solar_generation_sensor: sensor.foxessinverter_genload
grid_feed_in_sensor: sensor.foxessinverter_feed_in
grid_consumption_sensor: sensor.foxessinverter_grid_consumption
battery_charge_sensor: sensor.foxessinverter_battery_charge
battery_discharge_sensor: sensor.foxessinverter_rpower
battery_soc_sensor: sensor.foxessinverter_battery_soc
load_power_sensor: sensor.foxessinverter_load_power
todays_cost_sensor: sensor.todays_energy_cost
inverter_temp_sensor: sensor.foxessinverter_invtemp
ambient_temp_sensor: sensor.foxessinverter_ambtemp
battery_temp_sensor: sensor.foxessinverter_battery_temp
cell_temp_low_sensor: sensor.foxessinverter_bms_cell_temp_low
cell_temp_high_sensor: sensor.foxessinverter_bms_cell_temp_high
grid_voltage_sensor: sensor.foxessinverter_rvolt
grid_current_sensor: sensor.foxessinverter_rcurrent
battery_soh_sensor: sensor.foxessinverter_battery_soh
inverter_fault_sensor: sensor.foxessinverter_inverter_fault_code
inverter_state_sensor: sensor.foxessinverter_inverter_state
work_mode_select: select.foxessinverter_work_mode
pv1_power_sensor: sensor.foxessinverter_pv1_power
pv1_current_sensor: sensor.foxessinverter_pv1_current
pv1_voltage_sensor: sensor.foxessinverter_pv1_voltage
pv2_power_sensor: sensor.foxessinverter_pv2_power
pv2_current_sensor: sensor.foxessinverter_pv2_current
pv2_voltage_sensor: sensor.foxessinverter_pv2_voltage
pv3_power_sensor: sensor.foxessinverter_pv3_power
pv3_current_sensor: sensor.foxessinverter_pv3_current
pv3_voltage_sensor: sensor.foxessinverter_pv3_voltage
pv4_power_sensor: sensor.foxessinverter_pv4_power
pv4_current_sensor: sensor.foxessinverter_pv4_current
pv4_voltage_sensor: sensor.foxessinverter_pv4_voltage
day_cycle_boolean: input_boolean.energy_house_image_day_cycle
details_overlay_boolean: input_boolean.energy_vision_details
weather_entity: weather.alexandra_hills_hourly
background_image: /local/energy-house.png
```

### Config options reference

| Key | Description | Domain |
|-----|-------------|--------|
| `solar_generation_sensor` | Solar generation in **kW** | `sensor` |
| `grid_feed_in_sensor` | Grid export / feed-in in **kW** | `sensor` |
| `grid_consumption_sensor` | Grid import / consumption in **kW** | `sensor` |
| `battery_charge_sensor` | Battery charge power in **kW** | `sensor` |
| `battery_discharge_sensor` | Battery discharge power in **kW** | `sensor` |
| `battery_soc_sensor` | Battery state of charge **%** | `sensor` |
| `load_power_sensor` | Home load power in **kW** | `sensor` |
| `todays_cost_sensor` | Today's energy cost | `sensor` |
| `inverter_temp_sensor` | Inverter temperature °C | `sensor` |
| `ambient_temp_sensor` | Ambient temperature °C | `sensor` |
| `battery_temp_sensor` | Battery temperature °C | `sensor` |
| `cell_temp_low_sensor` | Battery cell low temp °C | `sensor` |
| `cell_temp_high_sensor` | Battery cell high temp °C | `sensor` |
| `grid_voltage_sensor` | Grid voltage V | `sensor` |
| `grid_current_sensor` | Grid current A | `sensor` |
| `battery_soh_sensor` | Battery state of health % | `sensor` |
| `inverter_fault_sensor` | Inverter fault code | `sensor` |
| `inverter_state_sensor` | Inverter state string | `sensor` |
| `work_mode_select` | Work mode select entity | `select` |
| `pv1_power_sensor` … `pv4_power_sensor` | PV string power kW | `sensor` |
| `pv1_current_sensor` … `pv4_current_sensor` | PV string current A | `sensor` |
| `pv1_voltage_sensor` … `pv4_voltage_sensor` | PV string voltage V | `sensor` |
| `day_cycle_boolean` | Toggle day/night sky cycle | `input_boolean` |
| `details_overlay_boolean` | Toggle detail overlay | `input_boolean` |
| `weather_entity` | Weather entity for cloud/rain effects | `weather` |
| `background_image` | Path to background image | string |

---

## Required Helper Entities

Create these two helpers in HA (**Settings → Helpers → + Create Helper → Toggle**):

| Entity ID | Purpose |
|-----------|---------|
| `input_boolean.energy_house_image_day_cycle` | Enables the day/night sky gradient and weather effects |
| `input_boolean.energy_vision_details` | Shows the detail overlay (PV strings, temps, fault codes) |

You can then add these to your dashboard as toggle buttons to control the card.

---

## License

MIT
