# TURNING-ON-SERVO-MOTOR-BY-USING-A-KEYPAD
# 1.	ABSTRACT
 Today’s life is being controlled by technology and Every individual needs to feel secure in their daily lives. In our security pattern, access control of servo motor by using keypad plays an essential role. control of servo motor using keypad systems allow authorized persons to access restricted areas. Also this project of controlling a servo motor using keypad it can especially use where we need more safe and secure like locking and unlocking.   Conventional locks are not as secure as they once were; anyone can break in if they break these locks. We need a framework that provides benefit. It is controlled by an Arduino. The password is entered using a keypad. The entered password is compared with the known password when setting a combination password and by using 1-6 digits. A correct password opens the door by rotating the servo motor. If the password is wrong the servo motor remains stop. And also a servo motor it will operate or rotate only if the password in keypad is correct and the green indicator bright to show you that the password is correct and doors opens servo motor rotate in forward direction at specified angle and even if the door is open and it return back automatically after a certain period set and the led indicator bright to show you that the door is closed and servo motor rotate in reverse direction at specified angle. And also this system it can be used in difference field where we need to provide more safe and secure.

# 2.	 PROBLEM STATEMENT

In previous year most of people face the problem of providing the safer and secure to their valuables, and also This project of controlling a servo motor by using a keypad come to solve that issues and also designed to provide security for rotating a servo motor by difference purpose and this will have done by the help of keypad and Arduino Uno. The Arduino compares the password with the default password. A servo motor will be rotated according to the password. This project allows the servo motor to move to an angle specified by the use. The servo motor rotates when the password is correct as of the default password that was set. And also It is very useful for people who are looking for more secure examples doors and lockers. Mechanical and electronic components both are used in these techniques, making them highly efficient. The password-based control a servo motor by using keypad system will provide maximum security so that users are able to fully satisfy their needs and to keep their valuables safe and secure. For example, by replacing mechanical door locks with electronic door locks by using a servo motor to control the rotation of that doors and this system of controlling a servo motor by using keypad promises to make a bold step for the future.


# 3.	BLOCK DIAGRAM OF CONTROLLING A SERVO MOTOR BY USING A KEYPAD AND IT’S DISCRIPTION.
 ![image](https://user-images.githubusercontent.com/104324985/164995333-2eb61381-cdaa-45fc-b902-409e39f382f9.png)


This block diagram of turning on a servo motor using keypad it composed with three main components which are keypad, Arduino Uno and a servo motor. So This 4x4 matrix keypad has 16 built-in pushbutton contacts connected to row and column lines.  A microcontroller can scan these lines for a button-pressed state.  In the keypad library, the Propeller sets all the column lines to input, and all the row lines to input.  Then, it picks a row and sets it high.  After that, it checks the column lines one at a time.  If the column connection stays low, the button on the row has not been pressed. If it goes high, the microcontroller knows which row (the one it set high), and which column, (the one that was detected high when checked. By pressing the keypad button closes the switch between a column and a row trace, allowing current to flow between a column pin and a row pin. The Arduino (micro controller) detects which button is pressed by detecting the row and column pin that's connected to the button. And also the servo motor has connector with three pins which Connect the power cable that in all standards should be red to 5V on the Arduino and Connect the remaining line on the servo connector to a digital pin on the Arduino and the servo motor will rotate according to the password entered using the keypad so, when the password is correct compared with the known password the servo motor will rotate respect to the angle specified. And also when the password is incorrect the servo motor it will remain stop.  


# 4.CIRCUIT DIAGRAM OF TURNING ON SERVO MOTOR USING KEYPAD
 
![image](https://user-images.githubusercontent.com/104324985/164995346-4255f2fe-f9ec-4928-b372-fba522168a89.png)


# 5.CIRCUIT DIAGRAM OF TURNING ON SERVO MOTOR USING KEYPAD IN FRITZING
 ![image](https://user-images.githubusercontent.com/104324985/164995365-d2ff398c-38e8-450d-acad-9cb35a3a83dc.png)

# 6. SIMULATION OF TURNING ON SERVO MOTOR BY USING KEYPAD IN PROTEUS
 
![image](https://user-images.githubusercontent.com/104324985/164995382-6be77e4b-ab98-48b4-aeef-3d647ffea995.png)


# 7. SOURCE CODE OF TURNING ON SERVO MOTOR BY USING KEYPAD

#include <Keypad.h>
#include <Servo.h>
#define ROW_NUM    4  
#define COLUMN_NUM 4 
#define SERVO_PIN  A0
Servo servo; 

char keys[ROW_NUM][COLUMN_NUM] = {
  {'1', '2', '3','A'},
  {'4', '5', '6','B'},
  {'7', '8', '9','C'},
  {'*', '0', '#','D'}
};

byte pin_rows[ROW_NUM] = {9,8, 7, 6}; 
byte pin_column[COLUMN_NUM] = {5,4, 3, 2}; 
Keypad keypad = Keypad( makeKeymap(keys), pin_rows, pin_column, ROW_NUM, COLUMN_NUM );

const String password = "809"; 
String input_password;

int angle = 0; 
unsigned long lastTime;
int redled = 12;
int greenled = 13;
void setup() {
  Serial.begin(9600);
  input_password.reserve(4); 
  servo.attach(SERVO_PIN);
  servo.write(0); 
  lastTime = millis();
  pinMode(redled, OUTPUT);
  pinMode(greenled, OUTPUT);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    Serial.println(key);

    if (key == '*') {
      input_password = "";
    } else if (key == '#') {
      if (input_password == password) {
        Serial.println("The password is correct, rotating Servo Motor to 90°");
        Serial.println("DOOR UNLOCKED");
        digitalWrite(12,LOW);
        digitalWrite(13,HIGH);
        angle = 90;
        servo.write(angle);
        lastTime = millis();
      } else {
        Serial.println("The password is incorrect, try again");
        Serial.println("DOOR LOCKED");
        digitalWrite(12,HIGH);
        digitalWrite(13,LOW);
      }

      input_password = ""; 
    } else {
      input_password += key; 
    }
  }
  if (angle == 90 && (millis() - lastTime) > 5000) {
    angle = 0;
    servo.write(angle);
    Serial.println("Rotating Servo Motor to 0°");
    Serial.println("DOOR LOCKED");
    digitalWrite(12,HIGH);
    digitalWrite(13,LOW);
  }
}
