---
PG_TITLE: How to use the DeviceSourceManager
---

# Introduction
The [DeviceSourceManager](https://doc.babylonjs.com/api/classes/babylon.devicesourcemanager) is a class that will manage the connections for various user input devices and provide methods of querying those devices for their current state.  
This class supports several methods of input:
- **Keyboard** *(DeviceType: BABYLON.DeviceType.Keyboard, Inputs: number)*
- **Mouse** *(DeviceType: BABYLON.DeviceType.Mouse, Inputs: BABYLON.PointerInput)*
- **Touch** *(DeviceType: BABYLON.DeviceType.Pointer, Inputs: BABYLON.PointerInput)*
- **DualShock Gamepad** *(DeviceType: BABYLON.DeviceType.DualShock, Inputs: BABYLON.DualShockInput)*
- **Xbox Gamepad, One or 360** *(DeviceType: BABYLON.DeviceType.Xbox, Inputs: BABYLON.XboxInput)*
- **Switch Gamepad, L+R JoyCon Grip or Pro Controller** *(DeviceType: BABYLON.DeviceType.Switch, Inputs: BABYLON.SwitchInput)*
- **Generic/Other Gamepad** *(DeviceType: BABYLON.DeviceType.Generic, Inputs: number)*

Here's an example of the DeviceSourceManager in use
* [Playground Example](https://playground.babylonjs.com/#C7PM2B)

To use the DeviceSourceManager, first create an instance of it.  You will need to provide an engine object.
```javascript
var deviceSourceManager = new BABYLON.DeviceSourceManager(scene.getEngine());
```

Within your scene's render/game loop, you can query the DeviceSourceManager for the current state of a specific input.  First, you will need to get the DeviceSource object.  With this object, you can then query for a specific input's status.

```javascript
// If the device has been registered in the DeviceSourceManager
if (deviceSourceManager.getDeviceSource(BABYLON.DeviceType.Xbox)) {
    // And the A button was pressed
    if (deviceSourceManager.getDeviceSource(BABYLON.DeviceType.Xbox).getInput(BABYLON.XboxInput.A) == 1) {
        // Do something
    }
}
```

It should also be noted that you can use optional chaining to make checks fit into a single line
```javascript
if (deviceSourceManager.getDeviceSource(BABYLON.DeviceType.Xbox)?.getInput(BABYLON.XboxInput.A) == 1) {
    // Do something
}
```

# Events and Observables
You can use the following Observables to work with identifiers for a given device
```javascript
// Before a device is registered
onBeforeDeviceConnectedObservable.add((device) => {
    // You can get the device type by using device.deviceType
    // You can also get the device slot (only applicable to gamepads and touch) by using device.deviceSlot
});

onBeforeDeviceDisconnectedObservable.add((device) => {
    // You can get the device type by using device.deviceType
    // You can also get the device slot (only applicable to gamepads and touch) by using device.deviceSlot
});

// After a device is registered
onAfterDeviceConnectedObservable.add((device) => {
    // You can get the device type by using device.deviceType
    // You can also get the device slot (only applicable to gamepads and touch) by using device.deviceSlot
});

onAfterDeviceDisconnectedObservable.add((device) => {
    // You can get the device type by using device.deviceType
    // You can also get the device slot (only applicable to gamepads and touch) by using device.deviceSlot
});
```

For Keyboards and Pointers, you can use an event based system to get the current input and previous input when an input is activated
```javascript
deviceSourceManager.getDeviceSource(BABYLON.DeviceType.Keyboard).onInputChangedObservable.add((device) => {
    // device.inputIndex is the activated input
    // device.currentState is the current value
    // device.previousState is the previous value (before activation)
});
```