# GPIO

# SPI
## gpiozero
应用层的GPIO封装，省去了基本的IO操作，可以直接将IO设置成LED，Button等对象  
包含了这些对象的常用动作（如LED闪烁，按键按下等）
### LED
* on()
* off()
* toggle()
* blink()
``` python
from gpiozero import LED
from time import sleep

led = LED(17)

while True:
    led.on()
    sleep(1)
    led.off()
    sleep(1)
```
### Button
* is_pressed
* is_held
* when_pressed
* when_released
* when_held 
* wait_for_press() 
* wait_for_release()
``` python
from gpiozero import Button
from time import sleep

button = Button(2)

while True:
    if button.is_pressed:
        print("Pressed")
    else:
        print("Released")
    sleep(1)
```
> https://gpiozero.readthedocs.io/en/stable/index.html

## RPi.GPIO  
传统的树莓派GPIO控制库，更偏向于C  
* 确定引脚编号
* 设置输入输出
* 设置/读取电平

``` python
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BOARD) #GPIO.setmode(GPIO.BCM)
GPIO.setup(12,GPIO.IN)
GPIO.setup(13,GPIO.OUT)

chan_list = [10,11]
GPIO.setup(chan_list,GPIO.OUT)

print(GPIO.input(12))

GPIO.output(13,GPIO.LOW)
GPIO.output(chan_list,(GPIO.HIGH,GPIO.LOW))
```

> https://sourceforge.net/p/raspberry-gpio-python/wiki/Home/

> https://pypi.org/project/RPi.GPIO/

> https://www.raspberrypi.org/documentation/usage/gpio/python/README.md
# UART

# IIC

# 
