```js
input.onButtonPressed(Button.A, function () {
    isOpen = 1
    handleOpen()
})
function handleOpen () {
    if (isOpen == 0) {
        Kitronik_ACCESSbit.barrierControl(Kitronik_ACCESSbit.BarrierPosition.Down)
    } else {
        Kitronik_ACCESSbit.barrierControl(Kitronik_ACCESSbit.BarrierPosition.Up)
    }
}
input.onButtonPressed(Button.B, function () {
    isOpen = 0
    handleOpen()
})
radio.onReceivedValue(function (name, value) {
    if (name == "isOpen") {
        if (isOpen != value) {
            isOpen = value
            handleOpen()
        }
    }
})
let isOpen = 0
radio.setGroup(1)
isOpen = 0
basic.forever(function () {
	
})

```