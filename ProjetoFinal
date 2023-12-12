import machine, neopixel
import time, dht

sensor = dht.DHT22(machine.Pin(14))
button = machine.Pin(12, machine.Pin.IN, machine.Pin.PULL_UP)
buzzer = machine.PWM(machine.Pin(22, machine.Pin.OUT))
buzzer.init(freq=1, duty=0)

n = 1
p = 23
np = neopixel.NeoPixel(machine.Pin(p), n)

while True:
    sensor.measure()
    temp = sensor.temperature()
    print('Temperatura: %3.1f C' %temp)
    if temp < 20:   
        np[0] = (0, 0, 255)
        np.write()

    if temp >= 20 and temp <= 23:
        np[0] = (0,255,0)
        np.write()
        buzzer.duty(0)
        
    if temp > 23:
        np[0] = (255,0,0)
        np.write()
        buzzer.freq(1500)
        buzzer.duty(10)
        
    if not button.value():
        np[0] = (0,0,0)
        np.write()
        break;
    
buzzer.deinit()
