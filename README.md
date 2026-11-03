# Water-tank-monitor-
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

// Pin Configuration
#define TRIG 2
#define ECHO 3
#define SET_BUTTON 10
#define MODE_BUTTON 11
#define BUZZER 12
#define RELAY 13

// Tank Settings
const float TANK_HEIGHT = 20.0;
const int LOW_LEVEL = 30;
const int HIGH_LEVEL = 85;

// Relay Setting
const bool RELAY_ACTIVE_LOW = false;

// System Variables
bool autoMode = true;
bool manualPump = false;
bool pumpOn = false;
bool buzzerOn = false;

bool oldSet = HIGH;
bool oldMode = HIGH;

float distance = -1;
int waterLevel = 0;

unsigned long lastPrint = 0;
unsigned long lastLCD = 0;

// Ambulance Siren Variables
unsigned long lastSiren = 0;
bool sirenHigh = false;

// Pump Control
void controlPump(bool state) {

  pumpOn = state;

  if (RELAY_ACTIVE_LOW) {
    digitalWrite(RELAY, state ? LOW : HIGH);
  } else {
    digitalWrite(RELAY, state ? HIGH : LOW);
  }
}

// Ambulance Buzzer Control
void controlBuzzer(bool state) {

  buzzerOn = state;

  if (!state) {
    noTone(BUZZER);
    sirenHigh = false;
    return;
  }

  if (millis() - lastSiren >= 500) {

    lastSiren = millis();
    sirenHigh = !sirenHigh;

    if (sirenHigh) {
      tone(BUZZER, 5000);
    } else {
      tone(BUZZER, 1200);
    }
  }
}

// Distance Measurement
float getDistance() {

  digitalWrite(TRIG, LOW);
  delayMicroseconds(3);

  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  unsigned long duration =
      pulseIn(ECHO, HIGH, 30000);

  if (duration == 0) {
    return -1;
  }

  return duration * 0.0343 / 2.0;
}

// LCD Display
void displayLCD(bool sensorOK) {

  lcd.setCursor(0, 0);

  if (sensorOK) {
    lcd.print("Water:");
    lcd.print(waterLevel);
    lcd.print("%       ");
  } else {
    lcd.print("Sensor Error    ");
  }

  lcd.setCursor(0, 1);

  lcd.print("Pump:");
  lcd.print(pumpOn ? "ON " : "OFF");

  lcd.print(autoMode ? " AUTO " : " MAN  ");
}

// Setup
void setup() {

  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);

  pinMode(SET_BUTTON, INPUT_PULLUP);
  pinMode(MODE_BUTTON, INPUT_PULLUP);

  pinMode(BUZZER, OUTPUT);
  pinMode(RELAY, OUTPUT);

  controlPump(false);
  controlBuzzer(false);

  Serial.begin(9600);

  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("WATER TANK");

  lcd.setCursor(0, 1);
  lcd.print("MONITOR SYSTEM");

  Serial.println("WATER TANK MONITOR STARTED");

  delay(1500);
  lcd.clear();
}

// Main Loop
void loop() {

  // 1. Read Ultrasonic Sensor
  distance = getDistance();

  bool sensorOK =
      (distance >= 2 && distance <= TANK_HEIGHT);

  if (sensorOK) {

    waterLevel = round(
      ((TANK_HEIGHT - distance) / TANK_HEIGHT) * 100
    );

    waterLevel = constrain(waterLevel, 0, 100);
  }

  // 2. MODE Button
  bool modeButton = digitalRead(MODE_BUTTON);

  if (modeButton == LOW && oldMode == HIGH) {

    autoMode = !autoMode;
    manualPump = false;

    delay(200);
  }

  oldMode = modeButton;

  // 3. SET Button
  bool setButton = digitalRead(SET_BUTTON);

  if (setButton == LOW && oldSet == HIGH) {

    if (!autoMode && sensorOK) {
      manualPump = !manualPump;
    }

    delay(200);
  }

  oldSet = setButton;

  // 4. Pump and Buzzer Control
  if (!sensorOK) {

    manualPump = false;

    controlPump(false);
    controlBuzzer(false);

  } else {

    if (autoMode) {

      if (waterLevel <= LOW_LEVEL) {
        controlPump(true);
      }

      if (waterLevel >= HIGH_LEVEL) {
        controlPump(false);
      }

    } else {

      controlPump(manualPump);
    }

    // Ambulance Siren at 85%
    controlBuzzer(waterLevel >= HIGH_LEVEL);
  }

  // 5. LCD Update
  if (millis() - lastLCD >= 500) {

    lastLCD = millis();

    displayLCD(sensorOK);
  }

  // 6. Serial Monitor
  if (millis() - lastPrint >= 1000) {

    lastPrint = millis();

    Serial.println("----------------------");

    if (sensorOK) {

      Serial.print("Distance: ");
      Serial.print(distance);
      Serial.println(" cm");

      Serial.print("Water Level: ");
      Serial.print(waterLevel);
      Serial.println("%");

    } else {

      Serial.println("Sensor Error!");
    }

    Serial.print("Pump: ");
    Serial.println(pumpOn ? "ON" : "OFF");

    Serial.print("Mode: ");
    Serial.println(autoMode ? "AUTO" : "MANUAL");

    Serial.print("Buzzer: ");
    Serial.println(buzzerOn ? "ON" : "OFF");
  }

  delay(100);
}
