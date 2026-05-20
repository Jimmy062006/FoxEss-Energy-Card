# Fox ESS Energy Card

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/hacs/integration)
[![GitHub Release](https://img.shields.io/github/release/DJS91/FoxEss-Energy-Card.svg)](https://github.com/DJS91/FoxEss-Energy-Card/releases)

An animated solar/battery/grid energy flow dashboard for Home Assistant, based on Fox ESS app visuals but better — all sensors are configurable via the visual editor. Supports **one or two inverters**, each with its own PV and battery, sharing a single grid and home load.

Best used if you are using the [FoxESS - Modbus](https://github.com/nathanmarlor/foxess_modbus) integration's available sensors, but not required.

<p float="left">
<img src="docs/Demo.gif" width="65%"/>
<img src="docs/Demo-forceCharge.gif" width="34%"/>
</p>

## Features
- Keep it clean or enable optional overlays for detailed monitoring.
- Animated energy flow on solar, battery, grid and home wires
- Day/night sky gradient linked to current time with sun rise/set
- Live weather clouds and rain overlay (sunny/Rainy/Cloudy states)
- Detail overlay: MPPT/PV data, system temps, fault codes, battery health and more
- Force Charge / Force Discharge work mode indicators and unique animations
- Fully configurable — map any sensor to any field in the visual editor
- **Dual inverter support** — INV2 is fully opt-in; leave all `inv2_*` keys blank for single-inverter use

---

## Breaking Change — v2.0 Key Rename

> **All per-inverter config keys now use an `inv1_` prefix.**
> Shared keys (`grid_*`, `load_power_sensor`, overlay toggles) are **unchanged**.

### Migration: find-and-replace in your YAML

| Old key | New key |
|---------|---------|
| `solar_generation_sensor` | `inv1_solar_generation_sensor` |
| `solar_label` | `inv1_solar_label` |
| `battery_charge_sensor` | `inv1_battery_charge_sensor` |
| `battery_discharge_sensor` | `inv1_battery_discharge_sensor` |
| `battery_soc_sensor` | `inv1_battery_soc_sensor` |
| `inverter_state_sensor` | `inv1_inverter_state_sensor` |
| `work_mode_select` | `inv1_work_mode_select` |
| `inverter_temp_sensor` | `inv1_inverter_temp_sensor` |
| `ambient_temp_sensor` | `inv1_ambient_temp_sensor` |
| `battery_temp_sensor` | `inv1_battery_temp_sensor` |
| `cell_temp_low_sensor` | `inv1_cell_temp_low_sensor` |
| `cell_temp_high_sensor` | `inv1_cell_temp_high_sensor` |
| `battery_soh_sensor` | `inv1_battery_soh_sensor` |
| `inverter_fault_sensor` | `inv1_inverter_fault_sensor` |
| `pv1_power_sensor` | `inv1_pv1_power_sensor` |
| `pv1_current_sensor` | `inv1_pv1_current_sensor` |
| `pv1_voltage_sensor` | `inv1_pv1_voltage_sensor` |
| `pv2_power_sensor` | `inv1_pv2_power_sensor` |
| `pv2_current_sensor` | `inv1_pv2_current_sensor` |
| `pv2_voltage_sensor` | `inv1_pv2_voltage_sensor` |
| `pv3_power_sensor` | `inv1_pv3_power_sensor` |
| `pv3_current_sensor` | `inv1_pv3_current_sensor` |
| `pv3_voltage_sensor` | `inv1_pv3_voltage_sensor` |
| `pv4_power_sensor` | `inv1_pv4_power_sensor` |
| `pv4_current_sensor` | `inv1_pv4_current_sensor` |
| `pv4_voltage_sensor` | `inv1_pv4_voltage_sensor` |

Keys that did **not** change: `grid_feed_in_sensor`, `grid_consumption_sensor`, `grid_voltage_sensor`, `grid_current_sensor`, `load_power_sensor`, `weather_entity`, `day_cycle_boolean`, `details_overlay_boolean`, `background_image`.

---

## Installation

### Via HACS (recommended)

1. In HACS → **⋮** (three dots top right) → Custom Repositories
2. paste in **https://github.com/DJS91/FoxEss-Energy-Card** as Repository and select **Dashboard** Type and click **Add**
3. Search for **Fox ESS Energy Card** and install
4. Hard Refresh Browser and the card is now available.

<!--
1. In HACS → **Frontend** → **+ Explore & Download Repositories**
2. Search for **Fox ESS Energy Card** and install
3. Add the resource in **Settings → Dashboards → Resources** (HACS does this automatically)
-->

### Manual

1. Copy `dist/energy-flow-card.js` to `/config/www/energy-flow-card.js`
2. In HA: **Settings → Dashboards → Resources → Add Resource**
   - URL: `/local/energy-flow-card.js`
   - Type: `JavaScript module`

### Required Helper Entities - >> YOU'LL NEED TO CREATE THESE SEPARATELY <<

<img width="531" height="51" alt="toggles" src="https://github.com/user-attachments/assets/95abc188-6067-479d-b9af-f83092291a96" />

Create these two helpers in HA (**Settings → Helpers → + Create Helper → Toggle**):

| Entity ID | Purpose |
|-----------|---------|
| `input_boolean.energy_house_image_day_cycle` | Enables the day/night sky gradient and weather effects |
| `input_boolean.energy_vision_details` | Shows the detail overlay (PV strings, temps, fault codes) |

You can then add these to your dashboard as toggles using the "Entities" card to control the card Details and Weather overlays.

---


## Card Configuration

Add the card to a dashboard and use the **visual editor** to map each sensor. All fields are optional — unmapped sensors default to `0` / `'N/A'`.

### Manual YAML example — single inverter

```yaml
type: custom:energy-flow-card
# Shared sensors (unchanged)
grid_feed_in_sensor: sensor.foxessinverter_feed_in
grid_consumption_sensor: sensor.foxessinverter_grid_consumption
grid_voltage_sensor: sensor.foxessinverter_rvolt
grid_current_sensor: sensor.foxessinverter_rcurrent
load_power_sensor: sensor.foxessinverter_load_power
# Inverter 1 — Base
inv1_solar_generation_sensor: sensor.foxessinverter_genload
inv1_solar_label: GEN LOAD
inv1_battery_charge_sensor: sensor.foxessinverter_battery_charge
inv1_battery_discharge_sensor: sensor.foxessinverter_rpower
inv1_battery_soc_sensor: sensor.foxessinverter_battery_soc
inv1_inverter_state_sensor: sensor.foxessinverter_inverter_state
inv1_work_mode_select: select.foxessinverter_work_mode
# Inverter 1 — Details
inv1_inverter_temp_sensor: sensor.foxessinverter_invtemp
inv1_ambient_temp_sensor: sensor.foxessinverter_ambtemp
inv1_battery_temp_sensor: sensor.foxessinverter_battery_temp
inv1_cell_temp_low_sensor: sensor.foxessinverter_bms_cell_temp_low
inv1_cell_temp_high_sensor: sensor.foxessinverter_bms_cell_temp_high
inv1_battery_soh_sensor: sensor.foxessinverter_battery_soh
inv1_inverter_fault_sensor: sensor.foxessinverter_inverter_fault_code
# Inverter 1 — PV Strings
inv1_pv1_power_sensor: sensor.foxessinverter_pv1_power
inv1_pv1_current_sensor: sensor.foxessinverter_pv1_current
inv1_pv1_voltage_sensor: sensor.foxessinverter_pv1_voltage
inv1_pv2_power_sensor: sensor.foxessinverter_pv2_power
inv1_pv2_current_sensor: sensor.foxessinverter_pv2_current
inv1_pv2_voltage_sensor: sensor.foxessinverter_pv2_voltage
inv1_pv3_power_sensor: sensor.foxessinverter_pv3_power
inv1_pv3_current_sensor: sensor.foxessinverter_pv3_current
inv1_pv3_voltage_sensor: sensor.foxessinverter_pv3_voltage
inv1_pv4_power_sensor: sensor.foxessinverter_pv4_power
inv1_pv4_current_sensor: sensor.foxessinverter_pv4_current
inv1_pv4_voltage_sensor: sensor.foxessinverter_pv4_voltage
# Overlay Toggles
weather_entity: weather.alexandra_hills_hourly
day_cycle_boolean: input_boolean.energy_house_image_day_cycle
details_overlay_boolean: input_boolean.energy_vision_details
```

### Adding a second inverter

Simply add the `inv2_*` keys for your second inverter. Leave any `inv2_*` key blank to skip it. The card automatically shows a symmetric dual-inverter layout when INV2 sensors are configured.

```yaml
# Inverter 2 — leave blank to use single-inverter layout
inv2_solar_generation_sensor: sensor.foxessinverter2_genload
inv2_solar_label: GEN LOAD
inv2_battery_charge_sensor: sensor.foxessinverter2_battery_charge
inv2_battery_discharge_sensor: sensor.foxessinverter2_rpower
inv2_battery_soc_sensor: sensor.foxessinverter2_battery_soc
inv2_inverter_state_sensor: sensor.foxessinverter2_inverter_state
inv2_work_mode_select: select.foxessinverter2_work_mode
inv2_inverter_temp_sensor: sensor.foxessinverter2_invtemp
inv2_ambient_temp_sensor: sensor.foxessinverter2_ambtemp
inv2_battery_temp_sensor: sensor.foxessinverter2_battery_temp
inv2_cell_temp_low_sensor: sensor.foxessinverter2_bms_cell_temp_low
inv2_cell_temp_high_sensor: sensor.foxessinverter2_bms_cell_temp_high
inv2_battery_soh_sensor: sensor.foxessinverter2_battery_soh
inv2_inverter_fault_sensor: sensor.foxessinverter2_inverter_fault_code
inv2_pv1_power_sensor: sensor.foxessinverter2_pv1_power
inv2_pv1_current_sensor: sensor.foxessinverter2_pv1_current
inv2_pv1_voltage_sensor: sensor.foxessinverter2_pv1_voltage
inv2_pv2_power_sensor: sensor.foxessinverter2_pv2_power
inv2_pv2_current_sensor: sensor.foxessinverter2_pv2_current
inv2_pv2_voltage_sensor: sensor.foxessinverter2_pv2_voltage
inv2_pv3_power_sensor: sensor.foxessinverter2_pv3_power
inv2_pv3_current_sensor: sensor.foxessinverter2_pv3_current
inv2_pv3_voltage_sensor: sensor.foxessinverter2_pv3_voltage
inv2_pv4_power_sensor: sensor.foxessinverter2_pv4_power
inv2_pv4_current_sensor: sensor.foxessinverter2_pv4_current
inv2_pv4_voltage_sensor: sensor.foxessinverter2_pv4_voltage
```

### Config options reference

**Shared Sensors** (unchanged from previous versions)

| Key | Description | Domain |
|-----|-------------|--------|
| `grid_feed_in_sensor` | Grid export / feed-in in **kW** | `sensor` |
| `grid_consumption_sensor` | Grid import / consumption in **kW** | `sensor` |
| `grid_voltage_sensor` | Grid voltage V | `sensor` |
| `grid_current_sensor` | Grid current A | `sensor` |
| `load_power_sensor` | Home load power in **kW** | `sensor` |

**Inverter 1 / Inverter 2 — Base Sensors**

Replace `inv1_` with `inv2_` for the second inverter. All `inv2_*` keys are optional.

| Key (inv1_ shown) | Description | Domain |
|-------------------|-------------|--------|
| `inv1_solar_generation_sensor` | Solar generation in **kW** | `sensor` |
| `inv1_solar_label` | Label shown above the solar node (default: `GEN LOAD`) | string |
| `inv1_battery_charge_sensor` | Battery charge power in **kW** | `sensor` |
| `inv1_battery_discharge_sensor` | Battery discharge power in **kW** | `sensor` |
| `inv1_battery_soc_sensor` | Battery state of charge **%** | `sensor` |
| `inv1_inverter_state_sensor` | Inverter state string | `sensor` |
| `inv1_work_mode_select` | Work mode select entity | `select` |
| `inv1_inverter_temp_sensor` | Inverter temperature °C | `sensor` |
| `inv1_ambient_temp_sensor` | Ambient temperature °C | `sensor` |
| `inv1_battery_temp_sensor` | Battery temperature °C | `sensor` |
| `inv1_cell_temp_low_sensor` | Battery cell low temp °C | `sensor` |
| `inv1_cell_temp_high_sensor` | Battery cell high temp °C | `sensor` |
| `inv1_battery_soh_sensor` | Battery state of health % | `sensor` |
| `inv1_inverter_fault_sensor` | Inverter fault code | `sensor` |

**Inverter 1 / Inverter 2 — PV Strings**

Replace `inv1_` with `inv2_` for the second inverter. Supports up to 4 PV strings per inverter.

| Key pattern | Description | Domain |
|-------------|-------------|--------|
| `inv1_pv1_power_sensor` … `inv1_pv4_power_sensor` | PV string power kW | `sensor` |
| `inv1_pv1_current_sensor` … `inv1_pv4_current_sensor` | PV string current A | `sensor` |
| `inv1_pv1_voltage_sensor` … `inv1_pv4_voltage_sensor` | PV string voltage V | `sensor` |

**Overlay Toggles**

| Key | Description | Domain |
|-----|-------------|--------|
| `weather_entity` | Weather entity for cloud/rain effects. Needs to return "sunny", "rainy" or "cloudy" | `weather` |
| `day_cycle_boolean` | Toggle day/night sky cycle | `input_boolean` |
| `details_overlay_boolean` | Toggle detail overlay | `input_boolean` |
| `background_image` | Custom background image URL (overrides built-in) | string |


---

## License
MIT

## Disclaimer
This card is not affiliated with Fox ESS brand or company and is a custom fan creation.
