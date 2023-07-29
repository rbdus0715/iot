# 1. base
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

# 2. LED
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
