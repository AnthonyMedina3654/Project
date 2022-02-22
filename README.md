# Project

* [Project CAD](#Project_CAD)
* [Vectorized Image Code](#Vectorized_Image_Code)
* [Stepper Motor Code](#Stepper_Motor_Code)
* [Potentiometer Code](#Potentiometer_Code)
---

## Project_CAD

### Description

digital representation of our project, put together our mechanical arm digitaly. 
### Evidence


### Reflection

### Images
---

## Vectorized_Image_Code

### Description

### Evidence

### Reflection

### Images
---

## Stepper_Motor_Code

### Description

### Evidence
# Write your code here :-)

import time
import board
import digitalio
from adafruit_motor import stepper

DELAY = 0.02
STEPS = 5 000

# You can use any available GPIO pin on both a microcontroller and a Raspberry Pi.
# The following pins are simply a suggestion. If you use different pins, update
# the following code to use your chosen pins.

# To use with CircuitPython and a microcontroller:
```python
Code goes here
coils = (
    digitalio.DigitalInOut(board.D4),  # A1
    digitalio.DigitalInOut(board.D5),  # A2
    digitalio.DigitalInOut(board.D6),  # B1
    digitalio.DigitalInOut(board.D7),  # B2
)

coils2 = (
    digitalio.DigitalInOut(board.D8),  # A1
    digitalio.DigitalInOut(board.D9),  # A2
    digitalio.DigitalInOut(board.D10),  # B1
    digitalio.DigitalInOut(board.D11),  # B2
)


for coil in coils:
    coil.direction = digitalio.Direction.OUTPUT

for coil in coils2:
    coil.direction = digitalio.Direction.OUTPUT

motor = stepper.StepperMotor(coils[0], coils[1], coils[2], coils[3], microsteps=None)
motor2 = stepper.StepperMotor(coils2[0], coils2[1], coils2[2], coils2[3], microsteps=None)

for step in range(STEPS):
    motor.onestep()
    motor2.onestep()
    time.sleep(DELAY)

for step in range(STEPS):
    motor.onestep(direction=stepper.BACKWARD)
    motor2.onestep(direction=stepper.BACKWARD)
    time.sleep(DELAY)

for step in range(STEPS):
    motor.onestep(style=stepper.DOUBLE)
    motor2.onestep(style=stepper.DOUBLE)
    time.sleep(DELAY)

for step in range(STEPS):
    motor.onestep(direction=stepper.BACKWARD, style=stepper.DOUBLE)
    motor2.onestep(direction=stepper.BACKWARD, style=stepper.DOUBLE)
    time.sleep(DELAY)

for step in range(STEPS):
    motor.onestep(style=stepper.INTERLEAVE)
    motor2.onestep(style=stepper.INTERLEAVE)
    time.sleep(DELAY)

for step in range(STEPS):
    motor.onestep(direction=stepper.BACKWARD, style=stepper.INTERLEAVE)
    motor2.onestep(direction=stepper.BACKWARD, style=stepper.INTERLEAVE)
    time.sleep(DELAY)
motor.release()

```

### Reflection

### Images
---

## Potentiometer_Code

### Description

### Evidence

### Reflection

### Images
---
