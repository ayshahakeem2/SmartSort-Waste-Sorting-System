#include <Servo.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
// ---- PINS ----
const int TRIG = 9;
const int ECHO = 8;
const int MOISTURE_PIN = A0;
const int SERVO_PIN = 11;
// ---- OBJECTS ----
Servo myServo;
LiquidCrystal_I2C lcd(0x27, 16, 2);
// ---- SETTINGS ----
int MOISTURE_THRESHOLD = 900; // INCREASED: Now it will detect even slightly damp items
int WASTE_DETECT_DIST = 8;    
int POS_CENTER = 90;
int POS_WET    = 140;         
int POS_DRY    = 40;          
// ---- COUNTERS ----
int wetCount = 0;
int dryCount = 0;
long readDistance() {
  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);
  long duration = pulseIn(ECHO, HIGH, 30000);
  return (duration / 2) / 29.1;
}
void showCounts() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Wet:"); lcd.print(wetCount);
  lcd.print(" Dry:"); lcd.print(dryCount);
  lcd.setCursor(0, 1);
  lcd.print("Total: "); lcd.print(wetCount + dryCount);
  delay(3000);
  lcd.clear();
}
void setup() {
  Serial.begin(9600);
  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);
  myServo.attach(SERVO_PIN);
  myServo.write(POS_CENTER);
  delay(1000);
  myServo.detach(); 
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("    SmartSort    ");
  delay(2000);
  lcd.clear();
}
void loop() {
  long wasteDist = readDistance();
  
  lcd.setCursor(0, 0);
  lcd.print("Ready for Waste ");
  lcd.setCursor(0, 1);
  lcd.print(" Drop item here ");
  if (wasteDist > 0 && wasteDist < WASTE_DETECT_DIST) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Waste Detected! ");
    lcd.setCursor(0, 1);
    lcd.print("PLACING ITEM... ");
    // --- INCREASED DELAY ---
    // You now have 5 seconds to place the item properly on the probe
    delay(5000); 
    int moistureValue = analogRead(MOISTURE_PIN);
    Serial.print("Moisture Level: ");
    Serial.println(moistureValue);
    myServo.attach(SERVO_PIN);
    if (moistureValue < MOISTURE_THRESHOLD) {
      wetCount++;
      myServo.write(POS_WET); // TILT LEFT
      lcd.clear();
      lcd.print(">>  WET WASTE <<");
      lcd.setCursor(0, 1);
      lcd.print(" Going LEFT bin ");
    } else {
      dryCount++;
      myServo.write(POS_DRY); // TILT RIGHT
      lcd.clear();
      lcd.print(">>  DRY WASTE <<");
      lcd.setCursor(0, 1);
      lcd.print("Going RIGHT bin ");
    }
    delay(4000); 
    myServo.write(POS_CENTER);
    delay(1000); 
    myServo.detach(); 
    showCounts();
  }
  delay(300);
}
