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



## 기준 설정 

### Lighthouse(등대)

**Lighthouse**는 사이트 및 페이지의 품질을 향상 시키기는데 도움이되는 도구이 사용하기 쉽다!! 또한 성능이나 접근성, 그리고 PWA에 대한 감사를 실사하여 문제해결방법을 설명하거나 부족한 부분을 알려준다.

#### 실행 방법

크롬 개발자도구(F12를 누르면 나옴)에서 Aduits에서 run Audits를 클릭하면 실행된다.



#### 우리가 해결해나갈 오류들

1. Current page does not respond with a 200 when offline.
2.  `start_url` does not respond with a 200 when offline.
3.  Does not register a service worker that controls page and `start_url.`
4.  Web app manifest does not meet the installability requirements.
5.  Is not configured for a custom splash screen.
6.  Does not set an address-bar theme color.

-----------------

## 웹 앱 미니페스트 만들기

### 웹 앱 매니페스트란?

간단한 JSON파일로 개발자가 앱의 사용자에게 표시되는 방식을 제어 할 수 있는 기능을 제공한다!



### 웹 앱 매니페스트 생성

우리의 프로젝트 public 폴더 안에 manifest.json을 생성합니다.

그후 아래의 코드를 (입력 || 복붙) 합시다 **(!!복붙할시에는 주석 제거)**

```json
{
  // name과 이름이 너무 길 경우 표시되는short_name
  "name": "Weather",
  "short_name": "Weather",
  // 설치 및 화면에 뿌려줄 아이콘들
  // tip: 설치가능한 PWA를 위해서는 적어도 192 X 192px 아이콘과 512 X 512px 아이콘을 포함해야한다.
  "icons": [{
    "src": "/images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    }, {
      "src": "/images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    }, {
      "src": "/images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    }, {
      "src": "/images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }, {
      "src": "/images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }, {
      "src": "/images/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }],
  //앱을 열면 시작될 시작 URL을 정의
  "start_url": "/index.html",
  //display의 모드를 설정할 수 있다.(3가지의 모드가 있다) 
  //tip: 설치가능한 PWA를 위해서는 standalone혹은 fullscreen으로 설정해야한다.
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
```



### 웹 앱 매니페스트 연결 및 확인 

아래의 설명에 맞는 링크들를 index.html에 head에 연결시켜준다. 

매니페스트 연결을 위해 아래의 링크를 연결해준다.

```html
<link rel="manifest" href="/manifest.json">
```

 이제 매니페스트 연결을 마쳤다면

개발자도구 => Application => Manifest에서 우리가 만든 매니페스트가 적용된 것을 알 수 있다.

#### iOS를 위한 meta tag

iOS의 사파리는 아직 웹 앱 매니페스트를 지원하지 않습니다... (슬픔) 그래서 우리는 아래와 같이 고전적인? 메타태그를 직접 달아둬야합니다

```html
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="apple-mobile-web-app-title" content="Weather PWA">
<link rel="apple-touch-icon" href="/images/icons/icon-152x152.png">
```

#### 앱의 설명 작성

SEO의 Audits결과중에  Document does not have a meta description.가 있다. 이것은 설명이 없다는 이야기인데 설명은 구글 검색결과에 표시될 수 있습니다.  즉, 설명을 자세하고 잘 쓰면 사용자들이 검색하여 많은 사람들이 사용할 수 있습니다.

아래와 같이 혹은 원하는 설명을 적어보겠습니다.

```html
<meta name="description" content="A sample weather app">
```



####  주소창 색상 지정

PWA Audits 결과중 Does not set an address-bar theme color가 있는데 우리가 만드는 브랜드의 색상에 맞게 변경할 수 있는 기능입니다.  필수는 아니지만 권장합니다.

```html
<meta name="theme-color" content="#2F3BA2" />
```



다시 Run Audits를 실행하보면 우리는 기존 6개의 오류중 4,5,6번 오류를 해결했고 1,2,3번의 오류가 남은 것을 볼 수 있다.

-----------

