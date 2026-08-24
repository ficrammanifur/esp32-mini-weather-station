# Panduan Pengkabelan ESP32 Mini Weather Station

## 📋 Gambaran Umum

Panduan ini memberikan instruksi pengkabelan langkah demi langkah untuk merakit Stasiun Cuaca Mini ESP32 menggunakan **ESP32-C3 SuperMini**.

---

## 🛠️ Alat yang Diperlukan

| Komponen | Jumlah | Keterangan |
|----------|--------|------------|
| ESP32-C3 SuperMini | 1 | Board utama |
| SSD1306 OLED 128x64 | 1 | Tampilan I2C |
| DHT22 Sensor | 1 | Suhu ruangan |
| TTP223 Touch Sensor | 1 | Wake-up dari sleep |
| Kabel jumper M-F | 8-10 | Koneksi antar komponen |
| Breadboard | 1 | Tempat merangkai |
| Resistor 4.7kΩ | 2 | Pull-up I2C |
| Resistor 10kΩ | 1 | Pull-up DHT22 |
| Multimeter | 1 | Opsional, untuk cek koneksi |

### ⚠️ Catatan Keselamatan
- Gunakan **3.3V** untuk semua komponen
- Jangan melebihi **5V** pada pin GPIO
- Ground diri Anda untuk mencegah ESD

---

## 🔌 Pinout ESP32-C3 SuperMini

<p align="center">
  <img src="/assets/ESP32-C3 pinout.jpeg" alt="ESP32-C3 Pinout" width="400"/>
</p>

### Layout Board
```
┌───────────────────────────────────────────────────────┐
│                                                       │
│  ┌─────────────────────────────────────────────────┐  │
│  │                ESP32-C3 SuperMini               │  │
│  │                                                 │  │
│  │  ┌──────────────┐          ┌──────────────────┐ │  │
│  │  │    5V        │          │   GPIO05         │ │  │
│  │  │    GND       │          │   GPIO06         │ │  │
│  │  │    3.3V      │          │   GPIO07         │ │  │
│  │  │   GPIO08     │          │   BOOT1          │ │  │
│  │  │   GPIO09     │          │   GPIO10         │ │  │
│  │  │   GPIO20     │          │   GPIO21         │ │  │
│  │  │   GPIO22     │          │                  │ │  │
│  │  └──────────────┘          └──────────────────┘ │  │
│  └─────────────────────────────────────────────────┘  │
│                                                       │
└───────────────────────────────────────────────────────┘
```

### PCB Size
<p align="center">
  <img src="/assets/PCB size.jpeg" alt="PCB Size" width="400"/>
</p>

### Schematic
<p align="center">
  <img src="/assets/ESP32-C3-SuperMini Schematic.jpeg" alt="ESP32-C3 SuperMini Schematic" width="500"/>
</p>

---

## 📊 Tabel Koneksi Pin

| Pin Board | Fungsi | Koneksi | Resistor | Catatan |
|-----------|--------|---------|----------|---------|
| **3.3V** | Power | VCC semua komponen | — | JANGAN pakai 5V! |
| **GND** | Ground | GND semua komponen | — | Ground bersama |
| **GPIO08** | I2C SDA | OLED SDA | 4.7kΩ ke 3.3V | Strapping pin |
| **GPIO09** | I2C SCL | OLED SCL | 4.7kΩ ke 3.3V | Strapping pin |
| **GPIO05** | Data | DHT22 Data | 10kΩ ke 3.3V | Pull-up penting |
| **GPIO07** | Touch | TTP223 OUT | — | Active LOW |
| **GPIO20** | UART RX | Serial monitor | — | Opsional debugging |
| **GPIO21** | UART TX | Serial monitor | — | Opsional debugging |

> ⚠️ **Boot Mode:** GPIO08 dan GPIO09 adalah **strapping pins**. Pastikan dalam keadaan HIGH saat boot (default sudah pull-up internal).

---

## 🔗 Wiring Diagram

<p align="center">
  <img src="/assets/Schematic-Weather-Station.png" alt="Schematic Diagram" width="600"/>
</p>

### Diagram Ringkas
```
┌────────────────────────────────────────────────────────────────┐
│                        ESP32-C3 SuperMini                      │
│                                                                │
│   ┌──────────────┐          ┌──────────────────────────────┐   │
│   │   3.3V       │──────────│  Power Rail (VCC)            │   │
│   │   GND        │──────────│  GND Rail                    │   │
│   └──────────────┘          └──────────────────────────────┘   │
│                                                                │
│   ┌──────────────┐          ┌──────────────────────────────┐   │
│   │   GPIO08     │──────────│  OLED SDA                    │   │
│   │   GPIO09     │──────────│  OLED SCL                    │   │
│   │   GPIO05     │──────────│  DHT22 Data                  │   │
│   │   GPIO07     │──────────│  TTP223 OUT                  │   │
│   └──────────────┘          └──────────────────────────────┘   │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## 🔧 Koneksi per Komponen

### 1. OLED SSD1306 (I2C)

```
┌─────────────┐          ┌─────────────┐
│  ESP32-C3   │          │    OLED     │
├─────────────┤          ├─────────────┤
│   GPIO08    │──────────│    SDA      │
│   GPIO09    │──────────│    SCL      │
│   3.3V      │──────────│    VCC      │
│   GND       │──────────│    GND      │
└─────────────┘          └─────────────┘

     ╔════╗    ╔════╗
     ║4.7k║    ║4.7k║  ← Pull-up ke 3.3V
     ╚════╝    ╚════╝
      │         │
      └────┬────┘
           │
          3.3V
```

### 2. DHT22 Sensor

```
┌─────────────┐          ┌─────────────┐
│  ESP32-C3   │          │   DHT22     │
├─────────────┤          ├─────────────┤
│   GPIO05    │──────────│   Data      │
│   3.3V      │──────────│   VCC       │
│   GND       │──────────│   GND       │
└─────────────┘          └─────────────┘

     ╔════╗
     ║10k ║  ← Pull-up ke 3.3V
     ╚════╝
      │
      └──── 3.3V
```

### 3. TTP223 Touch Sensor

```
┌─────────────┐          ┌─────────────┐
│  ESP32-C3   │          │   TTP223    │
├─────────────┤          ├─────────────┤
│   GPIO07    │──────────│   OUT       │
│   3.3V      │──────────│   VCC       │
│   GND       │──────────│   GND       │
└─────────────┘          └─────────────┘
```

---

## 💻 Konfigurasi di `sketch.ino`

Update pin configuration di firmware:

```cpp
// ==== OLED CONFIG ====
#define OLED_SDA 8   // GPIO08
#define OLED_SCL 9   // GPIO09

// ==== DHT CONFIG ====
#define DHTPIN 5     // GPIO05
#define DHTTYPE DHT22

// ==== TOUCH CONFIG ====
#define TOUCH_PIN 7  // GPIO07 (untuk TTP223)
```

---

## ✅ Verifikasi

### 1. Tes OLED
**Upload `test/oled_test.ino`**

```cpp
#include <Wire.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_SDA 8
#define OLED_SCL 9

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

void setup() {
  Wire.begin(OLED_SDA, OLED_SCL);
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED gagal!");
    for(;;);
  }
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(10, 20);
  display.println("Hello OLED!");
  display.display();
}

void loop() {}
```

✅ **Expected:** Layar menampilkan "Hello OLED!"

### 2. Tes DHT22
✅ **Expected:** Serial monitor menampilkan suhu & kelembaban

### 3. Tes Touch
✅ **Expected:** Serial monitor berubah saat TTP223 disentuh

---

## 🐞 Troubleshooting Cepat

| Masalah | Kemungkinan Penyebab | Solusi |
|---------|---------------------|--------|
| **OLED kosong** | Kabel longgar / pull-up hilang | Cek SDA/SCL, tambah 4.7kΩ pull-up |
| **DHT NaN** | Pull-up hilang / timing salah | Tambah 10kΩ pull-up, delay >2s |
| **Touch tidak responsif** | VCC/GND terbalik | Cek wiring, TTP223 active LOW |
| **ESP32 tidak boot** | Strapping pin salah | Cek GPIO08 & GPIO09 pull-up |
| **WiFi tidak connect** | Antena / channel | Gunakan 2.4GHz only, cek antenna |

---

## 📚 Referensi

- [ESP32-C3 Datasheet](https://documentation.espressif.com/esp32-c3_datasheet_en.html)
- [ESP32-C3 SuperMini Schematic](https://www.wemos.cc/en/latest/c3/c3_mini.html)
- [Adafruit SSD1306 Guide](https://learn.adafruit.com/monochrome-oled-breakouts)
- [DHT22 with ESP32](https://randomnerdtutorials.com/esp32-dht11-dht22-temperature-humidity-sensor-arduino-ide/)

---


## 🔗 Navigasi

- [⬅ Kembali ke Home](../README.md)

*Terakhir Diperbarui: 06 November 2025*
