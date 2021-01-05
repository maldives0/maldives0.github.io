---
title: localStorage와 sessionStorage 그리고 cookie
author: juyoung
date: 2021-01-05 13:43:00 +0800
categories: [html, concept]
tags: [html]
---

# 1. cookie

* 서버와 클라이언트 간의 지속적인 데이터 교환을 위해 만들어졌다.
* 쿠키는 브라우저가 서버로 HTTP 요청을 보낼 때 서버에 누가 요청을 보냈는지를 알려준다. 그래서 서버가 브라우저에서 보낸 요청에 대한 응답을 다시 보낼 곳을 알려준다.
* 4kb 용량 제한을 가진다. 쿠키가 용량이 크면 매번 왔다갔다 하므로 서버 트래픽을 많이 낭비될 수 있다.
  

# 2. localStorage와 sessionStorage

* 브라우저에서 쿠키를 사용하는 것보다 훨씬 직관적으로 key/value 데이터를 안전하게 저장할 수 있는 메커니즘으로 이 두 저장소의 데이터는 쿠키와 다르게 서버로 자동 전송되지 않는다. 
* 두 저장소 모두 window 객체 안에 들어 있으며 Storage 객체를 상속받는다.
>  window.localStorage.setItem(key, value)  

  key와 value값으로는 문자열, 불린, 숫자, null, undefined 등을 저장할 수 있지만, 모두 문자열로 변환된다.
* 브라우저별로, 기기별로 다르긴 하지만 모바일은 2.5mb, 데스크탑은 5mb~10mb의 용량을 갖는다.
* 두 저장소의 차이점은 데이터의 영구성에 있다.
 예를 들어 localStorage는 로컬 스토리지의 데이터는 사용자가 지우지 않는 이상 계속 브라우저에 남아 있다. 
 세션 스토리지의 데이터는 윈도우나 브라우저 탭을 닫을 경우 제거된다.
그래서 자동 로그인 정보는 localStorage에 저장하고, 일회성 로그인 정보는 sessionStorage에 저장한다.





출처:  
[zerocho blog: HTML&DOM](https://www.zerocho.com/category/HTML&DOM)  

[MDN: Web_Storage_API](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)