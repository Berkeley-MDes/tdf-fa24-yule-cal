# Week 11
# Week of 11/14/2024

Project: Initial Planning for TDF Project 4 - Mix, Master, Extend, and Evolve

Summary: This week, I met with team members Chuhua (Charlie) Ding and Ryugwang (Josh) Jang to discuss ideas for Project 4. We focused on developing a concept that aligns with the project’s emphasis on blending technology with creative problem-solving in a connected ecosystem. After brainstorming, we agreed on an idea aimed at promoting healthier habits for children.

<img src ="https://github.com/user-attachments/assets/11b422bf-dd01-4c37-b999-d4f4837c037f" width = "80%">

Project Idea: Wearable Device with Calorie Tracking for Snack Access Control Our concept involves designing a wearable device that tracks a child’s calorie expenditure in real-time. This device would connect to a snack box with a lockable lid that only opens when a certain calorie threshold is met, encouraging kids to engage in more physical activity before accessing snacks. By integrating the Particle Photon2, we plan to establish cloud communication for data tracking, using sensors for activity monitoring, and an actuated mechanism to control snack box access.

Next Steps:
Refine the technical requirements for the wearable device and snack box system.
Outline a series of experiments to test calorie tracking accuracy and the actuation of the snack box lock.
Develop a project proposal including milestones and feasibility metrics for the “Design Showcase” on December 12th.
Images of our brainstorming session will be added to this update.


# Week 10
# Week of 11/07/2024

Video: https://youtu.be/KAFnNR242yk?si=bHYh3BsRr4lKRL2H

Project: AI-Powered Portfolio Summarization Agent on ZeroWidth
Overview:
This project focuses on creating an AI-powered agent using ZeroWidth that effectively summarizes my portfolio. Inspired by my desire to streamline the text-heavy nature of my portfolio, I developed an agent flow that condenses key information, making it concise and engaging for viewers.

Process:
Knowledge Base Setup:
Uploaded my complete portfolio into the ZeroWidth Knowledge Base. This served as the foundational data source for the agent.

Single Prompt Structure:
Created a Single Prompt setup, connecting two main prompts:

Prompt 1: Extracts specific project details, including project year and my role.
Prompt 2: Generates responses with an engaging tone, adding subtle praise to highlight my achievements.
Variable Adjustment:
Integrated key variables (e.g., Date, Role, and Outcomes) to ensure that the agent provided structured and relevant information. Adjusted parameters like Number of Chunks and Similarity Threshold for better accuracy.

Results:
The agent successfully responds to queries about my work (e.g., “Any hardware work?” or “Any software work?”) with accurate details on project dates, roles, and outcomes, while maintaining an encouraging tone. This setup not only made the portfolio more approachable but also provided an engaging user experience that highlights my accomplishments.

Reflections:
ZeroWidth proved valuable for prototyping AI-driven service ideas. Although it feels like an advanced, AI-integrated version of Grasshopper, for simpler use cases, directly using GPT-4.0 might sometimes be more efficient.

Future Directions:
I aim to refine the agent further, exploring additional customizations to make the responses even more adaptive to specific queries.

# Week 9
# Week of 10/31/2024

I approached ZeroWidth with curiosity and a bit of caution, knowing that each new tool brings its own specific learning curve. The structured, interactive nature of the class helped me get comfortable with the platform more easily, making the initial steps less daunting.

For my first exploration, I set up a test flow, which I called “Trial Flow,” as a way to experiment and become familiar with ZeroWidth’s interface. I appreciated that its design had some similarities to tools I’ve used previously, such as Figma and Miro, with its organized layout and accessible icon-driven functionality. This intuitive setup let me dive straight into building without too much time spent figuring out the basics.

As I began building my trial flow, I discovered how well ZeroWidth is designed for iterating on LLM workflows. The tool’s flexibility in managing multiple steps, organizing functions, and visualizing interactions between components made it clear how it supports rapid development. With each new element added, I felt my confidence grow, as the tool enabled me to test out a variety of LLM-based processes quickly and with clarity.

Overall, this experience has given me a fresh perspective on the value of prototyping tools in the context of LLMs. I’m looking forward to leveraging ZeroWidth to test out practical LLM agent designs, which will allow me to merge theoretical knowledge with hands-on design capabilities in meaningful ways.

# Week 8
# Week of 10/24/2024
We (Josh and I) are fully immersed in the 2nd TDF final work and making the video this week.

Here is our work! :)

[TDF Project 2 Keeper](https://www.youtube.com/watch?v=z6CSoZwMAkM)

This project enhances a posture monitoring system built with the Photon 2 microcontroller by incorporating multi-level feedback using LED color changes and vibration intensity. The system uses an MPU6050 motion sensor to track deviations from a calibrated baseline posture and provides feedback based on the severity of the deviation.

1. Features
1-1. Calibration:
- A calibration button defines the current posture as the standard baseline using accelerometer and gyroscope data.

1-2. Deviation Monitoring:
- The system continuously monitors posture and compares it to the baseline.
- Deviations are classified into three levels: mild, moderate, and severe.

1-3. Multi-Level Feedback:
- LED Indicators:
No deviation: No LED output (system is idle).
Mild deviation: Yellow LED.
Moderate deviation: Orange LED.
Severe deviation: Red LED.

- Vibration Feedback:
Mild deviation: Light vibration.
Moderate deviation: Medium vibration.
Severe deviation: Strong vibration.

1-4. Power Control:
- A power button toggles the system ON/OFF.

**2. Initialization**
- Set up the MPU6050 sensor, DRV2605 motor, and RGB LED pins. Configure the power and calibration buttons.

```
#include <Wire.h>
#include <MPU6050.h>
#include "Adafruit_DRV2605.h"

// Pin definitions
#define POWER_BUTTON_PIN D6
#define CALIBRATION_BUTTON_PIN D7
#define LED_RED_PIN D8
#define LED_GREEN_PIN D9
#define LED_BLUE_PIN D10

// MPU6050 and DRV2605 objects
MPU6050 accelgyro;
Adafruit_DRV2605 drv;

// Sensor data
int16_t ax, ay, az;
int16_t gx, gy, gz;

// Baseline data
int16_t baselineAx = 0, baselineAy = 0, baselineAz = 0;
int16_t baselineGx = 0, baselineGy = 0, baselineGz = 0;

// Constants
const int MILD_THRESHOLD = 500;    // Mild deviation threshold
const int MODERATE_THRESHOLD = 1000; // Moderate deviation threshold
const int SEVERE_THRESHOLD = 1500;  // Severe deviation threshold
bool powerState = false;
bool calibrationActive = false;

void setup() {
    pinMode(POWER_BUTTON_PIN, INPUT);
    pinMode(CALIBRATION_BUTTON_PIN, INPUT);
    pinMode(LED_RED_PIN, OUTPUT);
    pinMode(LED_GREEN_PIN, OUTPUT);
    pinMode(LED_BLUE_PIN, OUTPUT);

    Wire.begin();
    Serial.begin(9600);

    // Initialize MPU6050
    accelgyro.initialize();
    if (accelgyro.testConnection()) {
        Serial.println("MPU6050 connected successfully.");
    } else {
        Serial.println("MPU6050 connection failed.");
        while (1);
    }

    // Initialize DRV2605
    if (!drv.begin()) {
        Serial.println("DRV2605 initialization failed.");
        while (1);
    }
}

```

**3. Calibration**
- When the calibration button is pressed, the current posture is saved as the baseline. This data is used as the reference for detecting deviations.

```
void calculateBaseline() {
    int sumAx = 0, sumAy = 0, sumAz = 0;
    int sumGx = 0, sumGy = 0, sumGz = 0;

    for (int i = 0; i < 5; i++) {
        accelgyro.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);
        sumAx += ax; sumAy += ay; sumAz += az;
        sumGx += gx; sumGy += gy; sumGz += gz;
        delay(50);
    }

    baselineAx = sumAx / 5;
    baselineAy = sumAy / 5;
    baselineAz = sumAz / 5;
    baselineGx = sumGx / 5;
    baselineGy = sumGy / 5;
    baselineGz = sumGz / 5;

    calibrationActive = true;
    Serial.println("Calibration complete.");
}
```
**4. Multi-Level Feedback**
- Deviation Detection: The system calculates the absolute difference between real-time and baseline sensor values to determine the deviation level.

```

void checkDeviation() {
    accelgyro.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);

    int deviation = max({
        abs(ax - baselineAx),
        abs(ay - baselineAy),
        abs(az - baselineAz),
        abs(gx - baselineGx),
        abs(gy - baselineGy),
        abs(gz - baselineGz)
    });

    if (deviation > SEVERE_THRESHOLD) {
        provideFeedback("severe");
    } else if (deviation > MODERATE_THRESHOLD) {
        provideFeedback("moderate");
    } else if (deviation > MILD_THRESHOLD) {
        provideFeedback("mild");
    } else {
        resetFeedback();
    }
}

```
4-1. Feedback Levels:
- Provide feedback using LED colors and vibration intensities based on the deviation level.

```
void provideFeedback(String level) {
    if (level == "mild") {
        setLEDColor(255, 255, 0); // Yellow
        playHapticFeedback(30);  // Light vibration
    } else if (level == "moderate") {
        setLEDColor(255, 165, 0); // Orange
        playHapticFeedback(50);  // Medium vibration
    } else if (level == "severe") {
        setLEDColor(255, 0, 0); // Red
        playHapticFeedback(70);  // Strong vibration
    }
}

void resetFeedback() {
    setLEDColor(0, 0, 0); // Turn off LEDs
    drv.stop();           // Stop vibration
}

```

4-2. RGB LED Control:
- Set the color of the RGB LED.

```
void setLEDColor(int red, int green, int blue) {
    analogWrite(LED_RED_PIN, red);
    analogWrite(LED_GREEN_PIN, green);
    analogWrite(LED_BLUE_PIN, blue);
}

```

4-3. Vibration Feedback:
- Control the DRV2605 motor for different vibration intensities.

```
void playHapticFeedback(int effect) {
    drv.setWaveform(0, effect); // Set vibration effect
    drv.setWaveform(1, 0);      // End vibration
    drv.go();
}

```

**5. Main Control Logic**
- The main loop handles system power, calibration, and deviation monitoring.

```
void loop() {
    if (digitalRead(POWER_BUTTON_PIN) == HIGH) {
        powerState = !powerState;
        delay(500);
    }

    if (powerState) {
        if (calibrationActive) {
            checkDeviation();
        }

        if (digitalRead(CALIBRATION_BUTTON_PIN) == HIGH) {
            calculateBaseline();
        }
    } else {
        resetFeedback(); // Ensure no feedback when the system is OFF
    }

    delay(100);
}

```

**6. Summary of Features**

6-1. Power Control:
- A power button toggles the system ON/OFF.
- LED indicates the system state.

6-2. Calibration:
- A calibration button sets the baseline posture.

6-3. Multi-Level Feedback:
- LED Colors: Indicate the severity of the deviation.
- Vibration: Alert the user with varying intensities.

**7. Testing and Use Cases**

7-1. Power ON:
- Press the power button to activate the system. The LED turns on to indicate it's operational.

7-2. Calibration:
- Assume a standard posture and press the calibration button. The system stores this posture as the baseline.

7-3. Deviation Detection:
- Move out of the standard posture. The system will trigger vibration feedback if deviations exceed the threshold.

8. Final Code: Posture Monitoring System

```
#include <Wire.h>
#include <MPU6050.h>
#include "Adafruit_DRV2605.h"

// Pin definitions
#define POWER_BUTTON_PIN D6
#define CALIBRATION_BUTTON_PIN D7
#define LED_RED_PIN D8
#define LED_GREEN_PIN D9
#define LED_BLUE_PIN D10

// MPU6050 and DRV2605 objects
MPU6050 accelgyro;
Adafruit_DRV2605 drv;

// Sensor data
int16_t ax, ay, az;  // Accelerometer
int16_t gx, gy, gz;  // Gyroscope

// Baseline data
int16_t baselineAx = 0, baselineAy = 0, baselineAz = 0;
int16_t baselineGx = 0, baselineGy = 0, baselineGz = 0;

// Constants
const int MILD_THRESHOLD = 500;    // Mild deviation threshold
const int MODERATE_THRESHOLD = 1000; // Moderate deviation threshold
const int SEVERE_THRESHOLD = 1500;  // Severe deviation threshold
bool powerState = false;
bool calibrationActive = false;

void setup() {
    // Pin setup
    pinMode(POWER_BUTTON_PIN, INPUT);
    pinMode(CALIBRATION_BUTTON_PIN, INPUT);
    pinMode(LED_RED_PIN, OUTPUT);
    pinMode(LED_GREEN_PIN, OUTPUT);
    pinMode(LED_BLUE_PIN, OUTPUT);

    // Initialize I2C
    Wire.begin();
    Serial.begin(9600);

    // Initialize MPU6050
    Serial.println("Initializing MPU6050...");
    accelgyro.initialize();
    if (accelgyro.testConnection()) {
        Serial.println("MPU6050 connected successfully.");
    } else {
        Serial.println("MPU6050 connection failed.");
        while (1);
    }

    // Initialize DRV2605
    Serial.println("Initializing DRV2605...");
    if (!drv.begin()) {
        Serial.println("DRV2605 initialization failed.");
        while (1);
    }
}

void calculateBaseline() {
    int sumAx = 0, sumAy = 0, sumAz = 0;
    int sumGx = 0, sumGy = 0, sumGz = 0;

    for (int i = 0; i < 5; i++) {
        accelgyro.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);
        sumAx += ax; sumAy += ay; sumAz += az;
        sumGx += gx; sumGy += gy; sumGz += gz;
        delay(50); // Sampling delay
    }

    // Calculate average
    baselineAx = sumAx / 5;
    baselineAy = sumAy / 5;
    baselineAz = sumAz / 5;
    baselineGx = sumGx / 5;
    baselineGy = sumGy / 5;
    baselineGz = sumGz / 5;

    calibrationActive = true; // Enable monitoring
    Serial.println("Calibration complete.");
}

void checkDeviation() {
    accelgyro.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);

    int deviation = max({
        abs(ax - baselineAx),
        abs(ay - baselineAy),
        abs(az - baselineAz),
        abs(gx - baselineGx),
        abs(gy - baselineGy),
        abs(gz - baselineGz)
    });

    if (deviation > SEVERE_THRESHOLD) {
        provideFeedback("severe");
    } else if (deviation > MODERATE_THRESHOLD) {
        provideFeedback("moderate");
    } else if (deviation > MILD_THRESHOLD) {
        provideFeedback("mild");
    } else {
        resetFeedback();
    }
}

void provideFeedback(String level) {
    if (level == "mild") {
        setLEDColor(255, 255, 0); // Yellow
        playHapticFeedback(30);  // Light vibration
    } else if (level == "moderate") {
        setLEDColor(255, 165, 0); // Orange
        playHapticFeedback(50);  // Medium vibration
    } else if (level == "severe") {
        setLEDColor(255, 0, 0); // Red
        playHapticFeedback(70);  // Strong vibration
    }
}

void resetFeedback() {
    setLEDColor(0, 0, 0); // Turn off LEDs
    drv.stop();           // Stop vibration
}

void setLEDColor(int red, int green, int blue) {
    analogWrite(LED_RED_PIN, red);
    analogWrite(LED_GREEN_PIN, green);
    analogWrite(LED_BLUE_PIN, blue);
}

void playHapticFeedback(int effect) {
    drv.setWaveform(0, effect); // Set vibration effect
    drv.setWaveform(1, 0);      // End vibration
    drv.go();
}

void loop() {
    if (digitalRead(POWER_BUTTON_PIN) == HIGH) {
        powerState = !powerState;
        delay(500); // Debounce
    }

    if (powerState) {
        if (calibrationActive) {
            checkDeviation();
        }

        if (digitalRead(CALIBRATION_BUTTON_PIN) == HIGH) {
            calculateBaseline();
        }
    } else {
        resetFeedback(); // Ensure no feedback when the system is OFF
    }

    delay(100); // Loop delay
}

```



# Week 7
# Week of 10/17/2024

Preparation Process
1. Board Update
To ensure the board operates smoothly, we updated and initialized it.
We separated the board from the sensor components using wires for several reasons:
  - Flexibility in expanding functionality
  - To prevent damage to the board’s pins

<img src ="https://github.com/user-attachments/assets/b0c3d0e5-2caa-4308-9cb3-3f09566c78b2" width = "80%">
<img src ="https://github.com/user-attachments/assets/8a1494c9-347d-447b-b0e5-85fcf7dcdae4" width = "80%">

2. Hardware Soldering
  - Soldered the QT connector board and pins
  - Soldered the vibration motor and control board

3. Development Process - Output
  - Before integrating the entire program, we needed to verify the successful operation of each module (both hardware and software).
  - The syntax structure is similar to Arduino, making it easier to work with.

<img src ="https://github.com/user-attachments/assets/39925e8a-1ea4-4d00-bd5b-c2db7ce779ae" width = "80%">

4. LED - Verification
- We ran a simple blinking code to test the LED, where the light blinks every second.


https://github.com/user-attachments/assets/5029e72d-ffd7-40eb-863a-1497f84d6182

5. Vibration Motor
- Capable of producing various vibration effects

I plan to complete the remaining tasks over the weekend and will continue fine-tuning the system based on feedback from the next class.


# Week 6
# Week of 10/10/2024

<img width="496" alt="Screenshot 2024-10-10 at 1 38 34 PM" src="https://github.com/user-attachments/assets/6d9495e7-9fd6-4662-980b-92794ca73b8a">

This week, we explored the Particle ecosystem in more depth. We learned how to control the Particle.io Photon 2 microcontroller, integrate hardware components for sensing and output, manage actuation and status relays, and use web APIs to gain insights into global activities or specific engagements. Additionally, we worked on code to tie all these elements together.

<img src ="https://github.com/user-attachments/assets/ed855807-c65b-426e-b4de-feb6009a733c" width = "80%">

We also need to organize a team for the Digital Ecosystem Project. I came up with an idea that I have always wanted to try to solve my issue. The concept is to monitor and provide haptic feedback for poor neck posture, especially during extended periods of computer or smartphone use. I formed a team with Josh, who also has experience struggling with this.

<img src ="https://github.com/user-attachments/assets/b81dfa43-5fe5-4e49-b366-d85c758a18dd" width = "70%">

We’ve developed a diagram that utilizes a gyroscope sensor and incorporates a calibration process. Since everyone has different postures due to variations in height and angle, the calibration allows the system to set a standard posture based on the user’s upright position. The system then uses this data as a reference, and if the user slouches for too long, it triggers a haptic motor as a form of feedback.

Today, we’re presenting this idea in class and plan to incorporate feedback from our classmates and instructor.

# Week 5
# Week of 10/03/2024

<img src ="https://github.com/user-attachments/assets/e21ae40e-6493-467f-a5ea-2ef7dc4aac54" width = "70%">

This week, we received the PARTICLE customizable IoT kit. I’ve heard of Particle, a company that specializes in IoT (Internet of Things) solutions. They offer a complete platform for developing, connecting, and managing IoT devices, providing tools like development kits, cloud services, and connectivity options. Particle is well-known for its hardware modules, such as the Photon and Boron, which are designed for both hobbyist and industrial use, supporting everything from prototypes to large-scale deployments. 

<img width="896" alt="Screenshot 2024-10-03 at 2 01 34 PM" src="https://github.com/user-attachments/assets/7e2d12c7-d7d4-4286-a4c9-1550766a4e7d">

I’m already familiar with this type of IoT kit, as I have prior experience developing an IoT device for posture correction (more details can be found [here](https://www.yulekim.bio/keeper)). However, during that project, we used Arduino rather than Visual Studio Code, and my focus was more on user experience design and Project Manager rather than the actual coding itself.

<img width="974" alt="Screenshot 2024-10-03 at 1 59 36 PM" src="https://github.com/user-attachments/assets/fc6a8450-5132-4211-9e26-1778897d6e6e">

Last time, I connected the kit to my laptop and learned the basics, such as printing “Hello World” through Visual Studio Code. Now, I’m working on getting more familiar with the Visual Studio Code platform. Next week, there’s a specific topic set, and I plan to explore the platform further and experiment with something fun.

# Week 4
# Week of 09/26/2024
My Interaction Ecosystems

For this assignment, I was tasked with analyzing my daily media and entertainment ecosystem, focusing on how I use various devices and applications throughout the day. The goal was to break down my app usage, categorize it, and then creatively represent these categories using abstract symbols. This process helped me not only understand my screen time but also reflect on how different areas of my digital life interact with one another.

Devices and Screen Time Analysis
First, I began by identifying the digital devices I primarily use: my iPhone, iPad, and MacBook. I accessed the Screen Time feature on each of these devices to see which apps I use most frequently.

<img src ="https://github.com/user-attachments/assets/a2ba3b13-c934-4670-9c07-fbbfb8d4b861" width = "70%">

Next, I categorized these apps into larger groups, such as Social, Entertainment, and Productivity & Finance, and calculated the percentage of time spent in each category.
<img src ="https://github.com/user-attachments/assets/37f76cb4-c4dd-47e4-8fe9-4aeeec5e5ad4" width = "70%">

Abstract Representation of Categories
After categorizing the apps and calculating their usage percentages, I moved on to the creative step: selecting abstract objects that could represent these categories and their proportions.

Gears (Social, Productivity & Finance, Utility)
First, I chose gears to represent Social, Productivity & Finance, and Utility. The gears symbolize how these areas of my life are interconnected, with Social being the largest gear driving the others. This highlights how these categories work together to keep my digital life functioning smoothly.

Tree (Entertainment, Shopping & Food, Education)
Next, I used a tree metaphor to represent Entertainment, Shopping & Food, and Education. Entertainment, being the largest category, is depicted as the tree’s lush leaves, while Shopping & Food serves as the branches that support it. Education forms the roots, symbolizing foundational growth.

Sand Timer (Social, Entertainment, Productivity & Finance)
Finally, I chose a sand timer to show the relationship between Social, Entertainment, and Productivity & Finance. Social is the wide upper chamber, reflecting the largest portion of my time, while Entertainment flows through the middle, balancing with work. Productivity & Finance is concentrated at the bottom, symbolizing its essential but compressed role.

Reflection and Alignment with Assignment Goals
By categorizing my app usage, calculating the percentages, and using abstract objects like gears, a tree, and a sand timer, I was able to gain a deeper understanding of my digital habits. The assignment encouraged me to reflect not just on my screen time, but also on the balance and interaction between productivity, social activities, and entertainment.
![Untitled](https://github.com/user-attachments/assets/f6d4d61e-bbf2-4a32-b06c-2eb1b89996de)

# Week 3
# Week of 09/19/2024

Project Overview: Bottle Holder Design

This project started with a simple problem: after pouring water from my bottle, moisture would always collect at the bottom, leaving annoying marks on surfaces. I wanted to solve this everyday issue by designing a 3D-printed holder that would keep the bottle dry and stable on any surface.

Challenges & Solutions:

I explored Grasshopper for computational design, trying different geometric combinations to create the final holder. Initially, I attempted to 3D-scan the bottle for precision but eventually switched to manual modeling after encountering several issues. Iteration was key—fine-tuning parameters in Grasshopper made a huge difference in getting the design right.

Results & Feedback:

The final design worked as intended, solving the moisture issue while being functional and aesthetically pleasing. Feedback from peers encouraged me to experiment with more shapes, make the design adjustable for different bottle sizes, and explore more durable materials for better heat resistance.

Conclusion:

This project was a great learning experience. I got hands-on with Grasshopper and Rhino, and despite some failed attempts along the way (looking at you, 3D-scanning!), the process taught me the value of persistence and iteration. It’s more than just a bottle holder—it’s about solving real problems with practical design and pushing my skills in computational design.

For more details on this dynamic process, check out the video below!

https://github.com/user-attachments/assets/1a5ac7ce-e2b2-4dff-ae05-c7801ec9d7eb

# Week 2
# Week of 09/12/2024

After Tuesday's class, I reviewed the class materials and Grasshopper notes in more detail to further structure the diagrams. I then worked on organizing and simplifying the diagrams for better clarity.

![Untitled](https://github.com/user-attachments/assets/22be9055-41cf-4fd0-a31a-e810b118a91a)
[Link](https://www.figma.com/board/lUDPzkBLpdVS7jWdDrvnrN/Week-2_Yule_091224?node-id=0-1&t=dbdX0OTPoVb2UH4U-1)

During the last class, I relearned how to adjust values using the Control Panel, which made it easier for me to fine-tune my desired design. By the way, recently, a new iPhone was released. I have always thought that my current phone is smaller than my hand size, so I'm considering purchasing the new, bigger iPhone model. Therefore, I adjusted the width, height, and thickness values to match the size of the iPhone Pro Max.

![Screenshot-2024-09-12-at-2 55 22 PM](https://github.com/user-attachments/assets/673758ef-d27a-4287-906a-52ebced9a6a7)

While working, I came up an idea: what if the phone stand could allow viewing from multiple angles rather than being fixed at just one? So I focused on this, and after several attempts, I adjusted the values to enable the phone to be positioned at both steeper and graduer angles."

<img width="970" alt="Screenshot 2024-09-12 at 2 55 58 PM" src="https://github.com/user-attachments/assets/22996ec8-5c54-4a4d-b6c8-3c1851ff6157">

After baking the model, I was able to identify the design I wanted. However, there was one issue. To allow the phone to be placed at a more gradual angle, a groove, marked in red, needed to be carved out to fit the phone. Next week, I plan to study Rhino and try to create this groove to match my (future) phone's thickness.

<img width="973" alt="Screenshot 2024-09-12 at 2 57 15 PM" src="https://github.com/user-attachments/assets/92c0bddc-c082-4501-9c6c-6cd378a1d097">

# Week of 09/10/2024

Thie week, I explored Rhino 3D and Grasshopper more and tried to figure out the system of files shared.

<img width="1350" alt="Screenshot 2024-09-08 at 9 46 12 PM" src="https://github.com/user-attachments/assets/f09cfc8d-43a9-47f7-9b7c-2a57fbeec24a">

I’ve understood that the task involves creating diagrams to represent the example files, and it’s important to explore different approaches while being open to experimentation and learning from early failures. The process includes steps like setting up the plane, adjusting parameters, manipulating the geometry, and testing the design for stability and volume. It’s essential to focus on key elements like sphere positioning, void subtraction, and parameter adjustments, while refining the final model for 3D printing. I’ll review everything again tomorrow to make sure it’s on track.

<img width="1020" alt="Screenshot 2024-09-08 at 9 17 16 PM" src="https://github.com/user-attachments/assets/eb3c7e68-68fc-4d7b-be55-22b491dd7588">

To begin, I left the void width and height unchanged, as they were already suitable for the design. Then, I adjusted the parameters for phone width, length, corner radius, and thickness to align with the exact specifications of my iPhone 15. Following this, I fine-tuned the center Z values for both Sphere 1 and Sphere 2, ensuring the height was appropriate for the stand’s overall structure. Additionally, I modified the radius and length of the cylinder to match the dimensions of my phone more accurately.

<img width="774" alt="Screenshot 2024-09-08 at 9 29 46 PM" src="https://github.com/user-attachments/assets/791af10b-02f7-4dd8-8549-26f3192a364b">

While adjusting the parameters, I accidentally made an error where the phone buttons became separated from the phone frame. I tried several times to fix it, but couldn’t figure out how to resolve the issue. I decided to leave it as is, knowing I could ask about it during the next class session.

<img width="766" alt="Screenshot 2024-09-08 at 9 22 03 PM" src="https://github.com/user-attachments/assets/217209ee-8333-4c63-9030-03bbaea9bb3f">

Once the adjustments were complete, I pressed Bake Me to finalize the changes and baked the model into the design. To further organize and review the outcome, I created a separate layer titled iPhone 15 to visualize and verify the results. This layer allowed me to clearly inspect how the modified parameters fit with the overall design, ensuring the stand would securely hold my phone while maintaining stability.


# Week 1
# Week of 09/05/2024
This week, I completed General Workshop Safety training and printed a sample using a laser cutter. 

# Reflection
As an international student, I’m still adjusting to the classes at Berkeley, so it’s been a bit overwhelming. I haven’t come across anything particularly surprising yet. I completed the General Workshop Safety online training as part of the assignments, but I struggled with using the equipment, specifically with the laser cutter. Thankfully, my cohort member Josh Jang helped me with printing, and I’m very grateful for his assistance.

# Images & Video
![IMG_2736](https://github.com/user-attachments/assets/9fdbc9c2-d951-4afc-bad1-c5c11a078f9e)

# Speculations
Recently, I bought an electric bike and have been debating whether to buy a basket to carry a bag, but I’m considering making one through the Jacobs Makerspace instead.

# Images & Video

![IMG_2699](https://github.com/user-attachments/assets/3d650f6d-8aba-4ad1-8591-616e19836b1e)
