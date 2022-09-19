#include <DigitShield.h>                // 디지털쉴드를 사용하기 위해 헤더파일 추가한 코드

int AOUTpin = A4;                       // A4번핀을 AOUTpin 변수로 선언해준다
int spPin = 12;                         // 12번핀을 spPin 변수로 선언해준다

int trigPin = 9;                        // 9번핀은 trigPin 변수로 선언해준다
int echoPin = 10;                       // 10번핀을 echopin 변수로 선언해준다

#define MOTOR_A_a 3                     // 상수 및 매크로 함수를 정의하는 코드로 MOTOR_A_a_ -> 3
#define MOTOR_A_b 11                    // 상수 및 매크로 함수를 정의하는 코드로 MOTOR_A_b_ -> 11
#define MOTOR_B_a 5                     // 상수 및 매크로 함수를 정의하는 코드로 MOTOR_B_a_ -> 5
#define MOTOR_B_b 6                     // 상수 및 매크로 함수를 정의하는 코드로 MOTOR_B_b_ -> 6
#define MOTOR_SPEED 100                 // 상수 및 매크로 함수를 정의하는 코드로 MOTOR_SPEED_ -> 100

unsigned char m_a_spd = 0, m_b_spd = 0;  // 모터의 속력을 지정하는 전역변수
int value;                              // 변수 value 값 선언해준다


Void setup() { // 프로그램이 시작될 때 처음 한 번만 실행한다

  pinMode(MOTOR_A_a, OUTPUT); // 모터A +출력으로 사용한다
  pinMode(MOTOR_A_b, OUTPUT; // 모터A -출력으로 사용한다
  pinMode(MOTOR_B_a, OUTPUT); // 모터B +출력으로 사용한다
  pinMode(MOTOR_B_b, OUTPUT); // 모터B -출력으로 사용한다

  pinMode(trigPin, OUTPUT); // trigPin(9번핀)을 출력으로 사용한다
  pinMode(echoPin, INPUT); // echoPin(10번핀)을 입력으로 사용한다

  pinMode(spPin, OUTPUT); // spin(12번핀)을 출력으로 사용한다

  DigitShield.begin(); // 디지틸쉴드 라이브러리 실행

  Serial.begin(9600); // UART 통신은 시리얼 연결 ON 해주고, Baud rate 1s에 9600번 켜고 R는 통신속도를 보여준다
}



void loop() {

  // 음주 축정
  value = analogRead(AOUTpin);      // 변수 value에 AOUT 핀에서의 값을 저장한다.
  Serial.print("Alcohol value:");   // 문자 “Alcohol value:”을 화면에 출력한다
  Serial.println(value);            // 알코올 측정값을 화면에 출력한다
  DigitShield.setValue(value);      // 측정된 value 값을 디지털쉴드 화면에 출력한다
  delay(5);                         // 0.005초 지연시간을 준다
  
  // 음주측정 센서 측정값 (부저)
  if (value > 400){                 // 알코올 측정값이 400 이상이면
  digitalWrite(spPin, HIGH);        // 부저는 울린다
  } else{                           // 아닐 경우
  digitalWrite(spPin, LOW);         // 아닐 경우, 부저는 꺼진다
  }
  
  // 초음파 거리 센서
  
  float duration, distance;       // 시간과 거리에 대한 두 개의 변수 선언
  digitalWrite(trigPin, HIGH);    // trigPin(9번핀)에 5V 출력
  delay(5);                       // 0.005초 지연시간을 준다
  digitalWrite(trigPin, LOW);     // trigPin(9번핀)에 5V 출력
  
  duration = pulseIn(echoPin, HIGH);          // echoPin(10번핀)에 5V 유지된 시간을 측정한다
  distance = ((float)(340*duration)/10000)/2; // 장애물과 센서에 대한 편도 거리를 구한 식
  
  Serial.print("\nDuration:"); Serial.print(distance); // 컴퓨터한테 보내고 싶은 값인 변수 duration에 대한 화면값을 출력한다
  Serial.print("cm\n");                                // 컴퓨터한테 보내고 싶은 값인 변수 distance에 대한 화면값을 출력한다
  delay(500);                                          // 0.5초 지연시간을 준다



  if (value > 400){                  // 알코올 측정값이 400 이상이면
      digitalWrite(MOTOR_A_b, HIGH); // 모터A- 동작을 멈추게 한다
      digitalWrite(MOTOR_B_b, HIGH); // 모터B- 동작을 멈추게 한다
      delay(10000);                  // 10초 지연시간을 준다
  
  } else if(distance < 40){          // 초음파 거리 측정값이 40 이하이면
      digitalWrite(trigPin, HIGH);   // 적외선 측정 센서가 작동한다
      digitalWrite(MOTOR_A_b, HIGH); // 모터A- 동작을 멈추게 한다
      digitalWrite(MOTOR_B_b, HIGH); // 모터B- 동작을 멈추게 한다
      delay(800);                    // 0.8초 지연시간을 준다
    
      digitalWrite(MOTOR_B_b, LOW);               // 모터B- 동작을 실행시켜준다
      m_b_spd = constrain(MOTOR_SPEED*1.0,0,255); // 모터B의 속력값 조정값을 변수에 저장한다
      analogWrite(MOTOR_B_b, m_b_spd);            // 조정된 속력값으로 모터B- 동작을 실행시켜준다
      delay(5);                                   // 0.005초 지연시간을 준다

    } else {
      digitalWrite(trigPin, LOW);   // 위의 경우가 모두 아닐 경우, 적외선 측정 센서가 작동되지 않는다
      digitalWrite(MOTOR_A_b, LOW); // 모터A- 동작을 멈추게 한다
      digitalWrite(MOTOR_B_b, LOW); // 모터B- 동작을 멈추게 한다
    }
}
