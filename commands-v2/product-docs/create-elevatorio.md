---
description: Start making your second subsystem!
icon: bicep
---

# Create ElevatorIO

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## 1. Start with design & goal

CAD comes to you and says "hey we have our initial robot design ready. It's a robot with an elevator, here is the cad file. This is how far the elevator can go up, and this is how far the elevator can go down. Make it move (please)"

{% hint style="warning" %}
Important: Learn how to use Onshape/the cad software the team is using so that your code properly matches the robot design.
{% endhint %}

Pretend this is the sketch of the cad of your robot with the information needed to program the elevator:



Talk with the drivers/gunners and figure out how they want to control the robot. Let's make this the goal for the elevator:

> #### Press the right bumper for the elevator to go to its highest position. Press the right trigger for the elevator to go to its lowest position.

## 2. Create new folder & new IO interface

Create a new folder called elevator under src >> main >> java >> frc >> robot >> subsystems.

Right click on the folder elevator >> Create a new class/command

Name it ElevatorIO and change from a class to interface.

Create static nested class and add variables.

Add methods.

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import org.littletonrobotics.junction.AutoLog;

/** Add your docs here. */
public interface ElevatorIO {

  @AutoLog
  public static class ElevatorIOInputs {
    public double elevatorPositionMeters = 0.0;
    public double elevatorVelocityMetersPerSec = 0.0;
    public double elevatorAppliedVolts = 0.0;
    public double elevatorCurrentAmps = 0.0;
    
    public double elevatorTempCelsius = 0.0;
  }

  public default void updateInputs(ElevatorIOInputs inputs) {}

  public default void setElevatorVoltage(double voltage) {}
  
  public default void resetElevatorPosition(double meters) {}

  public default void stopElevator() {}
}
```
{% endcode %}

If the elevator has more than one motor (talk to design), then there is always a Leader motor (gets the info) and the Follower motor (copies the leader) to make sure they never fight against each other.

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import org.littletonrobotics.junction.AutoLog;

/** Add your docs here. */
public interface ElevatorIO {

  @AutoLog
  public static class ElevatorIOInputs {
    public boolean elevatorLeaderConnected = false;
    public boolean elevatorFollowerConnected = false;
    
    public double elevatorPositionMeters = 0.0;
    public double elevatorVelocityMetersPerSec = 0.0;
    public double elevatorAppliedVolts = 0.0;
    public double elevatorLeaderCurrentAmps = 0.0;
    public double elevatorFollowerCurrentAmps = 0.0;
    
    public double elevatorLeaderTempCelsius = 0.0;
    public double elevatorFollowerTempCelsius = 0.0;
  }

  public default void updateInputs(ElevatorIOInputs inputs) {}

  public default void setElevatorVoltage(double voltage) {}
  
  public default void resetElevatorPosition(double meters) {}

  public default void stopElevator() {}
}
```
{% endcode %}
