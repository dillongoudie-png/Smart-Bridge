# Automated Smart Bridge
The Automated Smart Bridge models a real-life crossbridge that can both permit vehicular transportation and open outwards as a safety precuation when flooding occurs. The bridge includes a plethora of features including street lights that illuminate when their surroundings dim, an invisible trip-wire that detects obstacles on the bridge, and an ultrasound sensor that detects changes in water level. Working on this project and navigating difficulties facilitated not only my critical thinking skills but also my creativity, enhancing my aptitude to think like an engineer.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Dillon G | West Carer and Technical Academy | Structural Engineering | Incoming Senior

<img src="https://github.com/user-attachments/assets/d285090e-0e42-4e88-829c-061b0483e5e1" alt="Smart Bridge Project" style="max-width: 100%; height: auto; display: block; margin: 0 auto;" />

# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/AnuohnbM69U?si=LVP5NC9T2S9nbt0O" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Relative to the prior milestone, the development of the Smart Bridge consists of implementing new sensors to add safety features and resemble real-life bridges. 

## New Components
+ **Ultrasonic Sensor** - I replaced the moisture sensor with the ultrasonic sensor with the intention of adjusting bridge mechanics based on the distance from water the ultrasonic sensor detected. However, This never persisted due to time constraints, although, the sensor will still detect water within 10cm of itself which yields the bridge to open.
+ **Break-Beam Sensor** - This pair was installed, so the bridge could detect if there is a pedestrian, vehicle, or other obstruction on the bridge that could render dangerous as the bridge draws open. If an obstruction is detected, the sensor overrides the rest of the circuit and immediately closes the bridge until the obstruction has been moved. The most difficult part of the installation of this pair was the mounting onto the bridge. I used a hole puncher and multiple pieces of cardboard to stand each sensor up on the bridge.
+ **Second Servo Motor** - Most bridges today draw outward, hence, to replicate this in my project, I cut the middle of the bridge with the thought process of having two servo motors on either end that open each regarded side of the bridge. Although, the arm of the servo motor was not long enough to open each end independently, so I conjoined a stick to the arm via tape. Now, each bridge opens 90 degrees.
+ **OLED Screen** - I used an OLED screen in the previous milestone, but with the addition of the break beam sensors, the OLED screen can now render "Wait: Car on Bridge" when there is a disruption in the invisible trip-wire.


<!--# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/AnuohnbM69U?si=LVP5NC9T2S9nbt0O" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone -->

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/q_L0DmylsWw?si=6s-BIPYhw7kVViSL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The intial development of the Automated Smart Bridge consists of a moisture sensor as an input as well as a servo motor, LED, and OLED display module as outputs. When the soil moisture sensor detects water, it sends a signal to the Arduino Uno R3 to move the servo motor arm 90°, to the LED to light up, and to the OLED display to render "Flood Alert." Thus far, I have not run into any technical problems, yet I plan to reconstruct the bridge to open outwards from both sides, but hopefully little issues persists. In addition to the modification of the bridge, I also intend to include a sensor that can detect if pedestrians or obstructions are on the bridge when it is supposed to be opening as well as a water distance monitor, that can change how the bridge moves, given the amount of flooded water.

# Schematics 
<img width="754" height="503" alt="Screenshot 2026-07-15 at 7 23 43 AM" src="https://github.com/user-attachments/assets/52c8f03f-6108-4f96-8f70-4b73c7ca867c" />


# Code
Arduino Uno R3

```c++
#include <Servo.h>           // Include Servo library
#include <Wire.h>            // Include I2C library
#include <Adafruit_GFX.h>    // Include Adafruit GFX library
#include <Adafruit_SSD1306.h> // Include Adafruit SSD1306 OLED library

// OLED display configuration
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

Servo tap_servo;

// Pin definitions
const int sensor_pin = 5;    // Digital output (DO) pin of the soil moisture sensor
const int tap_servo_pin = 4; // Servo signal pin
const int sensor_vcc = 7;    // Power pin for the sensor (optional for controlled power)
const int led_pin = 6;       // LED to indicate flood alert

int val;

void setup() {
  pinMode(sensor_pin, INPUT);      // Soil sensor digital output
  pinMode(sensor_vcc, OUTPUT);    // Sensor power pin
  pinMode(led_pin, OUTPUT);       // LED pin
  digitalWrite(sensor_vcc, HIGH); // Turn on the sensor
  tap_servo.attach(tap_servo_pin);

  // Initialize the OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { // I2C address 0x3C
    Serial.println("SSD1306 allocation failed");
    for (;;); // Don't proceed, loop forever
  }

  // Display startup message
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(WHITE);
  display.setCursor(10, 25); // Position the text
  display.println("Safe Level"); // Display the initial status
  display.display();
  delay(2000); // Show startup message for 2 seconds
  display.clearDisplay(); // Clear the display
}

void loop() {
  val = digitalRead(sensor_pin); // Read the digital output (DO) pin of the sensor

  if (val == LOW) { // LOW means water detected
    tap_servo.write(90);         // Rotate servo to 90°
    digitalWrite(led_pin, HIGH); // Turn on LED
    displayStatus("Flood Alert");// Display flood alert message
  } else { // HIGH means no water detected
    tap_servo.write(0);          // Rotate servo back to 0°
    digitalWrite(led_pin, LOW);  // Turn off LED
    displayStatus("Safe Level"); // Display safe message
  }
  delay(500); // Short delay for stability
}

// Function to display status on OLED
void displayStatus(const char* message) {
  display.clearDisplay();          // Clear previous content
  display.setTextSize(2);          // Set text size
  display.setTextColor(WHITE);     // Set text color
  display.setCursor(10, 25);       // Position the text
  display.println(message);        // Display the message
  display.display();               // Update the OLED screen
}
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino Uno R3 Kit | Provides basic components and wires for arduino and breadboard | $44.99 | [Link](https://www.amazon.com/dp/B008GRTSV6/) |
| 0.96 Inch OLED Display Modules | Compact Visual Display. ex. illuminates so viewer knows when there is a flood alert | $9.99 | [Link](https://www.amazon.com/dp/B0D2RMQQHR/) |
| Digital Multimeter | Testing Current and Debugging | $11.98 | [Link](https://www.amazon.com/dp/B008GRTSV6/) |
| Soil Moisture Sensor | Detects water levels in soil | $7.87 | [Link](https://www.amazon.com/dp/B0FX2PRYFS/) |
| Flexible Straws | Base Support or Pillars for the Road Bridge | $2.29 | [Link](https://www.amazon.com/dp/B0D6T59JGQ/) |
| Tacky Glue | Adhesive for bridge parts such as straws and road | $8.94 | [Link](https://www.amazon.com/dp/B00178KLEY/) |
| Cardboard | Structural mateiral of the Bridge / Road | $4.99 | [Link](https://www.amazon.com/dp/B0B6GK2MFD/) |
| Styrofoam | Another structural material of the bridge | $9.99 | [Link](https://www.amazon.com/dp/B08M3FYVYW/) |
| SG90 Servo Motor | Additional servo motor, not included in kit, so each motor can raise each side of the bridge | $6.99 | [Link](https://www.amazon.com/dp/B09185SC1W/) |
| IR Break Beam Sensor | Invisible Trip-Wire that detects if there is an obstruction (car or pedestrian) still on the bridge | $7.99 | [Link](https://www.amazon.com/dp/B0FPCR98F2/) |
| Extra Long Male to Male Jumper Wires | Route power and signals from arduino to far sides of the bridge | $6.99 | [Link](https://www.amazon.com/dp/B09FP82PKT/) |
| Tape | Assembling Sensors and Wires to Bridge | $11.85 | [Link](https://www.amazon.com/dp/B0000DH8HQ/) |

# Other Resources/Examples
- [Example Project](https://www.instructables.com/Smart-Bridge-Using-Arduino-With-Auto-Height-Increa/)
- [Ultrasonic Sensor](https://sviatil0.github.io/Sviatoslav_BSE/](https://projecthub.arduino.cc/Isaac100/getting-started-with-the-hc-sr04-ultrasonic-sensor-7cabe1))
- [Break Beam Sensor](https://arneshkumar.github.io/arneshbluestamp/](https://learn.adafruit.com/ir-breakbeam-sensors/arduino))

