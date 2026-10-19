# Quadruped Bai Servo: build guide

Refurbished remix of Quadruped_bai_servo v1 by Shuang Peng (BSD-3-Clause). This guide puts printing, assembly, wiring and setup in build order.

## 1. Print the parts

PETG or PLA, about 70 % infill. Orient each part for strength along its long axis and add supports where the overhangs need them.

| File | Print |
|---|---|
| body_frame_1.stl | 1 |
| body_frame_2.stl | 1 |
| body_bridge_1.stl | 2 |
| pcb_holder.stl | 2 |
| joint_ab_holder.stl | 2 |
| hip_holder_2.stl / hip_holder_2_MIR.stl | 2 / 2 |
| hip_joint_holder_1.stl / hip_joint_holder_1_MIR.stl | 2 / 2 |
| servo_holder_1.stl / servo_holder_1_MIR1.stl | 2 / 2 |
| leg_1.stl / leg_1_MIR.stl | 2 / 2 |
| leg_1_2_MIR.stl | 4 (2 as is, 2 mirrored in the slicer) |
| leg_2.stl / leg_2_MIR.stl | 2 / 2 |
| leg_link_1.stl / leg_link_1_MIR.stl | 2 / 2 |
| leg_link-1.stl / leg_link-1_MIR.stl | 2 / 2 |
| leg_tip.stl / leg_tip_MIR.stl | 2 / 2 |

The left-hand leg_1_2 part is the mirror image of leg_1_2_MIR.stl: print two copies normally and two with the slicer's mirror tool.

Optional foot-contact sensors (one set per foot, up to four): feet_sensor_leg_tip_sensor.stl, feet_sensor_leg_tip_sensor2.stl, feet_sensor_sensor_shelf.stl. A sensor foot takes the place of the plain leg tip.

## 2. Buy the parts

See the shop list (bom.yaml / BOM PDF): 12 EMAX ES08A II servos, SF-1 0305 bushings, M3 and M2 screws, Arduino Nano, MPU-6050 module, TPS61088 boost module, prototype board, headers, XT30, 18650 cell and holder.

These parts are not in the shop list:

- M2 x 26 mm screws, about 20. This is not a standard stock length. Buy them from a metric fastener supplier that stocks M2 x 26 mm.
- Optional: the original custom controller board (ATmega328P, MPU-6050, XT30) and the TPS61088 boost board. Order them from PCBWay or another PCB assembly service using the upstream Gerber files. They replace the Nano carrier board and the boost module.

## 3. Calibrate the servos first

Before fitting any horn, drive every servo to the same command (for example 0 degrees or the firmware's zero) so all servos share one PWM-to-angle reference. Check that each servo's travel covers the joint limits of its leg linkage. Mark each servo with its joint.

## 4. Assemble the legs

1. Press an SF-1 0305 bushing into every 5 mm joint hole.
2. Build two legs, then two mirrored legs. Fasten the joints with M3 screws and nuts; servo and holder screws are M2.
3. Turn every joint by hand with the servo fitted. Each joint must move freely with no binding.
4. Glue small silicone pads to the foot tips for grip.

## 5. Build the body

1. Join body_frame_1 and body_frame_2 with the body bridges.
2. Fit the four legs (abduction joints) to the frame.
3. Mount the board holders and the battery holder under the frame.

## 6. Wire the electronics

- Carrier board: mount the Arduino Nano and the MPU-6050 module on the prototype board, add 12 three-pin servo headers.
- Servo signals: D2 to D13, in the order set in the firmware (`servo_pin[12]`).
- IMU: SDA to A4, SCL to A5, plus 5 V/3.3 V per your module and GND.
- Foot switches (optional): A0 to A3, each switch to GND. The firmware enables the internal pull-ups.
- Power: 18650 cell -> XT30 -> boost converter set to 5.5 V -> servo power rail and the Nano 5 V pin. Join all grounds. Keep the servo rail wiring short and thick.

## 7. Firmware

1. Install the Arduino IDE and the FlexiTimer2 library. Servo is built in.
2. Open `quad_bai_s_ctr.ino` from the upstream firmware folder, select Arduino Nano (ATmega328P) and upload over USB.
3. Serial monitor at 115200 baud for debug output.
4. Adjust the servo zero offsets in the firmware until all legs stand symmetric, then try the trot gait on a flat, grippy surface.

The custom controller PCB is programmed over ICSP with a USB ISP programmer instead of USB.

## Notes

- Hobby servos are not back-drivable or torque controlled, so the robot works at the kinematic level: it walks and balances, but it will not resist hard pushes.
- The foot switches detect impacts from some directions only. Treat them as an experiment.

## Credits

Quadruped_bai_servo v1 by Shuang Peng (github.com/psrobotics), BSD-3-Clause. Changes in this remix: combined purchase list, off-the-shelf electronics route, print table with counts, one build guide in build order. STL files are unmodified apart from the feet_sensor_ file name prefix.

## Software and source links

- Gait firmware (Arduino sketch): `source/upstream/software/firmware/quad_bai_s_ctr/` in this repository.
- Custom controller and boost board design files and Gerbers: `source/upstream/eletronic/` in this repository.
- STEP assembly and CoppeliaSim simulation scene: `source/upstream/hardware/step/` and `source/upstream/simulate/` in this repository.
- Joint range drawings and assembly pictures: `source/upstream/pic/` in this repository.
