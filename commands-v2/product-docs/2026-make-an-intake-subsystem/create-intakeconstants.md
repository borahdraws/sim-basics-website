---
description: Optional but easy to see why you always want this.
icon: bridge
---

# Create IntakeConstants

Let's say we have a number, for example the max voltage on a robot, and we think that number is 14 so we put 14 all over in our robot code in all our subsystem voltages.

Now someone walks over and says "hey the max voltage isn't 14 volts it's 12 volts you know that right 🤔"

Now we need to go through all the code we ever wrote to change every 14 to the number 12.

And then that same someone walks over and says "hey our battery doesn't like that so can we change max voltage to 11.55 please"

We can set up some variables that can be used by any files in the robot's code.

## What you'll need

* [ ] A computer with WPILib installed
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## 1. Create new class IntakeConstants

Open WPILib VS Code and open AdvantageKit\_TalonFXSwerveTemplate.

Open src >> main >> java >> frc >> robot >> subsystems >> intake. Create a new class called IntakeConstants.

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */

public class IntakeConstants {}
```
{% endcode %}

Add as many constants as you need. Constant variables are written in SCREAMING\_SNAKE\_CASE.

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */

public class IntakeConstants {
  public static final int INTAKE_CAN_ID = 5;
  public static final double INTAKE_VOLTAGE = 12.0;
}
```
{% endcode %}

When `static` is added to a variable, it means there's only one of that variable in the whole entire robot code.

## 2. Using the constants in your code

We have to import the variables every time we want to use them in a new class.

{% code overflow="wrap" %}
```java
import static frc.robot.subsystems.intake.IntakeConstants.INTAKE_CAN_ID;
```
{% endcode %}

By adding a star `*` (asterisk) we can import every constant in IntakeConstants.java.

{% code overflow="wrap" %}
```java
import static frc.robot.subsystems.intake.IntakeConstants.*;
```
{% endcode %}

Open IntakeIOTalonFX.java. Go to CAN ID and replace with the constant we created:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import com.ctre.phoenix6.hardware.TalonFX;
import com.ctre.phoenix6.controls.VoltageOut;

import static edu.wpi.first.units.Units.Radians;
import static edu.wpi.first.units.Units.RadiansPerSecond;
import static edu.wpi.first.units.Units.Amps;
<strong>import static frc.robot.subsystems.intake.IntakeConstants.INTAKE_CAN_ID;
</strong>
/** Add your docs here. */
public class IntakeIOTalonFX implements IntakeIO {
  
  private final TalonFX intakeTalon;
  private final VoltageOut request; 
  
  public IntakeIOTalonFX() {
<strong>    intakeTalon = new TalonFX(INTAKE_CAN_ID);
</strong>    request = new VoltageOut(0.0);
  }
  
  @Override
  public void updateInputs(IntakeIOInputs inputs) {
    inputs.intakePositionRadians = intakeTalon.getPosition().getValue().in(Radians); 
    inputs.intakeVelocityRadiansPerSec = intakeTalon.getVelocity().getValue().in(RadiansPerSecond);
    inputs.intakeAppliedVoltage = intakeTalon.getMotorVoltage().getValueAsDouble();
    inputs.intakeCurrentAmperage = intakeTalon.getSupplyCurrent().getValue().in(Amps);
  }

  @Override
  public void setIntakeVoltage(double voltage) {
    request.Output = voltage;
    intakeTalon.setControl(request);
  }
}
</code></pre>

Open Intake.java. Replace intake voltage in commands with the constant:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;
import edu.wpi.first.wpilibj2.command.Command;

<strong>import static frc.robot.subsystems.intake.IntakeConstants.INTAKE_VOLTAGE; 
</strong>
public class Intake extends SubsystemBase {
  private final IntakeIO io;
  private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
  
  /** Creates a new Intake. */
  public Intake(IntakeIO io) {
    this.io = io;
  }

  @Override
  public void periodic() {
    io.updateInputs(inputs);
    Logger.processInputs("Intake", inputs);
  }
  
  public Command moveForwards() { 
    return runEnd(
    () -> {
<strong>      io.setIntakeVoltage(INTAKE_VOLTAGE);
</strong>    },
    () -> {
      io.setIntakeVoltage(0);
    });
  
  public Command moveBackwards() { 
    return runEnd(
    () -> {
<strong>      io.setIntakeVoltage(-INTAKE_VOLTAGE);
</strong>    },
    () -> {
      io.setIntakeVoltage(0);
    });
  }
  
  public Command stop() {
    return runOnce(
    () -> {
     	io.setIntakeVoltage(0);
    });
  }
}
</code></pre>
