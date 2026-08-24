# 📙 Troubleshooting - ESP32 Mini Weather Station

## 📋 Daftar Masalah

- [OLED Tidak Menyala](#oled-tidak-menyala)
- [DHT22 Tidak Terbaca](#dht22-tidak-terbaca)
- [WiFi Gagal Connect](#wifi-gagal-connect)
- [Cuaca Tidak Update](#cuaca-tidak-update)
- [Loop Hang atau Delay](#loop-hang-atau-delay)
- [Animasi Mata Stuck](#animasi-mata-stuck)
- [Slide Tidak Berganti](#slide-tidak-berganti)
- [Deep Sleep Tidak Bekerja](#deep-sleep-tidak-bekerja)
- [ESP32 Tidak Boot](#esp32-tidak-boot)

---

## OLED Tidak Menyala

### Gejala
- Layar hitam, tidak ada tampilan
- Tidak ada response setelah upload

### Solusi

#### 1. Cek Wiring
```
OLED pins:
├─ VCC → 3.3V
├─ GND → GND
├─ SDA → GPIO 8
└─ SCL → GPIO 9
```

#### 2. Cek I2C Address
Upload sketch scan I2C:
```cpp
#include <Wire.h>
void setup() {
  Wire.begin(8, 9);
  Serial.begin(115200);
  for (byte address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    if (Wire.endTransmission() == 0) {
      Serial.print("Found: 0x");
      Serial.println(address, HEX);
    }
  }
}
```
Expected: `Found: 0x3C`

#### 3. Cek Pull-up Resistor
- Tambahkan resistor 4.7kΩ antara SDA/SCL dan 3.3V

#### 4. Cek Kode
```cpp
if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
  Serial.println(F("OLED gagal diinisialisasi"));
  for(;;); // Halt jika gagal
}
```

---

## DHT22 Tidak Terbaca

### Gejala
- `roomTemp` tetap "22" atau "NaN"
- Serial monitor: `nan` atau `-999`

### Solusi

#### 1. Cek Wiring
```
DHT22 pins:
├─ VCC → 3.3V
├─ GND → GND
└─ Data → GPIO 5 (SuperMini)
```

#### 2. Pull-up Resistor
- Tambahkan resistor 10kΩ antara Data dan VCC

#### 3. Cek Interval
```cpp
const unsigned long dhtInterval = 2000UL; // Minimal 2 detik
```

#### 4. Cek Kode
```cpp
float t = dht.readTemperature();
if (!isnan(t)) {
  roomTemp = String((int)round(t));
  Serial.println("DHT OK: " + roomTemp);
} else {
  Serial.println("DHT ERROR: NaN");
}
```

---

## WiFi Gagal Connect

### Gejala
- Serial: "Gagal connect WiFi, reboot..."
- Hotspot "MiniWeather-Setup" tidak muncul

### Solusi

#### 1. Reset WiFiManager
```cpp
WiFiManager wm;
wm.resetSettings(); // Hapus konfigurasi lama
```

#### 2. Cek SSID/Password
- Pastikan SSID dan password benar
- Hindari karakter khusus

#### 3. Cek Router
- Gunakan 2.4GHz only (bukan 5GHz)
- Channel: 1-11

#### 4. Manual Connect
```cpp
WiFi.begin("SSID", "password");
int attempts = 0;
while (WiFi.status() != WL_CONNECTED && attempts < 20) {
  delay(500);
  attempts++;
}
```

---

## Cuaca Tidak Update

### Gejala
- Data tetap "Loading..." atau fallback
- Serial: "JSON parse failed"

### Solusi

#### 1. Cek Internet
```cpp
if (WiFi.status() == WL_CONNECTED) {
  Serial.println("WiFi OK");
}
```

#### 2. Cek API URL
```cpp
Serial.println(weatherURL);
// Test di browser:
// https://api.open-meteo.com/v1/forecast?latitude=-6.1783&longitude=106.6319&hourly=temperature_2m,weathercode,uv_index&daily=weathercode,temperature_2m_max,temperature_2m_min&timezone=Asia%2FJakarta
```

#### 3. Cek JSON Parsing
```cpp
DynamicJsonDocument doc(4096);
DeserializationError error = deserializeJson(doc, payload);
if (error) {
  Serial.print("JSON parse failed: ");
  Serial.println(error.c_str());
  return;
}
```

---

## Loop Hang atau Delay

### Gejala
- Slides lambat atau stuck
- Display tidak responsif

### Solusi

#### 1. Cek millis() Overflow
```cpp
unsigned long now = millis();
// unsigned long overflow setelah ~49 hari
// Gunakan perbandingan dengan hati-hati:
if (now - lastUpdate >= interval) { ... }
```

#### 2. Kurangi delay()
```cpp
void loop() {
  // Hanya 1 delay di akhir
  delay(10);
}
```

#### 3. Monitor Memory
```cpp
Serial.printf("Free heap: %d\n", ESP.getFreeHeap());
```

---

## Animasi Mata Stuck

### Gejala
- Mata tidak blink
- Mata tidak bergerak

### Solusi

#### 1. Cek Random Seed
```cpp
void begin() {
  randomSeed(analogRead(0)); // ADC read
}
```

#### 2. Debugging
```cpp
Serial.print("Blinking: ");
Serial.println(blinking);
Serial.print("Offset: ");
Serial.println(currentOffsetIndex);
```

---

## Slide Tidak Berganti

### Gejala
- Stuck di satu slide
- Tidak ada rotasi

### Solusi

#### 1. Cek Interval
```cpp
const unsigned long slideInterval = 10000UL; // 10 detik
```

#### 2. Cek Logic
```cpp
if (now - lastSlideChange >= slideInterval) {
  lastSlideChange = now;
  currentSlide = (currentSlide + 1) % 4;
}
```

---

## Deep Sleep Tidak Bekerja

### Gejala
- ESP tidak masuk sleep
- Touch tidak wake-up

### Solusi

#### 1. Cek Pin (SuperMini)
```cpp
#define TOUCH_PIN 7  // GPIO 7 untuk TTP223
```

#### 2. Cek Wiring TTP223
```
TTP223 pins:
├─ VCC → 3.3V
├─ GND → GND
└─ OUT → GPIO 7
```

#### 3. Cek Wake-up Level
```cpp
gpio_wakeup_enable((gpio_num_t)TOUCH_PIN, GPIO_INTR_LOW_LEVEL);
// TTP223 active LOW
```

#### 4. Cek Inactivity Timeout
```cpp
const unsigned long INACTIVITY_TIMEOUT = 600000UL; // 10 menit
```

---

## ESP32 Tidak Boot

### Gejala
- Tidak ada output serial
- LED tidak menyala

### Solusi

#### 1. Cek Power
- USB cable: Coba kabel lain
- Port USB: Coba port lain
- Power supply: 5V 1A minimum

#### 2. Cek Boot Mode
```
Hold BOOT button → Press RESET → Release BOOT
Untuk masuk flash mode
```

#### 3. Cek Short Circuit
- Pastikan tidak ada short antara 3.3V dan GND

---

## 📚 Referensi

- [ESP32-C3 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf)
- [Arduino ESP32 Issues](https://github.com/espressif/arduino-esp32/issues)

---

## 🔗 Navigasi

- [⬅ Kembali ke Home](../README.md)

---

*Terakhir Diperbarui: 06 November 2025*
