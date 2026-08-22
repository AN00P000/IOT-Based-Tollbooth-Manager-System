#include <SPI.h>
#include <MFRC522.h>
#include <Servo.h>

#define SS_PIN 10
#define RST_PIN 9
#define SERVO_PIN 8

MFRC522 mfrc522(SS_PIN, RST_PIN);
Servo servoMotor;

// Replace this with your RFID card's UID
String authorizedUID = "53 A7 2C 19";

void setup() {
  Serial.begin(9600);

  SPI.begin();
  mfrc522.PCD_Init();

  servoMotor.attach(SERVO_PIN);
  servoMotor.write(0);

  Serial.println("RFID System Ready");
  Serial.println("Scan your RFID card...");
}

void loop() {

  // Check if a new RFID card is present
  if (!mfrc522.PICC_IsNewCardPresent()) {
    return;
  }

  // Read the RFID card
  if (!mfrc522.PICC_ReadCardSerial()) {
    return;
  }

  // Display UID
  String content = "";

  Serial.print("UID: ");

  for (byte i = 0; i < mfrc522.uid.size; i++) {

    if (mfrc522.uid.uidByte[i] < 0x10) {
      Serial.print("0");
      content += "0";
    }

    Serial.print(mfrc522.uid.uidByte[i], HEX);
    content += String(mfrc522.uid.uidByte[i], HEX);

    if (i < mfrc522.uid.size - 1) {
      Serial.print(" ");
      content += " ";
    }
  }

  content.toUpperCase();

  Serial.println();
  Serial.print("Message: ");

  // Check authorization
  if (content == authorizedUID) {

    Serial.println("Authorized access");

    // Open gate
    servoMotor.write(90);
    delay(3000);

    // Close gate
    servoMotor.write(0);
    delay(1000);

  } else {

    Serial.println("Access denied");
  }

  // Stop reading the card
  mfrc522.PICC_HaltA();
  mfrc522.PCD_StopCrypto1();

  delay(500);
}
