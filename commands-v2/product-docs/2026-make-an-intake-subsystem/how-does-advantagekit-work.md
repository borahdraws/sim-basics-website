---
description: Start with understanding how AdvantageKit robot code is structured and why
icon: umbrella
---

# How does AdvantageKit work?

## What you'll need

* [ ] A computer with WPILib installed
* [x] Internet connection

## Download the AdvantageKit\_KitBot2026Template robot code

In the 2026 FRC game, the goals was to drive the robot around and shoot balls into a goal.

FRC designs a 'kitbot' for teams that want to assemble a premade robot kit. FRC Team 6328: Mechanical Advantage has written robot code to control and record what this kitbot does. We will look at this code to explain the AdvantageKit structure.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

***

The downloading process is the same as downloading the AdvantageKit\_TalonFXSwerveTemplate we downloaded in the previous lesson.

Go to [docs.advantagekit.org](http://docs.advantagekit.org) and find the link to go to AdvantageKit's Github in the top right corner.

In the rightmost column, scroll down to find Releases and click on the latest release. Under Assets, download and extract AdvantageKit\_KitBot2026Template.zip.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Open WPILib VS Code and open the AdvantageKit\_KitBot2026Template folder.

In the leftmost column (VS Code calls it the Activity Bar), make sure the icon with two paper aka Explorer is selected to see our robot code.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

If you look at the big left column of VS Code (called the Side Bar), you will see there are several robot code files (ending in .java) and folders.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Technically you could try to write everything in a single file of code but that would be very difficult to scroll through to figure out what's in there, what's missing, what's working, what's not working and why.

Under AdvantageKit\_KitBot2026Template, open src >> main >> java >> frc >> robot >> subsystems.

VS Code will condense the folders to look like src / main >> java / frc / robot >> subsystems because it couldn't find code files to show inside src, java and frc.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

There are two subsystems in this kitbot code:

<table data-card-size="large" data-view="cards"><thead><tr><th align="center"></th><th></th></tr></thead><tbody><tr><td align="center">Drive</td><td>The drivetrain subsystem that lets the robot move around</td></tr><tr><td align="center">Superstructure</td><td><p>The subsystem that lets the robot eject, intake, and launch balls.</p><p></p><p>The Superstructure has two motors, the intake motor and the feeder (shooter) motor.</p></td></tr></tbody></table>

Look inside both the drive and superstructure folders. Both subsystems have at least four file types.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>SuperstructureConstants.java is something that makes the code more complicated but saves lots of time later, we will look at it later</p></figcaption></figure>

<table data-card-size="large" data-view="cards"><thead><tr><th align="center"></th><th></th></tr></thead><tbody><tr><td align="center"><strong>SubsystemIOSim.java</strong></td><td>The SIM file lets us control a <strong>sim</strong>ulated, virtual version of the motors on a virtual robot. </td></tr><tr><td align="center"><strong>SubsystemIOTalonFX.java</strong></td><td><p>The REAL file lets us control the real motors on a real robot. In this case, the motors are TalonFXs.</p><p></p><p>The kitbot template also contains SuperstructureIOSpark and SuperstructureIOTalonSRX, so instead of TalonFX motors you might use Spark motors or TalonSRX motors.</p></td></tr><tr><td align="center"><strong>SubsystemIO.java</strong></td><td><p>IO stands for Input Output. Start here when making a new subsystem.  </p><p></p><p>The IO file acts like a template to help set up the SIM and REAL files. Because we use this IO template, we can make sure that the SIM and REAL robot have the same motors and can be controlled with the same code.</p><p></p><p>Just as important, this IO file contains a box called SubsystemIOInputs that records data. Regardless of whether we drove the SIM robot or REAL robot we can replay the data (and see what went wrong 💀)</p></td></tr><tr><td align="center"><strong>Subsystem.java</strong></td><td><p>You might see this with Subsystem after the hardware name (Intake.java might be called IntakeSubsystem.java)</p><p></p><p>This is the file where we define what we want the subsystem to do based on the code in the IO files.</p><p></p><p>In this kitbot, the intake motor and the feeder (shooter) motor must always work together to keep balls from getting stuck, which is why the motors were coded together in one Superstructure subsystem.</p></td></tr></tbody></table>

## Why is the robot code designed this way?

{% stepper %}
{% step %}
Start with SubsystemIO.java so that we get two things:

1. Output: Make sure the SIM robot and REAL robot have the same hardware structure.
2. Input: Make sure we can record what the subsystem hardware is doing.
{% endstep %}

{% step %}
Make a SubsystemIOSim.java that implements SubsystemIO. SubsystemIOSim creates a virtual version of the hardware. Now while creating Subsystem.java, we can see exactly what our code does without a real robot.
{% endstep %}

{% step %}
Make Subsystem.java so that we can tell the hardware what to do, regardless of whether the hardware is SIM or REAL (What should the motors do when we say "start"? What should the motors do when we say "stop"?)
{% endstep %}

{% step %}
Add some game controller button mapping to the actions we created in Subsystem.java (maybe "start" is "press the left bumper" and "stop" is "press the left trigger") This will go inside RobotContainer.java which can be found in src >> main >> java >> frc >> robot
{% endstep %}

{% step %}
#### Our goal is to press that same button, but instead of spinning a virtual motor, we spin a real motor.&#x20;

Make a SubsystemIOReal.java that implements SubsystemIO. SubsystemIOReal can control the real life subsystem hardware.

(Now we can press the left trigger to stop the SIM subsystem or REAL subsystem depending on the mode we choose)
{% endstep %}

{% step %}
#### We can see data in real time, or we can make a recording to replay later.

Maybe the real robot does something really weird during a game competition match. We can see exactly what happened and how the motor voltages changed during those two minutes and fifteen seconds.
{% endstep %}
{% endstepper %}

### Another reason to structure the code using AdvantageKit:

What if the team decides "hey we can't use Krakens (TalonFX) anymore we need to swap to NEOs (SparkMax)"&#x20;

We want code files to know as little about each other as possible (this is called **decoupling**)&#x20;

👎Code that isn't decoupled: you change one thing, now we have to change everything because it's connected to the code we change.

👍Decoupled code: easier to change, because we can put aside the code that isn't connected to the code we are changing. We know which parts of code won't be affected by the change because they aren't connected to the changed code.

We are able to swap out the motor type in SubsystemIOReal.java without needing to change a line of code in Subsystem.java. That's much less work and less chance to make errors 🥳

This concept is called **dependency injection**.&#x20;

### Recap of dependency injection:

* Subsystem.java explains "go forwards" and "go backwards" without caring about the kind of motors it is using
* It's the files that implement the interface SubsystemIO (SubsystemIOSim.java and SubsystemIOReal.java) that actually explain what "go forwards" and "go backwards" does to the subsystem's motors.
  * SubsystemIOSim.java will explain how to change the voltage of fake virtual motors.
  * SubsystemIOTalonFX.java will explain how to change the voltage of TalonFX motors.
