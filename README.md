# Controlling-car-using-arduino-code-with-keyboard-commands
# Arduino code will be transferred to ESP8266 and it will connected to a motor drive in order control the car.All the runtime commands are provided in the code.We need to press the keys in order to move the car
// Motor control pins
const int motor1Pin1 = D1;  // IN1 for Motor 1
const int motor1Pin2 = D2;  // IN2 for Motor 1
const int motor1Enable = D3; // EN1 for Motor 1 (PWM)

const int motor2Pin1 = D6;  // IN1 for Motor 2
const int motor2Pin2 = D5;  // IN2 for Motor 2
const int motor2Enable = D4; // EN2 for Motor 2 (PWM)

// Default speed (0-1023 for ESP8266 PWM)
int motorSpeed = 200 ;

// Function to stop the motors
void stopMotors() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, LOW);
  
  analogWrite(motor1Enable, 0); // Stop motors by setting speed to 0
  analogWrite(motor2Enable, 0);
  
  Serial.println("Motors stopped");
}

void setup() {
  // Initialize serial communication
  Serial.begin(115200);

  // Set motor control pins as outputs
  pinMode(motor1Pin1, OUTPUT);
  pinMode(motor1Pin2, OUTPUT);
  pinMode(motor1Enable, OUTPUT); // PWM pin for motor 1

  pinMode(motor2Pin1, OUTPUT);
  pinMode(motor2Pin2, OUTPUT);
  pinMode(motor2Enable, OUTPUT); // PWM pin for motor 2

  // Stop motors initially
  stopMotors();

  Serial.println("Differential motor drive control with keyboard input and speed control.");
  Serial.println("Use the following keys to control the motors:");
  Serial.println("W: Forward");
  Serial.println("S: Backward");
  Serial.println("A: Forward Left");
  Serial.println("D: Forward Right");
  Serial.println("X: Stop");
  Serial.println("U: Increase speed");
  Serial.println("L: Decrease speed");
  Serial.println("C: Backward Right");
  Serial.println("B: Backward Left"); 
}

void loop() {
  if (Serial.available() > 0) {
    char input = Serial.read();

    switch (input) {
      case 'W': // Forward
      case 'w':
        moveForward();
        break;
      case 'S': // Backward
      case 's':
        moveBackward();
        break;
      case 'A': // Left
      case 'a':
        turnForwardLeft();
        break;
      case 'D': // Right
      case 'd':
        turnForwardRight();
        break;
      case 'X': // Stop
      case 'x':
        stopMotors();
        break;
      case 'U': // Increase speed
      case 'u':
        increaseSpeed();
        break;
      case 'L': // Decrease speed
      case 'l':
        decreaseSpeed();
        break;
      case 'C':
      case 'c':
        turnBackwardRight();
        break;
      case 'B':
      case 'b':
        turnBackwardLeft();
        break;
      default:
        Serial.println("Invalid input. Use W/A/S/D/X/U/L.");
        break;
    }
  }
}

// Function to move the robot forward
void moveForward() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
  
  analogWrite(motor1Enable, motorSpeed); // Control speed with PWM
  analogWrite(motor2Enable, motorSpeed);
  
  Serial.print("Moving forward at speed: ");
  Serial.println(motorSpeed);
}

// Function to move the robot backward
void moveBackward() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);
  
  analogWrite(motor1Enable, motorSpeed); // Control speed with PWM
  analogWrite(motor2Enable, motorSpeed);
  
  Serial.print("Moving backward at speed: ");
  Serial.println(motorSpeed);
}

// Function to turn the robot left
void turnForwardLeft() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1,LOW);
  digitalWrite(motor2Pin2, LOW);
  
  analogWrite(motor1Enable, motorSpeed); // Control speed with PWM
  analogWrite(motor2Enable, motorSpeed);
  
  Serial.print("Turning left at speed: ");
  Serial.println(motorSpeed);
}

// Function to turn the robot right
void turnForwardRight() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
  
  analogWrite(motor1Enable, motorSpeed); // Control speed with PWM
  analogWrite(motor2Enable, motorSpeed);
  
  Serial.print("Turning right at speed: ");
  Serial.println(motorSpeed);
}

void turnBackwardRight() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, LOW);
  
  analogWrite(motor1Enable, motorSpeed); // Control speed with PWM
  analogWrite(motor2Enable, motorSpeed);
  
  Serial.print("Turning Backwardright at speed: ");
  Serial.println(motorSpeed);
}

void turnBackwardLeft() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);
  
  analogWrite(motor1Enable, motorSpeed); // Control speed with PWM
  analogWrite(motor2Enable, motorSpeed);
  
  Serial.print("Turning BackwardLeft at speed: ");
  Serial.println(motorSpeed);
}
// Function to increase motor speed
void increaseSpeed() {
  motorSpeed += 100; // Increment by 100
  if (motorSpeed > 1023) motorSpeed = 1023; // Max PWM value for ESP8266
  
  Serial.print("Increased speed to: ");
  Serial.println(motorSpeed);
}

// Function to decrease motor speed
void decreaseSpeed() {
  motorSpeed -= 100; // Decrease by 100
  if (motorSpeed < 0) motorSpeed = 0; // Min PWM value
  
  Serial.print("Decreased speed to: ");
  Serial.println(motorSpeed);
}
  
