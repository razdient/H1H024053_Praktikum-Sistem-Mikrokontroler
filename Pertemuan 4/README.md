## Percobaan 1 : Analog to Digital Converter
### Foto
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%204/Foto%20Percobaan%201.jpeg?raw=true)
### Skema
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%204/Skema%20Percobaan%201.png?raw=true)
1. Perintah analogRead() berfungsi untuk membaca sinyal analog berupa tegangan dari potensiometer yang kemudian dikonversi menjadi data digital oleh ADC (Analog to Digital Converter) pada Arduino Uno. Nilai yang dihasilkan berada pada rentang 0 hingga 1023, yang menunjukkan representasi digital dari tegangan 0V hingga 5V. Dalam praktikum ini, nilai tersebut digunakan sebagai input utama untuk mengontrol sudut servo. Perubahan posisi potensiometer akan mengubah nilai tegangan yang dibaca, sehingga menghasilkan nilai ADC yang berbeda. Hal ini sesuai dengan konsep dasar ADC, di mana sinyal analog harus dikonversi menjadi bentuk digital agar dapat diproses oleh mikrokontroler
2. Fungsi map() diperlukan untuk mengubah rentang nilai input menjadi rentang nilai output yang sesuai dengan kebutuhan sistem. Pada praktikum ini, nilai dari analogRead() berada pada rentang 0–1023, sedangkan servo hanya dapat menerima perintah sudut dalam rentang 0–180 derajat. Tanpa proses konversi menggunakan map(), nilai yang dikirim ke servo tidak akan sesuai, sehingga dapat menyebabkan pergerakan yang tidak akurat atau bahkan tidak berfungsi dengan baik. Dengan menggunakan map(), nilai ADC dapat diskalakan secara proporsional menjadi sudut servo, sehingga pergerakan servo menjadi linear dan sesuai dengan putaran potensiometer. Hal ini sejalan dengan konsep pengolahan sinyal dalam sistem mikrokontroler, di mana data input sering kali perlu disesuaikan dengan karakteristik perangkat output agar sistem dapat bekerja secara optimal
3. Untuk membatasi pergerakan servo, dilakukan modifikasi pada fungsi map() dengan mengubah rentang output dari 0–180 menjadi 30–150 derajat. 
```cpp
pos = map(val, 0, 1023, 30, 150);
```
Modifikasi ini menyebabkan nilai ADC dari potensiometer tetap berada pada rentang 0–1023, namun hasil konversinya hanya menghasilkan sudut antara 30° hingga 150°. Dengan demikian, servo tidak akan bergerak ke posisi ekstrem (0° dan 180°). Pembatasan ini penting karena pada beberapa kondisi, pergerakan ekstrem dapat menyebabkan servo menjadi tidak stabil atau berpotensi merusak komponen mekaniknya. Selain itu, pembatasan sudut juga dapat meningkatkan presisi kontrol pada rentang tertentu yang lebih dibutuhkan. Konsep ini sesuai dengan prinsip pengendalian aktuator, di mana batas operasi sering diterapkan untuk menjaga kestabilan dan keandalan sistem

## Percobaan 2 : Pulse Width Modulation
### Foto
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%204/Foto%20Percobaan%202.jpeg?raw=true)
### Skema
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%204/Skema%20Percobaan%202.png?raw=true)
1. LED dapat diatur kecerahannya menggunakan fungsi analogWrite() karena fungsi ini menghasilkan sinyal PWM (Pulse Width Modulation), yaitu sinyal digital yang dimodulasi lebar pulsa (duty cycle)-nya sehingga menyerupai sinyal analog. Pada PWM, meskipun tegangan yang diberikan tetap 0V atau 5V, lama waktu sinyal berada pada kondisi HIGH dalam satu periode dapat diatur. Semakin besar duty cycle, semakin lama LED dalam kondisi menyala, sehingga LED tampak lebih terang. Sebaliknya, jika duty cycle kecil, LED tampak redup. Hal ini sesuai dengan konsep PWM yang digunakan untuk mengontrol daya rata-rata yang diberikan ke beban seperti LED tanpa perlu menggunakan tegangan analog sebenarnya
2. Nilai ADC (0–1023) merupakan hasil pembacaan sinyal analog dari potensiometer dengan resolusi 10-bit, sedangkan nilai PWM (0–255) merupakan nilai output dengan resolusi 8-bit yang digunakan oleh fungsi analogWrite(). Hubungan antara keduanya adalah proses penskalaan (scaling), di mana nilai ADC dikonversi menjadi nilai PWM menggunakan fungsi map(). Secara matematis, nilai ADC dibagi sekitar 4 untuk mendapatkan nilai PWM (karena 1023 ≈ 4 × 255).
| Nilai ADC | PWM   | Kondisi LED |
| :-: | :-: | :-: |
| Kecil | Kecil | Redup |
| Besar | Besar | Terang |
Proses ini memastikan bahwa perubahan pada potensiometer menghasilkan perubahan kecerahan LED secara proporsional, sesuai dengan konsep konversi data dalam sistem mikrokontroler.
4.
```cpp
if (pwm >= 50 && pwm <= 200) {
  analogWrite(ledPin, pwm);
} else {
  analogWrite(ledPin, 0);
}
```

Kode ini diletakkan pada bagian output PWM, tepatnya menggantikan baris analogWrite(ledPin, pwm); yang sebelumnya langsung mengirim nilai PWM ke LED.

Penambahan kondisi ini bertujuan untuk melakukan seleksi terhadap nilai PWM. Jika nilai PWM berada dalam rentang 50 hingga 200, maka LED akan menyala dengan tingkat kecerahan sesuai nilai tersebut. Namun, jika nilai PWM berada di bawah 50 atau di atas 200, maka LED akan dimatikan dengan memberikan nilai 0.

Secara konsep, penggunaan kondisi if ini merupakan bentuk pengendalian logika dalam mikrokontroler untuk membatasi output berdasarkan nilai tertentu.
