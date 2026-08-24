<h1 align="center">
🌤️ ESP32 Mini Weather Station
</h1>

<p align="center">
  <img src="/assets/mini_weather_station_banner.png?height=400&width=700" alt="ESP32 Mini Weather Station" width="700"/>
</p>

<p align="center">
  <em>Stasiun cuaca mini berbasis ESP32-C3 dengan OLED 128x64, animasi mata mochi, sensor DHT22, dan deep sleep.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/last_commit-today-brightgreen?style=for-the-badge" />
  <img src="https://img.shields.io/badge/language-C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white" />
  <img src="https://img.shields.io/badge/platform-ESP32--C3-00ADD8?style=for-the-badge&logo=espressif&logoColor=white" />
  <img src="https://img.shields.io/badge/framework-Arduino-00979D?style=for-the-badge&logo=arduino&logoColor=white" />
  <img src="https://img.shields.io/badge/API-Open--Meteo-7B68EE?style=for-the-badge&logo=weather&logoColor=white" />
</p>

---

## 📋 Daftar Isi

- [✨ Fitur Utama](#-fitur-utama)
- [📸 Demo](#-demo)
- [🧩 Komponen](#-komponen)
- [⚡ Mulai Cepat](#-mulai-cepat)
- [📚 Dokumentasi](#-dokumentasi)
- [🤝 Kontribusi](#-kontribusi)
- [📄 Lisensi](#-lisensi)

---

## ✨ Fitur Utama

| Fitur | Deskripsi |
|-------|-----------|
| 🖥️ **OLED Display** | Tampilan 128x64 dengan 4 slide informatif |
| 👀 **Mochi Eyes** | Animasi mata lucu yang berkedip & bergerak |
| 🌡️ **DHT22 Sensor** | Suhu ruangan akurat ±0.5°C |
| 🌤️ **Open-Meteo API** | Data cuaca real-time (suhu, UV, forecast) |
| 📶 **WiFi Auto-Connect** | Setup mudah via WiFiManager |
| 🔋 **Deep Sleep** | Hemat daya dengan wake-up touch |
| ⏱️ **Non-Blocking** | Loop berbasis millis() untuk performa optimal |

---

## 📸 Demo

<p align="center">
  <img src="/assets/weather_station_demo.gif?height=400&width=700" alt="Demo" width="700"/>
</p>

### Slide yang Tersedia

| Slide | Konten |
|-------|--------|
| 1 | Animasi mata mochi + status WiFi |
| 2 | Waktu & tanggal (format Indonesia) |
| 3 | Cuaca Tangerang (suhu, UV, forecast) |
| 4 | Suhu ruangan + termometer analog |

---

## 🧩 Komponen

| Komponen | Pin | Fungsi |
|----------|-----|--------|
| ESP32-C3 DevKit | - | Otak sistem |
| SSD1306 OLED | SDA=8, SCL=9 | Tampilan |
| DHT22 | GPIO 2 | Suhu ruangan |
| TTP223 Touch | GPIO 4 | Wake-up sensor |

> 📖 **Wiring Detail:** Lihat [Panduan Pengkabelan](docs/wiring_guide.md)

---

## ⚡ Mulai Cepat

### 1. Clone Repository
```bash
git clone https://github.com/ficrammanifur/esp32-mini-weather-station.git
cd esp32-mini-weather-station
```

### 2. Install Dependencies (Arduino IDE)
- **Board:** ESP32 (versi 3.0+)
- **Libraries:**
  - Adafruit SSD1306
  - Adafruit GFX
  - DHT sensor library
  - WiFiManager
  - ArduinoJson (v6.x)

### 3. Upload Firmware
```bash
# Buka firmware/esp32/sketch.ino
# Pilih board: ESP32C3 Dev Module
# Upload ke ESP32-C3
```

### 4. Setup WiFi
1. ESP32-C3 akan buat hotspot `MiniWeather-Setup`
2. Connect dan masukkan SSID/password WiFi
3. Selesai! Stasiun cuaca siap digunakan.

---

## 📚 Dokumentasi

| Dokumen | Deskripsi |
|---------|-----------|
| [📘 Panduan Pengkabelan](docs/wiring_guide.md) | Wiring diagram & assembly |
| [📗 Panduan FreeRTOS](docs/freertos_guide.md) | Multi-tasking lanjutan |
| [📕 Referensi API](docs/api_reference.md) | Fungsi & variabel penting |
| [📙 Troubleshooting](docs/troubleshooting.md) | Solusi masalah umum |

---

## 🤝 Kontribusi

Kontribusi sangat diterima! Beberapa ide pengembangan:

- [ ] Tambah kelembaban DHT22 ke slide
- [ ] Tambah sensor tekanan (BMP280)
- [ ] MQTT publish ke Home Assistant
- [ ] OTA update via WiFi
- [ ] Multi-kota support

**Cara berkontribusi:**
1. Fork repository
2. Buat branch fitur (`git checkout -b feature/NewFeature`)
3. Commit perubahan (`git commit -m 'Add NewFeature'`)
4. Push ke branch (`git push origin feature/NewFeature`)
5. Open Pull Request

---

## 📄 Lisensi

Distributed under the MIT License. See [LICENSE](LICENSE) for more information.

---

<div align="center">
  <strong>⭐ Star repo ini jika bermanfaat!</strong><br>
  <a href="#esp32-mini-weather-station">⬆ Back to Top</a>
</div>
```

---

## 📄 docs/wiring_guide.md (Sudah Ada, Perbaiki Format)

```markdown
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

<div align="center">

**Compact IoT Weather Monitoring with Arduino Loop & Deep Sleep**  
**Powered by ESP32-C3, Arduino, and Open Source**  
**Star this repo if you find it helpful!**
*Terakhir Diperbarui: 06 November 2025*

[⬆ Back on Top](#esp32-mini-weather-station-esp32-c3)

</div>
