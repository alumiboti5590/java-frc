## Controllers & Their Classes

Within the `controllers` package that we created with the Project Structure, we can now start creating a controller to use with the robot. Most notably we use the XBox 360 Controller, so we will use that as an example. Using a Port Mapping Software, figure out what port number each of the buttons/axis directly relate to. This is VERY IMPORTANT. You can use [previous year's code][1] as an example or just copy it, BUT UNDERSTAND IT either way. You will need a constructor and getters for all of the buttons.

```java
public class XboxController extends Joystick {
	
	public Button button1;
	
	public XboxController(int port) {
		super(port);
		button1 = new JoystickButton(this, 1);
	}

  public boolean getButton1() {
    return this.button1.get();
  }

	public double getThumbstick() {
		return this.getRawAxis(1);
	}
  
}
```



[1]: https://github.com/sjcirobotics/Stronghold/blob/staging/src/org/usfirst/frc/team5590/robot/controllers/XboxController.java
