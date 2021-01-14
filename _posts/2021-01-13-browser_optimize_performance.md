---
title: 브라우저 작동원리와 성능 최적화
author: juyoung
date: 2021-01-13 12:46:00 +0800
categories: [html, concept]
tags: [html]
---

[프런트엔드 성능 최적화](https://www.youtube.com/watch?v=G1IWq2blu8c)라는 동영상을 보며 브라우저에서 로딩과 렌더링 최적화가 얼마나 중요한지에 대한 설명을 듣던 중 DOMContentLoaded, virtual DOM, 런타임과 빌드타임, SSR 등등 실제 프로젝트를 하며 많이 들었던 개념들이 나왔다. 이런 개념들이 아직 애매하게 다가와 동영상의 내용을 정리하며 다시 집어보기로 한다.
<br />
<br />
우선 브라우저의 동작원리를 간단하게 설명하자면 브라우저의 렌더링 엔진은 다음의 과정을 걸쳐 화면에 사용자가 요청한 정보를 그리게 된다.

1.  HTML 파싱 후 DOM 트리(DOM Tree) 만들기 : HTML의 테그를 브라우저가 읽은 수 있는 DOM 트리로 변환하는 과정으로 웹 상에 나타날 내용을 만든다.
2.  렌더 트리(Render Tree) 만들기 : CSS/Style 데이터를 파싱하고 그 스타일 데이터들로 렌더 트리(Render Tree)를 만드는 과정으로 이때 브라우저 상에 내용이 어떻게 나타날지 시각적 요소가 결정된다.
3.  렌더 트리(Render Tree) 레이아웃 만들기: 각 노드들에게 스크린의 어느 공간에 위치해야 할지 각각의 값(Positionm, Size)을 부여하는 과정이다.
4.  렌더 트리 페인팅 (Renter Tree Painting) : 앞선 과정에서 만들어진 레이아웃에 맞게 브라우저 화면에 그려준다.

![webkit main flow](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcEgC5C%2FbtqCoQ0Mmec%2F7CKjkqxPgtLFTwl6wIKTN1%2Fimg.png)  
<br />

# 1. 로딩 최적화

<br />

## - 브라우저 기준의 로딩 최적화<br />

HTML의 테그를 바꾸는데 영향을 줄 수 있는 css와 js로 인해 이 두 파일이 html의 parsing을 막는 블록 리소스가 발생되면서 `DOMContentLoaded` 시간이 길어진다. 로딩 최적화를 위해 `DOMContentLoaded` 시간을 앞당길 수 있으려면 js와 css 로드 시점을 앞당겨야 한다. <br />
![block parsing](https://image.slidesharecdn.com/webapp03-190201074444/95/2018-16-638.jpg?cb=1549007284)
<br />

js의 경우 `script` 테그를 `</body>`테그 앞에 위치시키거나 `<script async>`와 같이 async를 넣어 동기처리를 한다.<br />
![js load](https://image.slidesharecdn.com/webapp03-190201074444/95/2018-20-638.jpg?cb=1549007284)
<br />

css의 경우 외부에서 `<link>`로 파일을 불러오는 것보다 html에 `<style>`테그로 직접 인라인 스타일을 넣어준다.<br />
![css load](https://image.slidesharecdn.com/webapp03-190201074444/95/2018-24-638.jpg?cb=1549007284)<br />

## - 사용자 기준의 로딩 최적화<br />

로딩창을 일반 사이트보다 더 빠르게 보여주며 사용자와의 interaction을 높여주는 react, vue, angular와 같은 SPA는 우선 프론트서버로부터 html과 css를 받아와 로딩창을 먼저 그리고 사용자가 다른 페이지를 불러오면 그 후에 이에 필요한 데이터를 서버에 요청해 가져온다. 그러나 이는 로딩 프로세스바를 화면에 노출시키는 시간은 단축시키나 사용자에게 의미있는 콘텐츠(FMP, First Meaningful Painting)가 그려지는 시간을 줄여주지는 않는다. 그래서 필요한 것이 SSR이다. <br />  
 SSR에도 SSR과 prerendering이라는 두가지 방식이 존재한다. SSR 방식은 첫화면을 그리는 시점부터 서버에서 데이터를 모두 가져와 준비를 해놓고 사용자가 요청하는 페이지를 데이터를 그려준다. 이는 검색엔진에서 순위를 올리기위한 SEO(검색 엔진 최적화)를 위해 필요하다. preredering 방식은 SEO가 사이트를 방문하면 SSR방식으로 가져오고, 일반 사용자라면 SPA방식으로 데이터를 가져온다. SSR이 런타임에 실행된다면 preredering은 build(또는 compile)하는 시점에 실행된다. 이 preredering 방식을 이용해 사용자 기준의 로딩 최적화를 이룰 수 있는데 웹팩 prerender-loader를 사용해 html과 css를 더 빠르게 불러올 수 있다.
<br />

# 2. 렌더링 최적화

<br />

## - 레이아웃 스레싱 제거<br />

js에서 DOM과 css를 조작하면 DOM의 일부 element의 프로퍼티(예를 들어 `element.offsetLeft`, `element.offsetTop`, `element.offsetWidth` 등)는 읽기만 해도 레이아웃이 매번 새로 그려지는 강제 동기 레이아웃이 발생하는데, 강제 동기 레이아웃이 빈번하게 발생하는 것을 레이아웃 스레싱이라 한다. 그래서 강제 동기 레이아웃을 발생시킬 수 있는 코드는 한 번 쓰고 캐싱을 할 필요가 있다.
<br />

## - Virtual DOM 사용<br />

실제 DOM의 데이터를 바꾸기 전에 가상 DOM의 데이터를 바꿔주고 후에 이를 실제 DOM에 붙여주며 실제 DOM을 조작할 때 걸리는 rendering시간을 줄인다. [Virtual DOM에 대해](https://medium.com/sjk5766/virtual-dom%EC%97%90-%EB%8C%80%ED%95%B4-7222d752ee65)라는 글에 그 작동 원리가 잘 설명되어 있다.
<br />

## - 웹 워커 사용 <br />

웹펙의 worker-loader 설정으로 렌더링 시간을 단축할 수 있다.
![worker-loader](https://image.slidesharecdn.com/webapp03-190201074444/95/2018-64-638.jpg?cb=1549007284)
<br />
이렇게 설정하고 사용될 함수를 작성한 파일 이름을 `job.worker.js`와 같이 worker를 붙여만든 뒤

![job.worker.js](https://image.slidesharecdn.com/webapp03-190201074444/95/2018-65-638.jpg?cb=1549007284)
<br />
이 함수를 쓸 파일에서 import를 해준면 된다.

![import file](https://image.slidesharecdn.com/webapp03-190201074444/95/2018-66-638.jpg?cb=1549007284)
<br />

무엇보다 브라우저가 실제 어떤 원리로 동작하는지를 알아야 브라우저의 성능을 최적화하는데 중요하다는 점을 알게 되었다.<br /><br />

참고:<br />
[브라우저의 작동원리](https://it-ist.tistory.com/110)<br />
[[2018] 프런트엔드 성능 최적화 교육자료](https://www.slideshare.net/NHNFORWARD/2018-130108045) <br />
