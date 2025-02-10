# Plant-Water-monitoring-system-
#include<Wire.h>
#include <LiquidCrystal_I2C.h>

// Correct the I2C address to 0x27
LiquidCrystal_I2C lcd(0x27, 16, 2);

int sensor_pin = A0;

int relay_pin = 7;

void setup() {
  Serial.begin(9600);
   lcd.init();
  lcd.backlight();
  pinMode(sensor_pin, INPUT);
  pinMode(relay_pin, OUTPUT);
}

void loop() {
  int sensor_data = analogRead(sensor_pin);
  Serial.print("sensor_data: ");
  Serial.print(sensor_data);
  Serial.print("\t | ");

  if (sensor_data >= 650) {
    Serial.println("no moisture, soil is dry");
    digitalWrite(relay_pin, HIGH);
    lcd.setCursor(0, 0);
    lcd.print("soil Dry   ");  // Ensure to overwrite previous text
    lcd.setCursor(0, 1);
    lcd.print("motor ON   ");  // Ensure to overwrite previous text
  } else if (sensor_data >= 400 && sensor_data <= 950) {
    Serial.println("there is some moisture, soil is medium");
    digitalWrite(relay_pin, LOW);
    lcd.setCursor(0, 0);
    lcd.print("soil Medium");  // Ensure to overwrite previous text
    lcd.setCursor(0, 1);
    lcd.print("motor OFF  ");  // Ensure to overwrite previous text
  } else if (sensor_data < 400) {
    Serial.println("soil is wet");
    digitalWrite(relay_pin, LOW);
    lcd.setCursor(0, 0);
    lcd.print("soil Wet   ");  // Ensure to overwrite previous text
    lcd.setCursor(0, 1);
    lcd.print("motor OFF  ");  // Ensure to overwrite previous text
  }

delay(100);
}
