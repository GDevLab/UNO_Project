# UNO_Project
~~~C++
#include <LiquidCrystal.h>

// define pin HC-SR04 1
#define echoPin1 2 // attach pin D2 Arduino to pin Echo of HC-SR04
#define trigPin1 3 //attach pin D3 Arduino to pin Trig of HC-SR04
// define pin HC-SR04 2
#define echoPin2 12 // attach pin D2 Arduino to pin Echo of HC-SR04
#define trigPin2 13 //attach pin D3 Arduino to pin Trig of HC-SR04di

// define pin buzzer
#define buzzer 11

// define pin potentiometer
int top = A0; // define pin potentiometer 1
int tym = A2; // define pin potentiometer 2

// define variables 1
long duration1; // variable for the duration of sound wave travel
int distance1; // variable for the distance measurement
// define variables 2
long duration2; // variable for the duration of sound wave travel
int distance2; // variable for the distance measurement

// LCD Display 16x2
const int rs = 9, en = 8, d4 = 4, d5 = 5, d6 = 6, d7 = 7;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

void setup() {
  // Start LCD Display
  lcd.begin(16, 2); 
  // Delegate PIN
  pinMode(trigPin1, OUTPUT); // Sets the trigPin1 as an OUTPUT
  pinMode(echoPin1, INPUT); // Sets the echoPin1 as an INPUT
  pinMode(trigPin2, OUTPUT); // Sets the trigPin2 as an OUTPUT
  pinMode(echoPin2, INPUT); // Sets the echoPin2 as an INPUT
  pinMode(top,INPUT);
  pinMode(tym,INPUT);
  pinMode(buzzer,OUTPUT);
  // Sets data rate to 9600 bps
  Serial.begin(9600);
}

void loop() {
  //ultrasonic 1
  /*******************************************/  
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  
  digitalWrite(trigPin1, LOW);
  
  duration1 = pulseIn(echoPin1, HIGH);
  
  distance1 = duration1 * 0.034 / 2;
  /***********************************************/

  //ultrasonic 2
  /****************************************/  
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  
  digitalWrite(trigPin2, LOW);
  
  duration2 = pulseIn(echoPin2, HIGH);
  
  distance2 = duration2 * 0.034 / 2;  
  /*******************************************/
  
  int diff = (distance2 - distance1);
  Serial.print("difference: ");
  Serial.println(diff);

  int A = analogRead(top); // Read potentiometer 1 give data to variables A 
  int C = analogRead(tym); // Read potentiometer 2 give data to variables C
 
  int a =0;
  int c =0;

  // Check value potentiometer (A) compare distance of objects (a)
  if( A<100) {
    a=1;
  } else if( A<200) {
      a=2;
  } else if( A<300) {
      a=3;
  } else if( A<400) {
      a=4;
  } else if( A<500) {
      a=5;
  } else if( A<600) {
      a=6;
  } else if( A<700) {
      a=7;
  } else if( A<800) {
      a=8;
  } else if( A<900) {
      a=9;
  } else if( A<1000) {
      a=10;
  } else {
      a=10;
  }
  
  // Check value potentiometer (C) compare distance of objects (c)
  if( C<100 ) {
    c=0;
  } else if( C<200 ) {
      c=2;
  } else if( C<300 ) {
      c=5;
  } else if( C<400) {
      c=10;
  } else if ( C<500) {
      c=15;
  } else if ( C<600) {
      c=30;
  } else {
      c=60;
  }

  int buff = c*1000; // 

  Serial.print("a");
  Serial.println(a);
  Serial.print("c"); 
  Serial.println(c);

  // Check distance of buzzer
  if(diff >= a) {
    delay(buff);
    digitalWrite(buzzer,HIGH);
  } else {
    digitalWrite(buzzer,LOW);
  }

  // LCD Display
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Calibrate: ");
  lcd.print(a);
  lcd.print(" cm");
  lcd.setCursor(0, 1);
  lcd.print("timer:");
  lcd.print(c);
  lcd.print(" Seconds");
  
  // Serial Monitor
  Serial.print("Distance1: ");
  Serial.print(distance1);
  Serial.println(" cm");
  Serial.print("Distance2: ");
  Serial.print(distance2);
  Serial.println(" cm");
}
~~~
