# Percobaan 1
```cpp
#include <Servo.h>        // Library untuk mengontrol servo

Servo myservo;            // Membuat objek servo

// PIN
const int potensioPin = A0;  // Pin analog untuk potensiometer
const int servoPin = 9;      // Pin PWM untuk servo

// VARIABEL
int pos = 0;             // Menyimpan sudut servo
int val = 0;             // Menyimpan nilai ADC dari potensiometer

void setup() {
  myservo.attach(servoPin);   // Menghubungkan servo ke pin 9
  Serial.begin(9600);         // Memulai komunikasi serial (baud rate 9600)
}

void loop() {
  val = analogRead(potensioPin);   // Membaca nilai analog (0–1023)

  pos = map(val, 0, 1023, 0, 180); // Mengubah nilai ADC ke sudut 0–180 derajat

  myservo.write(pos);              // Menggerakkan servo sesuai sudut

  Serial.print("ADC Potensio: ");  // Menampilkan teks
  Serial.print(val);               // Menampilkan nilai ADC
  Serial.print(" | Sudut Servo: "); 
  Serial.println(pos);             // Menampilkan sudut servo

  delay(50);                       // Delay agar sistem stabil
}
```
Program dimulai dengan inisialisasi servo dan komunikasi serial pada fungsi setup(), kemudian pada fungsi loop() Arduino membaca nilai analog dari potensiometer menggunakan analogRead() dalam rentang 0–1023. Nilai tersebut selanjutnya dikonversi menjadi sudut servo 0–180 derajat menggunakan fungsi map(), lalu dikirim ke servo melalui myservo.write() sehingga servo bergerak sesuai perubahan input. Selain itu, nilai ADC dan sudut servo ditampilkan pada Serial Monitor untuk monitoring, dan proses ini diulang terus-menerus dengan delay singkat agar sistem bekerja stabil.

# Percobaan 2
```cpp
#include <Arduino.h>      // Library dasar Arduino

// PIN
const int potPin = A0;    // Pin analog untuk potensiometer
const int ledPin = 9;     // Pin PWM untuk LED

// VARIABEL
int nilaiADC = 0;         // Menyimpan nilai ADC
int pwm = 0;              // Menyimpan nilai PWM (0–255)

void setup() {
  pinMode(ledPin, OUTPUT); // Mengatur pin LED sebagai output
  Serial.begin(9600);      // Memulai komunikasi serial
}

void loop() {
  nilaiADC = analogRead(potPin);   // Membaca nilai analog (0–1023)

  pwm = map(nilaiADC, 0, 1023, 0, 255); // Mengubah ADC ke PWM

  analogWrite(ledPin, pwm);       // Mengatur kecerahan LED dengan PWM

  Serial.print("ADC: ");          // Menampilkan teks
  Serial.print(nilaiADC);         // Menampilkan nilai ADC
  Serial.print(" | PWM: ");
  Serial.println(pwm);            // Menampilkan nilai PWM

  delay(50);                      // Delay agar pembacaan stabil
}
```
Program diawali dengan inisialisasi pin LED sebagai output dan komunikasi serial pada setup(), kemudian pada loop() Arduino membaca nilai analog dari potensiometer menggunakan analogRead(). Nilai ADC yang diperoleh dikonversi menjadi nilai PWM 0–255 menggunakan fungsi map(), lalu dikirim ke LED melalui analogWrite() untuk mengatur tingkat kecerahan. Data nilai ADC dan PWM juga ditampilkan pada Serial Monitor sebagai monitoring, dan seluruh proses berlangsung secara berulang dengan delay untuk menjaga kestabilan pembacaan dan output.

