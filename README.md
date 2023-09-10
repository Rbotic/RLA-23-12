
The following documentation is for the RLA-23-12 Actuator. Available for purchase [here](https://rboticlabs.com/products/RLA-23-12-Actuator-v2-p554194928). 

Its motor controller is a COTS dgm controller. Original firmware can be found [here](https://github.com/codenocold/dgm).

# Quick Start Guide
Follow these 3 steps to get your RLA-23-12 Actuator up and running as fast as possible!
1. **Hardware setup:**
   - Connect RLA-23-12's XT30 connector to a [Cando Pro Isolated CAN bus ](https://rboticlabs.com/products/Cando-Pro-Isolated-USB-to-CAN-p586100123) module. These connections handle the CAN bus high and low signals.
   - Connect RLA-23-12's XT60 connector to a power supply capable of delivering 12-50V and >30W. Please ensure proper polarity. The power required will largely depend on mechanical loading.

2. **Software Setup:**
   - Download the [DGM Tool debugging software](https://github.com/Rbotic/RLA-23-12/blob/main/dgm%20tool/dgm_tool-en-x64-0.3.exe). For Windows 8 and above.
    <img src="https://github.com/Rbotic/RLA-23-12/blob/main/media/dgm%20tool.png" alt="DGM Tool" width="400">

   - Change the Baudrate to 1000K in the DGM Tool software and click connect.
   ![Baudrate Configuration](https://github.com/Rbotic/RLA-23-12/blob/main/media/baudrate%20and%20connect.png)

4. **Motor Control:**
   - Click on "Enable Motor" and send a position command in turns. The motor will rotate to the requested position. The actuator's output will rotate by $ (turns / 21.302) $. The exact reduction ratio is $ (60/13)^2 ≈ 21.3018 $

   ![Motor Control](https://github.com/Rbotic/RLA-23-12/blob/main/media/enable%20and%20pos%20motor.png)

   - Play around with it! Experiment with live plotting of variables such as position or motor phase current.

## **Important Notes**

- **Power Supply Considerations:** Ensure that your power supply can handle sourcing power (when accelerating) and sinking power (when decelerating). This is especially critical when rapidly moving substantial masses. A Li-ion battery is recommended if you have doubts about power supply.

 - RLA-23-12 uses a DGM motor controller with custom firmware enchancing usability and CAN bus performance. Refrain from reflashing with original firmware.

# What's Next?
## CAN bus
If you intend to use the actuator without the Cando Pro module, you will need to generate the CAN bus messages using an external module of your choice. Refer to the [CAN bus documentation](CAN_BUS.md) for guidance.

## CAD
CAD can be found [here](https://github.com/Rbotic/RLA-23-12/blob/main/CAD/Actuator%20T23R12%20v2%20External%20CAD.step) for mechanical integration.
