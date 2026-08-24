# 📗 Panduan FreeRTOS untuk ESP32 Mini Weather Station

## 📋 Gambaran Umum

Framework Arduino di ESP32 berjalan di atas FreeRTOS. Panduan ini menjelaskan cara memanfaatkan FreeRTOS untuk operasi konkuren seperti baca sensor paralel.

**Catatan:** `sketch.ino` yang disediakan **tidak** menggunakan FreeRTOS secara eksplisit, melainkan menggunakan `millis()` non-blocking. Panduan ini untuk kustomisasi lanjutan.

---

## 🎯 Mengapa FreeRTOS?

| Keuntungan | Deskripsi |
|------------|-----------|
| **Multi-Tasking** | Jalankan task secara paralel |
| **Prioritas** | Task penting (display) dapat diprioritaskan |
| **Komunikasi** | Queue & semaphore untuk sharing data |
| **Stabilitas** | Preemptive multitasking yang mature |

---

## 🛠️ Konsep Dasar

### Task
```cpp
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>

void myTask(void *pvParameters) {
  while (1) {
    // Lakukan sesuatu
    vTaskDelay(pdMS_TO_TICKS(1000)); // Delay 1 detik
  }
}

// Di setup()
xTaskCreate(myTask, "MyTask", 2048, NULL, 1, NULL);
```

### Prioritas
| Prioritas | Deskripsi |
|-----------|-----------|
| **0** | Terendah (idle) |
| **1-10** | Normal |
| **11-24** | Tinggi (hati-hati) |

### Stack Size
- Minimum: 2048 words (~8KB)
- Monitoring: `uxTaskGetStackHighWaterMark(NULL)`

---

## 📨 Komunikasi Antar Task

### Queue (Antrian)
```cpp
QueueHandle_t tempQueue;

// Kirim data
float temp = 26.5;
xQueueSend(tempQueue, &temp, portMAX_DELAY);

// Terima data
float received;
if (xQueueReceive(tempQueue, &received, 0) == pdTRUE) {
  // Proses data
}
```

### Mutex (Sinkronisasi)
```cpp
SemaphoreHandle_t oledMutex;

xSemaphoreTake(oledMutex, portMAX_DELAY);
// Akses OLED di sini
display.clearDisplay();
display.display();
xSemaphoreGive(oledMutex);
```

---

## 📝 Contoh Implementasi

### Task DHT Terpisah
```cpp
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>
#include <freertos/queue.h>

QueueHandle_t tempQueue;

void dhtTask(void *pvParameters) {
  while (1) {
    float t = dht.readTemperature();
    if (!isnan(t)) {
      xQueueSend(tempQueue, &t, portMAX_DELAY);
    }
    vTaskDelay(pdMS_TO_TICKS(2000));
  }
}

// Di setup()
tempQueue = xQueueCreate(5, sizeof(float));
xTaskCreate(dhtTask, "DHT_Task", 2048, NULL, 2, NULL);
```

### Task Fetch Cuaca
```cpp
void weatherTask(void *pvParameters) {
  while (1) {
    fetchData();
    vTaskDelay(pdMS_TO_TICKS(900000)); // 15 menit
  }
}

xTaskCreate(weatherTask, "Weather_Task", 4096, NULL, 1, NULL);
```

---

## ⚡ Monitoring

### Cek Stack Usage
```cpp
UBaseType_t highWaterMark = uxTaskGetStackHighWaterMark(NULL);
Serial.printf("Stack free: %d words\n", highWaterMark);
```

### Daftar Semua Task
```cpp
vTaskList(); // Print ke Serial
```

---

## ⚠️ Kesalahan Umum

| Masalah | Gejala | Solusi |
|---------|--------|--------|
| **Stack Overflow** | Crash/reset | Naikkan stack size |
| **Priority Inversion** | Task lambat | Gunakan mutex dengan priority inheritance |
| **Deadlock** | Hang | Hindari nested locking |
| **Queue Full** | Data hilang | Naikkan queue size |

---

## 📚 Referensi

- [ESP-IDF FreeRTOS Docs](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/freertos.html)
- [FreeRTOS.org](https://www.freertos.org/)
- [Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-freertos-tasks-arduino-ide/)

---

## 🔗 Navigasi

- [⬅ Kembali ke Home](../README.md)

---

*Terakhir Diperbarui: 06 November 2025*
