# Sensors and Feedback Lab

In this lab you will configure a gear on a motor to always face the direction that it was intialized.

## Diagram

- Whenever the board turns clockwise, the motor should turn the same amount counterclockwise
    - And vice-versa
- This way the gear on the motor will always face the way it was initialized

![diagram of the motor board](images/MotorBoardDiagram.png)

## Creating a Project

1. Create a new WPILib Java Project
2. Type `Ctrl + Shift + P`
3. Then search for the `WPILib: Create a new project` option
4. Then fill out the boxes accordingly
    - Select a project type: `Template -> java -> Timed Robot`
    - Base Folder: you decide where to put this, since it is your computer
    - Project Name: `NRG948FeedbackLab`
    - Team Number: `0948`
5. Then click generate project

## Importing Vendor Libraries

1. Go to the tab on the side with the WPILib logo
2. Then scroll and install `CTRE-Phoenix` and `REVLib`

![vendor libraries image](images/VendorLibraries.png)

3. The other libraries aren't needed

## Creating the variables for the Encoder and Gyro

1. Go to `Robot.java` which is under `src/main/java/frc/robot`
2. Go to the right place as shown by the below code block

```java
public class Robot extends TimedRobot {
  private static final String kDefaultAuto = "Default";
  private static final String kCustomAuto = "My Auto";
  private String m_autoSelected;
  private final SendableChooser<String> m_chooser = new SendableChooser<>();

  // [your-code-goes-here]
```

3. Create a variable for a motor controller, relative encoder, and gyro

```java
  SparkMax neoMotorController = new SparkMax(3, MotorType.kBrushless);
  RelativeEncoder neoMotorEncoder;
  Pigeon2 gyro = new Pigeon2(1);
  StatusSignal<Angle> boardRotation;
```

4. Now right below that, create another variable for our feedback system
    - Note: this variable is our feedback system; we will cover how a PID controller works in the future.

```java
    PIDController feedbackSystem = new PIDController(0.005, 0, 0);
```

5. Now go find the `Robot()` method for the `Robot` class (as shown below)
    - This method will be the first part of the code to run

```java
public Robot() {
    m_chooser.setDefaultOption("Default Auto", kDefaultAuto);
    m_chooser.addOption("My Auto", kCustomAuto);
    SmartDashboard.putData("Auto choices", m_chooser);
  }
```

6. Inside the curly braces go ahead and configure the relative encoder and feedback system

```java
    boardRotation = gyro.getYaw();
    neoMotorEncoder = neoMotorController.getEncoder();
    feedbackSystem.setSetpoint(0.0); // The value the feedback system is trying to get to
    feedbackSystem.setTolerance(0.75); // How many degrees off the system can be and still be fine
    feedbackSystem.enableContinuousInput(0, 360); // Allows it to wrap angles around from 0 degrees to 360 degrees
```

## Calculating the Motor Movement

1. Go to the `robotPeriodic()` method
2. Now to able to seek to a specific angle, we need to know the rotation of the board and angle of the motor
3. So you need to find out how to get the motor angle off the `RelativeEncoder`
    - Note: I already gave you how to find the board's rotation!

```java
    double currentMotorAngle = // [insert-method-here]
    double currentBoardRotation = boardRotation.refresh().getValueAsDouble() % 360;
```

4. Now, we need to calculate the speed the motor should go at

```java
    double speed = feedbackSystem.calculate(currentMotorAngle, -currentBoardRotation);
    // Tells the feedback system the current motor angle and then tells it rotate the opposite amount of the board's rotation

    if (feedbackSystem.atSetpoint()) { // If already at the goal angle, then don't move the motor
      speed = 0.0;
    }
```

5. Lastly, set the speed of the motor

```java
    neoMotorController.set(MathUtil.clamp(speed, -0.5, 0.5));
    // The speed is limited to -0.5-0.5 because we don't want the motor moving really fast
```

6. Now you're done, build your code and test!
