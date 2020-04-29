/*
   Traffic Light Seguence
   Project 5 - Traffic Lights
   Teresa Miller

*/

//LED pin assignments

#define westButton 3
#define eastButton 13
#define westRed 2
#define westYellow 1
#define westGreen 0
#define eastRed 12
#define eastYellow 11
#define eastGreen 10
#define westPedRed 4
#define westPedGreen 5
#define eastPedRed 7
#define eastPedGreen 6
#define yellowBlinkTime 1000 //yellow blink rate

boolean trafficWest = true; // west = true, east = false
int flowTime = 10000; // time traffic flows
int changeDelay = 2000; // delay between light changes

void setup() {

  // setup digital I/O pins
  pinMode(westButton, INPUT);
  pinMode(eastButton, INPUT);
  pinMode(westRed, OUTPUT);
  pinMode(westYellow, OUTPUT);
  pinMode(westGreen, OUTPUT);
  pinMode(eastRed, OUTPUT);
  pinMode(eastYellow, OUTPUT);
  pinMode(eastGreen, OUTPUT);
  pinMode(westPedRed, OUTPUT);
  pinMode(westPedGreen, OUTPUT);
  pinMode(eastPedRed, OUTPUT);
  pinMode(eastPedGreen, OUTPUT);

  // set initial state for lights
  digitalWrite(westRed,  LOW);
  digitalWrite(westYellow,  LOW);
  digitalWrite(westGreen,  HIGH);
  digitalWrite(eastRed,  HIGH);
  digitalWrite(eastYellow,  LOW);
  digitalWrite(eastGreen, LOW);
  digitalWrite(westPedRed,  HIGH);
  digitalWrite(westPedGreen,  LOW);
  digitalWrite(eastPedRed,  LOW);
  digitalWrite(eastPedGreen, HIGH);
}

void blinkYellowLight(int yellowLED)
{
  for ( int i = 0; i < 5; i++ )
  {
    //yellow light blink pattern
    digitalWrite(yellowLED, LOW);
    delay(yellowBlinkTime);
    digitalWrite(yellowLED, HIGH);
    delay(yellowBlinkTime);
  }
}

void greenToYellow(int green, int yellow)
{
  digitalWrite(green, LOW); //change east - facing lights from green to yellow to red
  digitalWrite(yellow, HIGH);
  delay(changeDelay);
}

void yellowToRed(int yellow, int red)
{
  digitalWrite(yellow, LOW);
  digitalWrite(red, HIGH);
  delay(changeDelay);
}

void greenLightOn(int yellow, int red, int green)
{
  digitalWrite(westYellow, LOW);
  digitalWrite(westRed, LOW); // change lights from red to green
  digitalWrite(westGreen, HIGH);
}
void pedLightsRed (int green, int red)
{
  digitalWrite(westPedGreen, LOW);
  digitalWrite(westPedRed, HIGH); // change lights from red to green
}
void pedLightsGreen (int green, int red)
{
  digitalWrite(eastPedGreen, HIGH);
  digitalWrite(eastPedRed, LOW);
}

void loop() {

  if ( digitalRead(westButton) == HIGH )
  {
    if ( trafficWest != true )
      // only continue if traffic is moving east
    {
      trafficWest != true;
      delay(flowTime);

      greenToYellow(eastGreen, eastYellow);

      yellowToRed(eastYellow, eastRed);

      blinkYellowLight(westYellow);

      greenLightOn (westYellow, eastRed, eastGreen);

      pedLightsRed (westPedGreen, westPedRed);

      pedLightsGreen (eastPedGreen, eastPedRed);
    }
  }

  if ( digitalRead(eastButton) == HIGH ) // request east>west traffic flow
  {
    if ( trafficWest == true ) // only continue if traffic flow is in the opposite (west) direction
    {
      trafficWest = false; // change traffic flow flag to east>west delay(flowTime);
      delay(flowTime);

      greenToYellow(westGreen, westYellow);

      yellowToRed(westYellow, westRed);

      blinkYellowLight(eastYellow);

      greenLightOn (westYellow, eastRed, eastGreen);

      pedLightsRed (westPedGreen, westPedRed);

      pedLightsGreen (eastPedGreen, eastPedRed);
    }
  }
}
