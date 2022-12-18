# CoreBluetooth
- iOS Framework provided by Apple to build Bluetooth Low Energy applications that communicate with hardware gadgets
- Major players: Central - Peripheral
- Peripheral advertises to let other devices know, or broadcasts small data in the form of advertising packets
- Central discovers Peripheral’s advertising and sends a connection request
- NSBluetoothAlwaysUsageDescription Usage description key is required

## Communication Workflow
<img src="https://user-images.githubusercontent.com/38341386/208288690-50c8083f-25c4-451f-8119-61de226c90c8.png" width = "70%">

## Main Classes and Protocol
- CBCentralManager, CBCentralManagerDelegate
- CBPeripheral, CBPeripheralDelegate
- CBService: services of a remote peripheral. Services are either primary or secondary and may contain multiple characteristics or included services
- CBCharacters: represent further information about a peripheral’s service. Here is where we can read, write, and subscribe to the data from the device (ex: battery level, temperature, LED light)


## 출처
- [Core Bluetooth (BLE)- Swift](https://medium.com/macoclock/core-bluetooth-ble-swift-d2e7b84ea98e)
- [Core Bluetooth documentation](https://developer.apple.com/documentation/corebluetooth)