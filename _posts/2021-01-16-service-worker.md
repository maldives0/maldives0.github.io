---
title: daon 동영상 로딩 오류 문제 해결하기
author: juyoung
date: 2021-01-16 22:11:00 +0800
categories: [project, resolution]
tags: [project]
---

# 1. 문제상황: video 로딩중 service worker에 에러 발생

```html
<video controls autoplay loop muted>
  <source src="img/main/water.mp4" type="video/mp4" />
</video>
```

다음과 같은 동영상 파일을 로드하는 과정에서 아래와 같은 오류가 발생했다.

> sw.js:1 Uncaught (in promise) TypeError: Failed to execute 'put' on 'Cache': Partial response (status code 206) is unsupported at sw.js:1

<br />

![mp4_206_error.jpg](/assets/img/mp4_206_error.jpg)

![sw_fetch.jpg](/assets/img/sw_fetch.jpg) > <br />

![provisional_header.jpg](/assets/img/provisional_header.jpg) <br />

HTTP 206 상태코드가 어떤 의미인지부터 찾아보니 206 Partial Content는 Range 헤더에 기술된 데이터 범위에 대한 요청이 성공적으로 응답되어 바디에 해당되는 데이터를 담고 있다는 것을 알려준다.

동영상 파일이 나누어 로딩되는 과정에서 오류가 발생된 듯했다.
stackoverflow에 올라온 그나마 비슷한 답변들에서 다음과 같은 해결방법을 발견했다.

> The trick is to understand the service worker scope. By default, its scope is the directory it's in. If you access your service worker script from https://your.domain.com/static/service-worker.js, its default scope will be /static. So, if you type cache.add('index.html'), it will actually request https://your.domain.com/static/index.html, which will result in an error if you were trying to get https://your.domain.com/index.html

서비스 워커 범위를 이해해야 이 문제를 풀 수 있는데 번역기의 도움을 받아가며 읽어본 결과 service-worker의 기본 경로가 /static이기 때문에 url 요청을 'index.html'와 같이 보내면 https://your.domain.com/static/index.html로 가버리는 오류가 발생한다는 것 같다.
<br />

그래서 상대 경로를 사용하여 파일에 액세스를 해야한다. 그래서 나의 비디오의 경로를 `img/main/water.mp4`에서 아래와 같이 변경하였음에도 같은 오류가 나타났다.

```html
<video controls autoplay loop muted>
  <source src="./img/main/water.mp4" type="video/mp4" />
</video>
```

<br />

# 2. 해결방법: 쌓인 캐시 삭제해주기

[현실적 PWA](https://www.slideshare.net/netil/pwa-65378869)에 나온 내용을 보던 중 서비스워커를 디버깅할 수 있는 툴이 설명되어 있어 크롬 devTools의 `Application` 탭으로 들어갔다. `clear storage`로 들어갔더니 케시가 가득차있는 그래프가 보여서 `clear site data`로 Cache를 삭제해줬더니 문제가 해결되었다. 결국 cache용량이 가득차서 put을 못해준 것이 원이이었던 것 같다.

![clear_storage.jpg](/assets/img/clear_storage.jpg) > <br />

# 3. 워커(worker)

워커는 HTML5에서 작동이 되는데 브라우저가 백그라운드에서 실행하는 스크립트로 웹페이지와는 별개로 작동하며 웹페이지 또는 사용자의 인터랙션이 필요하지 않은 기능만 제공한다.

### 워커는 싱글 스레드인 JavaScript에 멀티스레드 기능을 지원한다.

워커가 생성될 때마다 JavaScript를 실행할 수 있는 고유 스레드를 생성하여 속도 성능을 크게 향상시킬 수 있다. 로딩과 실행이 오래 걸리는 JavaScript 파일 실행할 때 복잡한 계산은 워커에게 맡기고 JavaScript은 그밖의 일을 처리하게 된다. 워커는 window객체의 속성 중 하나로 `window.worker`로 이용할 수 있다.
<br />
서비스 워커는 네트워크를 관리하는데 PWA를 가능케 하는 기술이다. 웹페이지에 PWA 기술을 도입하면 오프라인에서도 돌아가고 Push 알림도 보내고 모바일에서 설치할 수도 있다. <br />

<br /><br />

참고:<br />

[stackoverflow: Uncaught (in promise) TypeError: Request failed](https://stackoverflow.com/questions/49844781/uncaught-in-promise-typeerror-request-failed)
<br />

[MDN: 206 Partial Content](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/206)
<br />

[현실적 PWA](https://www.slideshare.net/netil/pwa-65378869),
<br />

[zerocho blog:웹 워커](https://www.zerocho.com/category/HTML&DOM/post/5a85672158a199001b42ed9c),
[zerocho blog:PWA 시작하기](https://www.zerocho.com/category/HTML&DOM/post/5a9a638033c01a001bfa6912)
