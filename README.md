
The following documentation is for the RLA-23-12 Actuator. Available for purchase [here](https://rboticlabs.com/products/RLA-23-12-Actuator-v2-p554194928). 

Its motor controller is a COTS dgm controller. Original firmware can be found [here](https://github.com/codenocold/dgm).

# Quick Start Guide
Follow these 3 steps to get your RLA-23-12 Actuator up and running as fast as possible!
1. **Hardware setup:**
   - Connect RLA-23-12's XT30 connector to a [Cando Pro Isolated CAN bus ](https://rboticlabs.com/products/Cando-Pro-Isolated-USB-to-CAN-p586100123) module. These connections handle the CAN bus high and low signals.
   - Connect RLA-23-12's XT60 connector to a power supply capable of delivering 12-50V and >30W. Please ensure proper polarity. The power required will largely depend on mechanical loading.

2. **Software Setup:**
   - Download the [DGM Tool debugging software](https://github.com/Rbotic/RLA-23-12/blob/main/dgm%20tool/dgm_tool-en-x64-0.3.exe). For Windows 8 and above. The Cando module is required to use the Dgm Tool debugging software.

   - Change the Baudrate to 1000K in the DGM Tool software and click connect.
   ![Baudrate Configuration](https://github.com/Rbotic/RLA-23-12/blob/main/media/baudrate%20and%20connect.png)

4. **Motor Control:**
   - Click on "Enable Motor" and send a position command in turns. The motor will rotate to the requested position. The actuator's output will rotate by `(turns / 21.302)`. The exact reduction ratio is `(60/13)^2 ≈ 21.3018`
   ![Motor Control](https://github.com/Rbotic/RLA-23-12/blob/main/media/enable%20and%20pos%20motor.png)

   - Play around with it! Experiment with live plotting of variables such as position or motor phase current.

### **Important Notes**

- **Power Supply Considerations:** Ensure that your power supply can handle sourcing power (when accelerating) and sinking power (when decelerating). This is especially critical when rapidly moving substantial masses. A Li-ion battery is recommended if you have doubts about power supply. By default, undervoltage/overvoltage protection will trip at 12/44v respectively. This can be changed through CAN bus using the Dgm Tool.

 - RLA-23-12 uses a DGM motor controller with custom firmware enchancing usability and CAN bus performance. Avoid reflashing original firmware.

# What's Next?
## CAN bus
If you intend to use the actuator without the Cando Pro module, you will need to generate the CAN bus messages using an external module of your choice. Refer to the [CAN bus documentation](CAN_BUS.md) for guidance.

## CAD
CAD can be found [here](https://github.com/Rbotic/RLA-23-12/blob/main/CAD/Actuator%20T23R12%20v2%20External%20CAD.step) for mechanical integration.

## Internal control loop
Below is the control loop running internally in the motor controller. In depth understanding of this control loop is not required for most applications. It is useful to understand for determining the expected torque output for a given actuator state.
![Alt text](https://github.com/Rbotic/RLA-23-12/blob/main/media/control%20loop.png)

Source: [link](https://github.com/codenocold/dgm/blob/main/dgm_v2_0/dgm%E7%94%B5%E6%9C%BA%E9%A9%B1%E5%8A%A8%E5%99%A8%E7%94%A8%E6%88%B7%E6%89%8B%E5%86%8C_%E7%89%88%E6%9C%ACC.pdf)

# Contact
Should you have any inquiries or require further assistance, please don't hesitate to reach out. rboticlabs@gmail.com