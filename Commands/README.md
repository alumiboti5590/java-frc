## Commands & Their Classes

Commands are the actions of the Robot. When a button on the controller is hit, it will call an Event Listener that will then cause your command to operate on a specified subsystem. A Command needs these features to be a fully functioning command.

1. Must inherit from `Command` in package `edu.wpi.first.wpilibj.command.Command`. This will let the robot use them.
2. Must have a Constructor that takes no values or any number of values based on what is needed.
  * Constructor must have a line using `requires(Subsystem subsystem)` that will tell the Command what system is needed.
    * Ex: 
    ```java
    public Drive() {
    	requires(Robot.drivetrain);  // Robot imported elsewhere
    }
    ```
3. `protected void initialize()`: This function will be called when the Command is begun. Use it to set needed susbsystem to a position where it can correctly execute its job.
4. `protected void execute()`: This function will be called after `initialize()` and will contain the calls made to the subsystem to actually perform the action. It will be called continuously until stopped or interrupted.
5. `protected boolean isFinished()`: This function will be called every 20 milliseconds to determine if the command should finish or continue calling `execute()`.
6. `protected void end()`: This function will be called afted `isFinished()` returns true. It should be used to stop any peripherals/motors that are moving, and reset their positions.
7. `protected void interrupted()`: Very similar to `end()`, but this is called if the command is interrupted by another Command or if something goes horribly, horribly wrong.

*If you want a command to timeout after a certain amount of time, call `setTimeout(double seconds)` (void) in the constructor or in a function, and then check with `isTimedOut()` (boolean).*

**ALWAYS MAKE SURE THERE IS A FOOLPROOF WAY FOR A COMMAND TO END.** You don't want to be the one who blows up the robot because its Drivetrain kept driving full speed when a driver pressed something else.

![Wasted Robot](https://i.ytimg.com/vi/Ip3TXi88pDQ/maxresdefault.jpg)

Commands usually have limited logic code, but call a lot of Subsystem functions to do the work for them. Remember, the Command controls the Subsystem, *NOT THE ROBOT*.

An example Command and its subsystem is below.

**RotateLeft.java**
```java
import edu.wpi.first.wpilibj.command.Command;
import org.usfirst.frc.team5590.robot.subsystems.*;
import org.usfirst.frc.team5590.robot.Robot;

/**
 * Rotates the robot a certain number of degrees
 * (The amount passed through the constructor)
 */
public class Rotate extends Command {
	
	public Drivetrain drivetrain  = Robot.drivetrain;  // Remember the drivetrain because we'll use it
	
	private double degrees;

    public Rotate(double degrees) {
        requires(drivetrain);                       // Uses the drivetrain subsystem
        this.degrees = degrees;                     // Remember how far to turn
        //TODO Find time for rotation per second
        double rotationPerSecond = .1;
        setTimeout(degrees*rotationPerSecond);      // How long to run the command for
    }

    // Called just before this Command runs the first time
    protected void initialize() {
    	drivetrain.stop();                             // Make sure the drivetrain is stopped.
    	drivetrain.rotateRight(.5);                    // Call 'rotate' on the drivetrain to rotate it right
    }

    // Called repeatedly when this Command is scheduled to run
    protected void execute() {
    }

    // Make this return true when this Command no longer needs to run execute()
    protected boolean isFinished() {
        return isTimedOut();                        // Finish when timed out
    }

    // Called once after isFinished returns true
    protected void end() {
    	drivetrain.stop();
    }

    // Called when another command which requires one or more of the same
    // subsystems is scheduled to run
    protected void interrupted() {
    	drivetrain.stop();
    }
}
```

**Drivetrain.java**
```java
public class Drivetrain extends Subsystem {
  
  // FRC's library to drive a robot
	 private static RobotDrive robotDrive;
  
  /**
	 * Rotates the robot right
	 * @param speed
	 */
	 public void rotateRight(double speed) {
		 robotDrive.tankDrive(speed, -speed);
		 System.out.println("Speed: " + speed);
	 }
}
```
