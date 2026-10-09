---
description: Let's define go and stop!
icon: truck-moving
---

# Create Intake

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## Add a new Subsystem Class Intake to AdvantageKit\_TalonFXSwerveTemplate

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

## Add class variables&#x20;

Add two variables:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

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

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
<strong>import org.littletonrobotics.junction.Logger;
</strong>
public class Intake extends SubsystemBase {
  private final IntakeIO io;
  private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
  
  /** Creates a new Intake. */
  public Intake() {}

  @Override
  public void periodic() {}
}
</code></pre>

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

## Fill in class constructor

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

<details>

<summary>What is a class constructor?</summary>

Every time a new object is created using the class Intake, we call the class constructor once.

The value of the variable `io` inside of `Intake` is set to the value of the parameter `IntakeIO io` we put in while making a new `Intake` object.

Later we will be making these objects in RobotContainer.java.

</details>

## Fill in periodic

The method periodic will be called once per scheduler run (typically once every 20 milliseconds)



