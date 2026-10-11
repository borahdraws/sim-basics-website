---
description: This controller gives me never ending grief
---

# 8bitdo Ultimate 2C Controller never ending grief

## Bluetooth

Bluetooth probably won't work, plug in the USB-C wire.

## Drivers

If that also doesn't work, try downloading the 8BitDo Ultimate Software V2 controller driver. You need to open the .exe and have it running in the background.

## Button Mapping

When Sim GUI opens up we can see these inputs change when we move the joysticks and press the buttons:



The object `CommandXboxController` cannot be used with the 8bitdo Ultimate 2C Controller because the 8bitdo changes the numbers slightly different from a plain Xbox controller. For example it thinks the 8bitdo X button is the Y input, the left bumper doesn't register as a left bumper input etc..

If you open the CommandXboxController you will see that it extends the CommandGenericHID class.

Somewhere in your code, create a new class called EightBitDoController and extend CommandGenericHID.

Add a constructor class and `XboxController` variable, just like in the `CommandXboxController` class.

Create new triggers for each button and joysticks that have the corrected inputs.

{% code overflow="wrap" %}
```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot;

import edu.wpi.first.wpilibj.XboxController;
import edu.wpi.first.wpilibj2.command.button.CommandGenericHID;
import edu.wpi.first.wpilibj2.command.button.Trigger;

/** Add your docs here. */
public class EightBitDoController extends CommandGenericHID {
  private final XboxController m_hid;

  public EightBitDoController(int port) {
    super(port);
    m_hid = new XboxController(port);
  }

  public Trigger a() { return button(1); }
  public Trigger b() { return button(2); }
  public Trigger x() { return button(4); }
  public Trigger y() { return button(5); }

  public Trigger leftBumper() { return button(7); }
  public Trigger rightBumper() { return button(8); }
  public Trigger leftTrigger() { return button(9); }
  public Trigger rightTrigger() { return button(10); }

  public Trigger pressLeftJoystick() {return button(14);}
  public Trigger pressRightJoystick() {return button(15);}

  public double getLeftX() {
    return m_hid.getRawAxis(0);
  }

  public double getLeftY() {
    return m_hid.getRawAxis(1);
  }

  public double getRightX() {
    return m_hid.getRawAxis(3);
  }

  public double getRightY() {
    return m_hid.getRawAxis(4);
  }
}
```
{% endcode %}



