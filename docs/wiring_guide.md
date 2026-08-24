# Panduan Pengkabelan ESP32 Mini Weather Station

## 📋 Gambaran Umum

Panduan ini memberikan instruksi pengkabelan langkah demi langkah untuk merakit Stasiun Cuaca Mini ESP32 menggunakan **ESP32-C3 SuperMini**.

### Alat yang Diperlukan
- **ESP32-C3 SuperMini** (atau varian ESP32-C3 lainnya)
- **SSD1306 OLED** 128x64 (I2C)
- **DHT22** Sensor
- **TTP223** Touch Sensor
- Kabel jumper (male-to-female)
- Breadboard
- Resistor: 4.7kΩ (2x), 10kΩ (1x)
- Multimeter (opsional)

### Catatan Keselamatan
- Gunakan **3.3V** untuk semua komponen
- Jangan melebihi **5V** pada pin GPIO
- Ground diri Anda untuk mencegah ESD

---

## 🔌 ESP32-C3 SuperMini Pinout

Berikut adalah pinout untuk **ESP32-C3 SuperMini** yang Anda gunakan:

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │              ESP32-C3 SuperMini             │    │
│  │                                             │    │
│  │  ┌────────────┐      ┌──────────────────┐   │    │
│  │  │   5V       │      │   GPIO05         │   │    │
│  │  │   GND      │      │   GPIO06         │   │    │
│  │  │   3.3V     │      │   GPIO07         │   │    │
│  │  │   GPIO08   │      │   BOOT1          │   │    │
│  │  │   GPIO09   │      │   GPIO10         │   │    │
│  │  │   GPIO20   │      │   GPIO21         │   │    │
│  │  │   GPIO22   │      │                  │   │    │
│  │  └────────────┘      └──────────────────┘   │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Tabel Pin Penting untuk Proyek Ini

| Pin Board | Fungsi | Koneksi | Catatan |
|-----------|--------|---------|---------|
| **3.3V** | Power | VCC semua komponen | Jangan pakai 5V! |
| **GND** | Ground | GND semua komponen | — |
| **GPIO08** | I2C SDA | OLED SDA | Pull-up 4.7kΩ ke 3.3V |
| **GPIO09** | I2C SCL | OLED SCL | Pull-up 4.7kΩ ke 3.3V |
| **GPIO05** | Data | DHT22 Data | Pull-up 10kΩ ke 3.3V |
| **GPIO07** | Touch | TTP223 OUT | Active LOW |
| **GPIO20** | UART RX | Serial monitor | Optional untuk debugging |
| **GPIO21** | UART TX | Serial monitor | Optional untuk debugging |

> ⚠️ **Boot Mode:** GPIO08 dan GPIO09 adalah strapping pins. Pastikan dalam keadaan HIGH saat boot (default sudah pull-up internal).

---

## 🔗 Wiring Diagram

### Diagram Ringkas

```
┌─────────────────────────────────────────────────────────────────┐
│                        ESP32-C3 SuperMini                       │
│                                                                 │
│   ┌──────────────┐          ┌──────────────┐                    │
│   │   3.3V       │──────────│  Power Rail  │                    │
│   │   GND        │──────────│  GND Rail    │                    │
│   └──────────────┘          └──────────────┘                    │
│                                                                 │
│   ┌──────────────┐          ┌──────────────┐                    │
│   │   GPIO08     │──────────│  OLED SDA    │                    │
│   │   GPIO09     │──────────│  OLED SCL    │                    │
│   │   GPIO05     │──────────│  DHT22 Data  │                    │
│   │   GPIO07     │──────────│  TTP223 OUT  │                    │
│   └──────────────┘          └──────────────┘                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Koneksi per Komponen

#### 1. OLED SSD1306 (I2C)
```
ESP32-C3                    OLED
─────────                   ────
  GPIO08  ────────────────── SDA
  GPIO09  ────────────────── SCL
  3.3V    ────────────────── VCC
  GND     ────────────────── GND

 ╔════╗    ╔════╗
 ║4.7k║    ║4.7k║  ← Pull-up Resistor (SDA & SCL ke 3.3V)
 ╚════╝    ╚════╝
   │         │
   └────┬────┘
        │
       3.3V
```

#### 2. DHT22 Sensor
```
ESP32-C3                    DHT22
─────────                   ─────
  GPIO05  ────────────────── Data
  3.3V    ────────────────── VCC
  GND     ────────────────── GND

  ╔════╗
  ║10k ║  ← Pull-up Resistor (Data → 3.3V)
  ╚════╝
     │
     └──── 3.3V
```

#### 3. TTP223 Touch Sensor
```
ESP32-C3                    TTP223
─────────                   ──────
  GPIO07  ────────────────── OUT
  3.3V    ────────────────── VCC
  GND     ────────────────── GND
```

---

## 📝 Konfigurasi di `sketch.ino`

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
Upload dan jalankan:
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
✅ Expected: Layar menampilkan "Hello OLED!"

### 2. Tes DHT22
✅ Expected: Serial monitor menampilkan suhu & kelembaban

### 3. Tes Touch
✅ Expected: Serial monitor berubah saat TTP223 disentuh

---

## 🐞 Troubleshooting Cepat

| Masalah | Solusi |
|---------|--------|
| OLED kosong | Cek SDA/SCL, pull-up 4.7kΩ, alamat I2C 0x3C |
| DHT NaN | Cek pull-up 10kΩ, delay >2s antar baca |
| Touch tidak responsif | Cek VCC/GND, TTP223 active LOW |
| ESP32 tidak boot | Cek GPIO08 & GPIO09 pull-up, power supply |

---

## 📚 Referensi

- [ESP32-C3 Datasheet](https://documentation.espressif.com/esp32-c3_datasheet_en.html)
- [ESP32-C3 SuperMini Schematic](https://www.wemos.cc/en/latest/c3/c3_mini.html)
- [Adafruit SSD1306 Guide](https://learn.adafruit.com/monochrome-oled-breakouts)

---

*Terakhir Diperbarui: 06 November 2025*
