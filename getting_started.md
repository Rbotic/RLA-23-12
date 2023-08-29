# CAN Frame Format

Communication between the DGM drive node and a Master node employs CAN bus standard data frames. This is based on the COTS DGM controller.

## CAN Frame Structure

### CAN ID (11 bits)

The CAN ID is an 11-bit field that carries information about the message's source, destination, and purpose. In the context of communication between the DGM drive node and a Master node:

- Bits 0 to 5: Command ID (6 bits)
- Bits 6 to 10: Node ID (5 bits, ranging from 1 to 31)

The CAN ID is calculated using the formula:

```
can_id = (node_id << 6) | cmd_id
```

### CAN DLC (4 bits)

The Data Length Code (DLC) field is 4 bits long and indicates the number of data bytes present in the CAN DATA field. The count of bytes varies based on the specific command being sent.

### CAN DATA (8 bytes)

The CAN DATA field contains data specific to the command. The content of this field varies depending on the command being transmitted. Three data types can be used for the data payload:

- int32: A 4-byte signed integer in little-endian format.
- uint32: A 4-byte unsigned integer in little-endian format.
- float32: A 4-byte floating-point number in little-endian format.

An external Master node initiates communication and receives responses from the DGM drive (slave). Certain commands are also initiated by the DGM drive itself and sent to the user controller. These proactive commands are used to report internal errors or status changes and are sent using the `CAN_CMD_STATUSWORD_REPORT` command.

## Data Types Sent/Received

There are three data types that can be sent or received within the CAN DATA field:

- `int32`: A signed 4-byte integer stored in little-endian format.
- `uint32`: An unsigned 4-byte integer stored in little-endian format.
- `float32`: A 4-byte floating-point number stored in little-endian format.

Please note that the endianness (byte order) of the data is specified as little-endian.

## Example

**Aim:** To set the position of a motor to 3.456 turns, for an axis with a node ID of 14.

Using the table below, the CAN bus message would be structured as follows:

- **CAN ID:** `node_id << 6 | cmd_id = 14 << 6 | 4`
- **CAN DLC:** Set to 4 bytes (size of sent CAN DATA)
- **CAN DATA:** Convert 3.456 to float32 (4 bytes, little-endian)
- **RTR Bit:** Set to 0 (no reply expected)

## CAN Command Messages

- **RTR bit:** When a command requires a return reply, the CAN bus RTR (Remote Transmission Request) bit must be set in the CAN frame.
- **Execution result:** 0 indicates success, -1 indicates failure

| CMD ID | CMD Name                   | Description                                     | Sent Node ID            | Sent CMD ID | Sent CAN DLC | Sent Data                         | Return Node ID | Return CMD ID | Return CAN DLC | Return Data                                      |
|-------|---------------------------|-------------------------------------------------|-------------------------|-------------|--------------|----------------------------------|----------------|---------------|----------------|--------------------------------------------------|
| 0     | CAN_CMD_MOTOR_DISABLE     | Stop motor and enter idle mode                  | User Specified Motor ID | 0           | 0            | -                                | Motor ID       | 0             | 4              | Execution Result (int32): 0 or -1                |
| 1     | CAN_CMD_MOTOR_ENABLE      | Start motor and enter active mode               | User Specified Motor ID | 1           | 0            | -                                | Motor ID       | 1             | 4              | Execution Result (int32): 0 or -1                |
| 2     | CAN_CMD_SET_TORQUE        | Set motor target torque                        | User Specified Motor ID | 2           | 4            | Target Torque (Nm) (float32)      | -              | -             | -              | -                                                |
| 3     | CAN_CMD_SET_VELOCITY      | Set motor target velocity                      | User Specified Motor ID | 3           | 4            | Target Velocity (rev/s) (float32) | -              | -             | -              | -                                                |
| 4     | CAN_CMD_SET_POSITION      | Set motor target position                      | User Specified Motor ID | 4           | 4            | Target Position (turns) (float32) | -              | -             | -              | -                                                |
| 5     | CAN_CMD_SYNC              | Motion control command sync signal            | User Specified Motor ID | 5           | 0            | -                                | -              | -             | -              | -                                                |
| 6     | CAN_CMD_RESERVED          | RESERVED                                      | User Specified Motor ID | 6           | 4            | -                                | Motor ID       | -             | 8              | -                                                |
| 12    | CAN_CMD_SET_HOME          | Set current position as 0                     | User Specified Motor ID | 12          | 0            | -                                | Motor ID       | 12            | 4              | Execution Result (int32): 0 or -1                |
| 13    | CAN_CMD_ERROR_RESET       | Clear error codes                              | User Specified Motor ID | 13          | 0            | -                                | Motor ID       | 13            | 4              | Execution Result (int32): 0 or -1                |
| 14    | CAN_CMD_GET_STATUSWORD    | Get current status and error info             | User Specified Motor ID | 14          | 0            | -                                | Motor ID       | 14            | 8              | Status Info (uint32), Error Info (uint32)        |
| 15    | CAN_CMD_STATUSWORD_REPORT | Actively reports internal status and error info | Motor ID                | 15          | 8            | Status Info (uint32), Error Info (uint32) | -              | -             | -              | -                                                |
| 16    | CAN_CMD_GET_TORQUE        | Get current motor actual torque               | User Specified Motor ID | 16          | 0            | -                                | Motor ID       | 16            | 4              | Actual Torque (Nm) (float32)                    |
| 17    | CAN_CMD_GET_VELOCITY      | Get current motor actual velocity             | User Specified Motor ID | 17          | 0            | -                                | Motor ID       | 17            | 4              | Actual Velocity (rev/s) (float32)              |
| 18    | CAN_CMD_GET_POSITION      | Get current motor actual position             | User Specified Motor ID | 18          | 0            | -                                | Motor ID       | 18            | 4              | Actual Position (turns) (float32)              |
| 19    | CAN_CMD_GET_I_Q           | Get current motor current                      | User Specified Motor ID | 19          | 0            | -                                | Motor ID       | 19            | 4              | Motor Current (A) (float32)                    |
| 20    | CAN_CMD_GET_VBUS          | Get current bus voltage                        | User Specified Motor ID | 20          | 0            | -                                | Motor ID       | 20            | 4              | Bus Voltage (V) (float32)                      |
| 21    | CAN_CMD_GET_IBUS          | Get current bus current                        | User Specified Motor ID | 21          | 0            | -                                | Motor ID       | 21            | 4              | Bus Current (A) (float32)                     |
| 22    | CAN_CMD_GET_POWER         | Get current motor power                        | User Specified Motor ID | 22          | 0            | -                                | Motor ID       | 22            | 4              | Motor Power (W) (float32)                      |
| 63    | CAN_CMD_HEARTBEAT         | Heartbeat command for communication monitoring | User Specified Motor ID | 63          | 0            | -                                | Motor ID       | 63            | 0              | -                                                |


