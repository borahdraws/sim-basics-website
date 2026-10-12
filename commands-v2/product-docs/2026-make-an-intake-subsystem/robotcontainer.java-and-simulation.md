---
description: >-
  Almost there! We are so close to seeing the results of all the programming:
  press a button and see something MOVE
icon: gamepad-modern
---

# RobotContainer.java & Simulation!

## What you'll need

* [ ] A computer with WPILib installed
* [ ] The modified AdvantageKit\_TalonFXSwerveTemplate robot code
* [ ] A game controller, ideally the one the driver will use for competitions

## 1. Open RobotContainer.java

Open WPILib VS Code and open AdvantageKit\_TalonFXSwerveTemplate.&#x20;

Open src >> main >> java >> frc >> robot. Notice there is a file called RobotContainer.java.

Open the RobotContainer and it looks like a big mess.

## 2. Add Intake variable

The way drive is added gives us a hint on how to add intake.&#x20;

At the very top you will see that a `Drive` variable already exists for the `drive` subsystem. We add a variable for our `intake` right next to it (import `Intake` with Quick Fix...)

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// bunch of code up there

public class RobotContainer {
  // Subsystems
  private final Drive drive;
<strong>  private final Intake intake;
</strong>  
// bunch of code down there
</code></pre>

## 3. Create new CommandXboxController variable & object

In order to use a controller, we need a controller object created from the game controller class.&#x20;

Create a second controller variable below the one in the template. We will assign it to a new object we create:

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// bunch of code up there

public class RobotContainer {
  // Subsystems
  
  // Controller
  private final CommandXboxController controller = new CommandXboxController(0);
<strong>  private final CommandXboxController gunnerController = new CommandXboxController(1);
</strong>  
// bunch of code down there
</code></pre>

## 4. Add intake subsystem to RobotContainer class constructor

ok this is about to be so much code but I am shrinking stuff and removing comments which makes it much smaller.&#x20;

{% code overflow="wrap" %}
```java
// bunch of code up there
  public RobotContainer() {
    switch (Constants.currentMode) {
      case REAL:
        // REAL Drive code
        break;

      case SIM:
        // SIM Drive code
        break;

      default:
        // blank Drive code aka replay mode
        break;
    }
// bunch of code down there
```
{% endcode %}

### Switch Case

This is how we can change the mode of the robot to SIM or some other mode.

{% code overflow="wrap" %}
```java
switch (someThingy) {
  case iDontMatchTheThingy:
    // this stuff doesnt happen
    break;

  case iMatchTheThingy:
    // this stuff happens
    break;

  default:
    // just in case nothing matches the thingy
    break;
    }
```
{% endcode %}

Let's add our intake to each case (and import with Quick Fix...).

<pre class="language-java" data-overflow="wrap"><code class="lang-java">// bunch of code up there
  public RobotContainer() {
    switch (Constants.currentMode) {
      case REAL:
        // REAL Drive code
<strong>        intake = new Intake(new IntakeIOTalonFX());
</strong>        break;

      case SIM:
        // SIM Drive code
<strong>        intake = new Intake(new IntakeIOSim());
</strong>        break;

      default:
        // blank Drive code aka replay mode
<strong>        intake = new Intake(new IntakeIO() {});
</strong>        break;
    }
// bunch of code down there
</code></pre>

## 5. Button mapping

We are going to add our intake button mapping here:

{% code overflow="wrap" %}
```java
private void configureButtonBindings() {
  // all the drive commands
}
```
{% endcode %}

How do we know what to write for each button? You need to look in the documentation.

Go to https://docs.wpilib.org/, then look for API Documentation for Java. Documentation is sort of like a dictionary (anyone still using those?) to learn what we can put into our code.

Find and click on **edu.wpi.first.wpilibj2.command.button**.&#x20;

There are lots of different controllers! Open **CommandXboxController**. We can see a list of all the different buttons we can use.

Programmers want to be in close communication with their drivers to learn what robot movements the drivers want and what button bindings they prefer.

I will use left and right bumper for the intake. While we are holding down left bumper, the intake will move forwards. While holding down left trigger, the bumper will move backwards.

We can also add a default command. If our subsystem code had `run()` instead of `runEnd()`, the intake would keep moving after we let go of the buttons. We want the intake to stop right away, so we want the default command to be **stop**.

<pre class="language-java" data-overflow="wrap"><code class="lang-java">private void configureButtonBindings() {
  // all the drive commands
  
  intake.setDefaultCommand(intake.Stop());

<strong>  gunnerController
</strong><strong>    .leftBumper()
</strong><strong>    .whileTrue(
</strong><strong>      intake.moveForwards()
</strong><strong>    );
</strong>      
<strong>  gunnerController
</strong><strong>    .leftTrigger()
</strong><strong>    .whileTrue(
</strong><strong>      intake.moveBackwards()
</strong><strong>    );
</strong>}
</code></pre>

## 6. Run the Simulation

If everything was written right the sim will be running first try. If not, congrats you've encountered the first of countless failures welcome to programming this is fun promise 🙃

If you need a more detailed walkthrough of the summary below you can go [back to the sim lesson](../).

Plug in your game controller. It is helpful to make sure the game controller works first by opening an online gamepad tester.

Click the **Open WPILib Command Palette** icon in the top right, and type in **Simulate Robot Code**. Once the code is done compiling, the Command Center will prompt you again. Check the box next to **Sim GUI**, then choose **OK**.&#x20;

Click and drag the name of your controller from System Joysticks to Joystick\[1].

Click the **Open WPILib Command Palette** icon again, and type **Start Tool**. The Command Center will prompt you to Pick a tool. Select **AdvantageScope**.

{% hint style="danger" %}
Remember the sim only works when Robot State is Teleoparated AND Robot Simulation is the active, selected window. NOT AdvantageScope
{% endhint %}

Go to AdvantageScope. Go to File >> Connect to Simulator >> Default: NetworkTables 4.&#x20;

Click the plus to add 📉 Line Graph.

In the left column of AdvantageScope click the triangles to open up AdvantageKit >> Intake. Do the four values inside look familiar? These are the four variables we created inside of the static nested class `IntakeIOInputs`:

```java
public static class IntakeIOInputs {
  public double intakePositionRadians = 0.0;
  public double intakeVelocityRadiansPerSeconds = 0.0;
  public double intakeAppliedVoltage = 0.0;
  public double intakeCurrentAmperage = 0.0;
}
```

Now these four values are in AdvantageScope for us to look at. Click and drag IntakeAppliedVoltage to the box in the bottom area labeled Left Axis.

Go back to the Robot Simulation window. Click on Teleoperated to change the Robot State from Disconnected to Teleoperated.

![](<../.gitbook/assets/unknown (38).png>)

When you press the left bumper, we should see the graph spike up to INTAKE\_VOLTAGE, and when we let go the graph will go back to zero. When you press the left trigger, we should see the graph spike down negative INTAKE\_VOLTAGE, and then when we let go the graph will go back to zero.

Congrats on completing your first subsystem! 🥳
