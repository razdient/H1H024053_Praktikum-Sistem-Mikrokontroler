2A SEVEN SEGMENT
1.  ![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%202/Rangkaian%20Percobaan%201.jpg?raw=true)
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%202/Skema%20Percobaan%201.png?raw=true)
Rangkaian pada percobaan 1 terdiri dari Arduino Uno yang dihubungkan dengan seven segment melalui breadboard. Setiap pin segmen pada seven segment (a, b, c, d, e, f, g, dan dp) dihubungkan ke pin digital Arduino yaitu pin 7, 6, 5, 11, 10, 8, 9, dan 4 melalui resistor sebagai pembatas arus. Kaki common pada seven segment dihubungkan ke tegangan VCC karena menggunakan tipe common anode. Dengan konfigurasi ini, Arduino dapat mengontrol setiap segmen LED secara individual untuk menampilkan angka dan huruf sesuai dengan program yang dijalankan.

2.  Jika nilai num lebih dari 15, maka program akan mencoba mengakses data di luar batas array digitPattern karena array tersebut hanya memiliki indeks dari 0 hingga 15. Hal ini dapat
menyebabkan tampilan pada seven segment menjadi tidak sesuai atau acak karena data yang diambil tidak valid. Namun pada program yang digunakan dalam percobaan, kondisi ini telah diantisipasi dengan memberikan pembatasan nilai sehingga ketika nilai melebihi 15, counter akan kembali ke 0. Dengan demikian sistem tetap berjalan dengan stabil dan tidak mengalami error.

3. Program yang digunakan pada percobaan ini menerapkan jenis seven segment common anode. Hal ini dapat dilihat dari penggunaan operator logika NOT pada fungsi digitalWrite yang membalik nilai dari array digitPattern. Pada seven segment common anode, LED akan menyala ketika diberi logika LOW dan mati ketika diberi logika HIGH sehingga pembalikan logika ini diperlukan agar pola angka dan huruf dapat ditampilkan dengan benar sesuai dengan karakter yang diinginkan.

4. #include <Arduino.h> 
// Memanggil library Arduino untuk fungsi dasar seperti pinMode dan digitalWrite

const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4}; 
// Menyimpan pin Arduino yang terhubung ke seven segment (a sampai dp)

byte digitPattern[16][8] = {
  {1,1,1,1,1,1,0,0}, // 0
  {0,1,1,0,0,0,0,0}, // 1
  {1,1,0,1,1,0,1,0}, // 2
  {1,1,1,1,0,0,1,0}, // 3
  {0,1,1,0,0,1,1,0}, // 4
  {1,0,1,1,0,1,1,0}, // 5
  {1,0,1,1,1,1,1,0}, // 6
  {1,1,1,0,0,0,0,0}, // 7
  {1,1,1,1,1,1,1,0}, // 8
  {1,1,1,1,0,1,1,0}, // 9
  {1,1,1,0,1,1,1,0}, // A
  {0,0,1,1,1,1,1,0}, // b
  {1,0,0,1,1,1,0,0}, // C
  {0,1,1,1,1,0,1,0}, // d
  {1,0,0,1,1,1,1,0}, // E
  {1,0,0,0,1,1,1,0}  // F
};
// Array pola untuk menyalakan segmen sesuai angka/heksa

void displayDigit(int num)
{
  for(int i = 0; i < 8; i++)
  {
    digitalWrite(segmentPins[i], !digitPattern[num][i]); 
    // Mengirim sinyal ke tiap segmen (dibalik karena common anode)
  }
}
// Fungsi untuk menampilkan angka berdasarkan indeks array

void setup()
{
  for(int i = 0; i < 8; i++)
  {
    pinMode(segmentPins[i], OUTPUT); 
    // Mengatur semua pin segment sebagai output
  }
}
// Setup awal pin Arduino

void loop()
{
  for(int i = 15; i >= 0; i--) 
  // Perulangan dari F (15) ke 0
  {
    displayDigit(i); 
    // Menampilkan angka sesuai indeks

    delay(1000); 
    // Jeda 1 detik setiap perubahan angka
  }
}
// Loop utama program

2B KONTROL COUNTER PUSH BUTTON
1.![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%202/Rangkaian%20Percobaan%202.png?raw=true)
Rangkaian pada percobaan kedua merupakan pengembangan dari percobaan pertama dengan penambahan push button sebagai input. Seven segment tetap dihubungkan ke pin digital Arduino sesuai konfigurasi, yaitu segmen a sampai dp ke pin 7, 6, 5, 11, 10, 8, 9, dan 4 melalui resistor. Push button dihubungkan ke salah satu pin digital Arduino (misalnya pin 3) dengan konfigurasi menggunakan internal pull-up resistor, sehingga tidak memerlukan resistor eksternal. Dengan rangkaian ini, Arduino dapat menerima input dari tombol sekaligus mengontrol tampilan pada seven segment.
2. Penggunaan mode INPUT_PULLUP pada push button bertujuan untuk memanfaatkan resistor internal Arduino sehingga pin input berada pada kondisi HIGH secara default dan akan berubah menjadi LOW saat tombol ditekan. Keuntungan dari metode ini adalah rangkaian menjadi lebih sederhana karena tidak memerlukan resistor eksternal, serta mengurangi risiko kondisi floating yang dapat menyebabkan pembacaan sinyal tidak stabil.

3. Jika salah satu LED segmen tidak menyala, kemungkinan penyebabnya dapat berasal dari sisi hardware maupun software. Dari sisi hardware, hal ini bisa disebabkan oleh kabel yang tidak terhubung dengan baik, resistor yang rusak, kesalahan pemasangan pin, atau LED pada seven segment yang memang mengalami kerusakan. Dari sisi software, kemungkinan terjadi kesalahan pada penulisan array digitPattern, kesalahan mapping pin, atau logika program yang tidak sesuai sehingga segmen tertentu tidak mendapatkan sinyal yang benar.

4. #include <Arduino.h>
// Library dasar Arduino untuk fungsi input/output

const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};
// Pin Arduino yang terhubung ke segmen a sampai dp

const int buttonUp = 2;
// Push button untuk increment (tambah nilai)

const int buttonDown = 3;
// Push button untuk decrement (kurangi nilai)

int counter = 0;
// Variabel penyimpan nilai counter (0–15)

bool lastUpState = HIGH;
bool lastDownState = HIGH;
// Menyimpan status tombol sebelumnya untuk deteksi perubahan (edge detection)

byte digitPattern[16][8] = {
  {1,1,1,1,1,1,0,0}, //0
  {0,1,1,0,0,0,0,0}, //1
  {1,1,0,1,1,0,1,0}, //2
  {1,1,1,1,0,0,1,0}, //3
  {0,1,1,0,0,1,1,0}, //4
  {1,0,1,1,0,1,1,0}, //5
  {1,0,1,1,1,1,1,0}, //6
  {1,1,1,0,0,0,0,0}, //7
  {1,1,1,1,1,1,1,0}, //8
  {1,1,1,1,0,1,1,0}, //9
  {1,1,1,0,1,1,1,0}, //A
  {0,0,1,1,1,1,1,0}, //b
  {1,0,0,1,1,1,0,0}, //C
  {0,1,1,1,1,0,1,0}, //d
  {1,0,0,1,1,1,1,0}, //E
  {1,0,0,0,1,1,1,0}  //F
};
// Pola tampilan seven segment untuk heksadesimal 0–F

void displayDigit(int num)
{
  for(int i = 0; i < 8; i++)
  {
    digitalWrite(segmentPins[i], !digitPattern[num][i]);
    // Mengirim sinyal ke masing-masing segmen (dibalik karena common anode)
  }
}
// Fungsi untuk menampilkan angka berdasarkan indeks

void setup()
{
  for(int i = 0; i < 8; i++)
  {
    pinMode(segmentPins[i], OUTPUT);
    // Set semua pin seven segment sebagai output
  }

  pinMode(buttonUp, INPUT_PULLUP);
  // Push button increment aktif LOW dengan pull-up internal

  pinMode(buttonDown, INPUT_PULLUP);
  // Push button decrement aktif LOW dengan pull-up internal

  displayDigit(counter);
  // Menampilkan nilai awal counter
}
// Inisialisasi sistem

void loop()
{
  bool upState = digitalRead(buttonUp);
  bool downState = digitalRead(buttonDown);
  // Membaca kondisi tombol

  if(lastUpState == HIGH && upState == LOW)
  {
    counter++;
    // Jika tombol up ditekan, counter bertambah

    if(counter > 15) counter = 0;
    // Jika lebih dari F, kembali ke 0

    displayDigit(counter);
    // Update tampilan
  }

  if(lastDownState == HIGH && downState == LOW)
  {
    counter--;
    // Jika tombol down ditekan, counter berkurang

    if(counter < 0) counter = 15;
    // Jika kurang dari 0, kembali ke F

    displayDigit(counter);
    // Update tampilan
  }

  lastUpState = upState;
  lastDownState = downState;
  // Simpan status tombol untuk deteksi perubahan

  delay(150);
  // Delay untuk menghindari bouncing tombol
}
// Loop utama program
