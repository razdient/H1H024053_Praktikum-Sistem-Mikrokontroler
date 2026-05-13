## Percobaan 1 : Multitasking
### Foto
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%205/Foto%20Percobaan%201.jpeg?raw=true)
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%205/Foto%20Percobaan%201%20Potensio.jpeg?raw=true)
### Skema
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%205/Skema%20Percobaan%201.png?raw=true)
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%205/Skema%20Percobaan%201%20Potensio.png?raw=true)
1. Ketiga task pada program FreeRTOS berjalan secara concurrent atau terlihat berjalan secara bersamaan. Namun, pada Arduino Uno yang hanya memiliki satu prosesor, task sebenarnya dijalankan secara bergantian dengan sangat cepat oleh scheduler FreeRTOS. Scheduler mengatur pembagian waktu eksekusi untuk setiap task berdasarkan prioritas dan waktu delay yang diberikan.
Pada program, TaskBlink1, TaskBlink2, dan Taskprint memiliki prioritas yang sama sehingga scheduler menjalankan task secara bergantian menggunakan metode time slicing. Saat sebuah task menjalankan fungsi vTaskDelay(), task tersebut masuk ke kondisi blocked sementara sehingga scheduler memberikan kesempatan kepada task lain untuk berjalan. Akibatnya, LED dapat berkedip dengan interval berbeda sambil Serial Monitor tetap menampilkan output counter secara terus-menerus.
Hasil percobaan menunjukkan bahwa perubahan nilai delay mempengaruhi kecepatan eksekusi task dan kedipan LED. Semakin kecil delay maka task akan dieksekusi lebih sering, sedangkan delay yang lebih besar membuat task berjalan lebih lambat. Hal ini sesuai dengan konsep multitasking pada RTOS yang membagi waktu prosesor untuk menjalankan beberapa task secara teratur dan efisien.
2. Task keempat dapat ditambahkan dengan membuat fungsi task baru kemudian mendaftarkannya menggunakan fungsi xTaskCreate() pada bagian setup(). Langkah pertama adalah membuat deklarasi fungsi task baru, misalnya:
```cpp
void TaskBlink3(void *pvParameters);
```
Kemudian dibuat isi fungsi task yang berisi program untuk dijalankan, contohnya mengendalikan LED tambahan:
```cpp
void TaskBlink3(void *pvParameters)
{pinMode(6, OUTPUT);
while(1)
 {
   Serial.println("Task4");
  digitalWrite(6, HIGH);
   vTaskDelay(400 / portTICK_PERIOD_MS);
   digitalWrite(6, LOW);
   vTaskDelay(400 / portTICK_PERIOD_MS);
 }
}
```
Setelah itu, task didaftarkan pada fungsi setup() menggunakan xTaskCreate():
```cpp
xTaskCreate(
 TaskBlink3,
 "task4", 128, NULL, 1, NULL
);
```
Setelah program dijalankan, scheduler FreeRTOS akan mengatur task keempat bersama task lainnya sehingga seluruh task dapat berjalan secara concurrent. Penambahan task ini menunjukkan bahwa RTOS mampu mengelola banyak proses secara lebih terstruktur dibandingkan sistem tanpa RTOS [2].

3. Pada modifikasi program, potensiometer digunakan sebagai input analog untuk mengatur kecepatan kedipan LED. Nilai analog dari potensiometer dibaca menggunakan fungsi analogRead() kemudian digunakan sebagai nilai delay pada task LED. Semakin besar nilai potensiometer, semakin lambat kedipan LED, sedangkan semakin kecil nilainya maka LED berkedip lebih cepat.

Contoh potongan program:
```cpp
int potValue;

void TaskBlink1(void *pvParameters)
{
  pinMode(8, OUTPUT);

  while(1)
  {
    potValue = analogRead(A0);

    digitalWrite(8, HIGH);
    vTaskDelay(potValue / portTICK_PERIOD_MS);

    digitalWrite(8, LOW);
    vTaskDelay(potValue / portTICK_PERIOD_MS);

    Serial.println(potValue);
  }
}
```
Hasil percobaan menunjukkan bahwa perubahan posisi potensiometer mempengaruhi kecepatan LED secara langsung. Ketika potensiometer diputar ke nilai kecil, LED berkedip lebih cepat karena delay menjadi lebih singkat. Sebaliknya, ketika nilai potensiometer besar, LED berkedip lebih lambat karena nilai delay bertambah besar.

Perintah analogRead() digunakan untuk membaca tegangan analog dari potensiometer yang kemudian dikonversi menjadi data digital oleh ADC pada Arduino Uno dengan rentang nilai 0–1023. Nilai digital tersebut kemudian diproses oleh FreeRTOS untuk menentukan waktu delay task LED. Hasil percobaan menunjukkan bahwa input analog dapat digunakan untuk mengontrol perilaku task secara real-time, sesuai dengan konsep pengolahan sinyal analog pada sistem embedded.

## Percobaan 2 : Komunikasi Task
### Foto
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%205/Foto%20Percobaan%202.png?raw=true)
### Skema
![alt text](https://github.com/razdient/H1H024053_Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%205/Skema%20Percobaan%202.png?raw=true)
1. Kedua task pada program FreeRTOS berjalan secara concurrent atau terlihat seperti berjalan bersamaan. Namun, pada Arduino Uno yang hanya memiliki satu core prosesor, eksekusi task sebenarnya dilakukan secara bergantian dengan sangat cepat oleh scheduler FreeRTOS.

Task read_data berfungsi mengirimkan data ke queue, sedangkan task display menerima data dari queue untuk ditampilkan pada Serial Monitor. Scheduler akan mengatur eksekusi kedua task berdasarkan kondisi task, seperti apakah task sedang aktif atau dalam keadaan delay/blocked. Ketika vTaskDelay() atau proses pengiriman/penerimaan queue terjadi, task dapat masuk ke kondisi blocked sehingga task lain dapat dijalankan.

Dengan mekanisme ini, pengiriman dan penerimaan data tetap berjalan secara teratur tanpa saling mengganggu, sehingga sistem terlihat berjalan secara paralel meskipun sebenarnya bersifat time-sharing.

2. Program ini relatif kecil kemungkinannya mengalami race condition karena komunikasi antar task dilakukan menggunakan queue (xQueueSend dan xQueueReceive). Queue pada FreeRTOS sudah dirancang untuk menangani sinkronisasi data antar task sehingga akses data menjadi lebih aman.

Race condition biasanya terjadi ketika dua atau lebih task mengakses resource yang sama secara bersamaan tanpa mekanisme pengaman, misalnya variabel global tanpa proteksi. Pada program ini, data struct readings dikirim melalui queue, sehingga data tidak diakses langsung secara bersamaan oleh beberapa task.

Dengan demikian, queue bertindak sebagai mekanisme sinkronisasi yang mencegah terjadinya data corruption dan race condition dalam komunikasi antar task [2].

3. Pada modifikasi program, sensor DHT11 atau DHT22 digunakan untuk menggantikan data dummy pada task read_data. Sensor ini digunakan untuk membaca suhu dan kelembaban secara real-time, kemudian hasil pembacaan dikirim melalui queue ke task display.

Contoh perubahan pada task pembacaan data:
```cpp
#include <DHT.h>
#define DHTPIN 2
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

void read_data(void *pvParameters){
  struct readings x;

  dht.begin();

  for(;;){
    x.temp = dht.readTemperature();
    x.h = dht.readHumidity();

    xQueueSend(my_queue, &x, portMAX_DELAY);

    vTaskDelay(2000 / portTICK_PERIOD_MS);
  }
}
```
Pada task display, data yang diterima dari queue akan berubah sesuai kondisi lingkungan.
```cpp
void display(void *pvParameters){
  struct readings x;

  for(;;){
    if(xQueueReceive(my_queue, &x, portMAX_DELAY) == pdPASS){
      Serial.print("temp = ");
      Serial.println(x.temp);

      Serial.print("humidity = ");
      Serial.println(x.h);
    }
  }
}
```
Hasil percobaan menunjukkan bahwa nilai suhu dan kelembaban yang ditampilkan pada Serial Monitor berubah-ubah sesuai kondisi lingkungan sekitar sensor. Hal ini menunjukkan bahwa sistem sudah bersifat dinamis karena data tidak lagi statis, tetapi diperoleh langsung dari sensor secara real-time.

Penggunaan queue tetap menjaga komunikasi antar task tetap aman dan terstruktur, sehingga data dari sensor dapat diproses tanpa konflik antar task dalam sistem multitasking FreeRTOS [3].
