# 구글에서 제공해준 PWA 기본 예제



## Introduction

날씨를 알려주는 웹을 기본으로 우리는 점차 진화하는 PWA로 진화할것!

### 우리가 만들 것들

* 반응형 디자인으로 데스크탑과 모비일기기에서 사용이 가능하다
* 서비스워커를 사용하여 실행에 필요한 앱의 리소스를 미리캐시하고 런타임에 날씨 데이터를 캐시하여 성능을 향샹 시킨다
*  웹 앱 매니페스트를 이용하여 설치가능하게 하고 유저에게 설치 가능하다는 노티피케이션 이벤트를 주겠다.



### 우리가 배울 것들

* 웹 앱 미니페스트를 작성하는 방법을 배운다.
* 오프라인에서 일정부분 사용가능하게 만드는 방법을 배운다.
* 오프라인에도 완벽히 작동하는 앱을 만드는 방법을 배운다.



---------

##  SETUP

### 전체 환경 세팅

아래의 링크에서 파일을 다운 받거나  [https://glitch.com](https://glitch.com/). 를 이용한다. 나는 파일을 다운받아 VS code에서 진행하겠다. (VS code로 진행시 node.js 필수!)

#### 파일 다운로드 링크

https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip

### VS code에서 시작하기

1. 파일 압축을 풀어준 뒤 VS code 애서 폴더를 열어준다.
2. .env의 DARKSKY_API_KEY를 https://api.darksky.net/forecast/DARKSKY_API_KEY/40.7720232,-73.9732319로 수정해준다.
   * 이때 DARKSKY_API_KEY는 날씨를 받아오는 API
3. 패키지 파일들의 설치를 위해 npm i 커맨드 입력
4. 시작을 위한 스크립트 npm start 입력
5. 크롬 브라우져로  [http://localhost:8000](http://localhost:8000/)  열기