# Panduan Pengkabelan ESP32 Mini Weather Station

## 📋 Gambaran Umum

Panduan ini memberikan instruksi pengkabelan langkah demi langkah untuk merakit Stasiun Cuaca Mini ESP32.

### Alat yang Diperlukan
- ESP32-C3 DevKit
- SSD1306 OLED 128x64 (I2C)
- DHT22 Sensor
- TTP223 Touch Sensor
- Kabel jumper (male-to-female)
- Breadboard
- Resistor: 4.7kΩ (2x), 10kΩ (1x)
- Multimeter (opsional)

### Catatan Keselamatan
- Gunakan 3.3V untuk semua komponen
- Jangan melebihi 5V pada pin GPIO
- Ground diri Anda untuk mencegah ESD

---

## 🔌 Pinout

### ESP32-C3 Pin Assignment

| Pin | Fungsi | Terhubung Ke |
|-----|--------|--------------|
| GPIO 8 | I2C SDA | OLED SDA |
| GPIO 9 | I2C SCL | OLED SCL |
| GPIO 2 | Data | DHT22 Data |
| GPIO 4 | Touch | TTP223 OUT |
| 3.3V | Power | VCC semua komponen |
| GND | Ground | GND semua komponen |

---

## 🔗 Wiring Diagram

```
ESP32-C3 DevKit
├─ GPIO 8 ──── OLED SDA
├─ GPIO 9 ──── OLED SCL
├─ GPIO 2 ──── DHT22 Data
├─ GPIO 4 ──── TTP223 OUT
├─ 3.3V ────── OLED VCC
│              DHT22 VCC
│              TTP223 VCC
└─ GND ─────── OLED GND
               DHT22 GND
               TTP223 GND
```

### Pull-up Resistor
- **I2C (SDA/SCL):** 4.7kΩ ke 3.3V
- **DHT22 Data:** 10kΩ ke 3.3V

---

## 🖼️ Skematik

![Wiring Diagram](/assets/Schematic-Weather-Station.png)

---

## ✅ Verifikasi

### 1. Tes Daya
- Ukur voltase 3.3V pada rail
- Pastikan tidak ada short circuit

### 2. Tes OLED
Upload `test/oled_test.ino`:
```cpp
#include <Wire.h>
#include <Adafruit_SSD1306.h>
// ... kode test ...
```
Expected: Layar menampilkan "Hello OLED"

### 3. Tes DHT22
Upload `test/dht_test.ino`:
Expected: Serial monitor menampilkan suhu

### 4. Tes Touch
Upload `test/touch_test.ino`:
Expected: Serial monitor berubah saat disentuh

---

## 🐞 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| OLED kosong | Cek SDA/SCL, pull-up 4.7kΩ |
| DHT NaN | Cek pull-up 10kΩ, delay >2s |
| Touch tidak responsif | Cek VCC/GND, active LOW |
| ESP32 tidak boot | Cek power supply, USB cable |

---

## 📚 Referensi

- [Adafruit SSD1306 Guide](https://learn.adafruit.com/monochrome-oled-breakouts)
- [DHT22 with ESP32](https://randomnerdtutorials.com/esp32-dht11-dht22-temperature-humidity-sensor-arduino-ide/)
- [ESP32-C3 Pinout](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/hw-reference/esp32c3/user-guide-devkitm-1.html)

---

*Terakhir Diperbarui: 06 November 2025*
