```javascript
input.onButtonPressed(Button.A, function () {
    isMove = 1
    stop = 0
})
input.onButtonPressed(Button.B, function () {
    isMove = 0
    stop = 1
    motor.motorStop(motor.Motors.M1)
})
radio.onReceivedValue(function (name, value) {
    if (name == "enabled") {
        enabled = value
        if (enabled == 0) {
            stop = 1
        } else {
            stop = 0
        }
    }
})
let stop = 0
let isMove = 0
let enabled = 0
basic.showLeds(`
    # # # # #
    # # # # #
    # # # # #
    # # # # #
    # # # # #
    `)
enabled = 1
isMove = 0
let speed = 150
stop = 0
basic.forever(function () {
    if (stop == 0) {
        if (isMove == 1) {
            motor.MotorRun(motor.Motors.M1, motor.Dir.CW, speed)
        } else {
            motor.motorStop(motor.Motors.M1)
        }
    }
    if (input.lightLevel() >= 80) {
        isMove = 0
        radio.sendValue("arrived", 1)
        motor.motorStop(motor.Motors.M1)
        basic.pause(8000)
    } else {
        isMove = 1
    }
    led.setBrightness(input.lightLevel())
    basic.pause(500)
})

```