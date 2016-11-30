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

**ALWAYS MAKE SURE THERE IS A FOOLPROOF WAY FOR A COMMAND TO END.** You don't want to be the one who blows up the robot because its Drivetrain kept driving full speed when a driver pressed something else.

![Wasted Robot](https://i.ytimg.com/vi/Ip3TXi88pDQ/maxresdefault.jpg)

Commands usually have limited logic code, but call a lot of Subsystem functions to do the work for them. Remember, the Command controls the Subsystem, *NOT THE ROBOT*.
