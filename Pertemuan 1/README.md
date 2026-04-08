 Percabangan

1.  Program akan masuk ke blok if ketika kondisi yang ditentukan bernilai benar (true), yaitu saat nilai timeDelay <= 100. Pada kondisi ini, program akan melakukan reset nilai delay ke nilai awal setelah jeda tertentu.

2. Program akan masuk ke blok else ketika kondisi pada if tidak terpenuhi (false), yaitu saat timeDelay > 100. Pada kondisi ini, nilai delay akan dikurangi secara bertahap sehingga LED berkedip semakin cepat.

3. Perintah delay(timeDelay) berfungsi untuk memberikan jeda waktu dalam milidetik pada saat LED menyala maupun mati. Nilai delay ini menentukan kecepatan kedip LED, dimana semakin kecil nilainya maka LED akan berkedip semakin cepat. Hal ini sesuai dengan konsep pengaturan waktu pada mikrokontroler [2].

4. 
const int ledPin = 6; 
// Menentukan pin LED pada pin digital 6

int timeDelay = 100; 
// Nilai awal delay kecil → LED berkedip cepat

void setup() {
  pinMode(ledPin, OUTPUT); 
  // Mengatur pin LED sebagai output
}

void loop() {
  digitalWrite(ledPin, HIGH); 
  // Menyalakan LED

  delay(timeDelay); 
  // Menunggu sesuai nilai delay

  digitalWrite(ledPin, LOW); 
  // Mematikan LED

  delay(timeDelay); 
  // Menunggu lagi (membentuk 1 siklus kedip)

  if (timeDelay < 500) { 
    // Jika masih dalam kondisi cepat

    timeDelay += 100; 
    // Menambah delay → LED menjadi lebih lambat (menuju sedang)

  } else if (timeDelay < 1000) { 
    // Jika sudah masuk kondisi sedang

    timeDelay += 100; 
    // Menambah delay lagi → semakin lambat

  } else { 
    // Jika sudah mencapai batas maksimum

    while (true) { 
      // Perulangan tanpa henti (infinite loop)

      digitalWrite(ledPin, LOW); 
      // LED dipastikan tetap mati
    }
  }
}

Pengulangan
1. ![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%201/Skema%20Pengulangan.png?raw=true)

2. Program menggunakan perulangan for dengan nilai pin meningkat (increment). Setiap LED dinyalakan satu per satu dari pin rendah ke tinggi, sehingga terlihat seperti bergerak dari kiri ke kanan.

3. Program menggunakan perulangan for dengan nilai pin menurun (decrement). LED dinyalakan dari pin tinggi ke rendah, sehingga terlihat bergerak dari kanan ke kiri.

4. void setup() {
  for (int i = 2; i <= 7; i++) {
    // Perulangan dari pin 2 sampai 7

    pinMode(i, OUTPUT); 
    // Mengatur semua pin sebagai output
  }
}

void loop() {

  // MENYALAKAN LED KIRI
  for (int i = 2; i <= 4; i++) {
    // Perulangan untuk LED kiri (pin 2,3,4)

    digitalWrite(i, HIGH); 
    // Menyalakan LED kiri
  }

  for (int i = 5; i <= 7; i++) {
    // Perulangan untuk LED kanan

    digitalWrite(i, LOW); 
    // Mematikan LED kanan
  }

  delay(500); 
  // Memberi jeda sebelum pindah kondisi

  // MENYALAKAN LED KANAN
  for (int i = 5; i <= 7; i++) {
    // Perulangan untuk LED kanan (pin 5,6,7)

    digitalWrite(i, HIGH); 
    // Menyalakan LED kanan
  }

  for (int i = 2; i <= 4; i++) {
    // Perulangan untuk LED kiri

    digitalWrite(i, LOW); 
    // Mematikan LED kiri
  }

  delay(500); 
  // Jeda sebelum kembali ke awal loop
}
