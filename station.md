## Main Station

- Display "Hung Shui Kai Station"
- Operation
  - LED light on to make trigger `Train` LED light sensor to detect and trigger stop train action
  - Receive "arrived" message from `broadcast` to trigger a timer to stop LED light,
    - when LED off and the `Train` will start moving again
- 



```javascript
function showStation () {
    I2C_LCD1602.ShowString("Welcome to Hung", 16, 0)
    for (let index = 0; index < 16; index++) {
        I2C_LCD1602.shl()
        basic.pause(300)
    }
    I2C_LCD1602.ShowString(" Shui Kiu Station", 16, 0)
    for (let index = 0; index < 16; index++) {
        I2C_LCD1602.shl()
        basic.pause(300)
    }
    I2C_LCD1602.clear()
    I2C_LCD1602.ShowString("Welcome to Hung ", 0, 0)
    I2C_LCD1602.ShowString("Shui Kau Station", 0, 1)
}
input.onButtonPressed(Button.A, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.White))
})
input.onButtonPressed(Button.B, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.Black))
})
radio.onReceivedValue(function (name, value) {
    if (name == "arrived") {
        basic.pause(10000)
        strip.showColor(neopixel.colors(NeoPixelColors.Black))
        basic.pause(10000)
        strip.showColor(neopixel.colors(NeoPixelColors.White))
    }
})
let strip: neopixel.Strip = null
radio.setGroup(1)
basic.showIcon(IconNames.Yes)
I2C_LCD1602.LcdInit(39)
I2C_LCD1602.BacklightOn()
I2C_LCD1602.clear()
I2C_LCD1602.on()
strip = neopixel.create(DigitalPin.P14, 4, NeoPixelMode.RGB)
let enabled = 1
let arrived = 0
basic.forever(function () {
    showStation()
    basic.pause(3000)
})

```
