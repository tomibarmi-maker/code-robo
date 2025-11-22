int LS = 2; // Left sensor
int RS = 3; // Right sensor

// Motor control pins
int PWM1 = 10; // Right motor speed
int RM1 = 4;   // Right motor direction 1
int RM2 = 5;   // Right motor direction 2
int PWM2 = 11; // Left motor speed
int LM1 = 6;   // Left motor direction 1
int LM2 = 7;   // Left motor direction 2

// Motor speed
int speed1 = 90; // Right motor speed
int speed2 = 90; // Left motor speed

// Variables for controlling direction
bool reversing = false; // Flag to track whether the robot is reversing

void setup() {
  // Set sensor pins as input
  pinMode(LS, INPUT);
  pinMode(RS, INPUT);

  // Set motor control pins as output
  pinMode(PWM1, OUTPUT);
  pinMode(RM1, OUTPUT);
  pinMode(RM2, OUTPUT);
  pinMode(PWM2, OUTPUT);
  pinMode(LM1, OUTPUT);
  pinMode(LM2, OUTPUT);
}

void loop() {
  // Check if both sensors detect an obstacle (both HIGH)
  if (digitalRead(LS) == 1 && digitalRead(RS) == 1) {
    // Both sensors detect obstacles, reverse for 1 second
    if (!reversing) {
      reversing = true;
      analogWrite(PWM1, speed1);  // Right motor moves backward
      digitalWrite(RM1, LOW);
      digitalWrite(RM2, HIGH);

      analogWrite(PWM2, speed2);  // Left motor moves backward
      digitalWrite(LM1, LOW);
      digitalWrite(LM2, HIGH);
    }
  } 
  // Case when only left sensor detects an obstacle
  else if (digitalRead(LS) == 1 && digitalRead(RS) == 0) {
    // Stop reversing, then turn right
    if (reversing) {
      analogWrite(PWM1, 0);  // Stop right motor
      analogWrite(PWM2, 0);  // Stop left motor
      reversing = false;

      // Turn right
      analogWrite(PWM1, speed1);  // Right motor moves forward
      digitalWrite(RM1, HIGH);
      digitalWrite(RM2, LOW);

      analogWrite(PWM2, speed2);  // Left motor moves backward
      digitalWrite(LM1, LOW);
      digitalWrite(LM2, HIGH);
      delay(500); // Turn right for 0.5 second
    }
  }
  // Case when only right sensor detects an obstacle
  else if (digitalRead(RS) == 1 && digitalRead(LS) == 0) {
    // Stop reversing, then turn left
    if (reversing) {
      analogWrite(PWM1, 0);  // Stop right motor
      analogWrite(PWM2, 0);  // Stop left motor
      reversing = false;

      // Turn left
      analogWrite(PWM1, speed1);  // Right motor moves backward
      digitalWrite(RM1, LOW);
      digitalWrite(RM2, HIGH);

      analogWrite(PWM2, speed2);  // Left motor moves forward
      digitalWrite(LM1, HIGH);
      digitalWrite(LM2, LOW);
      delay(500); // Turn left for 0.5 second
    }
  }
  // Move forward if no obstacles are detected (both sensors LOW)
  else if (digitalRead(LS) == 0 && digitalRead(RS) == 0) {
    // Stop reversing and move forward
    analogWrite(PWM1, speed1);
    digitalWrite(RM1, HIGH);
    digitalWrite(RM2, LOW);

    analogWrite(PWM2, speed2);
    digitalWrite(LM1, HIGH);
    digitalWrite(LM2, LOW);
  }
}
