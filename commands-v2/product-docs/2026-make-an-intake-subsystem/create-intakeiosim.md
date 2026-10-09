---
description: Let's make a virtual motor for our virtual intake.
icon: cart-shopping-fast
---

# Create IntakeIOSim

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code

## Add a new class IntakeIOSim to AdvantageKit\_TalonFXSwerveTemplate

Let's make a IntakeIOSim class using our IntakeIO interface as a template. Open WPILib VS Code and open AdvantageKit\_TalonFXSwerveTemplate.

Open src >> main >> java >> frc >> robot >> subsystems >> intake.

Right click the folder intake >> Create a new class/command

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

The Command Center prompts you to Pick a command. We want to select the command:

{% code overflow="wrap" %}
```
Empty Class Create an empty class
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

The Command Center prompts you to Please enter a class name. Type in IntakeIOSim and press enter. The code should look like this:

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
public class IntakeIOSim {}
```
{% endcode %}

We are going to let the computer know we are using the IntakeIO interface as a template for this class. After the class name want to add `implements IntakeIO` so that the code looks like this:

<pre class="language-java" data-overflow="wrap"><code class="lang-java"><strong>// Copyright (c) FIRST and other WPILib contributors.
</strong>// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
<strong>public class IntakeIOSim implements IntakeIO {}
</strong></code></pre>

Now add the two methods inside the braces so that IntakeIOSim matches IntakeIO. We will also add one new private variable to IntakeIOSim. Check that your code matches the code below.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
public class IntakeIOSim implements IntakeIO {
<strong>  private double simVoltage = 0.0;
</strong>  
<strong>  @Override
</strong><strong>  public void updateInputs(IntakeIOInputs inputs) {}
</strong>
<strong>  @Override
</strong><strong>  public void setIntakeVoltage(double voltage) {}
</strong>}
</code></pre>

### Code breakdown

<details>

<summary>What is <code>@Override</code>?</summary>

We put @Override in front of each method so that IntakeIOSim uses the method we write in IntakeIOSim instead of the default method in the IntakeIO interface (which is blank).



</details>

<details>

<summary>Quick review of the components of a variable:</summary>

{% code overflow="wrap" %}
```java
private double simVoltage = 0.0;
```
{% endcode %}

* What is this variable's access modifier?
  * `private` - only IntakeIOSim will be able to see and edit this variable.
* What is this variable's data type?
  * `double` - a decimal number
* What is this variable's identifier?
  * `simVoltage` - some descriptive name that we make up
* What is this variable's value?
  * `0.0`

</details>

## Fill in the setIntakeVoltage method

Let's make the second method first. We want to use this method to change our variable _simVoltage_ value to the value of the parameter `voltage`.

We will add one line of code inside the method.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">@Override
  public void setIntakeVoltage(double voltage) {
<strong>		this.simVoltage = voltage;
</strong>  }
</code></pre>

{% hint style="info" %}
"=" means "is changed to the value of".

`this.simVoltage` is changed to the value of `voltage`.
{% endhint %}

<details>

<summary>What is this.?</summary>

What if we didn't want to make some special names for all of our class variables so our class variable `simVoltage` was named `voltage`?

Now our class variable and the parameter of `setIntakeVoltage` have the same name.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
public class IntakeIOSim implements IntakeIO {
<strong>  private double voltage = 0.0;
</strong>  
  @Override
  public void updateInputs(IntakeIOInputs inputs) {}

  @Override
  public void setIntakeVoltage(double voltage) {
<strong>    voltage = voltage;
</strong>  }
}
</code></pre>

The parameter variable `voltage` only exists while it's being used inside the method `setIntakeVoltage`.

The value of the parameter `voltage` is changed to whatever the current value the class variable `voltage` is. Then the method concludes, the parameter disappears and our sim voltage value remains unchanged.

This code will run without errors but it doesn't do what we want.

We add `this.` to let the computer know we want the class variable `voltage`, not the parameter `voltage`.

<pre class="language-java"><code class="lang-java">// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
public class IntakeIOSim implements IntakeIO {
<strong>  private double voltage = 0.0;
</strong>  
  @Override
  public void updateInputs(IntakeIOInputs inputs) {}

  @Override
  public void setIntakeVoltage(double voltage) {
<strong>    this.voltage = voltage;
</strong>  }
}
</code></pre>

Yes that does mean adding `this.` to `simVoltage` is redundant:

```java
// this works!
simVoltage = voltage;
```

We know the difference between the parameter and class variable because they have different names.

I include this. because it is good practice to make sure you know the difference between the class variable and parameter.

I get confused easily so I prefer to keep variable and parameter names different (but this will make it more difficult if you for example want to use the same code somewhere else and have to rename all the variables).

</details>

## Fill in the updateInputs method

We will add one line of code inside the method.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">@Override
  public void updateInputs(IntakeIOInputs inputs) {
<strong>    inputs.intakeAppliedVoltage = simVoltage;
</strong>  }
</code></pre>

What is the parameter we put into `updateInputs`?&#x20;

The name is `inputs` and the data type is `IntakeIOInputs`. That's the static nested class we made inside of IntakeIO.java.

```java
// remember this?
@AutoLog
public static class IntakeIOInputs {
  public double intakePositionRadians = 0.0;
  public double intakeVelocityRadiansPerSeconds = 0.0;
  public double intakeAppliedVoltage = 0.0;
  public double intakeCurrentAmperage = 0.0;
}
```

Look back at the line of code we added to the method `updateInputs`:

{% code overflow="wrap" %}
```java
inputs.intakeAppliedVoltage = simVoltage;
```
{% endcode %}

This code says:

* &#x20;"We put `IntakeIOInputs` into the method `updateInputs` and we give `IntakeIOInputs` the name `inputs`.\
  Grab the variable `intakeAppliedVoltage` from inside `inputs` (which is `IntakeIOInputs`).
* `intakeAppliedVoltage` is changed to the value of `simVoltage`.

After we use the method `updateInputs` to change the value of `intakeAppliedVoltage`, `@AutoLog` lets us see the values changing in AdvantageScope.

IntakeIOSim ready! Make sure you understand how these versions are different and that they both do the same thing:

{% tabs %}
{% tab title="Version 1" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
public class IntakeIOSim implements IntakeIO {
  private double simVoltage = 0.0;
  
  @Override
  public void updateInputs(IntakeIOInputs inputs) {}

  @Override
  public void setIntakeVoltage(double voltage) {
    this.simVoltage = voltage;
  }
}
```
{% endtab %}

{% tab title="Version 2" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems.intake;

/** Add your docs here. */
public class IntakeIOSim implements IntakeIO {
  private double voltage = 0.0;
  
  @Override
  public void updateInputs(IntakeIOInputs inputs) {}

  @Override
  public void setIntakeVoltage(double voltage) {
    this.voltage = voltage;
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Speed up fixing indentation in your code with Control + ] and Control + \[
{% endhint %}
