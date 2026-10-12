---
hidden: true
---

# Page 1

### `double JKgMetersSquared`

The moment of inertia J of the motor.

This is a value to help the simulation figure out how hard the motor has to work to start spinning, or how hard it has to work to slow it down once it's spinning.

Usually J is between 0.0001 (a light intake roller) and 0.05 (a heavy arm or flywheel)

In this example, we are using a method called `estimateMOI()` to roughly calculate moment of inertia J. The method `estimateMOI()` needs two parameters (which we can find info about by using control + click on the method):

{% stepper %}
{% step %}
### `double lengthMeters`

Length of arm in meters.

In this example, if we look at our drawing, our arm is 12 inches. We use Units to convert the 12 inch measurement to meters.
{% endstep %}

{% step %}
### `double massKg`

Mass of arm in Kg.

In this example, we use Units to convert the arm weight estimate from lbs to kgs.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
-90° = the arm is pointing straight down.

-45° = the arm is pointing down at an angle.

0° = the arm is pointing directly sideways.

45° = the arm is pointing up at an angle.

90° = the arm is pointing straight up.
{% endhint %}

<pre><code><strong>    // Celsius = (Fahrenheit - 32) * 5.0 / 9.0
</strong>    inputs.elevatorLeaderTempCelsius = (77 - 32) * 5.0 / 9.0;
    inputs.elevatorFollowerTempCelsius = (77 - 32) * 5.0 / 9.0;
</code></pre>
