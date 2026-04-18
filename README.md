# WRO 2026 Future Engineers - Team   XCLNC Lunar
![Main Robot View](image-18-04-26-09-10.jpeg) 
![Main Robot View](image-18-04-26-09-10-1.jpeg) 
![Main Robot View](image-18-04-26-09-10-2.jpeg) 
![Main Robot View](image-18-04-26-09-10-3.jpeg) 
![Main Robot View](image-18-04-26-09-10-4.jpeg) 
## 1. Introduction
Our robot is an autonomous vehicle developed for the WRO 2026 Future Engineers category. It is built using the **LEGO Spike Prime** platform and integrated with a **Pixy2** camera. The goal is to achieve high-speed navigation, obstacle avoidance, and precise parallel parking.

## 2. Mobility and Mechanical Design

### 2.1 Steering and Ackerman Geometry
For the steering system, we implemented a front-axle mechanism based on **Ackermann Steering Geometry**. 
* **Mechanism:** A Spike Prime Medium Motor drives a rack-and-pinion system.
* **Benefit:** This geometry ensures that the inner and outer wheels turn at different angles, significantly reducing friction and preventing the robot from skidding during tight turns on the track.
![Steering Mechanism](images/IMG_2482.jpeg) 

### 2.2 Drivetrain and Differential
The rear-wheel-drive system is powered by two Spike Prime Large Motors.
* **The Differential:** We integrated a **LEGO Technic Differential** in the rear axle.
* **Function:** The differential allows the rear wheels to rotate at different speeds when cornering. This is crucial for stability, as it prevents the drive wheels from slipping and ensures consistent traction.
![Rear Axle and Differential](images/IMG_2487.jpeg)

---

## 3. Power and Sensor Architecture
* **Controller:** LEGO Spike Prime Hub.
* **Vision:** Pixy2 Camera for real-time object signature tracking (Red/Green).
* **Distance:** Ultrasonic sensor for proximity detection during parking.
* **Power:** 2100 mAh Li-ion rechargeable battery.

---

## 4. Software Architecture & PID Control
The robot is programmed in **Python (Pybricks)**. We use a **PID Controller** to translate camera data into steering movements.

### Logic Breakdown:
1. **Detection:** Pixy2 identifies the X-coordinate of the obstacle.
2. **Error Calculation:** We calculate the distance between the obstacle and our "safe path".
3. **Output:** The PID algorithm adjusts the Steering Motor to steer the robot smoothly.

---

## 5. Engineering Process & Evolution

### 5.1 The Connection Challenge (Pixy2 & Pybricks)
The most difficult technical problem we faced was the communication between the Pixy2 camera and the Spike Hub. Initially, the Hub could not detect the camera values.

**The Solution:**
We discovered a specific initialization "Handshake" protocol. To make the system work, we must:
1. **Power on** the Hub.
2. **Connect** the Pixy2.
3. **Bridge to PC:** Temporarily connect the Pixy2 to a computer via USB to sync the firmware state.
4. **Run Script:** Only then does the I2C communication become active in Pybricks.
![Calibration Setup](images/IMG_2515.jpeg)

---

## 6. Source Code (Main Logic)
```python
from pybricks.hubs import PrimeHub
from pybricks.pupdevices import Motor, ColorSensor, UltrasonicSensor
from pybricks.parameters import Port, Direction, Stop
from pybricks.tools import wait
# INITIALIZATION PROTOCOL:
# 1. Power on Hub. 2. Plug in Pixy2. 3. Connect Pixy2 to PC. 4. Run.
hub = PrimeHub()
steering_motor = Motor(Port.A) # Ackermann Steering
drive_motors = Motor(Port.B) # Differential Drive
# Настройка Pixy2 и датчиков...
def get_pixy_data():
# Логика получения данных от Pixy2 pass def drive_logic():
while True:
# ПИД-регулятор и объезд препятствий
# Если Красный -> Руль влево
# Если Зеленый -> Руль вправо
wait(10)
drive_logic()
