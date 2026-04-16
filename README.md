 Button, Buzzer
from gpiozero import Button, Buzzer
from time import sleep

# Using your chosen pins
button = Button(17)
buzzer = Buzzer(22)

try: 
    while True:
        if button.is_pressed:
            buzzer.on()
            sleep(0.2)
            buzzer.off()
            # This 'debounces' the button so it only beeps once per press
            while button.is_pressed:
                sleep(0.1)
        sleep(0.1)
except KeyboardInterrupt:
    # This is the correct cleanup we discussed earlier
    button.close()
    buzzer.close()
    print("\nProgram stopped and pins cleaned up.")


LED 

from gpiozero import PWMLED, Button
from time import sleep

button = Button(17)
led = PWMLED(22) # Note: Make sure your LED is on GPIO 22

try:
    while True:
        if button.is_pressed:
            # You can use led.value = 0.5 for 50% brightness!
            led.value = 1 
            sleep(0.2)
            led.value = 0 # This is the same as led.off()
            
            while button.is_pressed:
                sleep(0.1)
                
        sleep(0.1)
except KeyboardInterrupt:
    button.close()
    led.close() # Fixed: must match the variable name 'led'
    print("\nCleanup successful.")



 Ultrasonic Sensor

from gpiozero import DistanceSensor
from time import sleep

s = DistanceSensor(echo=17, trigger=22)

try:
    while True:
       
        dist_cm = s.distance * 100

        print(f"Distance: {dist_cm:.1f} cm")

        sleep(0.5) 

except KeyboardInterrupt:
    s.close()
    print("\nSensor stopped.")



 
import Adafruit_DHT
from time import sleep

sensor = Adafruit_DHT.DHT22
pin = 4

try:
    while True:

        h, t = Adafruit_DHT.read_retry(sensor, pin) 

        if h is not None and t is not None: 
            print(f"Temp: {t}*C")
            print(f"Hum: {h}%")   
        else:
            print("Read error")
        sleep(2) 

except KeyboardInterrupt:
    print("sys close")

Alcohol sensor 

from gpiozero import DigitalInputDevice
from time import sleep

alcohol_sensor = DigitalInputDevice(17)

try:
    while True:
        
        if alcohol_sensor.value:
            print("Alcohol Detected!") 
        else:
            print("Safe")
            
        sleep(1)
except KeyboardInterrupt:
    alcohol_sensor.close()
    print("System Shutdown")

















Motion Sensor

from gpiozero import MotionSensor
from time import sleep

s = MotionSensor(4)

print("Sensor Warming Up...")
sleep(2) # PIR sensors need a moment to stabilize

try:
    while True:
        if s.motion_detected:
            print("Motion")
        else:
            print("No Motion") 

        sleep(1)

except KeyboardInterrupt:
    s.close()
    print("System Off")

RFID : 

Installation : git clone https://www.github.com/pimylifeup/MFRC522-python
Python setup.py install

from mfrc522 import SimpleMFRC522

reader = SimpleMFRC522()
text = "hi"

try:
    print("Waiting for tag to write...")
    reader.write(text)
    print("Written successfully!")
finally:
    # This prints even if the code crashes or finishes
    print("Write sequence finished")


—-----------------------------------------

from mfrc522 import SimpleMFRC522

reader = SimpleMFRC522()

try:
    print("Waiting for tag to read...")
    i, t = reader.read()
    print(i)
    print(t)
finally:
    print("Read sequence finished")

Gas Sensor: 

from gpiozero import DigitalInputDevice
from time import sleep

gas_sensor = DigitalInputDevice(17)

try:
    while True:
        if gas_sensor.value:
            print("Gas/Smoke Detected!")
        else:
            print("Air is Clear")
        sleep(1)
except KeyboardInterrupt:
    print("Stopping...")

Pressure 

import board
import adafruit_bmp280

i2c = board.I2C() 
bmp280 = adafruit_bmp280.Adafruit_BMP280_I2C(i2c)

try:
    while True:
        print(f"Temp: {bmp280.temperature:.1f} C")
        print(f"Pressure: {bmp280.pressure:.1f} hPa")
        sleep(2)
except KeyboardInterrupt:
    print("Done")
Pi camera 

from picamera import PiCamera
from time import sleep

camera = PiCamera()

try:
    print("Preview starting (5 seconds)...")
    camera.start_preview()
    sleep(5) # Give the camera time to adjust light
    camera.capture('/home/pi/Desktop/image.jpg')
    camera.stop_preview()
    print("Photo captured to Desktop!")
finally:
    camera.close()

7 segment display: 

from gpiozero import LEDCharDisplay
from time import sleep

# Define the pins connected to segments a, b, c, d, e, f, g
# This example uses GPIOs 4, 5, 6, 13, 19, 26, 21
display = LEDCharDisplay(4, 5, 6, 13, 19, 26, 21)

try:
    while True:
        # Display numbers 0 to 9
        for i in range(10):
            display.value = str(i)
            sleep(1)
except KeyboardInterrupt:
    display.close()




—---------------------

1. General Updates
Always run this first to make sure your package list is current.
Bash
sudo apt update && sudo apt upgrade -y

Gpio : 
sudo apt update
sudo apt install python3-gpiozero



2. Sensor-Specific Libraries
Component
Installation Command
DHT11 / DHT22
pip3 install Adafruit_DHT
RFID (RC522)
pip3 install spidev mfrc522
BMP280 (Pressure)
pip3 install adafruit-circuitpython-bmp280
Camera
sudo apt install python3-picamera
Gas / Alcohol / Motion
No install needed (uses built-in gpiozero)
7-Segment
No install needed (uses built-in gpiozero)


3. Enabling Hardware Interfaces
This is the part most people forget! Some sensors won't work even if the code is perfect unless the "doors" are open on the Pi.
Type: sudo raspi-config
Go to Interface Options
Enable the following:
I2C (For the BMP280 Pressure Sensor)
SPI (For the RFID Reader)
Legacy Camera (If using the older picamera library)

4. Troubleshooting Command
If you run your code and get a "Permission Denied" error for the GPIO pins, you might need to add your user to the GPIO group (though this is usually default on modern Pi OS):
Bash
sudo usermod -a -G gpio pi

5. Quick Check for I2C
If you have the Pressure Sensor wired up and want to see if the Pi actually "sees" it before you run your code, use this:
Bash
i2cdetect -y 1



		

