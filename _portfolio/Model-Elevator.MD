---
title: "Model Elevator"
excerpt: "A motorized 3D printed elevator shaft."
header:
  image: assets/img/cab_front_view.jpeg
  teaser: assets/img/cab_front_view.jpeg
gallery1:
  - image_path: assets/img/cab_front_view.jpeg
  - image_path: assets/img/cab_top_view.jpeg

---

## 3D-Printed Arduino Elevator

A three-floor model elevator that I designed, built, and programmed in C++ on an Arduino. Most of the structural and mechanical components are custom parts that I modeled in CAD and fabricated using FDM 3D printing. The project brings together mechanical design, sensor integration, and embedded programming to replicate how a real elevator moves, senses its position, and responds to users.

### How It Works

Pressing a button sends the elevator cab to the requested floor. A motor at the top of the shaft drives the cab, while a counterweight balances the load, the same principle used in full-size elevators. A magnet on the cab activates reed switches at each floor, allowing the Arduino to detect arrival and stop the motor precisely.

### Key Features

- **Custom CAD-designed components**, with most parts FDM 3D printed
- **Reed switch position sensing** at each of the three floors
- **Counterweighted motor drive** to reduce load on the motor
- **Automatic door** that opens on arrival and closes after a short delay
- **7-segment display** showing the current floor
- **Push-button control** for calling the elevator



## CAD Model
<iframe src="https://vanderbilt643.autodesk360.com/shares/public/SH286ddQT78850c0d8a408db1211465ff366?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>
{% include gallery id="gallery1" %}


### Source Code

This project is open source. The full Arduino sketch is included below.

<details>
<summary>View full source code</summary>

```cpp
const int input2 = 2;
const int input1 = 3;

const int input12 = 7;
const int input11 = 4;


const int num1 = A4;
const int num2 = A3;
const int num3 = A1;

const int enablePin = 5;

const int btn1 = 8;
const int btn2 = 12;
const int btn3 = 13;

const int swch1 = 9;
const int swch2 = 10;
const int swch3 = 11;


int home = 0;

int flr = 1;

int lastBtn1 = HIGH;
int lastBtn2 = HIGH;
int lastBtn3 = HIGH;

int lastSwch1 = HIGH;
int lastSwch2 = HIGH;
int lastSwch3 = HIGH;

void setup() {

  pinMode(input2, OUTPUT);
  pinMode(input1, OUTPUT);

  pinMode(input12, OUTPUT);
  pinMode(input11, OUTPUT);

  pinMode(enablePin, OUTPUT);

  pinMode(num1, OUTPUT);
  pinMode(num2, OUTPUT);
  pinMode(num3, OUTPUT);

  pinMode(btn1, INPUT_PULLUP);
  pinMode(btn2, INPUT_PULLUP);
  pinMode(btn3, INPUT_PULLUP);

  pinMode(swch1, INPUT_PULLUP);
  pinMode(swch2, INPUT_PULLUP);
  pinMode(swch3, INPUT_PULLUP);


  digitalWrite(num1, LOW);
  digitalWrite(num2, HIGH);
  digitalWrite(num3, HIGH);

}

void loop() {
  int state1 = digitalRead(btn1);
  int state2 = digitalRead(btn2);
  int state3 = digitalRead(btn3);

  int state11 = digitalRead(swch1);
  int state12 = digitalRead(swch2);
  int state13 = digitalRead(swch3);


  int state14 = digitalRead(btn1);

 
  analogWrite(enablePin, 150);

   if (home == 0){
    digitalWrite(input1, HIGH);
    digitalWrite(input2, LOW);
      while (digitalRead(swch1) == HIGH){
        delay(20);   
      }    
      home = 1;
      digitalWrite(input1, LOW);

  }

if (home == 1) {

  if (lastBtn1 == HIGH && state1 == LOW) {
    if (flr == 1) {
      digitalWrite(input1, LOW);
      digitalWrite(input2, LOW);
    }
    else if (flr == 2) {
      digitalWrite(input1, HIGH);
      digitalWrite(input2, LOW);
      while (digitalRead(swch1) == HIGH){
        delay(20);   
      }    
    }
    else if (flr == 3) {
      digitalWrite(input1, HIGH);
      digitalWrite(input2, LOW);
      while (digitalRead(swch1) == HIGH){
        delay(20);   
      }         
    }

    digitalWrite(input1, LOW);
    digitalWrite(input2, LOW);

    digitalWrite(num1, LOW);
    digitalWrite(num2, HIGH);
    digitalWrite(num3, HIGH);

    if (flr != 1) {

      digitalWrite(input11, HIGH);
      digitalWrite(input12, LOW);


      digitalWrite(input11, LOW);
      digitalWrite(input12, LOW);

      delay(5000);

      digitalWrite(input11, LOW);
      digitalWrite(input12, HIGH);


    }
    flr = 1;
  }

  if (lastBtn2 == HIGH && state2 == LOW) {
    if (flr == 1) {
      digitalWrite(input1, LOW);
      digitalWrite(input2, HIGH);
      while (digitalRead(swch2) == HIGH){
        delay(20);   
      }       
    }
    else if (flr == 2) {
      digitalWrite(input1, LOW);
      digitalWrite(input2, LOW);
    }
    else if (flr == 3) {
      digitalWrite(input1, HIGH);
      digitalWrite(input2, LOW);
      while (digitalRead(swch2) == HIGH){
        delay(20);   
      }        
    }
	
    digitalWrite(input1, LOW);
    digitalWrite(input2, LOW);

    digitalWrite(num2, LOW);
    digitalWrite(num1, HIGH);
    digitalWrite(num3, HIGH);

    if (flr != 2) {

      digitalWrite(input11, HIGH);
      digitalWrite(input12, LOW);


      digitalWrite(input11, LOW);
      digitalWrite(input12, LOW);

      delay(5000);

      digitalWrite(input11, LOW);
      digitalWrite(input12, HIGH);

    }
    flr = 2;
  }

  if (lastBtn3 == HIGH && state3 == LOW) {
    if (flr == 1) {

      digitalWrite(input1, LOW);
      digitalWrite(input2, HIGH);
      while (digitalRead(swch3) == HIGH){
        delay(20);   
      }       
    }
    else if (flr == 2) {

      digitalWrite(input1, LOW);
      digitalWrite(input2, HIGH);
      while (digitalRead(swch3) == HIGH){
        delay(20);   
      }       
    }
    else if (flr == 3) {
      digitalWrite(input1, LOW);
      digitalWrite(input2, LOW);
    }
    digitalWrite(input1, LOW);
    digitalWrite(input2, LOW);

    digitalWrite(num3, LOW);
    digitalWrite(num2, HIGH);
    digitalWrite(num1, HIGH);

    if (flr != 3) {


      digitalWrite(input11, HIGH);
      digitalWrite(input12, LOW);

  
      digitalWrite(input11, LOW);
      digitalWrite(input12, LOW);

      delay(5000);

      digitalWrite(input11, LOW);
      digitalWrite(input12, HIGH);


    }
    flr = 3;
  }
}
 
  digitalWrite(input11, LOW);
  digitalWrite(input12, LOW);


  lastBtn1 = state1;
  lastBtn2 = state2;
  lastBtn3 = state3;
  lastSwch1 = state14;


  digitalWrite(input1, LOW);
  digitalWrite(input2, LOW);

}
```

</details>


## 3D-Printed Components

| Component             | Material |
|-----------------------|----------|
| Enclosure Bottom      | PLA      |
| Enclosure Side Panel  | PLA      |
| Enclosure Top         | PLA      |
| Motor Mounting Piece  | ABS      |
| End Support           | ABS      |
| Carriage              | ABS      |
| Syringe Support       | ABS      |


## Off-the-Shelf Components

| Component                                   | Specification / Model                                  | Quantity |
|---------------------------------------------|--------------------------------------------------------|----------|
| Lead Screw                                  | 250 mm, 2 mm pitch, 2 mm lead                          | 1        |
| Lead Screw                                  | 250 mm, 2 mm pitch, 8 mm lead                          | 1        |
| Flexible Coupling                           | 1/4" x 8 mm                                            | 1        |
| Linear Rod                                  | 200 mm, 8 mm diameter                                  | 2        |
| Linear Bearing                              | LM8UU, for 8 mm rod                                    | 2        |
| Aluminum Extrusion                          | 2040, 1 ft length                                      | 1        |
| Stepper Motor                               | NEMA 17                                                | 1        |
| Power Supply                                | MEAN WELL RQ-65D AC-DC Quad Output (5V, 12V, 24V, 12V, 4A) | 1        |
| Arduino Board                               | Arduino Uno (GrabCAD model)                            | 1        |
| Stepper Driver                              | A4988                                                  | 1        |
| Breadboard                                  | Small                                                  | 1        |
| Push Button (Latching)                      | Panel-mount, On/Off                                    | 1        |
| Push Button (Momentary, bonus only)         | Panel-mount                                            | 1        |
| Limit Switch                                | Standard                                               | 1        |
| LED                                         | RGB, common cathode                                    | 1        |

{% include gallery id="gallery2" %}




