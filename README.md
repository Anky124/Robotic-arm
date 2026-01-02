# Robotic-arm
  Project Title: 5-DOF Bluetooth Robotic Arm Controller .It features a custom-built Android application created with MIT App Inventor that communicates over Bluetooth to an Arduino Uno.
# 5-DOF Robotic Arm Controller (Bluetooth & Serial)

An Arduino-based control system for a 5-Degree of Freedom (DOF) robotic arm. This project supports manual control via a custom MIT App Inventor Android application (Bluetooth HC-05) and direct Serial Monitor commands.

## Features
* **Multi-Mode Control:** Use sliders on a mobile app or type commands in the Arduino Serial Monitor.
* **5-Joint Support:** Controls Shoulder, Elbow, Wrist Pitch, Wrist Roll, and Gripper.
* **Neutral Start:** Servos automatically initialize to 90 degrees to prevent mechanical strain.

## Hardware Components
* Arduino Uno
* 5x Servo Motors(2:-MG968R, 3:-servo)
* HC-05 Bluetooth Module
* External 5V Power Supply
* Connecting Wires
* 3D Printed 5 DOF Robotic Arm

## Command Syntax
Commands are sent as a Letter + Number (0-180):
- `S[angle]` : Shoulder
- `E[angle]` : Elbow
- `P[angle]` : Wrist Pitch
- `R[angle]` : Wrist Roll
- `G[angle]` : Gripper


Code Used IN arduino IDE:
Define Servo objects for the 5 degrees of freedom
Servo shoulderServo;
Servo elbowServo;
Servo wristPitchServo;
Servo wristRollServo;
Servo gripperServo;

void setup() {
  // Start Serial communication for the Bluetooth module (HC-05/06)
  Serial.begin(9600);

  // Attach servos to PWM pins on your Arduino
  shoulderServo.attach(3); 
  elbowServo.attach(5);
  wristPitchServo.attach(6);
  wristRollServo.attach(9);
  gripperServo.attach(10);

  // Set all servos to a neutral home position (90 degrees)
  shoulderServo.write(90);
  elbowServo.write(90);
  wristPitchServo.write(90);
  wristRollServo.write(90);
  gripperServo.write(90);
}

void loop() {
  // Check if data is available from the Bluetooth module
  if (Serial.available() > 0) {
    // Read the prefix character (S, E, P, R, or G)
    char jointLabel = Serial.read();
    
    // Read the integer value following the character
    int angleValue = Serial.parseInt();

    // Move the corresponding servo based on the prefix
    switch (jointLabel) {
      case 'S': shoulderServo.write(angleValue);   break;
      case 'E': elbowServo.write(angleValue);    break;
      case 'P': wristPitchServo.write(angleValue); break;
      case 'R': wristRollServo.write(angleValue);  break;
      case 'G': gripperServo.write(angleValue);    break;
    }
  }
}
