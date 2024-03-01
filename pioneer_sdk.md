# pioneer_sdk Documentation
pioneer_sdk is a Python 3 module for programming Geoscan Pioneer with the Pizero module (or Geoscan Pioneer Mini).

## Installation
Run in terminal:
```shell
pip3 install pioneer-sdk
```

## API Docs
### Class Pioneer
#### Initialization
```python
Pioneer(name='pioneer', ip='192.168.4.1', mavlink_port=8001, connection_method='udpout', device='/dev/serial0', baud=115200, logger=True, log_connection=True)
```
##### Params:
* `name` - name of board
* `ip` - drone IP-address (only if `connection_method` is "udpout" or "udpin")
* `mavlink_port` - MAVLink messaging port (only if `connection_method` is "udpout" or "udpin")
* `connection_method` - MAVLink connection method (accepts "udpin", "udpout", "serial")
* `device` - serial port address (only if `connection_method` is "serial")
* `baud` - serial port baudrate (only if `connection_method` is "serial")
* `logger` - MAVLink message logs. True - output to the console; False – do not display
* `log_connection` - connection logs. True — output to the console; Lie - do not deduce
#### Pioneer class methods
```python
connected()
```
##### Result:
Returns False if the connection is lost (no messages from the drone within a second), returns True if the connection is stable.
<br>
<br>
```python
close_connection()
```
##### Result:
Closes the MAVLink connection to the drone.
<br>
<br>
```python
reboot_board()
```
##### Result:
Rebooting the drone. Returns True if the result is "ACCEPTED" or "DENIED";
Returns False if the result is "SEND_TIMEOUT", "TEMPORARILY_REJECTED", "UNSUPPORTED", "FAILED", "CANCELLED"
<br>
<br>
```python
set_logger(value=True)
```
##### Params:
`value` – принимает значения True или False
##### Result:
Setting the `logger` flag to True or False.
<br>
<br>
```python
set_log_connection(value=True)
```
##### Params:
`value` – принимает значения True или False
##### Result:
Установка флага `log_connection` в True или False.
<br>
<br>
```python
arm()
```
##### Result:
It turns on the quadcopter motors. Returns True if the result is "ACCEPTED" or "DENIED"; Returns False if the result is "SEND_TIMEOUT", "TEMPORARILY_REJECTED", "UNSUPPORTED", "FAILED", "CANCELLED";
<br>
<br>
```python
disarm()
```
##### Result:
Turn off the quadcopter motors. Returns True if the result is "ACCEPTED" or "DENIED"; Returns False if the result is "SEND_TIMEOUT", "TEMPORARILY_REJECTED", "UNSUPPORTED", "FAILED", "CANCELLED";
<br>
<br>
```python
takeoff()
```
##### Result:
Takeoff to the height of takeoffAlt. Returns True if the result is "ACCEPTED" or "DENIED"; Returns False if the result is "SEND_TIMEOUT", "TEMPORARILY_REJECTED", "UNSUPPORTED", "FAILED", "CANCELLED";
##### Note:
There are no expicit arguments. The takeoff height is set by the Flight_com_takeoffAlt=x autopilot parameter, where the x-height of the takeoff is in meters.
<br>
<br>
```python
land()
```
##### Result:
Выполняет команду на посадку. Возвращает True, если результат „ACCEPTED“ или „DENIED”; Возвращает False, если результат „SEND_TIMEOUT“, „TEMPORARILY_REJECTED”, „UNSUPPORTED“, „FAILED“, „CANCELLED”
<br>
<br>
```python
led_control(led_id=255, r=0, g=0, b=0)
```
###### Params:
* `led_id` - номер светодиода для управления. (255 - все светодиоды; 0-3 - светодиоды от 1 до 4).
* `r`, `g`, `b` - каналы по управлению красным зелёным и синим свечением светодиода 0-255 - интенсивность соответствующего канала.
###### Result:
Turn on the LEDs. Returns True if the result is "ACCEPTED" or "DENIED";
Returns False if the result is “SEND_TIMEOUT”, “TEMPORARILY_REJECTED”, “UNSUPPORTED”, “FAILED”, “CANCELLED”;
<br>
<br>
```python
go_to_local_point(x=None, y=None, z=None, yaw=None)
```
##### Params:
* `x`, `y`, `z` - координаты точки, в метрах.
* `yaw` - the yaw angle is specified in radians
##### Result:
Sending the flight command to the point. The coordinates are specified in the local coordinate system; Returns True if the command was sent successfully, False if the command failed to send or refused.
<br>
<br>
```python
go_to_local_point_body_fixed(x, y, z, yaw)
```
##### Params:
* `x`, `y`, `z` - coordinates of the point, in meters.
* `yaw` - the yaw angle is specified in radians
##### Result:
Sending the flight command to the point. The coordinates are specified in the coordinate system of the drone; Returns True if the command was sent successfully, False if the command failed to send or refused.
<br>
<br>
```python
set_manual_speed(vx, vy, vz, yaw_rate)
```
##### Params:
* `vx`, `vy`, `vz` - speed in m/s.
* `yaw_rate` - speed rad/s
##### Result:
Sending the flight command at the specified speed. The coordinates are specified in the local coordinate system;
Returns True if the command is sent successfully, False - if it was not possible to send or a refusal is received;
The `set_manual_speed` command should be sent not once, but constantly, while you need to fly at the specified speed!
<br>
<br>
```python
set_manual_speed_body_fixed(vx, vy, vz, yaw_rate)
```
##### Params:
* `vx`, `vy`, `vz` - speed in m/s.
* `yaw_rate` - speed rad/s
##### Result:
Sending the flight command at the specified speed. The coordinates are specified in the coordinate system of the drone;
Returns True if the command is sent successfully, False - if it was not possible to send or a refusal is received;
The `set_manual_speed_body_fixed` command should be sent not once, but constantly, while you need to fly at the specified speed.
<br>
<br>
```python
point_reached()
```
##### Result:
Returns the current state of the flag (True/False). The flag is set to True regularly when a new point is reached, and is reset to False after each call to the `point_reached` function and after sending `go_to_local_point` or `go_to_local_point_body_fixed`.
<br>
<br>
```python
get_local_position_lps(get_last_received=False)
```
##### Params:
`get_last_received` - if the argument get_last_received=True, it returns the values of [x, y, z] from the last message received;
Returns None if there were no messages with coordinates from the drone.
##### Result:
Array `[x, y, z]` with current coordinates in the local frame. Returns None if there is no new up-to-date data
<br>
<br>
```python
get_dist_sensor_data(get_last_received=False)
```
##### Params:
`get_last_received` - if the argument get_last_received=True, it returns data from the rangefinder from the last message;
Returns None if there were no messages with the rangefinder readings from the drone.
##### Result:
Returns the latest data from the rangefinder (in meters). Returns None if there is no new up-to-date data.
<br>
<br>
```python
get_battery_status(get_last_received=False)
```
##### Params:
`get_last_received` - if the argument get_last_received=True, it returns the battery voltage from the last message that came;
Returns None if there were no battery status messages from the drone.
##### Result:
Returns the current battery voltage. Returns None if there is no new up-to-date data.
