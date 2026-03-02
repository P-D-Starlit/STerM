# STerM

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
      while 1:
          if *COM[0]:
              re = COM[0].Read()
              CONSOLE << f"{re:c}"
          if kbhit():
              key = getch()
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
      logFile = file_t
      while 1:
          if *COM[0]:
              re = COM[0].Read()
              CONSOLE << f"{re:c}"

              logFile.Open("log.txt", "at", "\n") # File Saving Code
              logFile << f"{re:c}"
              logFile.Close()
          if kbhit():
              key = getch()
              COM[0] << f"{key:c}"
  
  ```




