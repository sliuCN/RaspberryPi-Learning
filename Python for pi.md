# GPIO
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

chan_list = [10,11] #or chan_list = (10,11)
GPIO.setup(chan_list,GPIO.OUT)

print(GPIO.input(12))

GPIO.output(13,GPIO.LOW) # or 0 or False
GPIO.output(chan_list,(GPIO.HIGH,GPIO.LOW))

pwm_pin=20
frequency=1000
duty=20 # %
p = GPIO.PWM(pwm_pin, frequency)

p.start(duty)

freq=2000
p.ChangeFrequency(freq)
duty=30
p.ChangeDutyCycle(duty)

p.stop()

GPIO.cleanup()
```
### BCM vs BOARD
模式设置（GPIO.setmode）用于确定引脚编号方式  
在树莓派中有两种不同的引脚编号方式：
1. BOARD number: 树莓派P1 2x20排针的顺序 
2. BCM number: Broadcom SOC的引脚编号，例如GPIO x
![pin_number](image/rpi_pin_number.png)

### 边沿检测
?是否需要首先设置为输入？
wait_for_edge()  
event_detected()  
``` python
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)

# wait for up to 5 seconds for a rising edge (timeout is in milliseconds)
channel = GPIO.wait_for_edge(channel, GPIO_RISING, timeout=5000)
if channel is None:
    print('Timeout occurred')
else:
    print('Edge detected on channel', channel)

GPIO.add_event_detect(channel, GPIO.RISING)  # add rising edge detection on a channel GPIO.RISING, GPIO.FALLING or GPIO.BOTH
do_something()
if GPIO.event_detected(channel):
    print('Button pressed')
```
### 多线程回调函数
* GPIO.add_event_callback(channel,my_callbackfunction)
* GPIO.add_enent_detect(channel, GPIO.RISING, callback=my_callbackfunction)
``` python
def my_callback(channel):
    print('This is a edge event callback function!')
    print('Edge detected on channel %s'%channel)
    print('This is run in a different thread to your main program')

GPIO.add_event_detect(channel, GPIO.RISING, callback=my_callback)  # add rising edge detection on a channel
```
``` python
def my_callback_one(channel):
    print('Callback one')

def my_callback_two(channel):
    print('Callback two')

GPIO.add_event_detect(channel, GPIO.RISING)
GPIO.add_event_callback(channel, my_callback_one)
GPIO.add_event_callback(channel, my_callback_two)
```
消抖处理  
* GPIO.add_event_detect(channel, my_callbackfunction,bouncetime=200) #in ms
* GPIO.add_event_callback(channel,my_callbackfunction, bouncetime=200)

删除事件
* GPIO.remove_event_detect(channel)

> https://sourceforge.net/p/raspberry-gpio-python/wiki/Home/
> https://pypi.org/project/RPi.GPIO/

> https://www.raspberrypi.org/documentation/usage/gpio/python/README.md
# SPI

# UART

# IIC

# 
