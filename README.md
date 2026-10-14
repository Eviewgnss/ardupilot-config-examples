# ArduPilot configuration examples for Eview GNSS receivers

Starting-point ArduPilot parameters for Eview RTK and dual-antenna heading receivers.
All Eview receivers are built on **Septentrio** modules and use ArduPilot's built-in
Septentrio SBF driver – no custom firmware needed.

| Eview receiver | Septentrio module | Use | Example file |
|---|---|---|---|
| [HB6](https://gnss-solutions.com/product/hb6-gnss-receiver/) | mosaic-X5 | Single-antenna RTK, up to 100 Hz | `hb6-single-antenna.param` |
| [HB52](https://gnss-solutions.com/product/hb52-gnss-receiver/) / [HBEV322](https://gnss-solutions.com/product/hbev322-rtk-smart-antenna/) | mosaic-G5 | Light single-antenna RTK | `hb6-single-antenna.param` (same settings) |
| [HB6 Pro](https://gnss-solutions.com/product/hb6-pro-gnss-receiver/) | mosaic-H | Dual-antenna RTK + GPS yaw | `dual-antenna-heading.param` |
| [HB10](https://gnss-solutions.com/product/hb10-gnss-receiver/) | AsteRx-m3 Pro+ | Dual-antenna heading, up to 100 Hz | `dual-antenna-heading.param` |

## How to use

1. Connect the receiver's COM port to a flight controller serial port (the examples use **SERIAL3 / GPS1**).
2. In Mission Planner or QGroundControl, load the matching `.param` file (or set the values by hand).
3. Reboot the flight controller and check the GPS status reaches *RTK Fixed* once corrections arrive.

## Key parameters (ArduPilot 4.5+ naming)

| Parameter | Value | Meaning |
|---|---|---|
| `SERIAL3_PROTOCOL` | 5 | GPS |
| `SERIAL3_BAUD` | 115 | 115200 baud (the driver can auto-negotiate) |
| `GPS1_TYPE` | 10 | Septentrio SBF (single antenna) |
| `GPS1_TYPE` | 26 | Septentrio SBF dual antenna (GPS-for-yaw) |
| `GPS1_RATE_MS` | 100 | 10 Hz weighting when blending GPS units |
| `GPS_INJECT_TO` | 0 | Send RTCM corrections to the first GPS |
| `GPS1_MB_TYPE` | 1 | Dual antenna: offsets relative to a custom base |
| `GPS1_MB_OFS_X/Y/Z` | metres | Dual antenna: main antenna position relative to the second antenna |
| `EK3_SRC1_YAW` | 2 | Dual antenna: use GPS yaw (3 = GPS with compass fallback) |

## Dual-antenna notes

- Mount the two antennas at least 0.5 m apart; a 1 m baseline gives better heading accuracy.
- Set `GPS1_MB_OFS_X/Y/Z` to match your mounting. The example file assumes the second antenna is 0.5 m to the right of the main antenna (`GPS1_MB_OFS_Y = -0.5`).
- GPS yaw is only accurate while the vehicle is roughly upright; avoid it on tailsitters.

## ROS 2

The [`ros2/`](ros2/) folder has starting-point configs for Septentrio's official ROS 2 driver
(`septentrio_gnss_driver`): `hb6-rover.yaml` for single-antenna RTK and `hb6-pro-heading.yaml` for dual-antenna heading.

## Important

These are **starting points**. Always check them against the current
[ArduPilot Septentrio documentation](https://ardupilot.org/copter/docs/common-gps-septentrio.html)
for your firmware version, and bench-test before flight.

## Datasheets, manuals and support

Datasheets and user manuals are sent by email: **[sales@gnss-solutions.com](mailto:sales@gnss-solutions.com)**
Website: [gnss-solutions.com](https://gnss-solutions.com/)

<sub>Septentrio, mosaic and AsteRx are trademarks of Septentrio NV. ArduPilot is a trademark of the ArduPilot project.</sub>
