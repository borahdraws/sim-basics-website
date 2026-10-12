---
description: We need a virtual mechanism for our elevator subsystem!
icon: mitten
---

# Create ElevatorIOSim

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## 1. Create a new class ElevatorIOSim

Create a new class called ElevatorIOSim under src >> main >> java >> frc >> robot >> subsystems >> elevator and implement ElevatorIO.

Add a `double` variable for the voltage we want the elevator to go to.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

/** Add your docs here. */
<strong>public class ElevatorIOSim implements ElevatorIO {
</strong><strong>  private double simElevatorAppliedVolts = 0.0;
</strong>}
</code></pre>

## 2. Create a new ElevatorSim

Add a simulated elevator variable and object to our simulation.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

<strong>import edu.wpi.first.wpilibj.simulation.ElevatorSim;
</strong><strong>import edu.wpi.first.math.system.plant.DCMotor;
</strong><strong>import edu.wpi.first.math.util.Units;
</strong>
/** Add your docs here. */
public class ElevatorIOSim implements ElevatorIO {
  private double simElevatorAppliedVolts = 0.0;
  
<strong>  ElevatorSim elevatorSim =
</strong><strong>    new ElevatorSim(
</strong><strong>      DCMotor.getKrakenX60(2),
</strong><strong>      40.0,
</strong><strong>      Units.lbsToKilograms(15),
</strong><strong>      Units.inchesToMeters(1),
</strong><strong>      Units.inchesToMeters(0),
</strong><strong>      Units.inchesToMeters(48),
</strong><strong>      true,
</strong><strong>      0.0
</strong><strong>    );
</strong>}
</code></pre>

We have created a new object from the class `ElevatorSim` called `elevatorSim`. This simulation object keeps track of position, velocity, and current draw over time.

{% code overflow="wrap" %}
```java
ElevatorSim elevatorSim = new ElevatorSim(...);
```
{% endcode %}

The class `ElevatorSim` needs eight parameters:

{% stepper %}
{% step %}
### `DCMotor gearbox`

`DCMotor gearbox` is a motor to help the sim calculate the sim motor's electrical behavior, like current draw and resistance.

In this example, we want our sim to have the electrical behavior of 2 KrakenX60s.
{% endstep %}

{% step %}
### `double gearing`

The reduction between motor and drum (thing that spins slower because of the reduction), as a ratio of output to input.

How many times does the motor have to spin to make the drum turn once? A high gear reduction (like 60:1) sacrifices speed for stronger force aka torque, while a low gear reduction (like 3:1) prioritizes speed over strength.

In this example, we have a 40:1 gear reduction.
{% endstep %}

{% step %}
### `double carriageMassKg`

The weight of the elevator in kg.

<details>

<summary>Units</summary>

By importing and using `Units` we can change values (from meters to inches, from radians to degrees and vice versa etc) You can control + click on `Units` to see all the possible conversions.

</details>

In this example, we use Units to convert the estimated elevator weight from lbs to kgs.
{% endstep %}

{% step %}
### `double drumRadiusMeters`

"The radius of the drum that the elevator spool is wrapped around."

In this example, we use Units to convert the measurement from inches to meters.
{% endstep %}

{% step %}
### `double minHeightMeters`

What's the lowest height the elevator can go down to?

In this example, we use Units to convert the measurement from inches to meters.
{% endstep %}

{% step %}
### `double maxHeightMeters`

What's the highest height the elevator can go up to?

In this example, we use Units to convert the measurement from inches to meters.
{% endstep %}

{% step %}
### `boolean simulateGravity`

We want gravity to exist in the simulation, so we put `true`.
{% endstep %}

{% step %}
### `double startingHeightMeters`

What position should the simulated elevator start in?

In this example, we use Units to convert the measurement from inches to meters.
{% endstep %}
{% endstepper %}

## 3. Fill in updateInputs method

Add the method `updateInputs`.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import edu.wpi.first.wpilibj.simulation.ElevatorSim;
import edu.wpi.first.math.system.plant.DCMotor;
import edu.wpi.first.math.util.Units;

/** Add your docs here. */
public class ElevatorIOSim implements ElevatorIO {
  private double simElevatorAppliedVolts = 0.0;
  
  ElevatorSim elevatorSim =
    new ElevatorSim(
      DCMotor.getKrakenX60(2),
      40.0,
      Units.lbsToKilograms(15),
      Units.inchesToMeters(1),
      Units.inchesToMeters(0),
      Units.inchesToMeters(48),
      true,
      0.0
    );
  
<strong>  @Override
</strong><strong>  public void updateInputs(SuperstructureIOInputs inputs) {
</strong><strong>    elevatorSim.update(0.02);
</strong><strong>    
</strong><strong>    inputs.elevatorLeaderConnected = true;
</strong><strong>    inputs.elevatorFollowerConnected = true;
</strong><strong>    
</strong><strong>    inputs.elevatorPositionMeters = elevatorSim.getPositionMeters();
</strong><strong>    inputs.elevatorVelocityMetersPerSec = elevatorSim.getVelocityMetersPerSecond();
</strong><strong>    inputs.elevatorAppliedVolts = simElevatorAppliedVolts;
</strong><strong>    
</strong><strong>    // divided by number of motors (aka 2)
</strong><strong>    inputs.elevatorLeaderCurrentAmps = elevatorSim.getCurrentDrawAmps() / 2.0;
</strong><strong>    inputs.elevatorFollowerCurrentAmps = elevatorSim.getCurrentDrawAmps() / 2.0;
</strong><strong>  }
</strong>}
</code></pre>

This line of code steps the physics model forward by 20ms:

<pre class="language-java"><code class="lang-java"><strong>elevatorSim.update(0.02);
</strong></code></pre>

The AdvantageKit inputs should come directly from the simulation values.

We get the variable `elevatorPositionMeters` from `inputs` (which is the static class `elevatorIOInputs`.)

`elevatorSim` is the object we made from `ElevatorSim`. Control + click on `ElevatorSim` to see the methods inside of it.&#x20;

One of the methods we can use is `getPositionMeters()` which will get the elevator position in meters.

See if you can find `getVelocityMetersPerSecond()` and `getCurrentDrawAmps()` as well.

## 4. Fill in setElevatorVoltage method

Create the method to change the value of `simElevatorAppliedVolts`.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.elevator;

import edu.wpi.first.wpilibj.simulation.ElevatorSim;
import edu.wpi.first.math.system.plant.DCMotor;
import edu.wpi.first.math.util.Units;
<strong>import edu.wpi.first.math.MathUtil;
</strong>
/** Add your docs here. */
public class ElevatorIOSim implements ElevatorIO {
  private double simElevatorAppliedVolts = 0.0;
  
  ElevatorSim elevatorSim =
    new ElevatorSim(
      DCMotor.getKrakenX60(2),
      40.0,
      Units.lbsToKilograms(15),
      Units.inchesToMeters(1),
      Units.inchesToMeters(0),
      Units.inchesToMeters(48),
      true,
      0.0
    );
  
  @Override
  public void updateInputs(SuperstructureIOInputs inputs) {
    elevatorSim.update(0.02);
    inputs.elevatorLeaderConnected = true;
    inputs.elevatorFollowerConnected = true;
    inputs.elevatorPositionMeters = elevatorSim.getPositionMeters();
    inputs.elevatorVelocityMetersPerSec = elevatorSim.getVelocityMetersPerSecond();
    inputs.elevatorAppliedVolts = simElevatorAppliedVolts;
    // divided by number of motors (aka 2)
    inputs.elevatorLeaderCurrentAmps = elevatorSim.getCurrentDrawAmps() / 2.0;
    inputs.elevatorFollowerCurrentAmps = elevatorSim.getCurrentDrawAmps() / 2.0;
  }
  
<strong>  @Override
</strong><strong>  public void setElevatorVoltage(double volts) {
</strong><strong>    simElevatorAppliedVolts =
</strong><strong>      MathUtil.clamp(
</strong><strong>        volts,
</strong><strong>        -12.0,
</strong><strong>        12.0
</strong><strong>      );
</strong><strong>
</strong><strong>    elevatorSim.setInputVoltage(simElevatorAppliedVolts);
</strong><strong>  }
</strong>}
</code></pre>

Hover over `MathUtil.clamp()` or control + click on it to find out what it does.

This method needs three parameters:

1. `double value` - `voltage` aka the value to adjust
2. `double low` - If `voltage` goes below -12.0, change `voltage` to -12.0.
3. `double high` - If `voltage` goes above 12.0, change `voltage` to 12.0.

We use the `ElevatorSim` method `setInputVoltage()` to change the adjusted voltage:

{% code overflow="wrap" %}
```java
elevatorSim.setInputVoltage(simElevatorAppliedVolts);
```
{% endcode %}
