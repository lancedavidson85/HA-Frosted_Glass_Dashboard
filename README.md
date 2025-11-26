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
  https://github.com/custom-cards/button-card
- **mod-card** → For styling + snow effect wrappers  
  https://github.com/thomasloven/lovelace-card-mod
- **weather-card** (if not using built-in weather forecast)  
  https://github.com/bramkragten/weather-card

### 4. (Optional) **Kiosk Mode**
For hiding sidebar and header on wall tablets.
- https://github.com/maykar/kiosk-mode

Add this to the top of the Dashboard RAW configuration editor file:
```yaml
kiosk_mode:
  kiosk: true
  hide_header: true
  admin_settings:
    kiosk: false
    hide_header: false
```
This will only hide for non admins so easy to edit when needing to.

---
## 🎨 Theme Configuration
Ensure your Home Assistant `configuration.yaml` contains:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

To enable the Frosted Glass theme you need to select it from each device under the user profile:
<img width="930" height="351" alt="image" src="https://github.com/user-attachments/assets/76a6645a-4057-4d69-807d-01bee79d8466" />


If the theme is not showing, reload themes via **Developer Tools → YAML → Reload Themes**.

---
## 🔧 Customisation Tips
### 🎅 Christmas Day Countdown
If you want to add the countdown box to your dashboard you will need to make sure you have the "button-card" installed.
Step 1:
Create a new sensor in your Configuration.yaml file for the countdown timer.
```yaml
template:
  - trigger:
      - platform: time_pattern
        seconds: "/1"
    sensor:
      - name: "Christmas Live Countdown"
        state: >
          {% set now = now() %}
          {% set target = now.replace(month=12, day=25, hour=0, minute=0, second=0, microsecond=0) %}
          {% if target < now %}
            {% set target = target.replace(year=now.year + 1) %}
          {% endif %}
          {% set diff = target - now %}
          {{ diff.days }}d {{ (diff.seconds // 3600) }}h {{ (diff.seconds % 3600) // 60 }}m {{ diff.seconds % 60 }}s
        attributes:
          days: >
            {% set now = now() %}
            {% set target = now.replace(month=12, day=25) %}
            {% if target < now %}
              {% set target = target.replace(year=now.year + 1) %}
            {% endif %}
            {{ (target - now.date()).days }}
```
Now create a new card - Select Manual - Paste the below yaml. 
```yaml
type: custom:button-card
entity: sensor.christmas_live_countdown
name: 🎅 Christmas Day Countdown 🎄
show_icon: false
show_state: true
show_name: true
tap_action:
  action: none
hold_action:
  action: none
custom_fields:
  bell: <div class="bell">🔔</div>
styles:
  card:
    - padding: 20px
    - border-radius: 20px
    - text-align: center
    - font-size: 22px
    - position: relative
    - overflow: hidden
    - background: rgba(0,0,0,0.1)
    - box-shadow: 0 0 15px rgba(255,255,255,0.15)
    - animation: |
        [[[
          const days = states['sensor.christmas_live_countdown']?.state.split(' ')[0];
          return (parseInt(days) <= 7) ? 'festiveGlow 3s ease-in-out infinite alternate' : 'none';
        ]]]
  name:
    - font-size: 21px
    - font-weight: bold
    - color: white
    - text-shadow: 0 0 6px rgba(255,255,255,0.9)
    - position: relative
    - z-index: 5
    - margin-bottom: 8px
  state:
    - font-size: 30px
    - font-weight: bold
    - color: white
    - text-shadow: 0 0 8px rgba(255,255,255,1)
    - position: relative
    - z-index: 5
    - animation: flip 0.6s ease 1
  custom_fields:
    bell:
      - position: absolute
      - top: 6px
      - right: 6px
      - font-size: 28px
      - animation: wiggle 1.8s infinite ease-in-out
      - z-index: 10
extra_styles: |
  /* 🎅 Santa Hat */
  ha-card h1::before, ha-card h2::before {
    content: "🎅";
    position: absolute;
    top: -10px;
    right: 50%;
    transform: translateX(40px) rotate(-15deg);
    font-size: 32px;
  }

  /* ✨ Soft Glow Christmas Lights Border */
  ha-card {
    border: 2px solid rgba(255,255,255,0.15);
    box-shadow:
      0 0 10px rgba(255,0,0,0.4),
      0 0 20px rgba(0,255,0,0.3),
      0 0 25px rgba(255,255,255,0.2);
    animation: lightPulse 4s ease-in-out infinite alternate;
  }

  @keyframes lightPulse {
    0% {
      box-shadow:
        0 0 8px rgba(255,0,0,0.35),
        0 0 14px rgba(0,255,0,0.25),
        0 0 18px rgba(255,255,255,0.2);
    }
    100% {
      box-shadow:
        0 0 15px rgba(255,0,0,0.55),
        0 0 26px rgba(0,255,0,0.45),
        0 0 32px rgba(255,255,255,0.35);
    }
  }

  /* 🟥🟩 Festive Card Glow (last week) */
  @keyframes festiveGlow {
    0% { box-shadow: 0 0 25px rgba(255,0,0,0.5); }
    100% { box-shadow: 0 0 25px rgba(0,255,0,0.5); }
  }

  /* ⏳ Flip Animation */
  @keyframes flip {
    0% { transform: rotateX(0deg); opacity: 0.4; }
    50% { transform: rotateX(90deg); opacity: 0.6; }
    100% { transform: rotateX(0deg); opacity: 1; }
  }

  /* 🌫 Depth Blur */
  ha-card::before {
    content: "";
    position: absolute;
    inset: 0;
    backdrop-filter: blur(2px);
    z-index: 0;
    opacity: 0.3;
  }

  /* ❄✨ Advanced Snowfall */
  ha-card::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 1;

    background-image:
      radial-gradient(white 1px, transparent 1px),
      radial-gradient(white 1px, transparent 1px),
      radial-gradient(white 2px, transparent 2px),
      radial-gradient(white 2px, transparent 2px),
      radial-gradient(white 3px, transparent 3px),
      radial-gradient(white 3px, transparent 3px);

    background-size:
      40px 40px,
      50px 50px,
      70px 70px,
      90px 90px,
      110px 110px,
      130px 130px;

    animation: snowDrift 16s linear infinite,
               snowTwinkle 3s ease-in-out infinite alternate;
    opacity: 0.45;
  }

  @keyframes snowDrift {
    0% {
      background-position:
        0px -80px,
        20px -120px,
        -10px -150px,
        15px -200px,
        -20px -250px,
        10px -300px;
    }
    50% {
      background-position:
        10px 80px,
        -20px 120px,
        15px 150px,
        -15px 200px,
        25px 250px,
        -10px 300px;
    }
    100% {
      background-position:
        -10px 260px,
        10px 300px,
        -15px 330px,
        20px 360px,
        -25px 400px,
        15px 440px;
    }
  }

  @keyframes snowTwinkle {
    0% { opacity: 0.30; }
    100% { opacity: 0.55; }
  }

  /* 🔔 Bell Wiggle */
  @keyframes wiggle {
    0%, 100% { transform: rotate(0deg); }
    25% { transform: rotate(8deg); }
    50% { transform: rotate(-8deg); }
    75% { transform: rotate(5deg); }
  }
triggers_update: all
```

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
