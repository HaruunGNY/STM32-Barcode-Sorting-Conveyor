# STM32 Tabanlı Barkod Okumalı Konveyör Ayrıştırma Sistemi 

Bu proje, lojistik merkezlerinde kullanılan otomatik paket ayrıştırma sistemlerinin STM32 mikrodenetleyicisi ile geliştirilmiş bir prototipidir. Sistem, konveyör üzerindeki ürünleri barkodlarına göre tanımlayarak farklı çıkış kanallarına yönlendirir.

##  Proje Tanıtım Videosu

https://github.com/user-attachments/assets/f7ec03a1-3e39-46a2-bd15-47a9da8ccc61

---

##  Donanım Özellikleri
* **Motor Sürücü:** **IRFZ44N N-Channel MOSFET** tabanlı özel anahtarlama devresi.
*    **Neden IRFZ44N?:** Yüksek akım kapasitesi ve düşük iç direnç sayesinde konveyör bandını süren DC motorun ısınmadan, yüksek verimle kontrol edilmesi sağlanmıştır.
* **MCU:** STM32F103C8T6 (ARM Cortex-M3)
* **Ekran:** 16x2 LCD (I2C Modülü ile)
* **Sensör:** IR Kızılötesi Sensör (Paket algılama)
* **Aktüatörler:** * 2x MG90S Servo Motor (Ayrıştırma kolları için PWM kontrolü)
    * DC Motor (Konveyör bant tahriki)
* **Giriş:** UART Barkod Okuyucu Modülü



##  Çalışma Algoritması
1. **Hazırlık:** Sistem açıldığında LCD üzerinde "Konveyor Aktif" mesajı belirir ve DC motor çalışmaya başlar.
2. **Algılama:** IR sensör bir paket tespit ettiğinde, işlem için konveyör motoru durdurulur.
3. **Okuma:** UART hattı üzerinden barkod verisi beklenir (`HAL_UART_Receive`).
4. **Ayrıştırma:**
   - **A Şehri:** Barkod "8680188130137" ise Servo-2 tetiklenir.
   - **B Şehri:** Barkod "8699015750417" ise Servo-1 tetiklenir.
   - **Tanımsız:** Barkod eşleşmezse LCD'de hata uyarısı verilir.
6. **Döngü


:** Yönlendirme bittikten 4 saniye sonra servolar başlangıç konumuna döner ve bant tekrar çalışır.

##  Yazılım Detayları
Proje **STM32CubeIDE** kullanılarak, **HAL kütüphaneleri** ile geliştirilmiştir.

* **PWM (Timer 2):** Servo motorların 50Hz frekansta hassas kontrolü için kullanılmıştır.
* **UART (USART2):** Barkod okuyucu ile 9600 baudrate hızında haberleşme sağlanmıştır.
* **I2C (I2C1):** 1602 LCD ekranın kablo kalabalığını azaltmak için I2C protokolü tercih edilmiştir.

##  Proje Yapısı
* `main.c`: Ana kontrol döngüsü ve çevre birimi başlatmaları.
* `i2c-lcd.h / .c`: LCD kontrol kütüphanesi.
* `IOC File`: Pin konfigürasyonları ve saat ayarları.

---
