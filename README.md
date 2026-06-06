# Guition ESP32-S3 4848S040 — ESPHome Smart Home Panel

---

## Hardware

- **Platform:** Guition ESP32-S3 4848S040 — 480×480 touchscreen panel running ESPHome on ESP-IDF
- **Flash:** 16MB
- **PSRAM:** Octal mode at 80MHz
- **Display:** ST7701S RGB at 12MHz
- **Touch:** GT911 capacitive
- **Backlight:** SPI via LEDC
- **Relays:** 3× GPIO

---

## UI — LVGL Multi-Tab Touchscreen

A full-screen tabview with a persistent status bar across the top, containing three tabs.

### **Tab 1 — Garage**

- Arc gauge showing roller door position (0–100%)
- Percentage label in large font
- Three contextual buttons: **Up / Stop / Down** — each dynamically enabled/disabled and colour-coded (blue = available, grey = not) based on HA binary sensors reflecting the door's current movement state
- Two animated light switch buttons (left = Garage light, right = Living Room group), with a shutter-fill animation that grows/shrinks an orange overlay to simulate the light turning on/off

### **Tab 2 — Scene**

- Two large scene buttons: **Away Home** and **Back Home**, each calling a HA scene action on tap

### **Tab 3 — Alarm (Alarmo)**

- Status label showing the current alarm state in colour-coded text (green = disarmed, amber = arming, orange = armed away, purple = armed night, red = triggered, etc.)
- PIN numpad (0–9, Clear) with masked display (`****`)
- Three action buttons: **Arm Away**, **Cat Away** (arm night), **Disarm** — each submits the current PIN to Alarmo via HA action

---

## Idle / Screensaver Page

Activates after 60 seconds of inactivity. Shows:

- Wallpaper background image (configurable JPEG, resized to 480×480)
- Month abbreviation + merged Day+Date (e.g. `JUN` / `SAT 07`) in large right-aligned text
- 3-day weather strip at the bottom: **Today / Tomorrow / Day After**, each showing a day label and min/max temperature sourced from HA sensors

---

## Status Bar *(always visible)*

- WiFi icon + signal percentage
- Clock (`HH:MM`), synced to HA time, updated every minute

---

## Backlight Management

- PWM-controlled via LEDC at 150Hz
- Touch wakes the screen to **80% brightness** and returns to the main page
- After inactivity timeout, dims to **35%** on the idle page
- Scheduled night window (configurable start/end hour via HA number entities) turns the backlight **fully off** during sleep hours and back on at the configured wake hour
- OTA flash suppresses backlight flicker during updates

---

## Connectivity & Integrations

- **Home Assistant API** — encrypted, bidirectional; reads sensors, text sensors, binary sensors; calls services/actions
- **WiFi** with fallback AP (`Guition-4Inch-V1`)
- **OTA** updates via ESPHome
- **Web server** and **captive portal** for local access
- **ESP-NOW** — receives a 4-byte command packet from a paired button device (MAC `98:A3:16:8E:C4:CC`), toggles Relay 3, then sends a 2-byte ACK back; debounced to 800ms
