---
description: Let's define go and stop!
icon: truck-moving
---

# Create Intake

## What you'll need

* [ ] A computer with WPILib installed
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## 1. Add a new Subsystem Class Intake

Let's make our own subsystem class! Open WPILib VS Code and open AdvantageKit\_TalonFXSwerveTemplate.

Open src >> main >> java >> frc >> robot >> subsystems >> intake.

Right click the folder intake >> Create a new class/command

The Command Center prompts you to Pick a command. We want to select the command:

{% code overflow="wrap" %}
```
Subsystem A robot subsystem.
```
{% endcode %}

The Command Center prompts you to Please enter a class name. Type in Intake and press enter. The code should look like this:

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Intake extends SubsystemBase {
  /** Creates a new Intake. */
  public Intake() {}

  @Override
  public void periodic() {}
}
```
{% endcode %}

<details>

<summary>What is extends? (Parent/Child)</summary>

So far we have two subsystems, Intake and Drive. We have a class called `SubsystemBase`, and we want that `SubsystemBase` inside of `Intake`. We also want that `SubsystemBase` code inside of `Drive`.&#x20;

(And maybe we will have more subsystems in the future that need that `SubsystemBase` code: Elevator, Arm...)

Let's say we copied and pasted the `SubsystemBase` code inside of into each of our subsystem files. What happens when we want to change a single variable name in a `SubsystemBase` method? Now we need to go through every subsystem file and figure out what we need to change. 🫩

Instead of copying and pasting the code from `SubsystemBase` into `Intake` and `Drive` (and future subsystems), we use `extends`. Now it's like that code is directly inside both Intake and Drive!

The class that we start with is the **parent** class, and the class that grabs code from the parent class is called the **child** class.

`SubsystemBase` is the parent class.

`Intake` is a child class that extends `SubsystemBase`. `Drive` is a child class that extends `SubsystemBase`.

</details>

## 2. Add class variables&#x20;

Add two variables:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
<strong>import org.littletonrobotics.junction.Logger;
</strong>
public class Intake extends SubsystemBase {
<strong>  private final IntakeIO io;
</strong><strong>  private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
</strong>  
  /** Creates a new Intake. */
  public Intake() {}

  @Override
  public void periodic() {}
}
</code></pre>

{% hint style="warning" %}
VS Code will prompt you for the missing import. There are multiple Logger classes you can import, make sure you import the correct one.
{% endhint %}

Let's look at the variables we've created.&#x20;

{% code overflow="wrap" %}
```java
private final IntakeIO io;
```
{% endcode %}

This is an Interface Variable, a variable that can hold anything that implements `IntakeIO`.

{% code overflow="wrap" %}
```java
private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
```
{% endcode %}

We create a variable called `inputs` that helps us update data properly in AdvantageKit (this will give you an error until you build the code once)

## 3. Fill in class constructor

Add a parameter and single line to the class constructor:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;

public class Intake extends SubsystemBase {
  private final IntakeIO io;
  private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
  
  /** Creates a new Intake. */
<strong>  public Intake(IntakeIO io) {
</strong><strong>    this.io = io;
</strong><strong>  }
</strong>
  @Override
  public void periodic() {}
}
</code></pre>

The value of the class variable `io` is changed to the value of the method parameter `io`.

## 4. Fill in periodic()

The method periodic will be called once per scheduler run (typically once every 20 milliseconds)

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;

public class Intake extends SubsystemBase {
  private final IntakeIO io;
  private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
  
  /** Creates a new Intake. */
  public Intake(IntakeIO io) {
    this.io = io;
  }

  @Override
  public void periodic() {
<strong>    io.updateInputs(inputs);
</strong><strong>    Logger.processInputs("Intake", inputs);
</strong>  }
}
</code></pre>

We have some kind of `IntakeIO` (`IntakeIOSim()` or `IntakeIOTalonFX()`), and we call the method `updateInputs` with `inputs` as the parameter.

What does `updateInputs` do if the value of `io` is `IntakeIOSim()`? Go back to the file IntakeIOSim.java.

```java
public void updateInputs(IntakeIOInputs inputs) {
  inputs.intakeAppliedVoltage = simVoltage;
}
```

What does `updateInputs` do if the value of `io` is `IntakeIOTalonFX()`? Go back to the file IntakeIOTalonFX.java.

```java
public void updateInputs(IntakeIOInputs inputs) {
  inputs.intakePositionRadians = intakeTalon.getPosition().getValue().in(Radians); 
  inputs.intakeVelocityRadiansPerSec = intakeTalon.getVelocity().getValue().in(RadiansPerSecond);
  inputs.intakeAppliedVoltage = intakeTalon.getMotorVoltage().getValueAsDouble();
  inputs.intakeCurrentAmperage = intakeTalon.getSupplyCurrent().getValue().in(Amps);
}
```

We need this line of code for us to see the changing values in AdvantageScope:

{% code overflow="wrap" %}
```java
Logger.processInputs("Intake", inputs);
```
{% endcode %}

## 5. Creating Intake commands

Review our goals for what we want the intake to do:

> #### Press the 'A' button on our game controller so that the intake motor voltage goes to 12 volts. Press the 'B' button so that the motor voltage goes to -12 volts. The motor voltage goes back to 0 volts when we let go of the button&#x20;

It looks like we will need at least three ways for the intake to move: move forwards, move backwards, don't move (stop).

### Create command moveForwards()

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;
import edu.wpi.first.wpilibj2.command.Command;

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
  
<strong>  public Command moveForwards() { 
</strong><strong>    return run(
</strong><strong>    () -> {
</strong><strong>     	io.setIntakeVoltage(12);
</strong><strong>    });
</strong><strong>  }
</strong>}
</code></pre>

Every robot program has a CommandScheduler, which is like a box. The box is constantly running any code inside of that box. We can put things inside and take things out of the CommandScheduler box.

{% tabs %}
{% tab title="Version 1" %}
{% code overflow="wrap" %}
```java
public Command nameOfTheCommand() { 
  return run(
  () -> {
   	  /* everything inside the curly brackets goes into the box */
  });
}
```
{% endcode %}
{% endtab %}

{% tab title="Version 2" %}
```java
public Command nameOfTheCommand() { 
  return run(
    () -> /* this line goes into the box */
  );
}
```
{% endtab %}
{% endtabs %}

`run()` isn't the only method we can use to put things into the box.

{% code overflow="wrap" %}
```java
public Command nameOfTheCommand() { 
  return runEnd(
  () -> {
 	 /* this happens while inside the box */
  },
  () -> {
 	 /* this happens when leaving the box */
  });
}
```
{% endcode %}

We could change the command to be more specific: "When I use the command moveForwards, motor voltage goes to 12 volts. When I stop using the command, the voltage goes to 0 volts."

{% code overflow="wrap" %}
```java
public Command moveForwards() { 
  return runEnd(
  () -> {
    io.setIntakeVoltage(12);
  },
  () -> {
    io.setIntakeVoltage(0);
  });
}
```
{% endcode %}

### Create commands moveBackwards() and stop()

We can add the command `moveBackwards()` using negative voltage and command stop() to change voltage to 0.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;
import edu.wpi.first.wpilibj2.command.Command;

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
      io.setIntakeVoltage(12);
    },
    () -> {
      io.setIntakeVoltage(0);
    });
  
<strong>  public Command moveBackwards() { 
</strong><strong>    return runEnd(
</strong><strong>    () -> {
</strong><strong>      io.setIntakeVoltage(-12);
</strong><strong>    },
</strong><strong>    () -> {
</strong><strong>      io.setIntakeVoltage(0);
</strong><strong>    });
</strong><strong>  }
</strong><strong>  
</strong><strong>  public Command stop() {
</strong><strong>    return run(
</strong><strong>    () -> {
</strong><strong>     	io.setIntakeVoltage(0);
</strong><strong>    });
</strong><strong>  }
</strong>}
</code></pre>

We could use `runOnce()` instead of `run()` for the `stop` command. This will make sure it only runs one time.

```java
  public Command stop() {
    return runOnce(
    () -> {
     	io.setIntakeVoltage(0);
    });
  }
```

## 6. Completed Intake.java

Code isn't easy to teach because everyone has their own definition of what is the right way to do stuff, and different teams will need different code. You can see in these examples that there are multiple ways to write the Subsystem commands. These are just two of several ways to prepare this subsystem.

{% tabs %}
{% tab title="Version I'll use" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;
import edu.wpi.first.wpilibj2.command.Command;

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
      io.setIntakeVoltage(12);
    },
    () -> {
      io.setIntakeVoltage(0);
    });
  
  public Command moveBackwards() { 
    return runEnd(
    () -> {
      io.setIntakeVoltage(-12);
    },
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
```
{% endtab %}

{% tab title="Another Version" %}
{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import org.littletonrobotics.junction.Logger;
import edu.wpi.first.wpilibj2.command.Command;

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
    return run(
    () -> {
      io.setIntakeVoltage(12);
    });
  
  public Command moveBackwards() { 
    return run(
    () -> {
      io.setIntakeVoltage(-12);
    });
  }
  
  public Command stop() {
    return run(
    () -> {
     	io.setIntakeVoltage(0);
    });
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

