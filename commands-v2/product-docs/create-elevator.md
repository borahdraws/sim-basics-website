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

## Create an enum named Goal

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Elevator extends SubsystemBase {
<strong>    public enum Goal {
</strong><strong>      LOWEST(
</strong><strong>        0.0
</strong><strong>      ),
</strong><strong>      HIGHEST(
</strong><strong>        Units.inchesToMeters(14.0)
</strong><strong>      );
</strong><strong>
</strong><strong>      final double elevatorCarriageTargetHeightMeters;
</strong><strong>
</strong><strong>      Goal(double elevatorHeightMeters) {
</strong><strong>      this.elevatorCarriageTargetHeightMeters = elevatorHeightMeters;
</strong><strong>    }
</strong><strong>  }
</strong>
  /** Creates a new Elevator. */
  public Elevator() {}

  @Override
  public void periodic() {}
}
</code></pre>

### Code breakdown #1

<details>

<summary>What is an enum?</summary>

We can use an `enum` (short for enumerator) to make a set of values with a special name.

Our enum is pretty simple for now, each set only has one measurement to keep track of (height), and there are two sets: LOWEST and HIGHEST.

But what if we had an elevator connected to an arm, and the arm needs to always be at a certain angle depending on the elevator height?

Or what if we want to introduce more heights like "SHELF\_1" and "SHELF\_2"?

By using an enum, we ensure that LOWEST always has a measurement of 0.0. Each set always has its own specific data, and we can't use any data other than the sets we have created.

</details>

<details>

<summary>What are the components of an enum?</summary>

{% stepper %}
{% step %}
#### Constants

The 'sets' that we create with descriptive names. This must be first


{% endstep %}

{% step %}
#### Fields

The variables that go inside the sets


{% endstep %}

{% step %}
#### Constructor

This is how we create a set


{% endstep %}
{% endstepper %}

</details>

## Add more variables

Create an interface variable and autologged variable & object. We are going to also create a variable to hold the enum we just made and set it to a default value.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
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

<strong>  private final ElevatorIO io;
</strong><strong>  private final ElevatorIOInputsAutoLogged inputs = new ElevatorIOInputsAutoLogged();
</strong><strong>  private Goal goal = Goal.LOWEST;
</strong>  
  /** Creates a new Elevator. */
  public Elevator() {}

  @Override
  public void periodic() {}
}
</code></pre>

## Create a ProfiledPIDController

### What is PID?

Let's say our elevator is at the lowest position and we want the elevator to go to the highest position. In an ideal world we would put 100% power to the elevator and then stop it the moment it's reached the top.

The reality is that you're probably going to blast the elevator off the robot trying to go from 0 to 100% power. The elevator will way overshoot its target height.

PID is a smart control system that determines how much power to send a motor based on the data it receives.

### Add ProfiledPIDController to Elevator

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
  
  private final ProfiledPIDController elevatorPID =
    new ProfiledPIDController(
      5.0, 
      0.0, 
      0.2, 
      new TrapezoidProfile.Constraints(
        8.0,
        6.0
      )
    );
  
  /** Creates a new Elevator. */
  public Elevator() {}

  @Override
  public void periodic() {}
}
```
{% endcode %}

### ProfiledPIDController needs four parameters:

{% stepper %}
{% step %}
#### `double Kp`

#### P (Proportional)

* _How far am I from the target?_
* If you're far from the target, P gives more power. If you're close to the target, P gives only a little power.
* For FRC, P is around:
  * 2.0 \~ 5.0 radians input
  * 0.03 - 0.08 degrees input
{% endstep %}

{% step %}
#### `double Ki`

#### I (Integral)

* _I still haven't reached my target and how much time has gone by?_
* I pushes you more as more time goes by without you reaching the target.
* For FRC, usually we leave this at 0.0
{% endstep %}

{% step %}
#### `double Kd`

#### D (Derivative)

* _How fast am I approaching my target?_
* D looks at velocity, not distance, and puts on the brakes to slow you to a smooth stop at the target.
* For FRC, I is around:
  * 0.05 to 0.2 radians input
  * 0.001 to 0.005 degrees input
{% endstep %}

{% step %}
#### `TrapezoidProfile.Constraints constraints`

Helps smooth the PID movement. `TrapezoidProfile.Constraints` needs two parameters:

{% stepper %}
{% step %}
#### `double maxVelocity`


{% endstep %}

{% step %}
#### `double maxAcceleration`


{% endstep %}
{% endstepper %}
{% endstep %}
{% endstepper %}

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

## Add FeedForward

### What is Feedforward control?

PID determines how much power to send to a motor as a reaction, which means it is already behind.&#x20;

Feedforward control allows us to calculate the input we want beforehand so that PID has to do less work. WPILib has a few feedforward classes for us to use. We will be using the class ElevatorFeedforward.

### Add ElevatorFeedforward to Elevator

### ElevatorFeedforward needs four parameters:

{% stepper %}
{% step %}
#### `double ks`


{% endstep %}

{% step %}
#### `double kg`


{% endstep %}

{% step %}
### `double kv`


{% endstep %}

{% step %}
### `double ka`


{% endstep %}
{% endstepper %}

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
wow! a blank box!
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
you can't see the dot it but it's there
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
a line!!
{% endcolumn %}
{% endcolumns %}



I want one line to represent the elevator base, then one line to represent the moving elevator carriage. That will help us see if the elevator carriage is moving too far above the elevator base.

Then I think I will have one more line coming out sideways from the carriage to represent the rollers of the intake system. It will be helpful in the sim to see if the elevator intake is bumping into things or not.





List of colors bc why not

<details>

<summary></summary>

*

</details>
