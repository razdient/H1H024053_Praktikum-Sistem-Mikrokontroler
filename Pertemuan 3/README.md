KOMUNIKASI SERIAL
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%203/Foto%20Percobaan%203A.jpeg?raw=true)
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%203/skema%20percobaan%203A.png?raw=true)
1. Proses dimulai ketika pengguna mengetikkan karakter seperti ‘1’ atau ‘0’ pada Serial Monitor di komputer. Data tersebut kemudian dikirim ke Arduino melalui komunikasi serial (UART). Arduino akan mengecek apakah ada data yang masuk menggunakan fungsi Serial.available(). Jika terdapat data, maka Arduino membaca karakter tersebut menggunakan Serial.read(). Selanjutnya, program akan melakukan pengecekan terhadap karakter yang diterima. Jika karakter yang diterima adalah ‘1’, maka Arduino akan memberikan logika HIGH pada pin LED sehingga LED menyala. Jika karakter yang diterima adalah ‘0’, maka Arduino memberikan logika LOW sehingga LED mati. Setelah itu, Arduino juga mengirimkan pesan kembali ke Serial Monitor sebagai bentuk umpan balik, seperti “LED ON” atau “LED OFF”. 

2.  Fungsi Serial.available() digunakan untuk memastikan bahwa terdapat data yang masuk ke buffer serial sebelum dilakukan proses pembacaan. Hal ini penting agar Arduino tidak membaca data kosong atau data yang belum tersedia. Jika fungsi ini dihilangkan, maka Serial.read() tetap akan dijalankan meskipun tidak ada data yang masuk, sehingga berpotensi membaca nilai yang tidak valid atau menyebabkan perilaku program menjadi tidak stabil. Oleh karena itu, penggunaan Serial.available() bertujuan untuk menjaga keandalan proses pembacaan data. p berjalan dengan stabil dan tidak mengalami error.

3. Modifikasi program dilakukan dengan menambahkan kondisi baru ketika Arduino menerima input ‘2’. Pada kondisi ini, sebuah variabel digunakan untuk menandai bahwa mode kedip (blink) aktif. Selama mode ini aktif, LED akan terus menyala dan mati secara bergantian dengan jeda waktu tertentu menggunakan fungsi delay(). Mode kedip ini akan terus berjalan hingga Arduino menerima perintah lain seperti ‘1’ atau ‘0’, yang sekaligus akan menonaktifkan mode kedip tersebut. Dengan demikian, program tidak hanya mengontrol LED secara statis, tetapi juga secara dinamis berdasarkan input pengguna. .
#include <Arduino.h>

const int PIN_LED = 12;     // Menentukan pin LED pada Arduino
bool modeBlink = false;     // Variabel untuk menyimpan status mode blink

void setup() {
  Serial.begin(9600);       // Memulai komunikasi serial dengan baud rate 9600
  pinMode(PIN_LED, OUTPUT); // Mengatur pin LED sebagai output
}

void loop() {

  if (Serial.available() > 0) {   // Mengecek apakah ada data dari Serial Monitor
    char data = Serial.read();    // Membaca 1 karakter dari input

    if (data == '1') {            // Jika input adalah '1'
      digitalWrite(PIN_LED, HIGH); // Menyalakan LED
      modeBlink = false;           // Mematikan mode blink
      Serial.println("LED ON");    // Menampilkan pesan ke Serial Monitor
    }

    else if (data == '0') {       // Jika input adalah '0'
      digitalWrite(PIN_LED, LOW); // Mematikan LED
      modeBlink = false;          // Mematikan mode blink
      Serial.println("LED OFF");  // Menampilkan pesan ke Serial Monitor
    }

    else if (data == '2') {       // Jika input adalah '2'
      modeBlink = true;           // Mengaktifkan mode blink
      Serial.println("BLINK MODE"); // Menampilkan pesan
    }

    else if (data != '\n' && data != '\r') { 
      // Jika input bukan '1', '0', '2', atau ENTER
      Serial.println("Perintah tidak dikenal"); // Menampilkan error
    }
  }

  if (modeBlink) {                // Jika mode blink aktif
    digitalWrite(PIN_LED, HIGH); // LED menyala
    delay(500);                  // Delay 0.5 detik
    digitalWrite(PIN_LED, LOW);  // LED mati
    delay(500);                  // Delay 0.5 detik
  }
}


4. Fungsi fs delay() digunakan untuk memberikan jeda  waktu dengan cara menghentikan sementara seluruh proses dalam program Arduino. Selama delay berlangsung, Arduino tidak dapat menjalankan perintah lain, termasuk membaca input baru dari Serial Monitor. Hal ini menyebabkan sistem menjadi kurang responsif. Sebaliknya, fungsi millis() memungkinkan pengaturan waktu tanpa menghentikan jalannya program. Dengan menggunakan millis(), Arduino tetap dapat menjalankan tugas lain secara bersamaan, seperti membaca input baru sambil menjalankan proses kedip LED. Oleh karena itu, penggunaan millis() lebih direkomendasikan untuk sistem yang membutuhkan respons cepat dan multitasking.

INTER-INTEGRATED CIRCUIT
![alt_text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%203/Foto%20Percobaan%203B.jpeg?raw=true)
![alt_text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%203/skema%20percobaan%203B.png?raw=true)

1.Komunikasi I2C antara Arduino dan LCD menggunakan dua jalur utama, yaitu SDA (Serial Data) dan SCL (Serial Clock). Arduino berperan sebagai master yang mengendalikan komunikasi, sedangkan LCD bertindak sebagai slave dengan alamat tertentu, misalnya 0x27. Arduino mengirimkan data berupa perintah atau karakter melalui jalur SDA yang disinkronkan dengan sinyal clock pada jalur SCL. Data tersebut kemudian diterima oleh modul LCD dan ditampilkan sesuai instruksi yang diberikan, seperti menampilkan teks atau memindahkan posisi kursor. Keunggulan komunikasi I2C adalah penggunaan pin yang lebih efisien karena hanya membutuhkan dua jalur komunikasi.

2. Pada rangkaian, potensiometer dihubungkan dengan kaki kiri ke GND, kaki kanan ke tegangan 5V, dan kaki tengah ke pin analog A0 sebagai input. Konfigurasi ini memungkinkan potensiometer bekerja sebagai pembagi tegangan, sehingga nilai tegangan yang masuk ke pin A0 dapat bervariasi sesuai posisi putaran. Jika posisi kaki kiri dan kanan ditukar, rangkaian tetap dapat berfungsi dengan normal, namun arah perubahan nilai akan menjadi terbalik. Artinya, jika sebelumnya nilai meningkat saat diputar ke kanan, maka setelah ditukar nilai justru akan meningkat saat diputar ke kiri. 

3. #include <Wire.h>                  // Library komunikasi I2C
#include <LiquidCrystal_I2C.h>     // Library untuk LCD I2C
#include <Arduino.h>               // Library dasar Arduino

LiquidCrystal_I2C lcd(0x27, 16, 2); // Inisialisasi LCD alamat 0x27 ukuran 16x2

const int pinPot = A0;             // Menentukan pin potensiometer di A0

void setup() {
  Serial.begin(9600);              // Memulai komunikasi serial (UART)
  lcd.init();                      // Inisialisasi LCD
  lcd.backlight();                 // Menyalakan lampu LCD
}

void loop() {
  int nilai = analogRead(pinPot);  // Membaca nilai ADC dari potensiometer (0-1023)

  float volt = nilai * (5.0 / 1023.0); // Mengubah nilai ADC menjadi tegangan (Volt)
  int persen = map(nilai, 0, 1023, 0, 100); // Mengubah nilai ADC menjadi persen (0-100%)

  // ===== OUTPUT KE SERIAL MONITOR (UART) =====
  Serial.print("ADC: ");           // Menampilkan teks "ADC:"
  Serial.print(nilai);             // Menampilkan nilai ADC
  Serial.print(" Volt: ");         // Menampilkan teks "Volt:"
  Serial.print(volt);              // Menampilkan nilai tegangan
  Serial.print(" V Persen: ");     // Menampilkan teks "Persen:"
  Serial.print(persen);            // Menampilkan nilai persen
  Serial.println("%");             // Menutup baris dengan "%"

  // ===== OUTPUT KE LCD (I2C) =====
  lcd.setCursor(0, 0);             // Mengatur kursor di kolom 0 baris 0
  lcd.print("ADC: ");              // Menampilkan teks "ADC:"
  lcd.print(nilai);                // Menampilkan nilai ADC
  lcd.print(" ");                  // Spasi untuk merapikan tampilan
  lcd.print(persen);               // Menampilkan persen
  lcd.print("%");                  // Menampilkan simbol %

  // ===== BAR (LEVEL) =====
  int panjangBar = map(nilai, 0, 1023, 0, 16); // Mengubah ADC ke panjang bar (0-16)

  lcd.setCursor(0, 1);             // Pindah ke baris kedua LCD
  for (int i = 0; i < 16; i++) {   // Perulangan untuk membuat bar sepanjang 16 karakter
    if (i < panjangBar) {          // Jika indeks masih kurang dari panjang bar
      lcd.print((char)255);        // Tampilkan blok penuh (bar)
    } else {                       // Jika melebihi panjang bar
      lcd.print(" ");              // Tampilkan spasi (kosong)
    }
  }

  delay(200);                      // Delay 200 ms agar tampilan stabil
}

4. Nilai tegangan diperoleh dari konversi nilai ADC menggunakan rumus perbandingan terhadap tegangan referensi 5V, sedangkan persentase diperoleh dari skala 0 hingga 100%. Berdasarkan perhitungan, untuk nilai ADC kecil seperti 1, tegangan mendekati 0 volt dan persentase sekitar 0%. Untuk nilai ADC yang lebih besar seperti 21, 49, 74, dan 96, tegangan dan persentase meningkat secara bertahap, namun tetap relatif kecil karena nilai ADC masih jauh dari maksimum 1023. Hal ini menunjukkan bahwa perubahan kecil pada putaran potensiometer akan menghasilkan perubahan nilai yang proporsional pada pembacaan ADC.

PERTANYAAN PRAKTIKUM
1.  Komunikasi UART (Universal Asynchronous Receiver Transmitter) memiliki keunggulan utama berupa implementasi yang sederhana karena hanya menggunakan dua jalur komunikasi, yaitu TX (transmit) dan RX (receive). Selain itu, UART tidak memerlukan sinyal clock karena bersifat asynchronous, sehingga lebih fleksibel dalam penggunaannya. UART sangat cocok digunakan untuk komunikasi langsung antara dua perangkat, seperti antara Arduino dan komputer melalui Serial Monitor. Namun, kelemahan UART adalah tidak mendukung banyak perangkat dalam satu jalur komunikasi (point-to-point), serta kecepatan komunikasi harus disesuaikan (baud rate) antara kedua perangkat agar tidak terjadi kesalahan data. Di sisi lain, I2C (Inter-Integrated Circuit) memiliki keunggulan dalam efisiensi penggunaan pin karena hanya membutuhkan dua jalur, yaitu SDA dan SCL, serta mampu menghubungkan banyak perangkat sekaligus dalam satu bus dengan menggunakan alamat unik. Hal ini sangat berguna dalam sistem seperti penggunaan LCD I2C dan sensor secara bersamaan. Akan tetapi, I2C memiliki kelemahan seperti kecepatan yang relatif lebih rendah dibandingkan protokol lain serta kompleksitas dalam pengaturan alamat dan komunikasi multi-device. Berdasarkan hasil percobaan, penggunaan UART lebih sederhana untuk input data dari pengguna, sedangkan I2C lebih efisien untuk output ke perangkat seperti LCD, sehingga kombinasi keduanya menjadi solusi yang optimal. Hal ini sejalan dengan konsep komunikasi mikrokontroler yang dijelaskan pada referensi [2] dan [5]. 
2.Pada komunikasi I2C, setiap perangkat yang terhubung dalam satu jalur harus memiliki alamat unik agar dapat dikenali oleh master, dalam hal ini Arduino. Alamat seperti 0x27 atau 0x20 merupakan identitas dari modul LCD I2C yang digunakan. Ketika Arduino ingin mengirimkan data ke LCD, Arduino akan mengirimkan alamat tersebut terlebih dahulu untuk memastikan bahwa data dikirim ke perangkat yang benar. Jika alamat yang digunakan dalam program tidak sesuai dengan alamat perangkat sebenarnya, maka LCD tidak akan merespons atau menampilkan data. Dalam praktikum, perbedaan alamat ini sering terjadi karena perbedaan jenis modul atau konfigurasi hardware. Oleh karena itu, penting untuk melakukan scanning alamat I2C sebelum pemrograman. Hasil percobaan menunjukkan bahwa ketika alamat yang digunakan sesuai, LCD dapat menampilkan data dengan baik, sedangkan jika tidak sesuai maka LCD tidak menampilkan apa pun. Hal ini sesuai dengan prinsip addressing pada komunikasi I2C yang dijelaskan dalam literatur [2] & [4]
3. Ketika UART dan I2C digunakan secara bersamaan dalam satu sistem, Arduino berperan sebagai pengendali utama yang mengelola kedua protokol komunikasi tersebut secara bergantian dalam loop program. Alur kerja sistem dimulai dari input yang diberikan melalui Serial Monitor (UART), di mana data dikirim dari komputer ke Arduino. Arduino kemudian membaca data tersebut menggunakan fungsi serial, memprosesnya sesuai dengan logika program, dan menghasilkan output. Output tersebut tidak hanya ditampilkan kembali ke Serial Monitor, tetapi juga dikirim ke LCD menggunakan komunikasi I2C. Arduino dapat mengelola kedua protokol ini secara bersamaan karena masing-masing menggunakan hardware dan library yang berbeda, sehingga tidak saling mengganggu selama penggunaannya diatur dengan benar dalam program. Berdasarkan hasil percobaan, sistem mampu membaca input dari UART sekaligus menampilkan hasil secara real-time pada LCD melalui I2C. Hal ini menunjukkan bahwa mikrokontroler dapat melakukan multitasking sederhana dengan mengatur alur eksekusi program dalam fungsi loop. Konsep ini sesuai dengan teori komunikasi data pada mikrokontroler yang menyatakan bahwa berbagai protokol dapat digunakan secara paralel selama tidak terjadi konflik sumber daya, sebagaimana dijelaskan pada referensi [1] dan [5]. 


