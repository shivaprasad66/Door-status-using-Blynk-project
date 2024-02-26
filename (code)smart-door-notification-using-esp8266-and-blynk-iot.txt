#define BLYNK_TEMPLATE_ID "TMPL34LioZ5gm"
#define BLYNK_TEMPLATE_NAME "Smart Door Notification"
#define BLYNK_AUTH_TOKEN "WsFYVUyAiqaD9wIwWzc0M1d_f32G47Cd"

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

#define DOOR_SENSOR  4
#define buzzer 2
#define ledgreen 22
#define ledred 23
#define ledcom 5
BlynkTimer timer;
char auth[] = BLYNK_AUTH_TOKEN; //Enter the authentication code sent by Blynk to Email
char ssid[] = "time"; //Enter WIFI SSID/name
char pass[] = "98765432"; //Enter WIFI Password
int previousState =0;

void notifyOnButtonPress()
{
  static int previousState = 0;
  int currentState = digitalRead(DOOR_SENSOR);
  if (currentState ==1 && previousState ==0) {
    Serial.println("Someone Opened the door");
    Blynk.logEvent("OPEN","****Someone OPENED the Door****");
    digitalWrite(buzzer, HIGH);
    digitalWrite(ledred, LOW);
    digitalWrite(ledcom, HIGH);
    digitalWrite(ledgreen, HIGH);

  
    previousState =1;
  }
  else if (currentState ==0 && previousState == 1)
  {
    previousState =0;
    Serial.println("Door Closed");
    Blynk.logEvent("CLOSE","****Someone CLOSED the Door****");
    digitalWrite(buzzer, LOW);
    digitalWrite(ledred, HIGH);
    digitalWrite(ledcom, HIGH);
    digitalWrite(ledgreen, LOW);
    

  }
}

void setup()
{
Serial.begin(9600);
Blynk.begin(auth, ssid, pass);
pinMode(DOOR_SENSOR,INPUT_PULLUP);
pinMode(buzzer, OUTPUT);
pinMode(ledgreen, OUTPUT);
pinMode(ledred, OUTPUT);
pinMode(ledcom, OUTPUT);

timer.setInterval(5000L,notifyOnButtonPress); 
}
void loop()
{
  Blynk.run();
  timer.run();
}
