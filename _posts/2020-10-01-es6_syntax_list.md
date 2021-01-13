---
title: ES6문법 개요와 Babel
author: juyoung
date: 2020-10-01 20:55:00 +0800
categories: [javascript, ES6]
tags: [javascript]
---

# <font color=skyblue>ES6란, </font> 
ES6는 EcmaScript6의 준말로 뒷의 숫자는 자바스크립트의 버전을 뜻한다. ECMA 라는 단체에서 기존의 결점을 보완한 표준 자바스크립트 버전을 매년 발표한다.  ES2015(ES6)는 기존 자바스크립트(ES5)에 비해 많은 부분이 달라졌는데 대표적으로 다음과 같다.
  

1. const(constant, 상수)와 let
2. Object(객체) 코드량
3. 화살표 함수 
4. default, rest, spread
5. 템플릿 문자열, Tagged Template Literal
6. class
7. 구조 분해 할당(destructuring)
8. for ~ of 문
9. Map, Set, WeakMap, WeakSet
10. Promise
11. 반복기(iterator), 생성기(generator)
12. 모듈 시스템
  
 
<br />

# <font color=skyblue>BabelJS</font> 
ES6 문법은 에버그린 브라우저만 지원하는 기능이기 때문에 구형 브라우저(대표적으로 IE)와는 호환이 안 된다는 단점이 있다. 이를 해결하기 위해 ES6 문법을 사용한 코드를 예전 ES5 자바스크립트 코드로 바꿔주는 도구들이 있는데 그중에서 가장 유명한 도구가 바벨이다.  
바벨 그 자체로는 ES6의 새로운 객체(Promise, Map, Set 등등)과 메소드(Array.find, Object.assign 등등)을 사용할 수 없다. 왜냐하면 구형 자바스크립트에는 그에 상응하는 코드가 없기 때문이다. 그래서 babel-polyfill을 설치해야 새로운 기능을 사용할 수 있다.<br /><br />
출처:
[zeroCho blog :ECMAScript](https://www.zerocho.com/category/ECMAScript/)
[zeroCho blog :BabelJS(바벨)](https://www.zerocho.com/category/ECMAScript/post/57a830cfa1d6971500059d5a)
[모던 javascript tutorials](https://ko.javascript.info/advanced-functions)