# 🛑 Arduino #19: MPU6050 ile Deprem Alarmı Sistemi

Bu projede **MPU6050 6 Eksen İvme ve Gyro Sensörü** kullanılarak temel bir **deprem alarmı sistemi** oluşturulmuştur. Gerçek bir deprem algılama sistemi kadar hassas olmasa da, eğitim ve deneysel amaçlarla temel bir ivme hareketine dayalı uyarı sistemi sunar.

🔗 [Web Siteme Bakmak İçin Tıkla](https://www.hakkiharmankaya.com/)  


> ⚠️ **Uyarı:** Bu sistem profesyonel bir deprem algılama cihazı değildir. Güvenlik amaçlı kullanılmadan önce detaylı testlerden geçirilmesi ve yerel yönetmeliklere uygun hale getirilmesi gerekir.

---

## 📦 Gerekli Malzemeler

- 1 adet **Arduino Uno / Nano**
- 1 adet **MPU6050 sensörü**
- 1 adet **Aktif buzzer**
- 1 adet **LED**
- 2 adet **330Ω direnç**
- **Breadboard**
- **Jumper kabloları**

---

## 🧭 Devre Bağlantıları

### 🔌 MPU6050 Bağlantısı

| MPU6050 Pin | Arduino Pin     |
|-------------|-----------------|
| VCC         | 5V              |
| GND         | GND             |
| SCL         | SCL (A5 - Uno)  |
| SDA         | SDA (A4 - Uno)  |

### 💡 LED & 🔊 Buzzer

| Bileşen       | Arduino Pin | Notlar              |
|---------------|-------------|---------------------|
| LED (+ bacak) | D11         | 330Ω direnç ile     |
| Buzzer (+)    | D10         | 330Ω direnç ile     |
| Ortak GND     | GND         | Breadboard üzerinden|

---

## 🧠 MPU6050 Kütüphanesi

Arduino IDE > Araçlar > Kütüphane Yöneticisi > `MPU6050 by Electronic Cats` veya `Jeff Rowberg` gibi bir kütüphaneyi yükleyin.

---

## 🧾 Arduino Kodları

```cpp
#include <MPU6050.h>
#include <Wire.h>

MPU6050 MPU;
int GyroX, GyroY, GyroZ;
int buzzer = 10;

void setup() {
  pinMode(11, OUTPUT); 
  Serial.begin(9600);
  Wire.begin();
  MPU.initialize();
}

void loop() {
  MPU.getRotation(&GyroX, &GyroY, &GyroZ);

  if (GyroX < -2000 || GyroX > 1000 || 
      GyroY > 1000 || GyroY < -1000 || 
      GyroZ > 1000 || GyroZ < -1000 ) {

    tone(buzzer, 1000);          // Alarm sesi
    digitalWrite(11, HIGH);      // LED yanar
    delay(1000);                 // Bekleme süresi

  } else {
    noTone(buzzer);              // Alarm kapat
    digitalWrite(11, LOW);      // LED kapat
  }
}
