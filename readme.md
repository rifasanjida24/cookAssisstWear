Project Overview – Cook Assist Mini

Cook Assist Mini is a smart kitchen assistant designed specifically for visually impaired people to help them cook safely, independently, and confidently. The system combines ingredient identification, quantity measurement, and kitchen safety monitoring into a single portable device.

Main Objective

The primary goal of the project is to reduce the challenges faced by visually impaired individuals during cooking, such as:

Identifying ingredients correctly
Measuring ingredient quantities accurately
Detecting dangerous situations like gas leaks and fire hazards
Providing real-time voice assistance without requiring visual support
How the System Works
Ingredient Identification
Each ingredient container is attached with an RFID tag.
When the container is scanned, the RFID reader detects the tag.
The Arduino identifies the ingredient and announces its name through a speaker.
Quantity Measurement
The container is placed on a load cell platform.
The load cell and HX711 module measure the weight.
Based on predefined thresholds, the system announces whether the quantity is Low, Medium, or High.
Safety Monitoring
An MQ-2 gas sensor continuously checks for gas leakage.
A KY-026 flame sensor detects fire hazards.
If danger is detected, the system immediately provides voice warnings such as:
"Gas leakage detected"
"Flame detected"
Key Features
Smart Ingredient Identification System
Smart Measurement Assistance System
Integrated Safety Monitoring System
Portable All-in-One Compact Design
Technologies and Components Used
Arduino Nano
MFRC522 RFID Reader
RFID Tags
DFPlayer Mini & Speaker
Load Cell with HX711 Amplifier
MQ-2 Gas Sensor
KY-026 Flame Sensor
Rechargeable Battery System
Conclusion

Cook Assist Mini is an affordable and portable assistive device that enables visually impaired users to identify ingredients, monitor ingredient quantities, and maintain kitchen safety through voice feedback. By integrating RFID technology, weight sensing, and hazard detection into a single system, the project promotes greater independence, confidence, and safety in everyday cooking activities.
