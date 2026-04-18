# WRO 2026 Future Engineers - Detailed Engineering Documentation
**Team:** XCLNC Lunar 
**Platform:** LEGO Spike Prime + Pixy2  
**Programming Language:** Python (Pybricks)

---

## 1. Project Overview & The "Evolution" Strategy
Our goal for the 2026 season was to move away from "brute force" designs toward high-precision engineering. We focused on three pillars: **Stability, Compactness, and Data Reliability.**

### 1.1 Comparison with Previous Design
In our previous season, our robot was significantly larger and less efficient. 
![Differential Assembly](images/differential_detail.jpg)
* **Old Design:** Heavy chassis, high center of gravity, and complex but "sloppy" mechanical linkages. 
* **New Design (Current):** 35% smaller, with a concentrated center of mass over the front axle and a reinforced drivetrain.
![Comparison: Old vs New Robot](images/old_robot_comparison.jpg)
*Caption: Our old design (left) struggled with tight turns due to its wheelbase length, leading us to the current optimized chassis (right).*

---

## 2. Mobility Management: Mechanical Excellence

### 2.1 The Steering Debate: Parallel vs. Ackermann
During the prototyping phase, we conducted a study on steering geometries. While **Ackermann Steering** is ideal for real-world cars to reduce tire scrub, we intentionally chose **Parallel Steering** for this robot.

**Why Parallel Steering?**
1. **Precision in Micro-movements:** At the small scale of LEGO parts, the "play" (mechanical backlash) in Ackermann linkages often absorbs the steering input. Parallel steering provides a more direct and rigid connection to the motor.
2. **Maximum Turning Angle:** Our parallel mechanism allows for a 70-degree wheel rotation without the linkages locking up. This is crucial for the Parallel Parking maneuver where space is extremely limited.
3. **Friction Compensation:** Since we use thin front tires, the slight "sliding" effect of parallel steering actually helps the robot pivot faster in sharp corners without the bouncing effect often seen in complex LEGO linkages.
![Steering Geometry Analysis](images/steering_geometry.jpg)

### 2.2 Rear Axle & Differential Logic
To ensure the robot doesn't skid during high-speed laps, we integrated a **LEGO Technic Differential**. 
* **The Problem:** A solid axle forces both wheels to rotate at the same speed, causing the inner wheel to lose traction in turns.
* **The Solution:** The differential allows the outer wheel to travel a longer path than the inner wheel. This results in smooth, predictable cornering and preserves the lifespan of our motors.
![Differential Assembly](images/differential_detail.jpg)

---

## 3. Sensor Fusion & Power Architecture

### 3.1 Pixy2 Vision Strategy
We use the **Pixy2.1 LEGO Edition** not just as a color sensor, but as a spatial coordinate generator.
* **Coordinate Mapping:** We map the X-center of detected signatures to a PID error value.
* **Frame Rate:** Running at 60 FPS allows the robot to react to an obstacle in less than 16ms, which is vital when moving at the robot's top speed of 1.2 m/s.

### 3.2 Electronics & Power Stability
* **Hub Placement:** The Spike Prime Hub is mounted horizontally to keep the center of gravity low.
* **Power Management:** We use the 2100 mAh Li-ion battery. We found that the Pixy2 draws ~140mA; to prevent I2C brownouts, we ensure the battery never drops below 7.2V before a competitive run.

---

## 4. Software Engineering: The Python Advantage

We utilize **Pybricks (MicroPython)** instead of standard Word Blocks. This allows us to implement multi-threading for sensor reading and motor control.

### 4.1 PID Control for Pathfinding
Our steering is controlled by a PID (Proportional-Integral-Derivative) algorithm:
```python
# PID Steering Logic Snippet
error = target_x - current_pixy_x
derivative = error - last_error
steering_output = (Kp * error) + (Kd * derivative)
steering_motor.run_target(steering_ou
