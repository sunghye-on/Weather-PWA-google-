# 구글에서 제공해준 PWA 기본 예제

## Introduction

날씨를 알려주는 웹을 기본으로 우리는 점차 진화하는 PWA로 진화할것!

### 우리가 만들 것들

- 반응형 디자인으로 데스크탑과 모비일기기에서 사용이 가능하다
- 서비스워커를 사용하여 실행에 필요한 앱의 리소스를 미리캐시하고 런타임에 날씨 데이터를 캐시하여 성능을 향샹 시킨다
- 웹 앱 매니페스트를 이용하여 설치가능하게 하고 유저에게 설치 가능하다는 노티피케이션 이벤트를 주겠다.

### 우리가 배울 것들

- 웹 앱 미니페스트를 작성하는 방법을 배운다.
- 오프라인에서 일정부분 사용가능하게 만드는 방법을 배운다.
- 오프라인에도 완벽히 작동하는 앱을 만드는 방법을 배운다.

---

## SETUP

### 전체 환경 세팅

아래의 링크에서 파일을 다운 받거나 [https://glitch.com](https://glitch.com/). 를 이용한다. 나는 파일을 다운받아 VS code에서 진행하겠다. (VS code로 진행시 node.js 필수!)

#### 파일 다운로드 링크

https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip

### VS code에서 시작하기

1. 파일 압축을 풀어준 뒤 VS code 애서 폴더를 열어준다.
2. .env의 DARKSKY_API_KEY를 https://api.darksky.net/forecast/DARKSKY_API_KEY/40.7720232,-73.9732319로 수정해준다.
   - 이때 DARKSKY_API_KEY는 날씨를 받아오는 API
3. 패키지 파일들의 설치를 위해 npm i 커맨드 입력
4. 시작을 위한 스크립트 npm start 입력
5. 크롬 브라우져로 [http://localhost:8000](http://localhost:8000/) 열기

## 기준 설정

### Lighthouse(등대)

**Lighthouse**는 사이트 및 페이지의 품질을 향상 시키기는데 도움이되는 도구이 사용하기 쉽다!! 또한 성능이나 접근성, 그리고 PWA에 대한 감사를 실사하여 문제해결방법을 설명하거나 부족한 부분을 알려준다.

#### 실행 방법

크롬 개발자도구(F12를 누르면 나옴)에서 Aduits에서 run Audits를 클릭하면 실행된다.

#### 우리가 해결해나갈 오류들

1. Current page does not respond with a 200 when offline.
2. `start_url` does not respond with a 200 when offline.
3. Does not register a service worker that controls page and `start_url.`
4. Web app manifest does not meet the installability requirements.
5. Is not configured for a custom splash screen.
6. Does not set an address-bar theme color.

---

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
  "icons": [
    {
      "src": "/images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
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
<link rel="manifest" href="/manifest.json" />
```

이제 매니페스트 연결을 마쳤다면

개발자도구 => Application => Manifest에서 우리가 만든 매니페스트가 적용된 것을 알 수 있다.

#### iOS를 위한 meta tag

iOS의 사파리는 아직 웹 앱 매니페스트를 지원하지 않습니다... (슬픔) 그래서 우리는 아래와 같이 고전적인? 메타태그를 직접 달아둬야합니다

```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<meta name="apple-mobile-web-app-title" content="Weather PWA" />
<link rel="apple-touch-icon" href="/images/icons/icon-152x152.png" />
```

#### 앱의 설명 작성

SEO의 Audits결과중에 Document does not have a meta description.가 있다. 이것은 설명이 없다는 이야기인데 설명은 구글 검색결과에 표시될 수 있습니다. 즉, 설명을 자세하고 잘 쓰면 사용자들이 검색하여 많은 사람들이 사용할 수 있습니다.

아래와 같이 혹은 원하는 설명을 적어보겠습니다.

```html
<meta name="description" content="A sample weather app" />
```

#### 주소창 색상 지정

PWA Audits 결과중 Does not set an address-bar theme color가 있는데 우리가 만드는 브랜드의 색상에 맞게 변경할 수 있는 기능입니다. 필수는 아니지만 권장합니다.

```html
<meta name="theme-color" content="#2F3BA2" />
```

다시 Run Audits를 실행하보면 우리는 기존 6개의 오류중 4,5,6번 오류를 해결했고 1,2,3번의 오류가 남은 것을 볼 수 있다.

---

## 기본적인 오프라인 기능 구현

이번에는 날씨앱에 대한 간단한 오프라인 페이지를 축해보도록 하겠습니다. 사용자가 오프라인 상태에서 앱을 로드하려고 하면 브라우저에 표시되는 일반적인 오프라인 페이지(공룡게임)말고 우리가 지정한 페이지가 표시되게 할 것이다.

이번 학습을 하면 우리가 위에서 봤던 1,2,3번의 오류를 해결 할 수 있습니다.

다음에는 완전한 오프라인 환경으로 바꿀것이다.

서비스 워커를 통해 제공되는 기능은 점진적 향상으로 간주되어야하며 브라우저가 지원하는 경우에만 추가해야합니다. 예를 들어 서비스 워커를 사용하면 네트워크가없는 경우에도 사용할 수 있도록 앱의 앱 셸과 앱의 데이터를 캐시 할 수 있습니다. 서비스 워커가 지원되지 않으면 오프라인 코드가 호출되지 않고 사용자에게 기본 경험(공룡게임)이 제공됩니다. 점진적 개선 기능을 제공하기 위해 기능 감지를 사용하면 오버 헤드가 거의 없으며 해당 기능을 지원하지 않는 이전 브라우저에서는 중단되지 않습니다.

```javascript
//tip
/*
*	서비스워커의 기능들은 HTTPS를 통해 액세스되는 페이지에서만 사용될 수 있습니다.
*	(http://localhost and equivalents also work to facilitate testing.)
```

### 서비스 워커 등록

서비스워커의 등록을위해 index.html의 script태그 부분에 아래 자바스크립트 코드를 삽입하여 서비스 워커를 등록해보자

```javascript
if ("serviceWorker" in navigator) {
  window.addEventListener("load", () => {
    navigator.serviceWorker.register("/service-worker.js").then(reg => {
      console.log("Service worker registered.", reg);
    });
  });
}
```

위의 코드는 서비스워커를 지원하면 실행되는 블럭이다. 그리고 로드이벤트가 발생시 우리의 서비스워커가 존재하는 위치를 지정해줌으로 서비스워커를 등록시킨다. 그후 콘솔로 성공사실을 알린다.

```javascript
//tip
/*	서비스워커는 자바스크립트들이 모여있는 script디렉토리에 존재하는 것이 아닌 루트 디렉토리에 존재한다. 
    이게 서비스 워커의 범위(scope)를 설정하는 가장 쉬운 방법이다. 루트디렉토리에 생성하면 서비스
	  워커는 모든 웹페이지에 요청을 제어 할 수 있다. 
*/
```

### Precache 오프라인 페이지

먼저 우리가 서비스워커에게 무엇을 미리 캐시할지 말해야합니다!! 이번 프로젝트에 미리 만들어진 오프라인 페이지(offilne.html)를 인터넷 연결이 없을때 보여주도록 합시다.

가장 먼저 서비스워커에 FILES_TO_CACHE에 오프라인 페이지를 넣어줍시다.

```javascript
const FILES_TO_CACHE = ["/offline.html"];
```

다음으로 우리는 서비스워커가 설치되는 install이벤트에서 우리가 지정한 FILES_TO_CACHE파일들을 캐시에 미리 저장합니다.

```javascript
self.addEventListener("install", evt => {
  console.log("[ServiceWorker] Install");
  // CODELAB: Precache static resources here.
  evt.waitUntil(
    caches.open(CACHE_NAME).then(cache => {
      console.log("[ServiceWorker] Pre-caching offline page");
      return cache.addAll(FILES_TO_CACHE);
    })
  );
  self.skipWaiting();
});
```

인스톨이벤트가 발생하면 제공된 캐시이름으로 cache.open()으로 캐시를 열수있다. 이때 캐시이름은 캐시의 버전을 관리하기 쉽게 만들어진다. 또한 캐시의 버전 관리를 하면 업데이트가 간편하다.

케시를 연이후에는 URL목록을 서버에서 가져와서 캐시에 response를 추가하는 cache.addAll()을 이용할 수 있다. URL을 요청하는 중에 하나라도 실패하면 전체가 실패한다.

### 개발자도구로 서비스워커 확인

크롬 개발자도구를 이용하여 서비스워커를 이해하고 디버깅하는 방법을 보겠습니다. 개발자도구에 Application에 Service Workers에 들어가보면 아무것도 없거나 서비스워커가 설치된 것을 볼수 있다. 아무것도 없다면 페이지를 새로고침해보자!

서비스워커에 관한 내용들이 있으면 현재 서비스워커가 작동중이라는 것이다.

### 이전 캐시를 지우기

우리는 activate이벤트를 사용하여 우리의 이전 캐시데이터를 지울 것이다. 이작업에서 서비스워커는 CACHE_NAME 의 번화를 확인하여 변화가 있을시(업데이트가 있을시) 이번의 데이터를 삭제한다. 아래의 코드에서 확인하자

```javascript
self.addEventListener("activate", evt => {
  console.log("[ServiceWorker] Activate");
  // CODELAB: Remove previous cached data from disk.
  evt.waitUntil(
    caches.keys().then(keyList => {
      return Promise.all(
        keyList.map(key => {
          if (key !== CACHE_NAME) {
            console.log("[ServiceWorker] Removing old cache", key);
            return caches.delete(key);
          }
        })
      );
    })
  );
  self.clients.claim();
});
```

### 인터넷 연결을 실패할때 캐시를 다루는 방법

마지막으로 우리는 fetch이벤트를 처리해야한다. 우리는 "Network falling back to cache" 방법을 사용할 것이며 서비스워커가 먼저 인터넷에서 fetch 리소르를 받아오고자 한다. 만약 실패하면 서비스워커가 캐시에 있던 오프라인 페이지를 가져오도록하는 방법이다.

아래와 같은 코드를 fetch이벤트 핸들러에 작성해보자

```javascript
self.addEventListener("fetch", evt => {
  console.log("[ServiceWorker] Fetch", evt.request.url);
  // CODELAB: Add fetch event handler here.
  if (evt.request.mode !== "navigate") {
    // Not a page navigation, bail.
    return;
  }
  evt.respondWith(
    fetch(evt.request).catch(() => {
      return caches.open(CACHE_NAME).then(cache => {
        return cache.match("offline.html");
      });
    })
  );
});
```

------------------------------------------------------- 그림 아래까지 했음 내일 마무리 필수!
