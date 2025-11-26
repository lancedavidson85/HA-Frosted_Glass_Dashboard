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

Add this to the top of the Dashboard RAW configuration editor file:
```yaml
kiosk_mode:
  kiosk: true
  hide_header: true
  admin_settings:
    kiosk: false
    hide_header: false
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
