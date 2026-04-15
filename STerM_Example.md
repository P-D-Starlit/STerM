# STerM Examples 1

STerM Example - LED 제어

## 1. Reference 보드 스펙

- LED는 PWM 핀과 연결되어 있다.
- 아래 2가지 Spec 중 1가지를 만족하는 회로로 구성된다.<br>
  |Pin Index|A(단색)|B(멀티컬러)|
  |--|--|--|
  |3|Red|Red1|
  |4|Blue/Green|Blue2|
  |5|-|Green1|
  |6|-|Blue1|
  |8|-|Green2|
  |9|-|Red2|
  |13|Orange(Option)|-|
  - 13번 핀 적용 : ATmega 기반 Arduino는 13번핀을 사용할 수도 있다.(PWM 미지원)
  - Nucleo 보드는 핀 번호가 정해지지 않으니 LED 연결 핀을 3/4번에 할당해서 명령어를 구성하면 된다.(PWM 지원 여부는 보드 스펙 확인 필요.)

- 보드는 아래 명령어에 의해 LED 제어가 가능하도록 한다.
  - `PinMode (핀번호) (모드)`
    - 핀 번호 : 0~13(보드 마다 지원 범위 다름)
    - 모드 : 0 - Input, 1 - Output, 2 - ~~Input_Pullup~~, 6 - PWM
      - Arduino에서는 analogWrite 함수 쓰면 자동으로 PWM 설정되므로 PinMode 명령어 불필요.
  - `PWM (핀번호) (값)` : LED를 특정 밝기로 출력하는 명령어.
  - `DigitalWrite (핀번호) (값)` : 특정 핀에 0 또는 1을 출력.

- 명령어 입력 시 TTY 규격에 준수하여 키보드가 입력되면 입력 내용이 그대로 화면에 출력되어야 한다.

## 2. LED 제어 예제

### 2-1. 키보드를 눌러 LED 제어하기

- 컨셉
  - 1 누름 : 빨간색 LED 켜짐/꺼짐
  - 2 누름 : 파란색 LED 켜짐/꺼짐

- 코드
  ```Py
  COMPORT = ["COM3"]


  main:
      COM[0].Open()
      ledbit = 0
      while 1:
          if kbhit():
              key = getch()
              when key:
                  case '1':
                      ledbit ^= 0x01
                  case '2':
                      ledbit ^= 0x02
              COM[0] << f"DigitalWrite 3 {ledbit & 1}"
              COM[0] << f"DigitalWrite 4 {(ledbit >> 1) & 1}"
  ```







