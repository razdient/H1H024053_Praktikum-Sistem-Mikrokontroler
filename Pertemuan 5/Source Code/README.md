# Percobaan 1
```cpp
#include <Arduino_FreeRTOS.h>

void TaskBlink1(void *pvParameters);
void TaskBlink2(void *pvParameters);
void TaskPrint(void *pvParameters);

void setup() {

  Serial.begin(9600);

  xTaskCreate(
    TaskBlink1,
    "Task1",
    128,
    NULL,
    1,
    NULL
  );

  xTaskCreate(
    TaskBlink2,
    "Task2",
    128,
    NULL,
    1,
    NULL
  );

  xTaskCreate(
    TaskPrint,
    "Task3",
    128,
    NULL,
    1,
    NULL
  );

  vTaskStartScheduler();
}

void loop() {
}

void TaskBlink1(void *pvParameters) {

  pinMode(8, OUTPUT);

  while (1) {

    Serial.println("Task1");

    digitalWrite(8, HIGH);
    vTaskDelay(200 / portTICK_PERIOD_MS);

    digitalWrite(8, LOW);
    vTaskDelay(200 / portTICK_PERIOD_MS);
  }
}

void TaskBlink2(void *pvParameters) {

  pinMode(7, OUTPUT);

  while (1) {

    Serial.println("Task2");

    digitalWrite(7, HIGH);
    vTaskDelay(300 / portTICK_PERIOD_MS);

    digitalWrite(7, LOW);
    vTaskDelay(300 / portTICK_PERIOD_MS);
  }
}

void TaskPrint(void *pvParameters) {

  int counter = 0;

  while (1) {

    counter++;

    Serial.println(counter);

    vTaskDelay(500 / portTICK_PERIOD_MS);
  }
}
```
Program menggunakan FreeRTOS untuk menjalankan tiga task secara concurrent pada Arduino Uno. Pada fungsi setup(), dibuat tiga task menggunakan xTaskCreate(), yaitu TaskBlink1, TaskBlink2, dan TaskPrint, kemudian scheduler dijalankan menggunakan vTaskStartScheduler(). TaskBlink1 bertugas menyalakan dan mematikan LED pada pin 8 dengan delay 200 ms, sedangkan TaskBlink2 mengendalikan LED pada pin 7 dengan delay 300 ms. Sementara itu, TaskPrint menampilkan nilai counter ke Serial Monitor setiap 500 ms. Fungsi vTaskDelay() digunakan agar setiap task berhenti sementara sehingga scheduler dapat menjalankan task lain secara bergantian. Dengan mekanisme ini, LED dapat berkedip dan Serial Monitor tetap menampilkan data secara bersamaan.

# Percobaan 2
```cpp
#include <Arduino_FreeRTOS.h>
#include <queue.h>

struct readings {
  int temp;
  int h;
};

QueueHandle_t my_queue;

void read_data(void *pvParameters);
void display(void *pvParameters);

void setup() {

  Serial.begin(9600);

  my_queue = xQueueCreate(1, sizeof(struct readings));

  xTaskCreate(
    read_data,
    "read sensors",
    128,
    NULL,
    0,
    NULL
  );

  xTaskCreate(
    display,
    "display",
    128,
    NULL,
    0,
    NULL
  );
}

void loop() {
}

void read_data(void *pvParameters) {

  struct readings x;

  for (;;) {

    x.temp = 54;
    x.h = 30;

    xQueueSend(my_queue, &x, portMAX_DELAY);

    vTaskDelay(100 / portTICK_PERIOD_MS);
  }
}

void display(void *pvParameters) {

  struct readings x;

  for (;;) {

    if (xQueueReceive(my_queue, &x, portMAX_DELAY) == pdPASS) {

      Serial.print("temp = ");
      Serial.println(x.temp);

      Serial.print("humidity = ");
      Serial.println(x.h);
    }
  }
}
```
Program menggunakan FreeRTOS untuk melakukan komunikasi antar task menggunakan queue. Pada fungsi setup(), dibuat queue menggunakan xQueueCreate() untuk menyimpan data bertipe struct readings yang berisi suhu dan kelembaban. Selanjutnya dibuat dua task, yaitu read_data dan display. Task read_data berfungsi membuat data sensor dummy berupa suhu dan kelembaban, kemudian mengirimkannya ke queue menggunakan xQueueSend(). Setelah itu task mengalami delay agar scheduler dapat menjalankan task lain. Task display bertugas menerima data dari queue menggunakan xQueueReceive() dan menampilkannya pada Serial Monitor. Dengan penggunaan queue, data dapat dipertukarkan antar task secara aman dan terstruktur tanpa menyebabkan konflik data pada sistem multitasking FreeRTOS.
