## Project Principle
1. Using Hand Gestures to control music
2. Customizable Hand Gesture Actions
3. Using Bluetooth 

## Implementation
1. Use Accelerometer and Gyroscope sensor to detect Gestures
2. Use that to implement an action that is Controlling Music
3. We do that by using Arduino Uno or Nano Board
4. Then send that to a Bluetooth speaker or mobile via Bluetooth Sensor

## Sensors
### List of Sensors Used:
1. **MPU 6050**, an Accelerometer and Gyroscope Sensor
2. **HC05**, a Bluetooth module for Arduino
3. **TPU 4056**, Battery Management System for Li-Po and Li-ion batteries
### MPU 6050
1. This package consists of Accelerometer and also Gyroscope Devices
2. It consists of 6 pins, they are
	- VCC 3.3 to 5v
	- GND for ground
	- SCL
	- SDA
	- XDA
	- XCL
	- AD0
	- INT
3. Out of these pins,  we use vcc, gnd and scl, sda for i2c commuication with arduino
4. it has 3+3 axis for Accelerometer and gyroscope 
5. We get the data using i2c pins and using getRotation and getAcceleration functions from mpu6050 library
### HC05
1. It acts as a transparent Bluetooth device for arduino using uart communication
2. it has 6 pins
	- state
	- vcc
	- tx
	- rx
	- gnd 
	- en
3. we use tx,rx as uart communication and vcc and gnd for power
4. we connect them to any 2 pins and use software serial library
5. and then we can use serial read and write to send and receive data

### TPU 4056
1. this is a bms module for 1s 1A li-po and li-ion batteries
2. this has usb-micro b or usb-c port for charging the battery
3. it also has safety features like
	1. over charge protection
	2. over discharge
	3. under and over currents
4. We solder the Battery terminals to B+ and B-, and solder wires to P+ and P- terminals
5. it has a potentiometer to control the charging current


# Communication Protocols: UART and I2C

## UART (Universal Asynchronous Receiver/Transmitter)

| Feature          | Description                                      |
|------------------|--------------------------------------------------|
| **Bus Type**     | Two-wire (TX - Transmit, RX - Receive)           |
| **Topology**     | Point-to-point                                   |
| **Addressing**   | No                                               |
| **Speed**        | Configurable baud rates (e.g., 9600, 115200 bps) |
| **Usage**        | Serial communication, GPS modules, Bluetooth     |
| **Advantages**   | Simple, widely supported, no clock needed        |
| **Disadvantages**| Typically only two devices, speed limitations    |

### UART Pinout

| Pin | Description   |
|-----|---------------|
| TX  | Transmit data |
| RX  | Receive data  |

## I2C (Inter-Integrated Circuit)

| Feature          | Description                                      |
|------------------|--------------------------------------------------|
| **Bus Type**     | Two-wire (SDA - Serial Data, SCL - Serial Clock) |
| **Topology**     | Multi-master, multi-slave                        |
| **Addressing**   | Yes (7-bit or 10-bit)                            |
| **Speed**        | Standard mode (100 kbps), Fast mode (400 kbps), High-speed mode (3.4 Mbps) |
| **Usage**        | Sensors, EEPROMs, displays                       |
| **Advantages**   | Simple wiring, multiple devices                  |
| **Disadvantages**| Slower, address conflicts                        |

### I2C Pinout

| Pin | Description  |
| --- | ------------ |
| SDA | Serial Data  |
| SCL | Serial Clock |
