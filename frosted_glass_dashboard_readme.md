# Frosted Glass Home Assistant Dashboard

A modern, elegant, and fully‑responsive Home Assistant dashboard designed with the **Frosted Glass Theme** by [wessamlauf](https://github.com/wessamlauf/homeassistant-frosted-glass-themes). This README is written as a complete guide for anyone who wants to install, customise, and run this dashboard as their Home Assistant homepage.

---
## 🌟 Features
- **Frosted Glass UI** for a clean, modern visual style
- **Dynamic time & date** display
- **Weather widget** with animated icons
- **Transparent card styling** for all elements
- **Live UK & Canada clocks**
- **Christmas countdown card** with optional snowfall animation
- **Device status section** (lights, switches, and scenes)
- Fully responsive layout (desktop, tablet, wall panel)
- Works with **any Lovelace layout** (tested with grid & vertical-stack)

---
## 📦 Prerequisites
Before installing the dashboard, ensure the following components are installed:

### 1. **HACS – Home Assistant Community Store**
Required for custom cards.
- Install from: https://hacs.xyz/

### 2. **Frosted Glass Theme (Required)**
Theme repository:
- https://github.com/wessamlauf/homeassistant-frosted-glass-themes

Follow theme installation instructions in the repo.

### 3. **Custom Cards Required**
Install from HACS:
- **button-card** → For custom interactive tiles
- **mod-card** → For styling + snow effect wrappers
- **weather-card** (if not using default)

### 4. (Optional) **Kiosk Mode**
For hiding sidebar and header on wall tablets.
- https://github.com/maykar/kiosk-mode

---
## 📁 File Structure
You should store your dashboard YAML inside:
```
config/
 └── dashboards/
       └── frosted-glass-dashboard.yaml
```

Enable dashboards in **Settings → Dashboards → Three dots → Add Dashboard**.

---
## 🏠 Dashboard: Home Page (Main File)
Below is the full example for the **Home Page YAML**, formatted for easy reuse.

```yaml
title: Frosted Glass Dashboard
path: home
icon: mdi:home
panel: false
cards:

  # --- HEADER CLOCK & DATE ---
  - type: custom:button-card
    template: header_time
    styles:
      card:
        - height: 120px
        - background: rgba(255,255,255,0.15)
        - backdrop-filter: blur(12px)
        - border-radius: 20px
    custom_fields:
      time: >
        [[[ return new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}); ]]]
      date: >
        [[[ return new Date().toLocaleDateString(); ]]]

  # --- WEATHER WIDGET ---
  - type: weather-forecast
    entity: weather.home
    show_hourly_forecast: false
    style: |
      ha-card {
        background: rgba(255,255,255,0.20) !important;
        backdrop-filter: blur(15px);
        border-radius: 20px;
      }

  # --- WORLD CLOCKS ---
  - type: horizontal-stack
    cards:
      - type: custom:button-card
        name: UK Time
        entity: sensor.time
        show_icon: false
        styles:
          card:
            - background: rgba(255,255,255,0.15)
            - backdrop-filter: blur(12px)
            - border-radius: 20px
        state_display: >
          [[[
            return new Date().toLocaleTimeString('en-GB', {hour: '2-digit', minute:'2-digit'});
          ]]]

      - type: custom:button-card
        name: Canada Time
        show_icon: false
        styles:
          card:
            - background: rgba(255,255,255,0.15)
            - backdrop-filter: blur(12px)
            - border-radius: 20px
        state_display: >
          [[[
            return new Date().toLocaleTimeString('en-CA', {hour: '2-digit', minute:'2-digit', timeZone:'America/Toronto'});
          ]]]

  # --- CHRISTMAS COUNTDOWN (Optional) ---
  - type: custom:button-card
    entity: sensor.christmas_live_countdown
    name: 🎅 Christmas Countdown
    show_icon: false
    styles:
      card:
        - background: rgba(255,255,255,0.15)
        - border-radius: 20px
        - text-align: center
    custom_fields:
      snow: >
        <div class="snow"></div>
    extra_styles: |
      @keyframes snow {
        0% { transform: translateY(0); }
        100% { transform: translateY(200px); }
      }
      .snow {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-image: url('/local/snow.png');
        animation: snow 8s linear infinite;
      }

  # --- DEVICE CONTROLS ---
  - type: grid
    square: false
    columns: 2
    cards:
      - type: custom:button-card
        entity: light.living_room
        name: Living Room
        icon: mdi:lightbulb-on
        styles:
          card:
            - background: rgba(255,255,255,0.20)
            - backdrop-filter: blur(15px)
            - border-radius: 20px

      - type: custom:button-card
        entity: switch.kitchen
        name: Kitchen Switch
        icon: mdi:toggle-switch
        styles:
          card:
            - background: rgba(255,255,255,0.20)
            - backdrop-filter: blur(15px)
            - border-radius: 20px
```

---
## 🎨 Theme Configuration
Ensure your Home Assistant `configuration.yaml` contains:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

Place the Frosted Glass theme folder inside:
```
/config/themes/frosted_glass/
```

Then reload themes via **Developer Tools → YAML → Reload Themes**.

---
## 🔧 Customisation Tips
### Increase or reduce card transparency
Edit:
```
background: rgba(255,255,255,0.15)
```
Increase the last value (0.15 → 0.3) for less transparency.

### Change the blur intensity
```
backdrop-filter: blur(12px)
```
Increase to blur more.

### Remove snowfall animation
Delete the entire `custom_fields: snow` and `extra_styles` block.

---
## 📱 Recommended Devices
| Device | Works Well? | Notes |
|--------|--------------|--------|
| Desktop / Laptop | ✅ | Full resolution experience |
| iPad / Tablet | ✅ | Perfect for wall tablets |
| Phone | ⚠️ | Works, but best with layout tweaks |

---
## 🚀 Future Improvements
- Add full navigation pages
- Add energy dashboard integration
- Add notifications panel
- Add media player section

---
## 🙌 Credits
- **Frosted Glass Theme** by wessamlauf
- Custom-card developers
- Home Assistant community

---
## 📜 License
This dashboard configuration is free to use, modify, and redistribute.

