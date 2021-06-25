# 캡스톤 보고서

# 프로젝트 개요

![%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%86%AB%20%E1%84%87%E1%85%A9%E1%84%80%E1%85%A9%E1%84%89%E1%85%A5%20f72ee53cc3714b42bc610bd426298068/Untitled.png](%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%86%AB%20%E1%84%87%E1%85%A9%E1%84%80%E1%85%A9%E1%84%89%E1%85%A5%20f72ee53cc3714b42bc610bd426298068/Untitled.png)

1. Android Application에서 소켓통신을 이용해 Raspberry PI에 출석 오픈을 요청합니다.
2. 출석이 오픈되면 Raspberry PI에 달려있는 카메라를 이용해 Stream을 엽니다.
3. Stream에서 OpenCV의 얼굴 인식을 통해 얼굴이 인식될때까지 기다립니다.
4. 얼굴이 인식되면 NAS에 있는 Target Image와 인식된 얼굴(Source Image)을 Azure API를 이용해 비교합니다.
5. Target Image와 Source Image가 일치하는 얼굴이 있다면 Arduino에 온도 측정을 요청합니다.
6. Arduino와 Raspberry PI는 시리얼 통신을 통해 온도정보를 주고받습니다.
7. Raspberry PI는 받은 온도정보와 얼굴 인식 정보, 날짜, 결과코드 등을 학번을 PK로 하는 JSON 형식으로 가공해 Firebase에 업로드 합니다.
8. Firebase DB에 변동사항이 생기면 Cloud Messaging Service를 이용해 교수자에게 Push 알림을 보냅니다.
9. 교수자는 Android Application을 통해 출석정보를 열람하고 출석을 종료할 수 있습니다.

---

# 라이브러리 다운로드 필요

## **Standard Library**

- configparser

    ⇒소스와 설정파일 분리를 위해 사용.

- threading

    ⇒ thread 사용.

- os

    ⇒ os 명령어 실행.

- datetime

    ⇒ 현재 날짜 체크와 날짜형식 지정을 위해 사용.

- time

    ⇒ sleep 기능을 위해 사용.

- socket

    ⇒ Android ↔ Raspberry PI 소켓통신을 위해 사용.

- json

    ⇒ Firebase에 JSON형식으로 업로드하기 위해 사용.

## **3rd party Library**

- azure

    ⇒ Azure API 사용.

- cv2

    ⇒ openCV 영상처리 사용.

- pyserial

    ⇒ Raspberry PI – Arduino Serial 통신 사용.

- simpleaudio

    ⇒ 소리 출력.

- pyfcm

    ⇒ Firebase Cloud Messaging Notification. 클라우드 메시징 사용

- firebase_admin

    ⇒ Firebase 사용.

- requests

    ⇒ NAS에 접근해 데이터 사용.

## **Local Application Library**

- ArduinoSerialProtocol

    ⇒ Arduino와 시리얼 통신 관련 기능 구현.

- FaceDetection

    ⇒ 얼굴 인식, 비교 관련 기능 구현.

- playSound

    ⇒ 사운드 출력 관련 기능 구현.

- SocetServer

    ⇒ Android 소켓통신 관련 기능 구현.

- UseFirebase

    ⇒ Firebase 관련 기능 구현.

## 한번에 설치 → `pip install -r requirements.txt`

---

# private_config.ini 생성 필요

```
[AZURE]
KEY = 
END_POINT = 
IMAGE_BASE_URL = 

[Firebase]
KEY = 
databaseURL = 

[firebase_cloudmessaging]
APIKEY = 
TOKEN_URL = 
```

---

# 프로젝트 구조

![%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%86%AB%20%E1%84%87%E1%85%A9%E1%84%80%E1%85%A9%E1%84%89%E1%85%A5%20f72ee53cc3714b42bc610bd426298068/Untitled%201.png](%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%86%AB%20%E1%84%87%E1%85%A9%E1%84%80%E1%85%A9%E1%84%89%E1%85%A5%20f72ee53cc3714b42bc610bd426298068/Untitled%201.png)

---