# STerM

# **중대 공지사항**

- 현재 상태로 다운로드 받을 경우 Windows 11에서 컴파일이 되지 않는 이슈가 생긴다는 제보가 있습니다. 패치되는대로 알려드리겠습니다. (윈도 10은 문제없이 작동)


- STerM(a.k.a. 뜨따뜨따)은 COM포트 관련 업무 자동화 Tool입니다.
  - Starlit 가상머신 기반 시리얼 데이터 확인 가능
  - 파일 입출력 제공
  - Console창 입출력 기능 제공
  - 최대 64개의 COM포트 동시 제어 기능 제공

## 개발 배경

- 회사 업무 중 PuTTY만 의존해서 진행 시 3.5개월 걸리는 업무를 획기적으로 줄일 순 없을까 생각해 보았고, Starlit 언어 체계를 시리얼 통신과 연계해 보면 어떨까 하는 생각에 이 프로그램을 제작하게 되었습니다.
- 기존 TTY 프로그램과 다르게 전적으로 매크로만을 사용하여 동작하는 방식을 채택했으며, 매크로를 사용하여 TTY 구현이 가능하며, 동시에 최대 64개의 COM포트를 동시에 제어할 수 있습니다.
- 기존 TTY의 불필요한 로그 출력을 개선하여 필요한 로그만 선별해서 콘솔 창에 출력하고, 로그 파일도 중요한 내용만 저장할 수 있게 설계하였습니다.

## 설치하기(개발환경 구성)

- 주소에 띄어쓰기와 한글을 포함하지 않는 위치에 zip파일을 압축해제한 후 바로 실행 가능합니다.
  - 텍스트 파일을 생성하여 파일 확장자명을 `.stm`으로 설정하고 연결 프로그램으로 STerM.exe를 선택하세요. 기본 실행 파일로는 STerM.exe를 사용합니다.
  - 처음에는 컴파일만 하다가 실행은 아무것도 되지 않을겁니다. 컴파일 중에 튕겨지는 경우도 드물게 발생될수 있습니다.
    - 연결프로그램 - 스크립텀/노트패드++/메모장 등을 이용하여 편집 가능합니다.
    - (스크립텀 깔린경우 한정)스타라잇으로 stm파일 실행 후 편집 기능 사용 시 scriptum이 열려 편집 가능합니다.
  - `.stm` 파일은 메모장 등의 텍스트 편집 프로그램을 이용해 수정 가능합니다.
    - 추천하는 편집기
      - Scriptum 24 R1 (HDL Works)
        - 공식 제공하는 패치 파일을 설명서에 맞게 패치한 후 사용하면 더욱 좋습니다.
      - Notepad++
        - 입맛에 맞게 하이라이팅해서 쓸 수 있습니다.
      - 메모장(구버전/신버전)
        - 메모장만 깔려 있으면 메모장으로 편집 가능합니다.
  - `.stm`파일을 실행하면 매크로 파일이 컴파일된 후 실행됩니다.
    - 컴파일된 파일은 `.stb`로 저장되며, `.stb` 파일도 STerM.exe를 연결 프로그램으로 설정하면 컴파일 과정 없이 실행 가능합니다.
    - `.stb` 파일은 바이너리 형식으로, 텍스트 편집기에서 편집되지 않을 수 있습니다.

## 사용설명서(코딩 가이드)

- 사용하기 전에 코딩 가이드 및 TTY 예제를 접하시면 매우 편리합니다.
- TTY 예제를 기반으로 프로그램을 만드는 것을 추천합니다.

## TTY 예제

- STerM은 TTY 기능이 내장되어 있지 않습니다. 다만, 아래의 TTY 예제를 통해 PuTTY의 기능을 사용하실 수 있습니다. (단, Ctrl+C 제외)
- 실행 중단을 의미하는 Ctrl+C를 사용하면 특수 이벤트가 발생합니다. 따라서 해당 건은 별도로 처리하셔야 합니다.
- TTY 기반으로 디버깅 진행 시 TTY 예제를 익히시고, 이를 기반으로 코드를 구성하시면 더욱 편리합니다.

- 로그 파일 저장을 따로 하지 않는 TTY 예제
  ```Py
  # TTY Program for STerM.
  # Open Source TTY Code Example.
  # Written in Starlit 5.1 Pythonic Style.

  COMPORT = ["COM7"]                   # Please change to exact COM Port.

  main:
      COM[0].Open()                    # Default : 115200baud, no parity, 8bit, 1bit stop
      st = str
      while 1:
          if *COM[0]:
              re = COM[0].Read(st)
              Print1 st
          if kbhit():
              key = getch()
              COM[0] << f"{key:c}"
  ```

- 상/하/좌/우 키가 필요한 경우(PuTTY, TeraTerm 호환 코드)
  - 실무에서는 PuTTY, Teraterm에 맞춰서 디버깅할 수 있게 구현된 경우가 많으므로 필요에 따라 형태를 변형해서 전송해야 하는 경우도 있다. 이 부분은 kbhit()쪽을 수정하면 된다. 아래는 Windows에서 PuTTY 구현하는 예제이다.
    ```Py
    # TTY Program for STerM.
    # Open Source TTY Code Example.
    # Written in Starlit 5.1 Pythonic Style.

    COMPORT = ["COM11"]                   # Please change to exact COM Port.

    main:
        COM[0].Open()                    # Default : 115200baud, no parity, 8bit, 1bit stop
        st = str
        while 1:
            if *COM[0]:
                re = COM[0].Read(st)
                Print1 st
            if kbhit():
                key = getch()
                if key == 224:
                    key = getch()
                    when key:
                        case 72:
                            COM[0] << f"{0x1B:c}[A"
                        case 80:
                            COM[0] << f"{0x1B:c}[B"
                        case 77:
                            COM[0] << f"{0x1B:c}[C"
                        case 75:
                            COM[0] << f"{0x1B:c}[D"
                else:
                    COM[0] << f"{key:c}"
    ```

- 전체 로그를 파일에 저장하는 TTY 예제

  ```Py
  # TTY Program for STerM.
  # Open Source TTY Code Example.
  # File Saving Mode
  # Written in Starlit 5.1 Pythonic Style.
  
  COMPORT = ["COM7"]                   # Please change to exact COM Port.
  
  main:
      COM[0].Open()                    # Default : 115200baud, no parity, 8bit, 1bit stop
      st = str
      logFile = FILE_Setup("log.txt", "ab", "\r\n")
      while 1:
          if *COM[0]:
              re = COM[0].Read(st)
              Print1 st
              logFile << st
          if kbhit():
              key = getch()
              COM[0] << f"{key:c}"
  
  ```

## InstantTTY 사용 시 꿀팁

- InstantTTY란, 앞의 예제의 TTY 코드를 기반으로 즉시 실행 가능하게 만든 TTY 실행 파일입니다. 엄연한 STerM 실행 바이너리 파일로 파일 형식은 `.stb`를 사용하지만, 기존 TTY와 다르게 초기 설정 없이 COM포트만 맞추면 즉시 사용할 수 있게 만든 TTY입니다.
  - InstantTTY에 없는 포트의 경우 당황하지 말고 예제 소스를 긁어서 컴파일 하면 InstantTTY를 생성할 수 있습니다.
  - 기본 사양은 115200baud, 8bit, no parity, 1 stop bit로, 상황에 맞게 사양을 변경할 수 있습니다.
- STerM InstantTTY도 엄연한 TTY다보니 PuTTY에서 지원되는 기능은 대부분 지원된다. 대표적으로 Shift + Insert를 눌러서 클립보드 내용을 넣을 수 있다.
- PuTTY, Tera Term과 다르게 드래그 상태에서 복제가 되지 않는다. Windows CMD 기반으로 설계된 탓에 Windows CMD의 복제 방식을 따라 Ctrl + C를 눌러야 복제가 된다.
- Ctrl+C를 InstantTTY를 실행하던 중 꺼져버리는 문제가 생겨 현재 꺼지지 않게만 조치했고, 추후 TTY상에서 실행 중지 구현 예정이다.(1.2버전)
- STerM InstantTTY는 STerM을 PuTTY나 Tera Term처럼 쓸 수 있게 만든 TTY이며, 본래 목적과 약간 차이가 있습니다. STerM 자체는 TTY를 위한 것이 아닌 자동화를 구현하여 실험을 설계하기 위한 Tool로, TTY는 이 Tool의 부가 기능일 뿐입니다.

## 버전 정보

### 1.0 STerM Release

### 1.1 STerM 버퍼 이슈 개선 및 함수 추가

- Buffer 이슈 개선 : Buffer 인덱싱 및 크기 변경(확대)
- 함수 추가 : Scan 및 관련 함수 최적화, 파일 입출력 함수 간소화 등.


