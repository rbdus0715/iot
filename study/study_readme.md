# base
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

**LED** : 다리가 긴 쪽이 (+) 짧은 쪽이 (-)</br>
![image](https://github.com/rbdus0715/iot/assets/85426187/8499b3c4-c5f6-43b7-bb5b-6b8e7e26c998)

**LED 활용 코드**
```c++
delay(NUMBER);
pinMode(PIN, OUTPUT or INPUT);
digitalWrite(PIN, HIGHT or LOW);
analogWrite(PIN, 0~255);
```
