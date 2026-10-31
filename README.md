# Jet-Engine-Demonstrator
ATmega328P-based Jet Engine Demonstrator using a DC motor to represent engine rotor rotation. The motor speed is controlled using PWM through a potentiometer, with push-button control and an OLED display for real-time system information.
Motor IN2 : D5
LED PWM : D9
Buzzer : D10
===========================================
*/

const int throttlePin = A0;

const int motorPWM = 6;
const int motorIN1 = 7;
const int motorIN2 = 5;

const int ledPin = 9;
const int buzzerPin = 10;

int adcValue;
int pwmValue;
int frequency;

void setup()
{
    pinMode(motorPWM, OUTPUT);
    pinMode(motorIN1, OUTPUT);
    pinMode(motorIN2, OUTPUT);

    pinMode(ledPin, OUTPUT);
    pinMode(buzzerPin, OUTPUT);

    // Motor always rotates forward
    digitalWrite(motorIN1, HIGH);
    digitalWrite(motorIN2, LOW);
}

void loop()
{
    // Read throttle
    adcValue = analogRead(throttlePin);

    // Convert 0-1023 to 0-255
    pwmValue = map(adcValue,0,1023,0,255);

    // Motor speed
    analogWrite(motorPWM,pwmValue);

    // Exhaust brightness
    analogWrite(ledPin,pwmValue);

    // Engine sound
    frequency = map(adcValue,0,1023,250,2000);

    tone(buzzerPin,frequency);

    delay(10);
}
