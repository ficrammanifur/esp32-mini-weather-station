# Referensi API - ESP32 Mini Weather Station

## 📋 Daftar Fungsi

### Setup & Inisialisasi

#### `setup()`
Fungsi utama inisialisasi sistem.
```cpp
void setup() {
  // Inisialisasi Serial, WiFi, OLED, DHT, NTP
  // Fetch data pertama
}
```

### Display Functions

#### `drawEyeScreen()`
Menampilkan slide animasi mata mochi.
```cpp
void drawEyeScreen() {
  display.clearDisplay();
  display.drawRoundRect(0, 0, 128, 64, 4, SSD1306_WHITE);
  eyeAnim.update();
  eyeAnim.draw(display);
  // Status WiFi
  display.display();
}
```

#### `drawTimeScreen()`
Menampilkan waktu dan tanggal.
```cpp
void drawTimeScreen() {
  // Format: HH:MM (besar)
  // Hari, Tanggal Bulan
}
```

#### `drawWeatherScreen()`
Menampilkan data cuaca dari Open-Meteo.
```cpp
void drawWeatherScreen() {
  // Suhu, kondisi, UV, forecast besok
}
```

#### `drawRoomTempScreen()`
Menampilkan suhu ruangan dari DHT22.
```cpp
void drawRoomTempScreen() {
  // Termometer analog + suhu
}
```

---

### Sensor Functions

#### `fetchData()`
Mengambil data cuaca dari Open-Meteo API.
```cpp
void fetchData() {
  HTTPClient http;
  http.begin(weatherURL);
  int httpResponseCode = http.GET();
  if (httpResponseCode == 200) {
    // Parse JSON
    // Update global variables
  }
  http.end();
}
```

**URL Format:**
```
https://api.open-meteo.com/v1/forecast?
  latitude=-6.1783&
  longitude=106.6319&
  hourly=temperature_2m,weathercode,uv_index&
  daily=weathercode,temperature_2m_max,temperature_2m_min&
  timezone=Asia%2FJakarta
```

#### `getWeatherDesc(int code)`
Konversi weathercode ke deskripsi.
```cpp
String getWeatherDesc(int code) {
  switch (code) {
    case 0: return "Cerah";
    case 1:
    case 2: return "Berawan";
    case 3: return "Mendung";
    case 61:
    case 63: return "Hujan";
    case 95: return "Petir";
    default: return "N/A";
  }
}
```

**Weather Codes:**
| Code | Deskripsi |
|------|-----------|
| 0 | Cerah |
| 1-2 | Berawan |
| 3 | Mendung |
| 61-63 | Hujan |
| 95 | Petir |

---

### Sleep Functions

#### `goToSleep()`
Masuk ke deep sleep mode.
```cpp
void goToSleep() {
  display.clearDisplay();
  display.display();
  gpio_wakeup_enable((gpio_num_t)TOUCH_PIN, GPIO_INTR_LOW_LEVEL);
  esp_sleep_enable_gpio_wakeup();
  esp_deep_sleep_start();
}
```

#### Wake-up Logic
Wake-up terjadi ketika TTP223 mendeteksi sentuhan (LOW level).
- **Single tap:** Kembali sleep
- **Double tap (dalam 2 detik):** Bangun & aktif

---

### Variabel Global

| Variabel | Tipe | Deskripsi |
|----------|------|-----------|
| `cuacaSekarang` | String | Kondisi cuaca saat ini |
| `cuacaBesok` | String | Forecast besok |
| `suhu` | String | Suhu luar (Celsius) |
| `uv` | String | UV Index |
| `highTemp` | float | Suhu maksimum besok |
| `lowTemp` | float | Suhu minimum besok |
| `roomTemp` | String | Suhu ruangan dari DHT22 |
| `currentSlide` | int | Slide aktif (0-3) |

---

### Timing Constants

| Konstanta | Nilai | Deskripsi |
|-----------|-------|-----------|
| `weatherInterval` | 900000 ms | Fetch cuaca (15 menit) |
| `dhtInterval` | 2000 ms | Baca DHT (2 detik) |
| `displayInterval` | 50 ms | Update display (20 FPS) |
| `slideInterval` | 10000 ms | Ganti slide (10 detik) |
| `INACTIVITY_TIMEOUT` | 600000 ms | Sleep (10 menit) |

---

### EyeAnimation Class

#### `EyeAnimation.begin()`
Inisialisasi animasi mata.
```cpp
void begin() {
  randomSeed(analogRead(0));
  nextBlinkTime = millis() + random(2000, 4000);
}
```

#### `EyeAnimation.update()`
Update state animasi.
```cpp
void update() {
  // Blink logic
  // Offset movement
}
```

#### `EyeAnimation.draw(display)`
Gambar mata di OLED.
```cpp
void draw(Adafruit_SSD1306 &display) {
  if (blinking) {
    // Draw closed eyes (horizontal lines)
  } else {
    // Draw open eyes (filled circles)
  }
}
```

**Eye Parameters:**
- Radius: 12 pixels
- Center Y: 22 pixels
- X Offset: [-4, -2, 0, 2, 4, 2, 0, -2]

---

## 📚 Referensi

- [Open-Meteo API Docs](https://open-meteo.com/en/docs)
- [Adafruit GFX Library](https://github.com/adafruit/Adafruit-GFX-Library)
- [ESP32 Sleep Modes](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/sleep_modes.html)

---

*Terakhir Diperbarui: 06 November 2025*
