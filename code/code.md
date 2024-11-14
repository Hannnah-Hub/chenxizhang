rainy
```C++
#include <Adafruit_NeoPixel.h>

#define LED_PIN1    5      
#define LED_PIN2    6      
#define LED_PIN3    9      
#define LED_PIN4    10     
#define LED_PIN5    11     
#define LED_COUNT   120    

Adafruit_NeoPixel strip1 = Adafruit_NeoPixel(LED_COUNT, LED_PIN1, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip2 = Adafruit_NeoPixel(LED_COUNT, LED_PIN2, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip3 = Adafruit_NeoPixel(LED_COUNT, LED_PIN3, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip4 = Adafruit_NeoPixel(LED_COUNT, LED_PIN4, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip5 = Adafruit_NeoPixel(LED_COUNT, LED_PIN5, NEO_GRB + NEO_KHZ800);

unsigned long lastTime[5]; 
int delayOffset[5];        

void setup() {
  strip1.begin();
  strip2.begin();
  strip3.begin();
  strip4.begin();
  strip5.begin();
  
  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();

  randomSeed(analogRead(0));  
  for (int i = 0; i < 5; i++) {
    delayOffset[i] = random(0, 100);  
    lastTime[i] = millis() + delayOffset[i]; 
  }
}

void loop() {
  simulateRain();
}

void simulateRain() {
  unsigned long currentTime = millis();

  for (int i = 0; i < LED_COUNT; i++) {
    if (currentTime - lastTime[0] > delayOffset[0]) {
      updateStrip(strip1, i);
    }
    if (currentTime - lastTime[1] > delayOffset[1]) {
      updateStrip(strip2, i);
    }
    if (currentTime - lastTime[2] > delayOffset[2]) {
      updateStrip(strip3, i);
    }
    if (currentTime - lastTime[3] > delayOffset[3]) {
      updateStrip(strip4, i);
    }
    if (currentTime - lastTime[4] > delayOffset[4]) {
      updateStrip(strip5, i);
    }

    strip1.show();
    strip2.show();
    strip3.show();
    strip4.show();
    strip5.show();

    delay(100);
  }

  strip1.clear();
  strip2.clear();
  strip3.clear();
  strip4.clear();
  strip5.clear();

  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}

void updateStrip(Adafruit_NeoPixel &strip, int i) {
  // 更新某个灯带的 LED 颜色
  strip.setPixelColor(i, strip.Color(173, 216, 230));  
  if (i > 0) {
    strip.setPixelColor(i - 1, strip.Color(87, 108, 115));  
  }
  if (i > 1) {
    strip.setPixelColor(i - 2, strip.Color(43, 54, 57));  
  }
  if (i > 2) {
    strip.setPixelColor(i - 3, strip.Color(0, 0, 0)); 
  }
}
```
sunny（渐变版）：
```C++
#include <Adafruit_NeoPixel.h>

#define NUM_LEDS 120
#define PIN1      6   
#define PIN2      7   
#define PIN3      8   
#define PIN4      9   
#define PIN5      10  

Adafruit_NeoPixel strip1 = Adafruit_NeoPixel(NUM_LEDS, PIN1, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip2 = Adafruit_NeoPixel(NUM_LEDS, PIN2, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip3 = Adafruit_NeoPixel(NUM_LEDS, PIN3, NEO_GRB + NEO_KHZ800); 
Adafruit_NeoPixel strip4 = Adafruit_NeoPixel(NUM_LEDS, PIN4, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip5 = Adafruit_NeoPixel(NUM_LEDS, PIN5, NEO_GRB + NEO_KHZ800);

void setup() {
  strip1.begin();
  strip2.begin();
  strip3.begin();
  strip4.begin();
  strip5.begin();

  setSunshineColor();
  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}

void loop() {
  for (int brightness = 0; brightness <= 255; brightness++) {
    setSunshineBrightness(brightness);
    delay(10);  
  }
  
  for (int brightness = 255; brightness >= 0; brightness--) {
    setSunshineBrightness(brightness);
    delay(10);
  }
}

void setSunshineColor() {
  for (int i = 0; i < NUM_LEDS; i++) {
    strip1.setPixelColor(i, strip1.Color(255, 70, 0));
    strip2.setPixelColor(i, strip2.Color(255, 70, 0));
    strip3.setPixelColor(i, strip3.Color(255, 70, 0)); 
    strip4.setPixelColor(i, strip4.Color(255, 70, 0));
    strip5.setPixelColor(i, strip5.Color(255, 70, 0));
  }
}

void setSunshineBrightness(int brightness) {
  for (int i = 0; i < NUM_LEDS; i++) {
    strip1.setPixelColor(i, strip1.Color(255 * brightness / 255, 70 * brightness / 255, 0));
    strip2.setPixelColor(i, strip2.Color(255 * brightness / 255, 70 * brightness / 255, 0));
    strip3.setPixelColor(i, strip3.Color(255 * brightness / 255, 70 * brightness / 255, 0)); 
    strip4.setPixelColor(i, strip4.Color(255 * brightness / 255, 70 * brightness / 255, 0));
    strip5.setPixelColor(i, strip5.Color(255 * brightness / 255, 70 * brightness / 255, 0));
  }

  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}
```
Sunny(不渐变版）：

```C++
#include <Adafruit_NeoPixel.h>

#define NUM_LEDS 120
#define PIN1      6   
#define PIN2      7   
#define PIN3      8   
#define PIN4      9   
#define PIN5      10 

// 创建多个灯带的 NeoPixel 对象
Adafruit_NeoPixel strip1 = Adafruit_NeoPixel(NUM_LEDS, PIN1, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip2 = Adafruit_NeoPixel(NUM_LEDS, PIN2, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip3 = Adafruit_NeoPixel(NUM_LEDS, PIN3, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip4 = Adafruit_NeoPixel(NUM_LEDS, PIN4, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip5 = Adafruit_NeoPixel(NUM_LEDS, PIN5, NEO_GRB + NEO_KHZ800);

void setup() {
  strip1.begin();
  strip2.begin();
  strip3.begin();
  strip4.begin();
  strip5.begin();

  setSunshineColor();
  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}

void loop() {
}

void setSunshineColor() {
  for (int i = 0; i < NUM_LEDS; i++) {
    strip1.setPixelColor(i, strip1.Color(255, 70, 0));
    strip2.setPixelColor(i, strip2.Color(255, 70, 0));
    strip3.setPixelColor(i, strip3.Color(255, 70, 0)); // 第三号接口灯环
    strip4.setPixelColor(i, strip4.Color(255, 70, 0));
    strip5.setPixelColor(i, strip5.Color(255, 70, 0));
  }

  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}
```
Windy:
```C++
#include <Adafruit_NeoPixel.h>

#define NUM_LEDS 120
#define PIN1      6
#define PIN2      7
#define PIN3      8
#define PIN4      9
#define PIN5      10

Adafruit_NeoPixel strip1 = Adafruit_NeoPixel(NUM_LEDS, PIN1, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip2 = Adafruit_NeoPixel(NUM_LEDS, PIN2, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip3 = Adafruit_NeoPixel(NUM_LEDS, PIN3, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip4 = Adafruit_NeoPixel(NUM_LEDS, PIN4, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel strip5 = Adafruit_NeoPixel(NUM_LEDS, PIN5, NEO_GRB + NEO_KHZ800);

void setup() {
  strip1.begin();
  strip2.begin();
  strip3.begin();
  strip4.begin();
  strip5.begin();
  setWindColor();
  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}

void loop() {
  simulateWindEffect();
}

void setWindColor() {
  for (int i = 0; i < NUM_LEDS; i++) {
    strip1.setPixelColor(i, strip1.Color(255, 255, 255));
    strip2.setPixelColor(i, strip2.Color(255, 255, 255));
    strip3.setPixelColor(i, strip3.Color(255, 255, 255));
    strip4.setPixelColor(i, strip4.Color(255, 255, 255));
    strip5.setPixelColor(i, strip5.Color(255, 255, 255));
  }
  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();
}

void simulateWindEffect() {
  int randomEffect = random(0, 100);
  int maxBrightness = 255;

  for (int i = 0; i < NUM_LEDS; i++) {
    int brightness = random(0, maxBrightness);

    if (randomEffect < 10) {
      brightness = random(150, 255);
    } else if (randomEffect < 30) {
      brightness = random(50, 150);
    } else if (randomEffect < 60) {
      brightness = random(0, 50);
    } else {
      brightness = random(100, 180);
    }

    strip1.setPixelColor(i, strip1.Color(brightness, brightness, brightness));
    strip2.setPixelColor(i, strip2.Color(brightness, brightness, brightness));
    strip3.setPixelColor(i, strip3.Color(brightness, brightness, brightness));
    strip4.setPixelColor(i, strip4.Color(brightness, brightness, brightness));
    strip5.setPixelColor(i, strip5.Color(brightness, brightness, brightness));
  }

  strip1.show();
  strip2.show();
  strip3.show();
  strip4.show();
  strip5.show();

  delay(50);
}
```
