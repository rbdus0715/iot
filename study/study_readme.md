# 목차
[1.base](#BASE)</br>
[2.LED](#LED)</br>
[3.tact 스위치와 풀업/다운 저항](#tact-스위치와-풀업/다운-저항)</br>
[4.psd 센서](#psd-센서)</br>
[5.초음파 센서](#초음파-센서)</br>
[6. 수분 수위 센서](#수분-수위-센서)</br>
[7. 기울기 센서](#기울기-센서)</br>
[8. 조도 센서](#조도-센서)</br>

# BASE
```c++
// 전처리
int var;
// 초기화
void setup()
// 루프
void loop()
```
**우노보드 구조**</br>
디지털 신호는 read write를 모두 할 수 있지만 아날로그 신호는 read 만 한다.</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/253febd7-aeb7-4760-9c3e-867f01f0e881" width="400"/>

**빵판** - 같은 선으로 연결된 부분들은 같은 전극을 띈다.</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/04345236-f7bd-4206-a62d-681fd39b0f5d" width="400"/>

**저항**</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/f5ef4a23-dca4-4800-815a-3cdddb0481d7" width="400"/>

# LED
**LED** : 다리가 긴 쪽이 (+) 짧은 쪽이 (-)</br>
![image](https://github.com/rbdus0715/iot/assets/85426187/8499b3c4-c5f6-43b7-bb5b-6b8e7e26c998)

**LED 활용 코드**
```c++
delay(NUMBER);
pinMode(PIN, OUTPUT or INPUT);
digitalWrite(PIN, HIGHT or LOW);
analogWrite(PIN, 0~255);
```

**LED 깜빡이기**
GND는 (-) 전원을 입력받는 곳이며 그라운드라고 함. 그라운드에는 0V의 전압이 있음.
```c++
int led = 13;

void setup() {
  pinMode(led, OUTPUT); 
}

void loop() {
  digitalWrite(led, HIGH);
  delay(1000);
  digitalWrite(led, LOW);
  delay(1000);
}
```

**LED 여러개 순차적으로 켜기**
```c++
void setup() {
  pinMode(13, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(11, OUTPUT);
}

void loop() {
  digitalWrite(13, HIGH);
  delay(1000);
  digitalWrite(12, HIGH);
  delay(1000);
  digitalWrite(11, HIGH);
  delay(1000);
}
```

**digitalWrite vs analogWrite**
digital은 0과 1로밖에 표현되지 않아 껐다 키는 동작밖에 구현하지 못한다.
반면에 analog는 0부터 255까지의 값을 표현할 수 있어 연속된 동작을 구현할 수 있다.

# tact 스위치와 풀업/다운 저항
![image](https://github.com/rbdus0715/iot/assets/85426187/243253d9-eddb-4c37-9a1c-93d6d1f3ca1c)
- 풀업 저항 : 스위치를 누르지 않았을 때 전류가 흐른다.
- 풀다운 저항 : 스위치를 눌렀을 때 전류가 흐른다.

**풀업저항 예시**
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/999f1fbf-7fc3-4a4f-8eba-e959fceb46ab" width="400"/>
```c++
int led = 13; // 아두이노 내부 저항은 13번 핀에 연결되어있다. 13번 핀에 led를 연결하지 않아도 내부 LED가 작동한다.
int tact = 2;
int tact_state;

void setup() {
  pinMode(led, OUTPUT);
  pinMode(tact, INPUT);
  Serial.begin(9600);
}

void loop() {
  tact_state = digitalRead(tact);
  Serial.println(tact_state);
  digitalWrite(led, tact_state);
}
```

**내부 풀업 저항 사용하기**
```c++
...
pinMode(tact, INPUT_PULLUP); // 이걸로 수정
...
```

**풀 다운 저항**
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/62207f89-bea4-4f06-b772-35d351677f9c" width="400"/></br>
코드는 풀업과 같다.

# psd 센서
**거리 측정**
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/12de143e-9610-4ab3-95f7-1e4a0cfe9350" width="400"/>
</br>
왼쪽부터 A0, GND, 5V
```c++
int psd = A0;
int distance = 0;

void setup() {
  pinMode(psd, INPUT); // 센서는 input 모드
  Serial.begin(9600);
}

void loop() {
  int psd_check = analogRead(psd);
  // Serial.println(psd_check);
  int volt = map(psd_check, 0, 1023, 0, 5000); // psd_check 불러온 0에서 1023의 값을 0~5000 비율로 계산하여 매핑
  distance = (21.61 / (volt - 0.1696)*1000; // 거리를 구하는 공식
  Serial.print(distance);
  Serial.println("cm");
  delay(500);
}
```
**psd 센서를 이용한 LED on/off**
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/4576d5ea-7273-4f73-b2f2-e41d2e9847dc" width="400"/>
</br>
```c++
// 전처리 코드 추가
int led = 13;
// setup 코드 추가
pinMode(led, OUTPUT);
// loop 코드 추가
if(distance <= 15) {
  digitalWrite(led, HIGH);
}
else {
    digitalWrite(led, LOW);
}
```

# 초음파 센서
- 20kHz 이상의 높은 주파수를 보낸 후 반사되어 오는 시간차로 거리 측정
- 구성 : 초음파가 나오는 트리거(trig)와 초음파를 받는 에코(echo)
- 핀맵 : 왼쪽부터 VCC, TRIG, ECHO, GND 이다.
- 전압 전류 : 5V, 15mA
- 거리를 측정하기 위해서 340 * (초음파가 물체로부터 반사되어 돌아오는 시간) / 10000 / 2 (편도) 를 계산한다.

**초음파 센서로 거리 측정**
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/d52f1212-a1a6-4673-8d09-1d61f5f75ad4" width="400"/>
</br>

```c++
int echo = 12;
int trig = 13;

void setup() {
  Serial.begin(9600);
  pinMode(echo, INPUT);
  pinMode(trig, OUTPUT);
}

void loop() {
  float duration, distance;
 
  digitalWrite(trig, HIGH);
  delay(10);
  digitalWrite(trig, LOW);
  
  duration = pulseIn(echo, HIGH);
  distance = ((float)(340*duration) / 10000) / 2;
  
  Serial.print(distance);
  Serial.println("cm");
  delay(500);
}
```

**초음파 센서와 LED on/off**
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/910a0f06-da61-4915-8890-c926ce718d8b" width="400"/>
</br>

```c++
int echo = 12;
int trig = 13;
int led = 11;

void setup() {
  Serial.begin(9600);
  pinMode(echo, INPUT);
  pinMode(trig, OUTPUT);
  pinMode(led, OUTPUT);
}

void loop() {
  float duration, distance;
 
  digitalWrite(trig, HIGH);
  delay(10);
  digitalWrite(trig, LOW);
  
  duration = pulseIn(echo, HIGH);
  distance = ((float)(340*duration) / 10000) / 2;
  
  Serial.print(distance);
  Serial.println("cm");
  
  if(distance <= 15) {
  	digitalWrite(led, HIGH);
    Serial.println("led on");
  }
  else {
  	digitalWrite(led, LOW);
    Serial.println("led off");
  }
  
  delay(500);
}
```

# 수분 수위 센서
- S : A0
- (+) : 5V
- (-) : GND
```c++
int sig = A0;
int height;

void setup() {
  Serial.begin(9600);
  pinMode(sig, INPUT);
}

void loop() {
  height = analogRead(sig);
  Serial.println(height);
  delay(100);
}
```

# 기울기 센서
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/06a48c6d-c7bd-4350-96a8-11e6d21bd972" width="400"/>
</br>
풀업풀다운 저항을 이용

```c++
int tilt = 8;

void setup() {
  pinMode(tilt, INPUT);
  Serial.begin(9600);
}

void loop() {
  int value = digitalRead(tilt);
  Serial.println(value);
  delay(100);
}
```

# 조도 센서
조도센서(photo resistor)는 주변 밝기를 측정하는 센서다. 조도 센서에는 극성은 없으나 빛의 양에 따라 전도율이 변함.</br>
빛의 양이 많아지면 전도율이 높아져 저항이 낮아진다. 하지만 전도율에 비례해 선형적으로 증가하는 것이 아니어서 대략적인 밝고 어두움만을 측정한다.
</br>
<img src="https://github.com/rbdus0715/iot/assets/85426187/8a15a10f-36cb-48e1-a450-eef9b870d943" width="400"/>
</br>

```c++
int bright = A0;

void setup() {
  pinMode(bright, INPUT);
  Serial.begin(9600);
}

void loop() {
  int value = analogRead(bright);
  Serial.println(value);
  delay(100);
}
```

# IR 센서 (적외선)

# 소리 감지 센서

# 온습도 센서

# 모터

# 1채널 릴레이

# 7세그먼트
