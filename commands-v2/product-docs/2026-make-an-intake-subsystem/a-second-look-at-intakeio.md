---
description: Review concepts that were introduced in IntakeIO
icon: eyes
---

# A second look at IntakeIO

One of the challenges of programming is that there is a lot you need to know, and you kinda need to know all of it at the same in order for things to start making sense. If you need to know A to understand B, and you need B to understand A, then how can you learn anything at all?

Personally I find it easier to learn new things when I have a couple examples to compare from.

This page is a quick "let's make sure you really understand what we have introduced so far".

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection
* [ ] The previously downloaded AdvantageKit\_TalonFXSwerveTemplate robot code
* [ ] The previously downloaded AdvantageKit\_KitBot2026Template robot code

<details>

<summary>Every AdvantageKit subsystem has four file types.</summary>

Open WPILib VS Code and open AdvantageKit\_KitBot2026Template.\
We will look at **drive** as an example subsystem.

1. DriveIO.java
   * Contains the interface, or template, that is used to make the DriveIOSim class and DriveIOTalonFX class.
   * Also contains the static nested class DriveIOInputs to record data.
2. DriveIOSim.java
   * Contains the class to make a 'simulated' drivetrain subsystem
3. DriveIOTalonFX.java
   * Contains the class to make the&#x20;
4. Drive.java
   * The subsystem file that explains what the drivetrain needs to do to drive, stop, get driving distance data and get driving speed data.

{% hint style="info" %}
RobotContainer.java is where we use the DriveIOSim class and DriveIOTalonFX class to make a DriveIOSim object and DriveIOTalonFX object, because we can't run the code inside a class. We need to make an object from the class in order to run the code inside.

RobotContainer.java is also where we connect the button inputs (aka commands) to the actions we created in Drive.java (go forwards, go backwards, etc.)
{% endhint %}

</details>

<details>

<summary>What are comments?</summary>

Comments can't be seen by the computer building the code, but essential to the humans writing the code.

Can you identify some examples of comments in DriveIO.java?

Here's one example:

{% code overflow="wrap" %}
```java
/** Run closed loop at the specified velocity. */
```
{% endcode %}

</details>

<details>

<summary>What is a package?</summary>

A package explains how some code is grouped together. How is the package defined in DriveIO.java?

{% code overflow="wrap" %}
```java
package frc.robot.subsystems.drive;
```
{% endcode %}

Try opening up a file inside of src >> main >> java >> frc >> robot >> subsystems >> drive, for example ModuleIO.java. You will see this because ModuleIO.java is inside the drive folder:

{% code overflow="wrap" %}
```java
package frc.robot.subsystems.drive;
```
{% endcode %}

</details>

<details>

<summary>What is an access modifier?</summary>

An access modifier on an interface:

* Access modifiers change whether other code files can see this interface.
* The interface DriveIO is `public`, so other code files can see it.

An access modifier on a class:

* Changes whether other code files can see this class.
* What is the access modifier for the Drive class inside of Drive.java? The class is `public`, so other code files can see it.

</details>

<details>

<summary>What is an interface?</summary>

This is a template that can be used by other code files.

</details>

<details>

<summary>What are braces?</summary>

Braces help the computer read the code. Java needs braces, but some different programming languages like Python don't need braces.

</details>

<details>

<summary>How to Import and why?</summary>

We import code that was already written so we don't have to write it again in our code file.

In AdvantageKit\_TalonFXSwerveTemplate, open the file RobotContainer.java. You can find many imports at the top of the file, including:

{% code overflow="wrap" %}
```java
import frc.robot.subsystems.drive.ModuleIO;
import frc.robot.subsystems.drive.ModuleIOSim;
import frc.robot.subsystems.drive.ModuleIOTalonFX;
```
{% endcode %}

Control + click on `ModuleIO` (or `ModuleIOSim`, or any of the other imports) to open that code file and look at all the code we didn't have to rewrite.

We use `import` so we can use that code in RobotContainer.java. Eventually we will add the IO, SIM and REAL files we make for our Intake subsystem to this RobotContainer.java:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">import frc.robot.subsystems.drive.ModuleIO;
import frc.robot.subsystems.drive.ModuleIOSim;
import frc.robot.subsystems.drive.ModuleIOTalonFX;
<strong>import frc.robot.subsystems.intake.IntakeIO;
</strong><strong>import frc.robot.subsystems.intake.IntakeIOSim;
</strong><strong>import frc.robot.subsystems.intake.IntakeIOTalonFX;
</strong></code></pre>

Here are some other examples using `import` that we will be using later:

{% code overflow="wrap" %}
```java
import edu.wpi.first.math.util.Units;
```
{% endcode %}

This will allow us to more easily write code converting units (like meters to inches, or feet to centimeters, or radians to degrees).

{% code overflow="wrap" %}
```java
import com.ctre.phoenix6.hardware.TalonFX;
```
{% endcode %}

This will allow us to more easily write code controlling a TalonFX motor in the REAL file.

</details>

<details>

<summary>Variables are usually the first thing you learn when learning coding.</summary>

Let's look at a variable in AdvantageKit\_KitBot2026Template. Navigate to src / main >> java / frc / robot >> subsystems/drive >> DriveIOSim.java.

![](<../.gitbook/assets/unknown (32).png>)

The variable is:

{% code overflow="wrap" %}
```java
private boolean closedLoop = false;
```
{% endcode %}

* What is this variable's access modifier?
  * `private` - only DriveIOSim can see, use and modify this variable.
* What is this variable's data type?
  * `bool` - it must contain true or false
* What is this variable's identifier?
  * `closedLoop`
* What is this variable's value?
  * `false`

{% hint style="info" %}
What happens if you forget to add an access modifier to a variable (or to a class or interface)?

Let's pretend the boolean looks like this instead:

{% code overflow="wrap" %}
```java
boolean closedLoop = false;
```
{% endcode %}

This variable can now be seen by any code inside the package. This level of access is called package-private. Let's go up to the top of the code to look for DriveIOSim's package:

{% code overflow="wrap" %}
```java
package frc.robot.subsystems.drive;
```
{% endcode %}

A package-private variable can be seen by any code that has the same package aka it's inside the folder named drive.&#x20;

<p align="center"><img src="../.gitbook/assets/unknown (33).png" alt="" data-size="original"></p>

These .java files all have the same package as DriveIOSim, so that means any of these code files could now accidentally change `closedLoop`. We don't want that, which is why we make sure `closedLoop` is `private`.
{% endhint %}

</details>

<details>

<summary>Methods have five components.</summary>

1. Modifier
2. Return type
3. Name
4. (optional) Parameter
5. Body

In AdvantageKit\_KitBot2026Template, open the file DriveIO.java (can be found in subsystems >> drive). How many methods are in this interface? What are the methods?

{% code overflow="wrap" %}
```java
public default void updateInputs(DriveIOInputs inputs) {}

public default void setVoltage(double leftVolts, double rightVolts) {}

public default void setVelocity(double leftRadPerSec, double rightRadPerSec, double leftFFVolts, double rightFFVolts) {}
```
{% endcode %}

There are three methods: updateInputs, setVoltage and setVelocity.

Take a look at setVoltage.

{% code overflow="wrap" %}
```java
public default void setVoltage(double leftVolts, double rightVolts) {}
```
{% endcode %}

**What is the Modifier?**

`public`

**What is the Return Type?**

`void`

**What is the method name?**

`setVoltage`

**Does this method have parameters? How many?**

setVoltage has two parameters.

_What is the data type and name of each parameter?_

* `double leftVolts`
  * Data type: double, aka a decimal
  * Name: leftVolts
* `double rightVolts`
  * Data type: double, aka a decimal
  * Name: rightVolts

</details>
