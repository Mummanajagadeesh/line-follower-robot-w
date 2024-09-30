# Line Follower Robot Simulation

Welcome to the **Line Follower Robot** simulation repository! This project simulates a line-following robot using the **Webots** robotics simulator. The robot, based on the **e-puck** model, follows a black track created in **Tinkercad** using two IR sensors.

## Features
- Simulation of a simple line-following robot using two infrared (IR) sensors placed on either side of the robot.
- Black track designed in Tinkercad, exported and used in the Webots simulation.
- Simple control logic based on IR sensor values to adjust the robot's movement.
- No PID controller is used; instead, the robot makes decisions using basic conditional statements to steer left or right based on sensor readings.

## Demo Video
Click the image below to watch a demo of the simulation in action:

[![Click to Watch!](http://img.youtube.com/vi/rejXYcaX9NQ/0.jpg)](http://www.youtube.com/watch?v=rejXYcaX9NQ)

## How It Works

### Robot Design
The robot used in this simulation is the **e-puck**, a simple differential drive robot with two IR sensors positioned on the left and right sides. These sensors detect the black line against the background, and based on their readings, the robot adjusts its movement.

- **Left IR Sensor (`ir0`)**: Detects the black line on the left side.
- **Right IR Sensor (`ir1`)**: Detects the black line on the right side.
- **Wheels**: Two differential drive motors control the movement of the robot (left and right wheels).

### Track Design
The black track was created in **Tinkercad** and exported into the simulation environment. You can find the track mesh file in the **meshes** folder.

### Control Logic
The robot's movement is controlled by checking the values of the left and right IR sensors and adjusting the wheel velocities accordingly:

- **Straight Movement**: If both sensors detect similar values, the robot moves forward.
- **Turning**:
  - If the left IR sensor detects the black line (i.e., its value increases), the robot turns **left** by reducing the left motor's speed and potentially reversing it.
  - If the right IR sensor detects the black line, the robot turns **right** by reducing the right motor's speed.

The control logic does not involve a **PID controller**. Instead, basic threshold-based conditions are used to decide the robot’s steering direction.

### Code Explanation

```python
def run_robot(robot):
    timestep = 32
    max_speed = 6.28 * 0.25  # Maximum angular velocity for the motors
    
    # Initialize motors
    left_motor = robot.getDevice('left wheel motor')
    right_motor = robot.getDevice('right wheel motor')
    
    left_motor.setPosition(float('inf'))  # Set motors to velocity control mode
    right_motor.setPosition(float('inf'))
    
    left_motor.setVelocity(0.0)  # Initially stop motors
    right_motor.setVelocity(0.0)
    
    # Enable IR sensors
    left_ir = robot.getDistanceSensor('ir0')
    right_ir = robot.getDistanceSensor('ir1')
    left_ir.enable(timestep)
    right_ir.enable(timestep)
    
    # Main loop
    while robot.step(timestep) != -1:
        left_ir_value = left_ir.getValue()
        right_ir_value = right_ir.getValue()
        
        left_speed = max_speed
        right_speed = max_speed
        
        # Simple control logic to adjust robot movement based on sensor readings
        if (left_ir_value > right_ir_value) and (6 < left_ir_value < 15):
            print('Go left')
            left_speed = -max_speed  # Reverse left wheel to turn left
        elif (right_ir_value > left_ir_value) and (6 < right_ir_value < 15):
            print('Go right')
            right_speed = -max_speed  # Reverse right wheel to turn right
        
        # Set the velocities of the motors
        left_motor.setVelocity(left_speed)
        right_motor.setVelocity(right_speed)
```

### Key Points:
- **Timestep**: The simulation steps are updated every 32ms.
- **Max Speed**: The maximum angular velocity for the motors is set to 25% of the full motor speed (6.28 rad/s).
- **IR Sensor Values**: The values of the IR sensors are used to detect the black line. A value between 6 and 15 indicates the robot is over the line, and the respective motor is slowed or reversed to turn the robot.
- **Motor Control**: The motors are set to velocity mode, and their speed is adjusted based on the sensor inputs. When one sensor detects a stronger signal, the robot turns in that direction.

## Installation and Usage

### Requirements
- **Webots**: Install the Webots robotics simulator from [here](https://cyberbotics.com/).
- **Python**: Ensure that you have Python installed to run the robot controller.

### Steps to Run
1. Clone this repository to your local machine:
   ```bash
   git clone https://github.com/Mummanajagadeesh/line-follower-robot-w.git
   cd line-follower-robot-w
   ```
2. Open Webots and load the **line_follower_robot.wbt** world file in the simulation folder.
3. Run the simulation and observe the robot following the line on the black track.

### Meshes
The **meshes** folder contains the black track design exported from **Tinkercad**. This is used in the Webots simulation for the robot to follow.

## Future Enhancements
- **PID Control**: Although the current implementation uses basic threshold logic, PID control can be added for smoother and more accurate line following.
- **Speed Optimization**: The robot speed can be adjusted dynamically based on how sharply it needs to turn.
- **Additional Sensors**: Adding more IR sensors could improve the robot’s accuracy when following complex curves or intersections in the track.

## Contributing
Feel free to fork this repository, create a branch, and submit a pull request if you would like to improve the robot or add new features.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
