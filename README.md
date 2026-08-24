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

### <p align="center">🖼️ Preview Slide</p>

<p align="center">
  <img src="/assets/slide-1.png?height=100&width=128" width="128" alt="Slide 1"/>&nbsp;&nbsp;
  <img src="/assets/slide-2.png?height=100&width=128" width="128" alt="Slide 2"/>&nbsp;&nbsp;
  <img src="/assets/slide-3.png?height=100&width=128" width="128" alt="Slide 3"/>&nbsp;&nbsp;
  <img src="/assets/slide-4.png?height=100&width=128" width="128" alt="Slide 4"/><br/>
  <em>Screenshot masing-masing slide</em>
</p>

---

<p align="center">
  <img src="/assets/Schematic-Weather-Station.png?height=400&width=700" alt="ESP32 Weather Station Wiring Diagram" width="700"/><br/>
  <em> Wiring Diagram ESP32-C3 Mini Weather Station</em><br/>
  ⚙️ <strong>Notes:</strong><br/>
  🔹 OLED terhubung via I2C: SDA (GPIO 8) & SCL (GPIO 9).  
  🔹 DHT22 terhubung ke GPIO 2 (data pin).  
  🔹 Common ground (GND) untuk semua komponen.  
  🔹 Power ESP32-C3 via USB atau 3.3V pin untuk testing.  
</p>

---

## 🧩 Komponen Utama dan Fungsinya
| Komponen | Fungsi | Keterangan |
|----------|--------|-----------|
| **ESP32-C3 DevKit** | Otak utama sistem | Menangani loop non-blocking, WiFi, fetch API, update OLED, baca DHT22 |
| **SSD1306 OLED 128x64** | Tampilan utama | Menampilkan slide animasi, teks, ikon cuaca; I2C (SDA=8, SCL=9) |
| **DHT22 Sensor** | Suhu & kelembaban ruangan | Terhubung ke GPIO 2, dibaca via millis() setiap 2 detik |
| **WiFi Antenna** | Koneksi internet | Fetch data cuaca dari MSN Weather API via HTTPClient |
| **Resistor Pull-up** | Stabilisasi I2C | Untuk OLED & DHT22 (4.7kΩ opsional) |
| **Power Supply 3.3V** | Sumber daya | Dari ESP32-C3 atau external 5V step-down; konsumsi ~50mA active |

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

[⬆ Back on Top](#esp32-mini-weather-station-esp32-c3)

</div>
*Terakhir Diperbarui: 06 November 2025*
