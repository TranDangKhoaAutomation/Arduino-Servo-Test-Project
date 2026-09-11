# 🎛️ Arduino Servo Angle Controller (Serial Input)

This is a simple Arduino demo project that shows how to control a servo motor using the **Serial Monitor**.  
Just type an angle from **0–180**, and the servo will rotate to that position.  

👉 Perfect for beginners who want to learn how to use the `Servo.h` library and practice basic Arduino–servo control.

---

## 📂 Repository Contents
- `main.ino` → Arduino sketch (servo control via Serial input).  
- `sweep_bb.png` → Breadboard wiring diagram.  
- `sweep_schem.png` → Circuit schematic.  

---

## 🔌 Hardware Setup

### Required Components
- Arduino Uno (or compatible board)  
- Servo motor (SG90, MG996R, or similar)  
- Jumper wires  
- (Optional) External power supply for high-torque servos  

### Wiring
- **Servo VCC (Red)** → Arduino `5V`  
- **Servo GND (Brown/Black)** → Arduino `GND`  
- **Servo Signal (Orange/Yellow)** → Arduino `D9`  

📷 Breadboard view:  
![Breadboard](/images/sweep_bb.png)

📷 Schematic view:  
![Schematic](/images/sweep_schem.png)

---

## 💻 Arduino Code

```cpp
#if defined(ESP32)
  #include <ESP32Servo.h>
#else
  #include <Servo.h>
#endif

Servo myServo;
int servoPin = 9;  // Servo pin (D9 on Arduino UNO)

void setup() {
  Serial.begin(9600);
  myServo.attach(servoPin);  // Attach servo to pin
  Serial.println("Nhap goc servo (0 - 180):");
}

void loop() {
  if (Serial.available()) {
    String input = Serial.readStringUntil('\n'); // Read until Enter key
    input.trim(); // Remove whitespace and \r \n
    
    if (input.length() > 0) {
      // Validate if input is a number
      bool isNumber = true;
      for (unsigned int i = 0; i < input.length(); i++) {
        if (!isDigit(input[i])) {
          isNumber = false;
          break;
        }
      }

      if (isNumber) {
        int angle = input.toInt();
        if (angle >= 0 && angle <= 180) {
          myServo.write(angle);
          Serial.print("✅ Servo da quay den goc: ");
          Serial.println(angle);
        } else {
          Serial.println("⚠️ Goc khong hop le! (0 - 180)");
        }
      } else {
        Serial.println("⚠️ Vui long nhap so nguyen!");
      }

      Serial.println("Nhap goc servo (0 - 180):"); // Prompt again
    }
  }
}
````

---

## ▶️ How to Use

### 1. Install Arduino IDE

* Download the [Arduino IDE](https://www.arduino.cc/en/software).
* Install it on your PC (Windows, macOS, or Linux).

### 2. Connect the Hardware

* Plug your Arduino into your PC via USB.
* Connect the servo motor according to the wiring diagram.

### 3. Upload the Sketch

* Open `main.ino` in Arduino IDE.
* Select **Board: Arduino Uno** (or your board).
* Select the correct **Port**.
* Click **Upload**.

### 4. Run the Demo

* Open **Serial Monitor** (`Ctrl+Shift+M`).
* Set baud rate to **9600**.
* Type any angle from `0` → `180` and press **Enter**.
* The servo will rotate to the specified angle.

---

## 🔧 Troubleshooting

* **Servo does not move**
  → Check wiring (Signal → D9, Power → 5V, GND → GND).

* **Servo jittering or unstable**
  → Use a stable power supply. For MG996R or other high-torque servos, use an **external 5–6V supply**.

* **Arduino resets when servo moves**
  → The servo draws too much current. Power it externally and connect **common GND**.

* **No response in Serial Monitor**
  → Ensure baud rate = `9600`.

---
Bạn có thể viết lại đoạn đó gọn gàng hơn, tránh lặp ý mà vẫn đầy đủ:

## 📜 License

This project is licensed under the [MIT License](LICENSE).  

© 2025 Trần Đăng Khoa / TranDangKhoaAutomation (TranDangKhoaAutomation)  
You may use, modify, and distribute this project freely, provided that proper attribution is given.  
See the [LICENSE](LICENSE) file for full details.
