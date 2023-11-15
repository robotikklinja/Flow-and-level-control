# Flow-and-level-control
PLC Flow and level control station
![image](https://github.com/robotikklinja/Flow-and-level-control/assets/3476653/9b3a8010-0771-45f3-a321-1a8b8c826639)
[Fusion360 Model](https://a360.co/3MrRBDC)



[3D models](3d_prints/)


[Water-connectors in PDF](rorkoblinger.drawio.pdf) and [Draw.io source file](pipe_connection_sizes.drawio)

## BOM control loop electronics
* [24V 3/4" motorized ball valve, aliexpress](https://www.aliexpress.com/item/1005003214000529.html)
* [24V 3/4" solenoid valve, aliexpress](https://www.aliexpress.com/item/32877075180.html)
* [24V 1/2" pump, aliexpress)[https://www.aliexpress.com/item/1005005034206009.html)
* [24V 4-20mA Liquid level sensor, aliexpress](https://www.aliexpress.com/item/1005004309828489.html)
* [24V Flow sensor, hall effect turbine, aliexpress](https://www.aliexpress.com/item/4001086173229.html)
* [24V SICK Capacitive sensor, CM18-12NPP-KC1](https://cdn.sickcn.com/media/pdf/7/47/247/dataSheet_CM18-12NPP-KC1_6020410_en.pdf)
* [PLC Siemens Logo! 8 6ED1052-1MD08-0BA1](https://mall.industry.siemens.com/mall/en/WW/Catalog/Product/6ED1052-1MD08-0BA1)


## Improvements

### Limit switch feedback
The motorized ball valve has built in end stops but does not signal the PLC when they are triggered. 
A circuit can be made that detects if theres is a current flowing through the motor.

[Here is an online simulation of the circuit in Falstad.](https://www.falstad.com/circuit/circuitjs.html?ctz=CQAgjCAMB0l3BWEBmaBOMAmLAOZa0cwFltkQAWczEBSWgUwFowwAoAdxU3p52945+7LqXpgA7ADYBIHPWRsALiEwSaYNDTHgtUEC1WoCJomjpUJEgzHhwwkZMgkIpPc5mSQ3FSiAAmDABmAIYArgA2SmwASuAUvvLxvhT89PQUvL7pUNAIbJLaPCjIMjqlaSDWvPrpeSAAwgD2EREMAMZKTQBOIABqLUohAOYMbL06mEKyTjLiUvBs-sly4gm6NDSBoZHRAM4rmK4rmjTiIKERe2NcYOuzM6XgnCd6mJi+pygvajRJd4l0i9JtNMoI0i8wapQalofxMONVB8Nqp1CjxHBEQDVMdsQ8smwAB4gKRoFCOcBTckyT6+AAiDQAOnsALZNLrdIklJBTBRoGRTcifegAZQAlsMAHYhCLMrrMgAKABkGly0e9rFhqF4-A4QEqxSyxUpmXsOMb2gALRG-VaojR6eh0RZcW1JHRJSBsIA)

### 4-20mA to 2-10V

The liquid level sensor emitts a 4-20mA current signal, we need to convert it to a voltage signal that the PLC can detect. We wil use a 500 Ohm pull down resistor attached to the PLC input pin to do this. 

### Fast digital input for flow meter pulses

The flow sensor will emitt pulses with potentially "high frequence" so we need to not have the PLC miss any. From the PLC manual:

"Digital inputs Digital inputs begin with the letter I. The number of the digital inputs (I1, I2, ...) corresponds to the number of the input connectors of the LOGO! Base Module and of the connected digital modules, in the order of their installation. You can use the fast digital LOGO! functions 4.1 Constants and connectors LOGO! System Manual, 7/2022, A5E33039675-AK 125 inputs I3, I4, I5, and I6 of the LOGO! versions LOGO! 12/24 RCE, LOGO! 12/24 RCEo, LOGO! 24 CE and LOGO! 24 CEo as fast counters. Note To avoid that the LOGO! Base Module fails to read input signals because its built-in MCU (Microcontroller Unit) is too sensitive and runs much faster than those in previous LOGO! devices, an on-/off-delay function is designed for LOGO!: • For LOGO! 230RCE and LOGO! 230RCEo, a 25 ms on-delay time and a 20 ms off-delay time are defined for digital inputs I1 to I8. • For all the other LOGO! versions, a 5 ms on-delay time and a 5 ms off-delay time are defined for all the digital inputs. Besides, when the LOGO! Base Module is in slave mode, a 5 ms on-delay time and a 100 ms signal-retentive-time are defined for all the digital inputs."

So we will use digital imputs I3-I6 for the flow sensor. And check the manual for how to use: "...the fast digital LOGO! functions 4.1 Constants and connectors LOGO! System Manual, 7/2022, A5E33039675-AK 125 "

