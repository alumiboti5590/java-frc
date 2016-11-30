## RobotMap Class

`RobotMap.java` is where you will create all of your connections to your peripherals, such as motors, solenoids, compressors, and so on. It gets called on the startup of the Robot to ensure that everything is properly wired. 

For our purposes, `RobotMap.java` will be very bare, as it will do no instantiation of the peripherals itself, but just call the separate subsystems to run their own creation scripts. This setup allows ease of control/configuration because it will enclose all of the information about a subsystem within its own subsystem class. Below is an example.

**Drivetrain.java (in subsystems)**
```java
public class Drivetrain extends Subsystem {

  // Declare the ports as static final variables in the class.
	private static final int leftTrackPWM = 1;
	private static final int rightTrackPWM = 0;

	// FRC's library to drive a robot
	private static RobotDrive robotDrive;
	
  .
  .   Other variables needed
  .

	/**
	 * Initializes Talon Speed Controllers without needing an existing instance.
	 */
	public static void initializeControllers() {

    // Create the speed controllers connected to their correct ports
		SpeedController leftTrackController = new TalonSRX(leftTrackPWM);
		SpeedController rightTrackController = new TalonSRX(rightTrackPWM);

    // Create a new RobotDrive and assign it to the subsystem's variable.
		robotDrive = new RobotDrive(leftTrackController, rightTrackController);
		robotDrive.setSafetyEnabled(false);
	}
  
  .
  .    Other class functions 
  .
 
}
```

**RobotMap.java**
```java
import org.usfirst.frc.team5590.robot.subsystems.Drivetrain;

public class RobotMap {
    
    public static void init(){
    
      // Call that function from DriveTrain to create itself
    	Drivetrain.initializeControllers();
    }
}
```
