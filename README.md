

<!--


import: https://raw.githubusercontent.com/liaTemplates/AVR8js/main/README.md

-->
[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/Supergecki/ino-clock/main/README.md#1)
[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/Amatetzu/LED_FIB/main/README.md#1)

# Our little project 

Task to visualize the fibonacci numerbs on a 12 LED stripe 
using a lot of colrs
---
## short refresh what numbers are these?

- A sequence of numbers where each number is the sum of the two preceding ones
- first two numvers are 0 and 1 
- Example: 0, 1, 1, 2, 3, 5, 8, 13, ...

---
## Where can you find the fiboncci numbers?

computer science

- fiboncci search technique 
- fibonacci heap 

    - helpful for Dijkstra algorithm



biological settings

- arrangement of leaves on a stem and pine cone`s bracts
- flowering of an artichoke


---



## The Code
<div id="matrix-experiment">
<wokwi-neopixel-matrix pin="6" cols="12" rows="1"></wokwi-neopixel-matrix>
<span id="simulation-time"></span>
</div>

```fibbo.cpp             Automata
#include <Arduino.h>
#include "FastLED.h"

#define DATA_PIN 6
#define BRIGHTNESS 180
#define NUM_LEDS 12

CRGB leds[NUM_LEDS]; // Array to hold the LED colors

int redColor = 0;
int greenColor = 0;
int blueColor = 0;

int secondLastFib = 0;
int lastFib = 1;
int bits[NUM_LEDS] = {0}; // Array to store binary representation of Fibonacci sequence

int counter = 0;
const int MAXCOUNTER = 1000; // Adjust as needed

void setup() {
  FastLED.addLeds<NEOPIXEL, DATA_PIN>(leds, NUM_LEDS); // Initialize FastLED library with NEOPIXEL type and LED data pin
  FastLED.setBrightness(BRIGHTNESS); // Set LED brightness
  Serial.begin(9600); // Initialize serial communication for debugging
}

void loop() {
  pickColor(); // Pick RGB color values
  calculateBinaryFibonacci(); // Calculate next Fibonacci number and convert it to binary

  // Set LEDs based on the bits array
  for (int i = 0; i < NUM_LEDS; i++) {
    if (bits[i]) {
      leds[i] = CRGB(redColor, greenColor, blueColor); // Set LED color to random RGB values
    } else {
      leds[i] = CRGB::Black; // Turn LED off if bit is 0
    }
  }

  FastLED.show(); // Display the LEDs with the updated colors

  delay(1000); // Delay for 1 second before repeating the loop
}

// Function to pick RGB color values
void pickColor() {
  int third_max = MAXCOUNTER / 3;
  int stepsize = 15; // Adjust as needed

  if (counter < third_max) {
    redColor -= stepsize;
    greenColor += stepsize;
  } else if (counter > MAXCOUNTER - third_max) {
    redColor += stepsize;
    blueColor -= stepsize;
  } else {
    greenColor -= stepsize;
    blueColor += stepsize;
  }
  
  counter += 1;
  if (counter >= MAXCOUNTER) {
    counter = 0;
  }
}

// Function to calculate the next Fibonacci number and convert it to binary
void calculateBinaryFibonacci() {
  int currentFib = secondLastFib + lastFib; // Calculate the next Fibonacci number

  // Reset Fibonacci sequence if current number exceeds the maximum representable by NUM_LEDS bits
  if (currentFib > calcMax(NUM_LEDS)) {
    secondLastFib = 0;
    lastFib = 1;
    currentFib = secondLastFib + lastFib;
    
    // Reset color variables to their initial values
    redColor = 0;
    greenColor = 0;
    blueColor = 0;
    
    // Turn off all LEDs
    for (int i = 0; i < NUM_LEDS; i++) {
      leds[i] = CRGB::Black;
    }
  } else {
    secondLastFib = lastFib;
    lastFib = currentFib;
  }

  // Convert Fibonacci number to binary and store in bits array
  for (int i = 0; i < NUM_LEDS; i++) {
    bits[i] = (currentFib >> i) & 1;  // Use bitwise operations to extract each bit of the Fibonacci number
  }
}

// Function to calculate the maximum Fibonacci number that fits within NUM_LEDS bits
int calcMax(int ledNum) {
  int max = 1;
  for (int i = 0; i < ledNum; i++) {
    max *= 2; // Calculate 2^ledNum
  }
  return max - 1; // Return the maximum Fibonacci number that fits within NUM_LEDS bits (2^ledNum - 1)
}


```
@AVR8js.sketch(matrix-experiment)



