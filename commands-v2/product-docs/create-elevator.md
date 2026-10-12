---
description: i don't have a real robot yet so let's skip to the fun stuff.
icon: person-military-pointing
---

# Create Elevator

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## 1. Add a new Subsystem Class Elevator

Open WPILib VS Code and open AdvantageKit\_TalonFXSwerveTemplate. Open src >> main >> java >> frc >> robot >> subsystems >> elevator.

Right click the folder elevator >> Create a new class/command >> Create a Subsystem called Elevator.

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Elevator extends SubsystemBase {
  /** Creates a new Elevator. */
  public Elevator() {}

  @Override
  public void periodic() {}
}
```
{% endcode %}

## Create an enum Goal

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Elevator extends SubsystemBase {
    public enum Goal {
      LOWEST(
        0.0
      ),
      HIGHEST(
        Units.inchesToMeters(14.0)
      );

      final double elevatorCarriageTargetHeightMeters;

      Goal(double elevatorHeightMeters) {
      this.elevatorCarriageTargetHeightMeters = elevatorHeightMeters;
    }
  }

  /** Creates a new Elevator. */
  public Elevator() {}

  @Override
  public void periodic() {}
}
```
{% endcode %}

Code breakdown

<details>

<summary>What is an enum?</summary>

enum (short for enumerator)&#x20;



</details>

## More variables

Create interface variable and autologged variable & object

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Elevator extends SubsystemBase {
    public enum Goal {
      LOWEST(
        0.0
      ),
      HIGHEST(
        Units.inchesToMeters(14.0)
      );

      final double elevatorCarriageTargetHeightMeters;

      Goal(double elevatorHeightMeters) {
      this.elevatorCarriageTargetHeightMeters = elevatorHeightMeters;
    }
  }

  private final ElevatorIO io;
  private final ElevatorIOInputsAutoLogged inputs = new ElevatorIOInputsAutoLogged();
  private Goal goal = Goal.LOWEST;
  
  /** Creates a new Elevator. */
  public Elevator() {}

  @Override
  public void periodic() {}
}
```
{% endcode %}

## Create a ProfiledPIDController

### What is PID?

Let's say our elevator is at the lowest position and we want the elevator to go to the highest position. In an ideal world we would put 100% power to the elevator and then stop it the moment it's reached the top.

The reality is that you're probably going to blast the elevator off the robot trying to go from 0 to 100% power. The elevator will way overshoot its target height.

PID is a smart control system that determines how much power to send a motor.

#### P (Proportional)

* _How far am I from the target?_
* If you're far from the target, P gives more power. If you're close to the target, P gives only a little power.
* For FRC, P is around:
  * 2.0 \~ 5.0 radians input
  * 0.03 - 0.08 degrees input

#### I (Integral)

* _I still haven't reached my target and how much time has gone by?_
* I pushes you more as more time goes by without you reaching the target.
* For FRC, usually we leave this at 0.0

#### D (Derivative)

* _How fast am I approaching my target?_
* D looks at velocity, not distance, and puts on the brakes to slow you to a smooth stop at the target.&#x20;
* For FRC, I is around:
  * 0.05 to 0.2 radians input
  * 0.001 to 0.005 degrees input

{% columns %}
{% column %}
**✅ Best PID**

Snaps smoothly to target without overshoot
{% endcolumn %}

{% column %}
**❌ Bad PID**

Super sluggish to get to target
{% endcolumn %}

{% column %}
**❌ Bad PID**

Bounces back and forth to get to target
{% endcolumn %}
{% endcolumns %}

## Add Mechanism2d

#### What is Mechanism2d?

Mechanism2d helps us create a simple visualization of a robot's moving parts.

### Mechanism2d needs at least 3 things:

{% columns %}
{% column width="66.66666666666666%" %}
#### Mechanism2d Container

Set width and height of the 'canvas' or 'viewport' we see the lines in. The dimensions of the canvas are unitless but often treated as meters.

{% code overflow="wrap" %}
```
// Some code
```
{% endcode %}
{% endcolumn %}

{% column width="33.33333333333334%" %}

{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="66.66666666666666%" %}
#### MechanismRoot2d

The invisible anchor point where the MechanismLigament2d line is attached to the robot. 0, 0 is bottom left.

{% code overflow="wrap" %}
```
// Some code
```
{% endcode %}

In this example we have 2.5, 2.5 so the root is at the center of the canvas (Mechanism2d Container) which is 5.0, 5.0
{% endcolumn %}

{% column width="33.33333333333334%" %}

{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column width="66.66666666666666%" %}
#### MechanismLigament2d

The line that extends from the root. Can be used to visualize elevators, arms, turrets and more.

{% code overflow="wrap" %}
```
// Some code

```
{% endcode %}
{% endcolumn %}

{% column width="33.33333333333334%" %}

{% endcolumn %}
{% endcolumns %}

List of colors bc why not



<details>

<summary></summary>

*

</details>
