# ai-based-currency-detector-

### **AI-Based Currency Detector**  
**A smart system that detects fake and real currency using AI and sensors.**  

---

## **1. Components & Cost (Under ₹2000 Budget)**  

| **Component**          | **Price (₹)**  |
|------------------------|--------------|
| Arduino Uno (or 8085 with ADC) | 600  |
| TCS3200 Color Sensor | 500  |
| IR Sensor | 300  |
| Ultrasonic Sensor | 300  |
| Camera Module (Optional for AI Model) | 1000  |

✅ **Total Cost:** ₹2000 - ₹2200 (without the camera, ₹1200)  

---

## **2. Working Principle**  
- The **TCS3200 color sensor** scans the **currency color pattern**.  
- The **IR sensor** checks the **note’s transparency** (real vs fake).  
- The **ultrasonic sensor** measures the **thickness** of the note.  
- If a **camera module is added**, it uses an **AI model (trained on currency images)** to verify authenticity.  
- The **Arduino/8085 microprocessor processes the data** and gives the output.  

---

## **3. Procedure**  

### **Step 1: Connect Sensors to Arduino/8085**  
- **TCS3200 Color Sensor** → Detects color patterns of notes.  
- **IR Sensor** → Checks transparency of the note.  
- **Ultrasonic Sensor** → Measures thickness.  
- **Camera (Optional)** → Captures note image for AI verification.  

### **Step 2: Train AI Model (If Using Camera)**  
- Collect **1000+ images** of real and fake notes.  
- Train an **image classification AI model** using **Google Colab + TensorFlow**.  
- Store trained data in the **Arduino-compatible format**.  

### **Step 3: Write the Code**  

#### **For Basic Arduino (Without AI, Using Sensors Only)**  
```cpp
#include <Wire.h>
#include <TCS3200.h>

TCS3200 tcs(4, 5, 6, 7);  // TCS3200 Sensor Pins
int irSensor = 8;
int ultrasonicTrig = 9;
int ultrasonicEcho = 10;

void setup() {
  Serial.begin(9600);
  pinMode(irSensor, INPUT);
  pinMode(ultrasonicTrig, OUTPUT);
  pinMode(ultrasonicEcho, INPUT);
}

void loop() {
  int colorValue = tcs.readColor();
  int irValue = digitalRead(irSensor);
  int thickness = measureThickness();

  if (colorValue > 50 && irValue == 1 && thickness == 1) {
    Serial.println("Real Currency Detected");
  } else {
    Serial.println("Fake Currency Detected!");
  }

  delay(1000);
}

int measureThickness() {
  digitalWrite(ultrasonicTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(ultrasonicTrig, LOW);
  int duration = pulseIn(ultrasonicEcho, HIGH);
  int distance = duration * 0.034 / 2;
  return (distance < 1) ? 1 : 0; // If distance < 1mm, real note
}
```

---

#### **For AI-Based Model (Python - Google Colab Model Training)**
```python
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Conv2D, Flatten
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# Load Dataset
data_gen = ImageDataGenerator(rescale=1./255, validation_split=0.2)
train_data = data_gen.flow_from_directory('currency_dataset/', target_size=(100,100), batch_size=32, class_mode='binary', subset='training')
val_data = data_gen.flow_from_directory('currency_dataset/', target_size=(100,100), batch_size=32, class_mode='binary', subset='validation')

# Create Model
model = Sequential([
    Conv2D(32, (3,3), activation='relu', input_shape=(100,100,3)),
    Flatten(),
    Dense(128, activation='relu'),
    Dense(1, activation='sigmoid')
])

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

# Train Model
model.fit(train_data, validation_data=val_data, epochs=10)

# Save Model
model.save('currency_detector.h5')
```

---

### **4. Testing & Optimization**  
- Test with **different note conditions** (crumpled, folded, under different lights).  
- Adjust **sensor thresholds** for better accuracy.  
- If using **AI**, add more **training images** for better detection accuracy.  

---

## **5. Why This Project is Unique?**  
✅ **AI-Powered & Sensor-Based Currency Detection**  
✅ **Can be Used for ATM and Counterfeit Detection Machines**  
✅ **Low-Cost & DIY Version of Expensive Bank Scanners**  


![image](https://github.com/user-attachments/assets/3d8bb652-19da-4d14-b14c-fd4112eb8a15)
![image](https://github.com/user-attachments/assets/e6d20009-520f-4f77-b1d5-32610244a786)
![image](https://github.com/user-attachments/assets/ad8a4a40-1bab-4f8a-ae11-b3e3306445a2)



