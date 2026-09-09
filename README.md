#include <LiquidCrystal.h>

// Pins
const int pressurePin = A0;
const int greenLED = 9;
const int redLED = 8;
const int buzzer = 10;

// LCD: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Pressure limits
const float MAX_PRESSURE = 100.0;
const float LOW_PRESSURE = 25.0;

void setup() {
  pinMode(greenLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(buzzer, OUTPUT);

  lcd.begin(16, 2);
  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("TYRE PRESSURE");

  lcd.setCursor(0, 1);
  lcd.print("MONITOR SYSTEM");

  delay(2000);
  lcd.clear();
}

void loop() {

  // Read potentiometer / pressure sensor
  int sensorValue = analogRead(pressurePin);

  // Convert sensor value to pressure
  float pressure = (sensorValue / 1023.0) * MAX_PRESSURE;

  if (pressure < LOW_PRESSURE) {

    // LOW PRESSURE
    digitalWrite(greenLED, LOW);
    digitalWrite(redLED, HIGH);
    digitalWrite(buzzer, HIGH);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("TYRE PRESSURE");
    lcd.setCursor(0, 1);
    lcd.print("LOW!");

  } else {

    // NORMAL PRESSURE
    digitalWrite(greenLED, HIGH);
    digitalWrite(redLED, LOW);
    digitalWrite(buzzer, LOW);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("PRESSURE OK");
    lcd.setCursor(0, 1);
    lcd.print(pressure);
    lcd.print(" PSI");
  }

  delay(500);
}
