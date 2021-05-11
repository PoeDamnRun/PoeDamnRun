### Hi there 👋

<!--
**PoeDamnRun/PoeDamnRun** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

#include <rgb_lcd.h>
// #include <Wire.h>

rgb_lcd lcd;
double x = 0;
double y = 0;
double a = 0;
double b = 0;
int girouette = A1;
int windSensor = A2;
float voltageMax = 2.0;
float voltageMin = .4;
float voltageConversionConstant = .5027;
//float voltageConversionConstant = .004882814;
float sensorVoltage = 0;

float windSpeedMin = 0;
float windSpeedMax = 32;

int windSpeed = 0;
int prevWindSpeed = 0;
void setup() {
Serial.begin(115200);

  // set up the LCD's number of columns and rows:
    lcd.begin(16, 2);
}
void loop() {
// Boucle girouette --------------------------------------------------------------------------------------

int sensorValue = analogRead(girouette);
Serial.println(sensorValue);
if (880 <= sensorValue && sensorValue<= 890){

// Print a message to the LCD.
    lcd.print("Nord");
}
else if (930 <= sensorValue && sensorValue<= 940){
// Print a message to the LCD.
    lcd.print("Est");
    
}
else if (750 <= sensorValue && sensorValue<= 753){
// Print a message to the LCD.
    lcd.print("Nord - Ouest");
  
}
else if (791 <= sensorValue && sensorValue<= 795){
// Print a message to the LCD.
    lcd.print("Sud - Ouest");
  
}
else if (866 <= sensorValue && sensorValue<= 868){
// Print a message to the LCD.
    lcd.print("Sud - Est");
  
}
else if (817 <= sensorValue && sensorValue<= 819){
// Print a message to the LCD.
    lcd.print("Nord - Est");
  
}
else if (947 <= sensorValue && sensorValue<= 950){
// Print a message to the LCD.
    lcd.print("Sud");
  
}
else if (845 <= sensorValue && sensorValue<= 850){
// Print a message to the LCD.
    lcd.print("Ouest");

}

delay(3000);
lcd.clear();
//---------------------------------------------------------------------------------------------------------
 
// Boucle anémomètre --------------------------------------------------------------------------------------

 int sensorValueWind= analogRead(windSensor);
 Serial.print("vent : ");
 Serial.println(sensorValueWind);

  float voltage = sensorValueWind* (5.0 / 1023.0);

  sensorVoltage = sensorValueWind* voltageConversionConstant;



  if (sensorVoltage <= voltageMin) {
    windSpeed = 0; //Check if voltage is below minimum value. If so, set wind speed to zero.
  } else {
    windSpeed = ((sensorVoltage - voltageMin) * windSpeedMax / (voltageMax - voltageMin)) * 2.232694; //For voltages above minimum value, use the linear relationship to calculate wind speed.
  }

  x = windSpeed;
  if (x >= y) {
    y = x;
  } else {
    y = y;
  }

  //Max voltage calculation

  a = sensorVoltage;
  if (a >= b) {
    b = a;
  } else {
    b = b;
  }
  if (windSpeed != prevWindSpeed) {
    Serial.println(windSpeed);
    Serial.println(sensorVoltage);
    prevWindSpeed = windSpeed;
  }
  
  lcd.setCursor(0, 0);
  lcd.print("Wind Speed ");
  lcd.setCursor(12, 0);
  lcd.print(windSpeed);
  lcd.print(" ");
  lcd.setCursor(11, 2);
  lcd.print("m/s");
  delay(3000);
  lcd.clear();

//---------------------------------------------------------------------------------------------------------
}
