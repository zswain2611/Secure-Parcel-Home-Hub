Project: The Secure-Parcel Home Hub
Developer: Zachary Swain
Program: Computer Programming - Internet of Things (CPIN)
Date: March 2026
Project Description
The Secure-Parcel Home Hub is an IoT-integrated security solution designed for automated package delivery. The system uses a multi-layered authentication process involving RFID (NFC) for couriers and a KeyPad for homeowners.
The hub features a physical locking mechanism powered by a stepper motor, visual feedback via an 8x8 LED Matrix, and an audio alert system. Most importantly, the system is integrated with a PC-based Processing Dashboard that provides real-time status updates and generates a permanent, timestamped security log (system_log.txt) for all access attempts.
________________________________________
Bill of Materials (Parts List)
Component	Function
Arduino Uno R3	Central Microcontroller
RC522 RFID Reader	NFC Authentication (Couriers/Tags)
I2C 4x4 KeyPad (0x20)	PIN Entry (Homeowner/Master Code)
MAX7219 8x8 LED Matrix	Visual Status Icons (Lock, Unlock, Error "X")
28BYJ-48 Stepper Motor	Physical Locking Actuator
ULN2003 Driver Board	Stepper Motor Controller
Active/Passive Buzzer	Audio Feedback (Success/Denied Tones)
Processing 4 (Software)	PC Dashboard and Data Logging Engine
________________________________________
Wiring & Pin Mapping
To avoid SPI/I2C address conflicts and ensure high-speed communication, the following pinout was utilized:
Core Communication
•	I2C Bus (SDA/SCL): Shared by the KeyPad and Arduino (A4/A5).
•	SPI Bus (MOSI/MISO/SCK): Dedicated to the RC522 RFID Reader.
Detailed Pinout
Component	Arduino Pin	Component Pin
RFID Reader	10	SS (Slave Select)
RFID Reader	9	RST (Reset)
RFID Reader	11, 12, 13	MOSI, MISO, SCK
LED Matrix	7	CS (Chip Select)
LED Matrix	6	DIN (Data In)
LED Matrix	A1	CLK (Clock)
Buzzer	8	Positive (+)
Stepper Motor	2, 3, 4, 5	IN1, IN2, IN3, IN4
I2C KeyPad	A4 (SDA)	SDA
I2C KeyPad	A5 (SCL)	SCL
________________________________________
Relevant Project Information
1. Visual Iconography
The system uses custom-designed bitmaps to communicate status without words:
•	Locked Icon: A solid padlock body with a rounded shackle.
•	Open Icon: A dynamic arrow pointing upward to indicate the hub is ready for a parcel.
•	Error Icon: A high-contrast "X" displayed for 2 seconds during unauthorized attempts.
2. Conflict-Free Matrix Timing
The LED Matrix utilizes pins 6 and A1 for Data and Clock. This specifically avoids using the standard SPI pins (11/13), preventing a "clash" between the RFID reader and the display during high-frequency scans.
3. Privacy-Aware NFC Logic
During development, it was noted that modern smartphones (iOS/Android) utilize Randomized UIDs for privacy. The current firmware is optimized for static physical tags to ensure 100% reliability for delivery personnel, while acknowledging the limitations of mobile-based NFC without a dedicated encrypted handshake.
4. Data Persistence
The Processing dashboard acts as the "Black Box" for the project. By monitoring the Serial stream, it captures every ACCESS_GRANTED and ACCESS_DENIED event, appending them to a local .txt file with a millisecond-accurate timestamp from the PC clock.

