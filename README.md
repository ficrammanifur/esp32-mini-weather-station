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

**Compact IoT Weather Monitoring with Arduino Loop & Deep Sleep**  
**Powered by ESP32-C3, Arduino, and Open Source**  
**Star this repo if you find it helpful!**
*Terakhir Diperbarui: 06 November 2025*

[⬆ Back on Top](#esp32-mini-weather-station-esp32-c3)

</div>
