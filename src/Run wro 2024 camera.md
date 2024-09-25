sonar_left = 0
sonar_Right = 0
huskylens.init_i2c()
huskylens.init_mode(protocolAlgorithm.ALGORITHM_COLOR_RECOGNITION)
pins.digital_write_pin(DigitalPin.P9, 0)
pins.digital_write_pin(DigitalPin.P11, 0)
pins.digital_write_pin(DigitalPin.P12, 1)
pins.digital_write_pin(DigitalPin.P13, 0)
pins.analog_write_pin(AnalogPin.P14, 400)
basic.pause(100)

def on_forever():
    global sonar_Right, sonar_left
    huskylens.request()
    sonar_Right = sonar.ping(DigitalPin.P1, DigitalPin.P0, PingUnit.CENTIMETERS)
    sonar_left = sonar.ping(DigitalPin.P8, DigitalPin.P2, PingUnit.CENTIMETERS)
    pins.digital_write_pin(DigitalPin.P12, 1)
    pins.digital_write_pin(DigitalPin.P13, 0)
    pins.analog_write_pin(AnalogPin.P14, 195)
    if huskylens.reade_box(1, Content1.Y_CENTER) >= 40 and (huskylens.reade_box(1, Content1.X_CENTER) >= 40 and huskylens.reade_box(1, Content1.X_CENTER) <= 180):
        pins.digital_write_pin(DigitalPin.P9, 0)
        pins.digital_write_pin(DigitalPin.P11, 1)
    elif huskylens.reade_box(2, Content1.Y_CENTER) >= 40 and (huskylens.reade_box(2, Content1.X_CENTER) >= 40 and huskylens.reade_box(2, Content1.X_CENTER) <= 180):
        pins.digital_write_pin(DigitalPin.P9, 1)
        pins.digital_write_pin(DigitalPin.P11, 0)
    else:
        if sonar_Right > 150 or sonar_Right == 0:
            pins.digital_write_pin(DigitalPin.P9, 0)
            pins.digital_write_pin(DigitalPin.P11, 1)
        elif sonar_left > 150 or sonar_left == 0:
            pins.digital_write_pin(DigitalPin.P9, 1)
            pins.digital_write_pin(DigitalPin.P11, 0)
        elif sonar_left > 30 and sonar_Right > 30:
            pins.digital_write_pin(DigitalPin.P11, 0)
            pins.digital_write_pin(DigitalPin.P9, 0)
        elif sonar_left < 30 and sonar_left > 3:
            pins.digital_write_pin(DigitalPin.P9, 0)
            pins.digital_write_pin(DigitalPin.P11, 1)
            basic.pause(200)
            pins.digital_write_pin(DigitalPin.P9, 0)
            pins.digital_write_pin(DigitalPin.P11, 0)
        elif sonar_Right < 30 and sonar_Right > 3:
            pins.digital_write_pin(DigitalPin.P9, 1)
            pins.digital_write_pin(DigitalPin.P11, 0)
            basic.pause(200)
            pins.digital_write_pin(DigitalPin.P9, 0)
            pins.digital_write_pin(DigitalPin.P11, 0)
basic.forever(on_forever)
