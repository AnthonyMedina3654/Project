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

Graham- We had a lot of procrastination issues. Due to the complexity of the project it would have been much easier if each group member did their assigned work to complete in time. The code was probably the worst of it because not a single person knew anything about it and it wasnt very prominent when we did research about it. Researching and creating code for the potentiometer would have helped a lot however since that did not get completed we were handicapped for the other section involving the wiring of two stepper motors. The onshape design was the part we worked the most on and it is the closest to completion with minimal problems that could be solved relativly quickly. Lesson for next time pick partners not based on comfort but on work ethic and mentality. 
### Images
---

## Vectorized_Image_Code

### Description
the code to shift pixels of an image into coordinates on a plane. 
### Evidence

### Reflection
It was very difficult to find any sample code on transfering pixels into coordinates and plugging them into Mu because no one knew a lot about g code and there wasnt anything on google about it.
### Images
---

## Stepper_Motor_Code

### Description
Code that will allow us to move two stepper motors together to form movement in any direction on a plane so we would be able to control the pencil for drawing.
### Evidence

```python

import time
import board
import digitalio
from adafruit_motor import stepper

DELAY = 0.0
STEPS = 5 000

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
There were some hiccups but overall it was straight forward. Getting the first one to work was easy but getting two to work in sync was a pain in the ass because there were many factors to take in including wiring issues and issues with the code that looked insignificant but could be overlooked very easily.

'''python


# These are the libraries needed to fade an LED, even if you imported elsewhere
import time
import board
import pwmio
import digitalio

lightBulb = digitalio.DigitalInOut(board.D13)       # I moved my RGBLED power wire from 5v
lightBulb.direction = digitalio.Direction.OUTPUT    # and plugged it into D13.  I'll explain later.

class LED:      # It's propper coding to always write a line explaining a class
        # with a "docstring."   Like this:
    '''LED is a class designed for a single color LED to fade in and out'''

    def __init__(self, ledpin, name):
        # init is like void Setup() from arduino.  Initialize your pins here
        self.led = pwmio.PWMOut(ledpin, frequency=5000, duty_cycle=0)
        self.name = name

    def fadedown(self):     # Fades LED from bright to dim
        for i in range(255):
            if i < (255/2):
                self.led.duty_cycle = int(i * 65535 / (255/2))
            print(self.name, ", ", self.led.duty_cycle)
            time.sleep(0.01)

    def fadeup(self):  # Fades LED from dim to bright
        for i in range(255):
            if i > (255/2):
                self.led.duty_cycle = 65535 - int((i - (255/2)) * 65535 / (255/2))
            print(self.name, ", ", self.led.duty_cycle)
            time.sleep(0.01)

    def on(self, brightness=65535):  # Remember "on" means duty cycles < 65535
        self.led.duty_cycle = 65535 - brightness
        lightBulb.value = 65535

    def off(self):      # "off" means duty cycle should be full.
        self.led.duty_cycle = 65535

class RGB:
    '''this class should impliment all 3 pins together to control an RGB LED'''
    from rgb import LED
    # Let's take a second to appreciate that we're using a class to call a class!
    # Let LED do all the nitty gritty work, this RGB class will be the "manager"

    def __init__(self, redPin, greenPin, bluePin):
        # To initialize an RGB LED, we need to initialize 3 LED pins.
        self.myRedLED = LED(redPin, "red")
        self.myBlueLED = LED(bluePin, "blue")
        self.myGreenLED = LED(greenPin, "green")

    def blue(self, brightness=65535):
        # Notice the brightness=65535?  Thats an OPTIONAL parameter!  So in main.py,
        # you can call "RGBLED1.blue() for full brightness, or "RGBLED1.blue(half) to
        # make it dimmer!
        self.myBlueLED.on(brightness)
        self.myGreenLED.off()
        self.myRedLED.off()

    def green(self, brightness=65535):
        # Notice the brightness=65535?  Thats an OPTIONAL parameter!  So in main.py,
        # you can call "RGBLED1.blue() for full brightness, or "RGBLED1.blue(half) to
        # make it dimmer!
        self.myBlueLED.off()
        self.myGreenLED.on(brightness)
        self.myRedLED.off()

    def red(self, brightness=65535):
        # Notice the brightness=65535?  Thats an OPTIONAL parameter!  So in main.py,
        # you can call "RGBLED1.blue() for full brightness, or "RGBLED1.blue(half) to
        # make it dimmer!
        self.myBlueLED.off()
        self.myGreenLED.off()
        self.myRedLED.on(brightness)

    def yellow(self, brightness=65535):
        # Notice the brightness=65535?  Thats an OPTIONAL parameter!  So in main.py,
        # you can call "RGBLED1.blue() for full brightness, or "RGBLED1.blue(half) to
        # make it dimmer!
        self.myBlueLED.off()
        self.myGreenLED.on(brightness)
        self.myRedLED.on(brightness)

    def cyan(self, brightness=65535):
        # Notice the brightness=65535?  Thats an OPTIONAL parameter!  So in main.py,
        # you can call "RGBLED1.blue() for full brightness, or "RGBLED1.blue(half) to
        # make it dimmer!
        self.myBlueLED.on(brightness)
        self.myGreenLED.on(brightness)
        self.myRedLED.off()

    def magenta(self, brightness=65535):
        # Notice the brightness=65535?  Thats an OPTIONAL parameter!  So in main.py,
        # you can call "RGBLED1.blue() for full brightness, or "RGBLED1.blue(half) to
        # make it dimmer!
        self.myBlueLED.on(brightness)
        self.myGreenLED.off()
        self.myRedLED.on(brightness)
    # Write a bunch
    # of more methods
    # here.

    def off(self):
        # This turns off all 3 LEDs, but my LEDs were still glowing a tiny bit.
        # To fix this, i took the RGB power wire out of 5V , and plugged it into D13.
        # Now when I want the LED to be off, it's truly off!
        self.myBlueLED.off()
        self.myGreenLED.off()
        self.myRedLED.off()
        lightBulb.value = 0


'''
### Images


```python
''' This file is the class-based version of making a single LED fade'''
import time
import board
from rgb import RGB

r1 = board.D8
b1 = board.D9
g1 = board.D10
r2 = board.D4
b2 = board.D5
g2 = board.D7       # D6 uses the same timer as D8,9,10.  Avoid!

full = 65535                # Max Brightness
half = int(65535/2)         # Half Brightness

myRGBled1 = RGB(r1, g1, b1)     # create a new RGB object, using pins 8, 9, & 10
myRGBled2 = RGB(r2, g2, b2)     # create a new RGB object, using pins 4, 5, & 7


while True:
    '''Shines two RGB LEDs in opposing colours, then rainbows!'''
    myRGBled1.blue(half)
    myRGBled2.yellow(half)
    time.sleep(1)
    myRGBled1.off()
    myRGBled2.off()
    time.sleep(.5)

    myRGBled1.red()
    myRGBled2.cyan()
    time.sleep(1)
    myRGBled1.off()
    myRGBled2.off()
    time.sleep(.5)

    myRGBled1.green()
    myRGBled2.magenta()
    time.sleep(1)
    myRGBled1.off()
    myRGBled2.off()
    time.sleep(.5)

    myRGBled1.Blinky(1)     # Obviously you should replace "rate1" with a real number...
    myRGBled2.Blinky(1)     # Sames
    time.sleep(1)

# extra spicy (optional):
# myRGB1.rainbow(rate1) # Fade through the colors of the rainbow at the given rate.  Oooooh, pretty!
# myRGB2.rainbow(rate2) # Fade through the colors of the rainbow at the given rate.  Oooooh, pretty!
# time.sleep(5)



```
## Potentiometer_Code

### Description

### Evidence

### Reflection

### Images
---
