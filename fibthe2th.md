


<!--


import: https://raw.githubusercontent.com/liaTemplates/AVR8js/main/README.md

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/Amatetzu/LED_FIB/main/fibthe2th.md#1)

# Fibonacci Numbers in Binary with Arduino and LED Strip

## Introduction

### Fibonacci Sequence
The **Fibonacci sequence** is a series of numbers where each number is the sum of the two preceding ones, usually starting with 0 and 1. For example: 0, 1, 1, 2, 3, 5, 8, 13, ...

#### History
- **Origins in India**
  - Known as early as the 6th century in Indian mathematics.
  - Described in Indian mathematical texts such as the *Chandahshastra* by Pingala (c. 200 BC) and later by Virahanka (c. 600 AD).

 

- **Introduction to the Western World**
  - Brought to Europe by Leonardo of Pisa, known as Fibonacci.
  - Described in his book *Liber Abaci* (The Book of Calculation), published in 1202.
  - Fibonacci introduced the sequence while discussing the growth of a rabbit population.


#### Applications
Fibonacci numbers appear in many different areas, including:

- **Nature**: Patterns of leaves, flowers, and shells often follow the Fibonacci sequence.
- **Art and Architecture**: The Golden Ratio, closely related to Fibonacci numbers, is used for aesthetically pleasing proportions.
- **Computer Science**: Algorithms for searching and sorting, data structures like heaps, and dynamic programming problems often utilize Fibonacci numbers.
- **Finance**: Technical analysts use Fibonacci retracement levels to predict stock price movements.

<br>

![](https://thesmarthappyproject.com/wp-content/uploads/2015/06/LucaPostpischi-sunflower.jpg)
---

# the code
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

#define STARTRED 255
#define STARTGREEN 0
#define STARTBLUE 0

CRGB leds[NUM_LEDS]; // Array to hold the LED colors

int redColor = STARTRED;
int greenColor = STARTGREEN;
int blueColor = STARTBLUE;

int secondLastFib = 0;
int lastFib = 1;
int bits[NUM_LEDS] = {0}; // Array to store binary representation of Fibonacci sequence

int counter = 0;
const int MAXCOUNTER = 765; // Adjust as needed

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
  int third_max = (MAXCOUNTER / 3);
  int stepsize = 15; // Adjust as needed

  if (counter < third_max) {
    redColor -= stepsize;
    greenColor += stepsize;
  } else if (counter > MAXCOUNTER - third_max) {
    blueColor += stepsize;
    greenColor -= stepsize;
  } else {
    redColor -= stepsize;
    blueColor -= stepsize;
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

## calculateBinaryFibonacci



# sources

text: 

1. [https://www.meisterdrucke.at/kunstwerke/1260px/Unknown_Artist_-_Portrait_of_Leonardo_Fibonacci_%281170-1245%29_%28Leonard_of_Pisa%29_Italian_mathematici_-_%28MeisterDrucke-927685%29.jpg](https://www.meisterdrucke.at/kunstwerke/1260px/Unknown_Artist_-_Portrait_of_Leonardo_Fibonacci_%281170-1245%29_%28Leonard_of_Pisa%29_Italian_mathematici_-_%28MeisterDrucke-927685%29.jpg)

img.: 

1. [https://thesmarthappyproject.com/wp-content/uploads/2015/06/LucaPostpischi-sunflower.jpg](https://thesmarthappyproject.com/wp-content/uploads/2015/06/LucaPostpischi-sunflower.jpg)
