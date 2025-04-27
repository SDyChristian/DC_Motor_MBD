# DC_Motor_MBD
In this repository, you will find Simulink simulations related to a DC motor

## 1 Electro-Mechanical Dynamic Model

![DC Motor Diagram](Images/Dc_Motor_Diagram.PNG)

The dynamic model of the DC motor was built using components from the Simscape library. 
- The parameters assigned to each component correspond to real-world systems: the BALDOR AP233001 DC motor and the IRF640 MOSFET.
- Sensors are assumed to be ideal for the purposes of these simulations.

## 2 Current PI Control

![PI](Images/PID_DCMotorDiagram.PNG)

This simulation implements a Proportional-Integral (PI) current controller. A manual switch block allows toggling between a tracking task and a regulation task as the reference input. 

### 2.1 Tunning

The tuning process was based on transfer function analysis using the pole placement method, taking into account the natural dynamics of the current.

![Tunning](Images/Tunning.png)

### 2.2 Results

The results are shown below

![Results](Images/Results.png)

These graphs show the control performance for the tracking task. In the error plot, it can be observed that, despite the PI controller not being specifically designed for tracking tasks, it is able to maintain the error oscillating around zero.

## 3 Cascade Control: Non-Linear Computed Torque + Kinematic Control and PI Control

![Cascade](Images/Cascade_Control.PNG)

This simulation models a mechanical link connected to the shaft of a DC motor. As a result, the overall system dynamics encompass both the dynamics of the DC motor and those of the mechanical link, which is treated as a constrained rigid body.
- The simulation employs Simscape Multibody blocks, including: World Frame, Revolute Joint, Rigid Transformation, and Body.
- The control architecture adopts a cascade structure, comprising a high-level controller and a low-level controller.
- The high-level controller applies a nonlinear computed torque strategy based on kinematic control. This method compensates for the nonlinear dynamics of the rigid body and enforces a desired second-order dynamic response.
- The high-level controller generates a desired torque, which is then converted into a current reference through a linear transformation. This current reference is subsequently provided to the low-level controller.

### 3.1 Tunning
The tuning process for the non-linear controller is carried out through kinematic control. The closed-loop equation obtained via the computed torque method results in a second-order system, allowing the controller gains to be calculated based on the characteristic equation of a second-order system. The design parameters used are the relative damping ratio and the natural frequency.

### 3.2 Results
Simulation results are shown below
![Cascade](Images/Results_CascadeControl.png)

In this simulation, a tracking task is implemented. The trajectory is defined by the trajectory block, where the desired position, velocity, and acceleration are specified. The current and angular position errors indicate that the system oscillates around the trajectory, although the errors cannot reach zero. This behavior is expected, as the convergence time of the PI controller is slower than the period imposed by the reference frequency. This effect can be mitigated by retuning the current controller, a process that should be repeated if the reference frequency changes again. A more effective approach to address this issue is to implement a unified control law that considers the full dynamics of the system but this is beyond the scope of this project.


