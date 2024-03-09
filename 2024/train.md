# Train

```js
function showStation () {
    OledKitten.drawText2X(0, 0, "Hung Shui")
    OledKitten.drawText2X(0, 1, "Kiu Station")
}
input.onButtonPressed(Button.A, function () {
    lightThreshold = lightThreshold + 50
    basic.showNumber(lightThreshold)
    basic.pause(1000)
})
function trainStop () {
    robotbit.MotorStop(robotbit.Motors.M1A)
    OledKitten.clear()
    OledKitten.drawText2X(0, 0, "Arrived")
    basic.pause(1000)
    OledKitten.clear()
    ledCountDown(5)
    showStation()
}
function trainStart () {
    robotbit.MotorRun(robotbit.Motors.M1A, 255)
    OledKitten.clear()
    OledKitten.drawText2X(0, 0, "Departing..")
    basic.pause(1500)
    OledKitten.clear()
    showStation()
}
input.onButtonPressed(Button.AB, function () {
    if (isEnabled == 0) {
        isEnabled = 1
        trainStart()
        basic.showIcon(IconNames.Yes)
    } else {
        isEnabled = 0
        trainStop()
        basic.showIcon(IconNames.No)
    }
})
function detechLightLv () {
    OledKitten.drawText2X(0, 0, convertToText(Sugar.Light(AnalogPin.P0)))
    basic.pause(1000)
    OledKitten.clear()
}
input.onButtonPressed(Button.B, function () {
    lightThreshold = lightThreshold - 50
    basic.showNumber(lightThreshold)
    basic.pause(1000)
})
function ledCountDown (num: number) {
    localCount = num
    for (let index = 0; index < num; index++) {
        OledKitten.drawText2X(0, 0, convertToText(localCount))
        basic.pause(1000)
        OledKitten.clear()
        localCount = localCount - 1
    }
    OledKitten.drawText2X(0, 0, "Hung Shui")
}
let localCount = 0
let lightThreshold = 0
let isEnabled = 0
radio.setGroup(1)
let strip = robotbit.rgb()
isEnabled = 1
let currentValue = 1
let toStop = 1
lightThreshold = 750
strip.showColor(neopixel.colors(NeoPixelColors.Black))
strip.setBrightness(10)
OledKitten.oledInit(OledKitten.OledType.oled12864)
basic.forever(function () {
    if (Sugar.Light(AnalogPin.P0) >= lightThreshold) {
        toStop = 1
        basic.showIcon(IconNames.No)
    } else {
        toStop = 0
        basic.showIcon(IconNames.Square)
    }
    if (isEnabled == 1) {
        if (currentValue != toStop) {
            currentValue = toStop
            radio.sendValue("arrived", toStop)
            if (toStop == 1) {
                trainStop()
            } else {
                trainStart()
            }
        }
    }
    basic.pause(300)
})

```