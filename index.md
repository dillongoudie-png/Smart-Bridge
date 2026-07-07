# Automated Smart Bridge
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

The Automated Smart Bridge models a real-life crossbridge that can.

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Dillon G | West Carer and Technical Academy | Structural Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug](https://www.youtube.com/embed/q_L0DmylsWw?si=51crw0dQSsmofGRf)" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/q_L0DmylsWw?si=6s-BIPYhw7kVViSL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The intial development of the Automated Smart Bridge consists of a moisture sensor as an input as well as a servo motor, LED, and OLED display module as outputs. When the soil moisture sensor detects water, it sends a signal to the Arduino Uno R3 to move the servo motor arm 90°, to the LED to light up, and to the OLED display to render "Flood Alert." Thus far, I have not run into any technical problems, yet I plan to reconstruct the bridge to open outwards from both sides, but hopefully little issues persists. In addition to the modification of the bridge, I also intend to include a sensor that can detect if pedestrians or obstructions are on the bridge when it is supposed to be opening as well as a water distance monitor, that can change how the bridge moves, given the amount of flooded water.

# Schematics 
<img width="1082" height="747" alt="Screenshot 2026-07-06 at 8 39 22 AM" src="https://github.com/user-attachments/assets/0bda7194-1202-45af-a22f-99e0237a4a12" />


# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

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
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino Uno R3 Kit | Provides basic components and wires for arduino and breadboard | $44.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/ELEGOO-Project-Tutorial-Controller-Projects/dp/B01D8KOZF4/ref=sr_1_1_sspa?crid=2XMRZQLWWU48P&dib=eyJ2IjoiMSJ9._L3JiWgIo_Asrnpq9JBCAusLrjnyMbhZZFfeuGm_mo5U2SKqUrGwKWwQWBL8Bh2hODR3qGHLmVqeB02MM53KDXbDMT4F2tNy6-pJWA61X6PMLDUTh9lLm6dCZAXCSwX9p-vOqN-v9PktM9yTwXOb5sBPf6PIXH0ZyY7fhj9FKRWXHl7ssjfc_6E79vbgRdt1VrOvXcJaTQHMUhW1wbBcQwlHBRet1ZY_O34OtMq6vfk.fqeLeSLdRVAqbKiRvI05KfPNc_RW2RcK-c-KwznEXAU&dib_tag=se&keywords=ELEGOO+UNO+R3+Project+Most+Complete+Starter+Kit+with+Tutorial+Compatible+with+Arduino+IDE+%28200%2B+Components%29&nsdOptOutParam=true&qid=1782915405&sprefix=elegoo+uno+r3+project+most+complete+starter+kit+with+tutorial+compatible+with+arduino+ide+200%2B+components+%2Caps%2C195&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| 0.96 Inch OLED Display Modules | Compact Visual Display. ex. illuminates so viewer knows when there is a flood alert | $9.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/ELEGOO-Display-Compact-Self-Luminous-Projects/dp/B0D2RMQQHR/ref=sr_1_1_pp?crid=3KN46F80LVX7O&dib=eyJ2IjoiMSJ9.wmW83Dmxfyl5RxdfjV1gHvBT9aPlfndWCq8RiWeu28E9EnwXbhHTqmqoKkHZks_ytP0H-DU9gOzpLQODuf6AtuTdrLG0hEl0b8gEbAUUV5D_4PLRYC2Y29tPw_4g7LVmIjzXQ4NvIaN-uv_oQAJzayAeO2auKjW3QsenhH9RfDT8PWsec4TGgoYMdV8U9fiZmtG38-FDW2pp6AW3_94zD7JKmruvAsrzS3DWzjWvvKQ.cS5OwRoADXHka-VgBc1G1kTDxNafb9Xt9tM36BjQ1b0&dib_tag=se&keywords=arduino%2Boled&qid=1781489074&sprefix=arduino%2Bole%2Caps%2C155&sr=8-1&th=1)"> Link </a> |
| Digital Multimeter | What the item is used for | $11.98 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Soil Moisture Sensor | Detects water levels in soil | $7.87 | <a href="https://www.amazon.com/hiBCTR-Moisture-Detection-Hygrometer-Automatic/dp/B0FX2PRYFS/ref=sr_1_1_sspa?crid=25DL9TUMOGGBO&dib=eyJ2IjoiMSJ9._jVNDuoGSARHHW-uQvLl4K24wGVKBk0BLmc2h932aMZnrwDqpax35HgWEwY-pJhg06UBW75NdX6tA3E0SavoGwMiWg8WX5UpUvYJ2hBC9y7ILIPN5A2hWLp5XTZ7dS9URr0DZIfvDmAZkARohhJLmVgLFcfaGgvadTxKY490BwANk5nWc1jXdMOj9zi7CcJ0AKEaGmtIIk_unBWqVONZVriJxNcDGpcyBlZzorstKSPlIwhyVvLBAYPjyuDjlNZKQiWH3gRKJMbcbg-Ol5Vcl6s7N4nTB4aO7wsxlG13Skc.RTAg8ctY-x2U4hfvhu86yji5UdeQmNRhMo97A1P-UEU&dib_tag=se&keywords=arduino+soil+sensor&qid=1781489099&sprefix=arduino+soil+sensor%2Caps%2C143&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Flexible Straws | Base Support or Pillars for the Road Bridge | $2.29 | <a href="https://www.amazon.com/Amazon-Basics-Disposable-Striped-Assorted/dp/B0D6T59JGQ/ref=sr_1_5?crid=1K62MGIWYEA9L&dib=eyJ2IjoiMSJ9.7sMDEGAMnJ_wunAvw4cLRv5Dwne4oUSR9c8fo2n9I3IZKJtjEe4cnsbSC2ET6EWWkLLAO0uHSgrAmglcwC8DlPXR1NcJvmn0IgNNxKeFhb0MKve6Fz35BDFQYiFh1Akl_reK8zzNg8Hbh__wGtmaYNgGg94iFcOzrLCKBXuuDznUmugCU3QE9SYoQIkbgq_KoC28YX6ta8Z0_rvPT49inm7N9zrZDgLlYauxlNuGHNU1KhqrZbsSq4gH6lBa0DGVzjFItH-83nQJ0cntIKTgTSVqRnkVzXtvofjcc7_EqlI.ZQvZ1LWv9iDMUbGRIdBxRBglJ_mIXu_3tTAzJ8lwT3Q&dib_tag=se&keywords=flexible%2Bstraws&qid=1782916840&sprefix=flexible%2Bstraw%2Caps%2C203&sr=8-5&th=1"> Link </a> |
| Tacky Glue | Adhesive for bridge parts such as straws and road | $8.94 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Aleenes-Purpose-Tacky-Glue-8-Ounce/dp/B00178KLEY/ref=sr_1_1?crid=GXE22TYDD741&dib=eyJ2IjoiMSJ9.8nuIIT_EIJgqrsM-17U9_l7DbillQ7zckkzH2Y2dVJcHD1XS4t69R5VN1a_LWFKlIKlZxcP8tOacfb2pWSJ-CyzZGZg9NnkCZrMCYkLLmG1kyGxcBRPe658WnTRw3CUtPBmi6NshGHh4Gd14uSwauBCJbUhxZARIJ8Dwtw_5djoqaZDcwqSKb_AdUF_3imvyaxcfWGqvK8vDgyhm9iMEV0S00WDeKe69-hom7gzfTT41lpOJshVmpFwpOuJUCrcpGBsY-pYcu73DAORQ70snpgPKP9-cPWsXE9-u9bv3FEs.3uw-_dYG3CQJJCwM1dUUQgtgCKHfQWstHw7EGQG9clg&dib_tag=se&keywords=aleene%27s%2Boriginal%2Btacky%2Bglue&qid=1782916995&sprefix=original%2Btacky%2Bglue%2Caps%2C253&sr=8-1&th=1)"> Link </a> |
| Cardboard | Structural mateiral of the Bridge / Road | $4.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/EcoSwift-Chipboard-Cardboard-Scrapbook-Scrapbooking/dp/B0B6GK2MFD/ref=sr_1_3?crid=1BMN8EQHI0ITG&dib=eyJ2IjoiMSJ9.xximf4qVenL0nm7v5zeOLzC6RL9HVrzAVi48RsManOigcvxOqnv6Em2ue1BtSKdzfzIf1p0I9gIVHGIxNYLH34adFGmU-EJi4yiLQ5K4Trj4ze1y0wKaSrkWTvKO1LCeauwGuzbE5MF2FUqCA1eKS4bgW8FbtEXjdVW1japQP7iU2JYLVpqACXAoDHc7_3x33w9GqG48JnEVgdnUNOVM1H1_hRInglkiePn8y3dDY4I.oWQPUV0POHGQpHKCeMF-4fcKAVltH3zsFa8OJzC68zQ&dib_tag=se&keywords=cardboard&qid=1783347123&sprefix=cardboar%2Caps%2C240&sr=8-3)"> Link </a> |
| Styrofoam | Another structural material of the bridge | $9.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/BENECREAT-Rectangle-Mounting-Modelling-Projects/dp/B08M3FYVYW/ref=sr_1_2?crid=1IAJHNOAFGUK0&dib=eyJ2IjoiMSJ9.bw4iZ2H9Yq2nv7nboOG-n4M2XUHTlxcGf3HzckO3nxxF0wRSNF6d1AQdfrYmHUtHovEeKYhRFii0fo9sqtvaqbrBujAwgXDvbyYzLW8ZYysQVgwysXdFcDPjWALTs2cMIu5f9CGFYKR-t3k4J-YTfaVQ5X9KYA2FeL6zkBRnvLvcEOuNa4j97qgx9fmheP3R-40SdYPldNv_Jhq-4Pt0JJLApO9qztDa-YLNdpQqiCFWhaN0Qhn6qBfwWrAtrXYEwa54vXFVmgZoI7GI_-fDHCYBFwIJMWJO5opR6G71lPI.8xGM7mqm1d3ua1qO0c6AHx7Orj11Qsp-Spgf2z1jUHY&dib_tag=se&keywords=styrofoam%2Bsheet&qid=1783347298&sprefix=styrofoam%2Bsheet%2Caps%2C232&sr=8-2&th=1)"> Link </a> |
| SG90 Servo Motor | Additional servo motor, not included in kit, so each motor can raise each side of the bridge | $6.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Sipytoph-Helicopter-Airplane-Walking-Control/dp/B09185SC1W/ref=sr_1_15?crid=E6T3Z2XULIIF&dib=eyJ2IjoiMSJ9.Z8zXoZs9nMkNwqQN2AI2Fpmdc9MzuuK-PqFIEDHJnO6gHj4NZWMqtADViTXlxuO9qFKf_cJMoeOs80zbVRyefDEeW3zVOep4vatPT6wIcHFRu3gl7xdkMJrxBp_hAoISL_OFAKN_mJ6HBcHhasqiuE_tfZg1zWlHmIyLnX61OaAsC-jixzimDd6Il70o5UtE1Wp4Fgcb2tP4z2ju3OtH_MwXqN1gupioIbzQewivbT2f5Rb9qcReNrkUxo2j8ngfA-zjFDirB1zqQV1YHHEEUO8JUxTd9rLj7-hwYVoejzU.mCT6aju_52NFHGALCxaV7Fapr-EYwFh9irdJaUIF644&dib_tag=se&keywords=sg90+servo+motor+singular&qid=1783346702&sprefix=sg90+servo+motor+singular%2Caps%2C162&sr=8-15)"> Link </a> |
| IR Break Beam Sensor | Invisible Trip-Wire that detects if there is an obstruction (car or pedestrian) still on the bridge | $7.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Sensor-Distance-Counting-Photoelectric-Through-Beam/dp/B0FPCR98F2/ref=sr_1_2_sspa?crid=3L28IXXCV9KMP&dib=eyJ2IjoiMSJ9.FIUf_oSN4SqwCmBKoMSTJGQYplRKY7marGwPqaFvz_621DKQZ8ZZxiY8iqg_IEWZ_o3fu6awV6UUYOkj1HMdyoOSSrQ68LPqZGhQ53e-VnNg2TnqD6pVtVdlovDUmZq-UtRdtNyBrtFFmseIuogCHMwrvZJzYfrNodkH2uYkd14BXoHMETREim6wZI7L3c2r-iUv-sRqwFGBPSdTYr-oDduKzY-mxiylZzakwtGtVNk.0YCIb_zDly6kwST-MABdMEZ4V_Xa6BwaIJGFeNYbhZo&dib_tag=se&keywords=IR%2BBreak-Beam%2BSensor%2BPair&qid=1783346777&sprefix=ir%2Bbreak-beam%2Bsensor%2Bpair%2Caps%2C199&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1)"> Link </a> |
| Extra Long Male to Male Jumper Wires | Route power and signals from arduino to far sides of the bridge | $6.99 | <a href="https://www.amazon.com/Solderless-Multicolored-Electronic-Breadboard-Protoboard/dp/B09FP82PKT/ref=sr_1_3?crid=3K37WVB5Y6MV7&dib=eyJ2IjoiMSJ9.RbMWgwKvUc0W9hvUMOHpdxBXkfNydTntclB-102Cje1D1DuPZmO6eRZassMvASxP_vKwSMM7aFqxRAL-WCtHfigEs76AX7-HgmOtrTwwBVsWJJ0QGVJpD19srwi7N0Wx5d4GACKcyIYGFJBYeDZFvZD1GLE5tStCjwgX1k1fJeUO4lPUe5lB8g7vjzeHErKS-EtXon5JmB1UhWgAflDxIeIL0obDTlYXaedttqCbJ7Y.mbDcSyGf4g-ssgtiLAnzTOuolvkiP6BxHpHn6rwDvRE&dib_tag=se&keywords=extra%2Blong%2Bmale%2Bto%2Bmale%2Bjumper%2Bwires%2B30%2Bcm&nsdOptOutParam=true&qid=1783346859&sprefix=extra%2Blong%2Bmale%2Bto%2Bmale%2Bjumper%2Bwires%2B30%2Bcm%2Caps%2C170&sr=8-3&th=1"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
