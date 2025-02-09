# Anti-sleep-alarm-project

#include <Wire.h>
#include <MPU6050.h>

// Define the buzzer and LED pins
int buzzerPin = 9;
int ledPin = 13;

// MPU6050 object
MPU6050 mpu;

// Variables for storing accelerometer readings
float ax, ay, az;
float angleX, angleY;

void setup() {
  // Initialize the serial communication
  Serial.begin(9600);
  
  // Initialize MPU6050
  Wire.begin();
  mpu.initialize();
  
  // Set buzzer and LED as output
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // Read accelerometer values
  mpu.getAcceleration(&ax, &ay, &az);
  
  // Calculate the tilt angle on the X and Y axes
  angleX = atan2(ay, az) * 180.0 / PI;
  angleY = atan2(ax, az) * 180.0 / PI;
  
  // Print the tilt angles for debugging
  Serial.print("Angle X: ");
  Serial.print(angleX);
  Serial.print("  Angle Y: ");
  Serial.println(angleY);
  
  // Check if the tilt angle exceeds a threshold (e.g., 45 degrees)
  if (abs(angleX) > 45 || abs(angleY) > 45) {
    // If tilt exceeds threshold, activate buzzer and LED
    digitalWrite(buzzerPin, HIGH);  // Turn on buzzer
    digitalWrite(ledPin, HIGH);     // Turn on LED
  } else {
    // If the tilt is within limits, turn off buzzer and LED
    digitalWrite(buzzerPin, LOW);   // Turn off buzzer
    digitalWrite(ledPin, LOW);      // Turn off LED
  }
  
  // Delay to prevent rapid triggering
  delay(100);
}

