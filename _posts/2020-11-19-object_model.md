---
title: javascript - Object Model
author: juyoung
date: 2020-11-19 16:08:00 +0800
categories: [javascript, syntax]
tags: [javascript]
---


![node객체](https://ko.javascript.info/article/browser-environment/windowObjects.svg)

# window : 브라우저의 요소들과 자바스크립트 엔진, 그리고 모든 변수를 담고 있는 객체  

 * 브라우저 전체를 담당하는 게 Window 객체이고, 웹사이트만 담당하는게 Document 객체  
 Window 객체가 창을 의미한다면 Document 객체는 윈도우에 로드된 문서
 (ex: chrome browser가 window객체,  
  document는 보여지는 각각의 탭-'네이버 지식인 탭', 'react api reference 탭' 등등)  
<br />  

# BOM(Browser Object Model): document는 뺀 나머지 브라우저에 대한 정보를 가지고 있는 객체  

* navigator : 브라우저나, 운영체제(OS) 대한 정보가 있는 객체
* screen : 화면에 대한 정보를 가지고 있는 객체
* location : 주소에 대한 정보를 가지고 있는 객체
* history : 페이지에 대한 정보를 가지고 있는 객체
<br />  

# DOM(Document Object Model): 웹페이지를 자바스크립트로 제어하기 위한 객체 모델

> document는 html에 관한 것들을 담당하는 객체이니만큼 대부분의 것들이 태그를 선택하고 조작하는 데 사용됩니다.
<br />


  ![node객체](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/904/2234.png)  
  

# Node객체 : 엘리먼트들의 관계를 보여주는 객체 

* 모든 DOM 객체는 Node 객체를 상속 받는다.
* 각각의 Node가 다른 Node와 연결된 정보를 보여주는 API를 통해서 문서를 프로그래밍적으로 탐색할 수 있다.

> Node는 태그 노드와 텍스트 노드 전체를 가리키고, Element는 텍스트 노드를 제외하고, 흔히 생각하는 태그만 가리킵니다

```
Node.childNodes
Node.firstChild
Node.lastChild
Node.nextSibling
Node.previousSibling

Element.classList
Element.className
Element.id
Element.tagName
```

<br />
<br /> 

- - -

불과 2개월 전까지는 봐도 이걸 왜 알아야하는지 몰랐는데 React공부를 하면서 이런 기초 지식이 없어 공식문서를 이해하지 못하자 찾아보게 됐다.
<br />

참고:
모던 javascript tutorials : <https://ko.javascript.info/browser-environment>

생활코딩 : <https://opentutorials.org/course/1375/6619>

zeroCho blog : <https://www.zerocho.com/category/JavaScript/>