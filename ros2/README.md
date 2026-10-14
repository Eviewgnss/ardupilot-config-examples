# ROS 2 examples (septentrio_gnss_driver)

Eview receivers are built on Septentrio modules, so they work with Septentrio's official ROS 2 driver,
[`septentrio_gnss_driver`](https://github.com/septentrio-gnss/septentrio_gnss_driver). No custom driver is needed.

| File | Receivers | What it publishes |
|---|---|---|
| `hb6-rover.yaml` | HB6, HB56, HB52, HBEV322 | NavSatFix, GPSFix, PVT, position covariance, AIM+ status |
| `hb6-pro-heading.yaml` | HB6 Pro, HB56H, HB10 | The above plus AttEuler heading/pitch and its covariance |

## Run

```bash
sudo apt install ros-$ROS_DISTRO-septentrio-gnss-driver
ros2 launch septentrio_gnss_driver rover_node.launch.py file_name:=hb6-pro-heading.yaml
```

Copy the YAML into the driver's `config/` folder (or your own package) so the launch file can find it.

## Notes

- `device`: the default uses the receiver's Ethernet-over-USB address. Use `serial:/dev/ttyACM0` for the USB serial port instead.
- RTK: fill in `rtk_settings.ntrip_1` with your NTRIP caster, or feed corrections another way.
- Solution type (RTK fixed, float, DGNSS, standalone) is in the `gpsfix` and `pvtgeodetic` topics. If you fuse GNSS in an EKF, consider setting measurement noise per solution type.
- For `robot_localization`, use `navsatfix` with `navsat_transform_node`; with a heading receiver, convert the AttEuler heading into the yaw input instead of relying on a magnetometer.
- These are starting points. Check the driver README for your ROS 2 distribution and bench-test before field use.

Datasheets and user manuals are sent by email: **[sales@gnss-solutions.com](mailto:sales@gnss-solutions.com)**
