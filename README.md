# cookAssisstWear

#include <SPI.h>
#include <MFRC522.h>
#include <SoftwareSerial.h>
#include <DFRobotDFPlayerMini.h>
#include <HX711.h>

// DFPLAYER

SoftwareSerial mySerial(2, 3);
DFRobotDFPlayerMini myDFPlayer;


// RFID

#define SS_PIN 10
#define RST_PIN 9

MFRC522 rfid(SS_PIN, RST_PIN);


// HX711 LOAD CELL

#define DT A1
#define SCK A0

HX711 scale;


// SENSORS

#define FLAME_SENSOR 5
#define GAS_SENSOR 6

bool flameState = false;
bool gasState = false;

String lastUID = "";

float lastWeight = 0;

void setup() {

  Serial.begin(9600);
  mySerial.begin(9600);

  pinMode(FLAME_SENSOR, INPUT);
  pinMode(GAS_SENSOR, INPUT);

  // RFID
  SPI.begin();
  rfid.PCD_Init();

  // DFPLAYER
  Serial.println("Initializing DFPlayer...");
  delay(2000);

  if (!myDFPlayer.begin(mySerial)) {
    Serial.println("DFPlayer Error");
    while (true);
  }

  myDFPlayer.volume(25);

  // HX711
  Serial.println("Initializing HX711...");

  scale.begin(DT, SCK);

  // CHANGE THIS VALUE AFTER CALIBRATION
  scale.set_scale(-2280.0);

  scale.tare();

  Serial.println("System Ready");
}
void loop() {

  // RFID SECTION

 if (rfid.PICC_IsNewCardPresent() &&
      rfid.PICC_ReadCardSerial()) {

    String uid = "";

    for (byte i = 0; i < rfid.uid.size; i++) {

      if (rfid.uid.uidByte[i] < 0x10)
        uid += "0";

      uid += String(rfid.uid.uidByte[i], HEX);
    }

    uid.toUpperCase();

    if (uid != lastUID) {

      lastUID = uid;

      Serial.print("UID: ");
      Serial.println(uid);

      // SALT
      // NOTE: "E996116" is 7 characters, but this loop always produces
      // an even-length string (2 hex digits per byte). That means this
      // will likely never match a real scanned tag. Scan your actual
      // Salt tag, copy the exact "UID: ..." string printed above, and
      // paste it here in place of "E996116".
      if (uid == "E996116") {

        Serial.println("SALT");
        myDFPlayer.play(1);
        delay(2500);
      }

      // RICE
      else if (uid == "C37F83F7") {

        Serial.println("RICE");
        myDFPlayer.play(2);
        delay(2500);
      }

      else {

        Serial.println("Unknown Tag");
      }
    }

    rfid.PICC_HaltA();
    rfid.PCD_StopCrypto1();
  }

  if (!rfid.PICC_IsNewCardPresent()) {
    lastUID = "";
  }

  // LOAD CELL SECTION
  float weight = scale.get_units(10);

  // Remove negative noise
  if (weight < 0)
    weight = 0;

  // Print only if weight changes
  if (abs(weight - lastWeight) > 2) {

    Serial.print("Weight: ");
    Serial.print(weight);
    Serial.println(" g");

    lastWeight = weight;
  }

  // FLAME SENSOR
  bool realFlame = (digitalRead(FLAME_SENSOR) == LOW);

  if (realFlame && !flameState) {

    Serial.println("FLAME DETECTED");

    myDFPlayer.play(4);

    flameState = true;

    delay(3000);
  }

  if (!realFlame) {
    flameState = false;
  }

  // GAS SENSOR

  bool realGas = (digitalRead(GAS_SENSOR) == LOW);

  if (realGas && !gasState) {

    Serial.println("GAS DETECTED");

    myDFPlayer.play(3);

    gasState = true;

    delay(3000);
  }

  if (!realGas) {
    gasState = false;
  }

  delay(100);
}
