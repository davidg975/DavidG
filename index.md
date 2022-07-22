Cyclone Project

A game where an LED circles around a ring where you have to press a button at exactly the right time so that you can move onto the next level, where each level gets harder, more precise and faster.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| David G | Nest+M | Mechanical Engineering | Incoming Sophmore

![Headstone Image](https://raw.githubusercontent.com/BlueStampEng/BSE_Template_Portfolio/de8633f62b5da2234992a0178a6a72fd6df7e7e1/branding/BlueStamp-Logo.svg)


![IMG-8014](https://user-images.githubusercontent.com/108947566/180487507-7f81c515-f3e9-4377-8db0-900faef3efe2.jpg)




# Final Project

![IMG-8015](https://user-images.githubusercontent.com/108947566/180487612-121e387b-4c6b-4839-b8a1-962420e73fe8.jpg)

# Demo Night
<iframe width="560" height="315" src="https://www.youtube.com/embed/K6TwVG1GlWU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Final Milestone
My final Milestone is the final product of the whole project. This includes the final code which has all of the movements for the led's as well as the different levels with increasing difficulty as you play the game. It also has the led fully wraping around the ring I built, with all the wires connected to their corresponding parts based off of the code. With all of this connected, this comes together as the final prduct for my project, a complete running game.

<iframe width="560" height="315" src="https://www.youtube.com/embed/vmo2cbQO-Vk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# First Milestone
  

My first milestone was to get the LED and the button connected to the Arduino. First, I made sure to have the LED working and focused first on just getting that to light up. Next, I worked on getting the button to also be connected to the Arduino. Then I ran the code that I needed so that the LED would light up and change colors at the click of the button. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/1v9f48WHBQU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Bill Of Materials

| Item | Qty | Price | Where to Buy |
| ------------- | ------------- | ------------- | ------------- |
| Neo Pixel LED  | 1  | $64.99  | (https://www.amazon.com/Adafruit-Indust-Digital-Weatherproof-LED-1m/dp/B01KHWGVJ4/ref=sr_ |
| Breadboard  | 1 |  $6.75  | https://www.amazon.com/BB400-Solderless-Plug-BreadBoard-tie-points/dp/B0040Z1ERO |
| Micro Servo | 1 | $5.95 | https://www.adafruit.com/product/169  |
| Jumper Wires  | 5 | 10¢/wire  |  https://www.amazon.com/Breadboard-Jumper-Wire-75pcs-pack/dp/B0040DEI9M |
| Arduino Uno Board  | 1  | $27.60  | https://store-usa.arduino.cc/products/arduino-uno-rev3?selectedStore=us  |
| Alligator clips   | 2 | $6.19  |  https://www.amazon.com/WGGE-WG-026-Pieces-Colors-Alligator/dp/B06XX25HFX/ref=sr_1_2_sspa?  |
|  Bread board Button  |  1  | $6.99 | https://www.amazon.com/MakerSpot-Momentary-Tactile-Breadboard-Friendly/dp/B06XT3FLVM/ref=sr_1_13?

# Code
```cp
<#include "FastLED.h"
#define NUM_LEDS 40
#define DATA_PIN 6
#define SCORE_PIN 2
#define SCORE_LEDS 4

CRGB leds[NUM_LEDS];
CRGB sleds[NUM_LEDS];

bool reachedEnd = false;
byte gameState = 0;
//byte ledSpeed = 0;
int period = 1000;
unsigned long time_now = 0;
byte Position = 0;
byte level = 0;

const byte ledSpeed[4] = {50, 40, 35, 20};

//Debounce
bool findRandom = false;
byte spot = 0;

void setup() {
  // put your setup code here, to run once:
  FastLED.addLeds<WS2812B, DATA_PIN, GRB>(leds, NUM_LEDS);
  FastLED.addLeds<WS2812B, SCORE_PIN, GRB>(sleds, SCORE_LEDS);
  pinMode(4, INPUT_PULLUP);
  Serial.begin(9600);
  Serial.println("Reset");
}

void loop() {
  // put your main code here, to run repeatedly:
  if (gameState == 0) {
    fill_rainbow(leds, NUM_LEDS, 0, 7); //2 = longer gradient strip
    fill_rainbow(sleds, SCORE_LEDS, 0, 7); //2 = longer gradient strip
    if (digitalRead(4) == LOW) {
      Position = 0;
      findRandom = true;
      delay(500);
      for (byte i = 0; i < NUM_LEDS; i++) {
        leds[i].setRGB(0, 0, 0);
        delay(40);
        FastLED.show();
      }
      for (byte i = 0; i < SCORE_LEDS; i++) {
        sleds[i].setRGB(0, 0, 0);
        delay(100);
        FastLED.show();
      }
      gameState = 1;
    }
    FastLED.show();
  }
  if (gameState == 1) {
    period = ledSpeed[0];
    if (millis() > time_now + period) {
      time_now = millis();
      if (findRandom) {
        spot = random(16) + 3;
        findRandom = false;
      }
      leds[spot - 1].setRGB(255, 140, 0);
      leds[spot].setRGB(0, 255, 0);
      leds[spot + 1].setRGB(255, 110, 0);
      sleds[0].setRGB(0, 255, 0);
      PlayGame(spot - 1, spot + 1);
    }
    if (digitalRead(4) == LOW) {
      delay(300);
      findRandom = false;
      if (Position > spot - 1 && Position < spot + 3) {
        level = gameState;
        gameState = 98;
      } else {
        gameState = 99;
      }
    }
  }
  if (gameState == 2) {
//    period = 320;
    period = ledSpeed[1];
    if (millis() > time_now + period) {
      time_now = millis();
      if (findRandom) {
        spot = random(16) + 3;
        findRandom = false;
      }
      leds[spot - 1].setRGB(255, 190, 0);
      leds[spot].setRGB(0, 255, 0);
      leds[spot + 1].setRGB(255, 190, 0);
      sleds[1].setRGB(255, 255, 0);
      PlayGame(spot - 1, spot + 1);
    }
    if (digitalRead(4) == LOW) {
      delay(300);
      if (spot - 1 && Position < spot + 3) {
        level = gameState;
        gameState = 98;
      } else {
        gameState = 99;
      }
    }
  }
  if (gameState == 3) {
    period = ledSpeed[2];
    if (millis() > time_now + period) {
      time_now = millis();
      if (findRandom) {
        spot = random(16) + 3;
        findRandom = false;
      }
      leds[spot].setRGB(0, 255, 0);
      sleds[2].setRGB(255, 50, 0);
      PlayGame(spot, spot);
    }
    if (digitalRead(4) == LOW) {
      delay(300);
      if (Position == spot+1) {
        level = gameState;
        gameState = 98;
      } else {
        gameState = 99;
      }
    }
  }
  if (gameState == 4) {
    period = ledSpeed[3];
    if (millis() > time_now + period) {
      time_now = millis();
      if (findRandom) {
        spot = random(16) + 3;
        findRandom = false;
      }
      leds[spot].setRGB(0, 255, 0);
      sleds[3].setRGB(255, 0, 0);
      PlayGame(spot, spot);
    }
    if (digitalRead(4) == LOW) {
      delay(300);
      if (Position == spot+1) {
        level = gameState;
        gameState = 98;
      } else {
        gameState = 99;
      }
    }
  }
  if (gameState == 98) {
    winner();
  }
  if (gameState == 99) {
    loser();
  }
}
void PlayGame(byte bound1, byte bound2) {
  leds[Position].setRGB(255, 0, 0);
  if (Position < bound1 + 1 || Position > bound2 + 1) {
    leds[Position - 1].setRGB(0, 0, 0);
  }
  FastLED.show();
  Position++;
  if (Position >= NUM_LEDS) {
    leds[Position - 1].setRGB(0, 0, 0);
    Position = 0;
  }
}

void winner() {
  for (byte i = 0; i < 3; i++) {
    for (byte j = 0; j < NUM_LEDS; j++) {
      leds[j].setRGB(0, 255, 0);
    }
    FastLED.show();
    delay(500);
    clearLEDS();
    FastLED.show();
    delay(500);
  }
  findRandom = true;
  Position = 0;

  gameState = level + 1;
  if (gameState > 4) {
    gameState = 0;
  }
}
void loser() {
  for (byte i = 0; i < 3; i++) {
    for (byte j = 0; j < NUM_LEDS; j++) {
      leds[j].setRGB(255, 0, 0);
    }
    FastLED.show();
    delay(500);
    clearLEDS();
    FastLED.show();
    delay(500);
  }
  gameState = 0;
}
void clearLEDS() {
  for (byte i = 0; i < NUM_LEDS; i++) {
    leds[i].setRGB(0, 0, 0);
  }
}
void winAll(){
  
}>
```
