---
title: 시맨틱(Semantic)
author: juyoung
date: 2020-10-10 09:00:00 +0800
categories: [html, concept]
tags: [html]
---

# 1. 웹표준

### <font color=skyblue>웹 표준(Web Standards) 이란</font>

'웹에서 표준적으로 사용되는 기술이나 규칙'으로 표준화 단체인 W3C가 권고한 표준안에 따라 웹사이트를 작성하는 것이다. 어떤 운영체제나 브라우저를 사용하더라도 웹페이지가 동일하게 보이고 정상 작동해야함을 의미한다.
웹 표준의 기술로는 XHTML (eXtensible Hypertext Markup Language), CSS(Cascading Style Sheets), XML(eXtensible Markup Language), DOM(Document Object Model), ECMAScript 등이 있다. W3C에서 제공하는 [W3C Markup Validation](http://validator.kldp.org/)와 [W3C CSS Validation](https://jigsaw.w3.org/css-validator/)을 통해 자신이 만든 사이트가 웹표준에 맞는지 자가진단을 해볼 수 있다.
내가 만든 프로젝트 주소를 W3C Markup Validation에 입력해 검사해봤더니 수정이 필요한 부분은 아래와 같이 어떤 부분이 문제인지 친절하게 보여준다.
![W3C Markup Validation](/assets/img/w3c_markup_check.jpg)<br />
<br /><br />

# 2. 웹접근성

### <font color=skyblue>웹 접근성(Web Accessibility) 이란</font>

장애인, 고령자 등이 웹 사이트에서 제공하는 정보에 비장애인과 동등하게 접근하고 이해할 수 있도록 보장하는 것이다. 특정 대상에 한정되지 않고 모든 사용자가 웹 사이트에서 제공하는 모든 정보에 접근할 수 있도록 보장하는 것이다. 장애인차별금지법이 생기면서 2013년 4월 11일이후 모든 공공기관과 법인의 웹사이트에서 웹 접근성 준수가 의무화됐다.
웹접근성을 지키기 위해서는 페이지의 확대/축소 기능이나 고대비 보기를 제공하고, 스크린 리더와 같은 대체 소프트웨어를 이용해 사이트를 읽어 왔을때 사이트의 콘텐츠가 순서대로 읽힐 수 있도록 마크업을 하고, 이미지가 제공될 경우 alt="대체텍스트"를 통해 이미지가 어떤 의미로 사용 되었는지를 알 수 있도록 대체텍스트 제공하는 등이 있다.
(프론트엔드 면접 때 웹접근성 기준 문서를 읽어봤는지에 대한 질문이 있어 찾아보다 알게 된 사이트인데) [웹접근성 연구소 개발자 아카이브](https://www.wah.or.kr:444/Participation/technique.asp)에 웹접근성을 지키기위한 지침을 예시와 함께 확인할 수 있다.
참고로 [웹 접근성 진단도구](https://nuli.navercorp.com/education/tools)에 그밖의 다양한 진단 사이트가 소개되어 있다. 크롬사용자라면 확장프로그램인 OpenWAX를 통해 자체 진단을 할 수 있다.
<br /><br />

# 3. 시맨틱 마크업

### <font color=skyblue>시맨틱(Semantic) 이란</font>

시맨틱(Semantic)은 "의미론적인"이란 뜻을 가진 단어이다.
즉 의미있는 태그를 제공함으로서 해당 웹페이지가 전체적으로 어떤 구조를 가지고, 최상위 제목이 무엇이며, 그 안에 컨텐츠가 어떻게 구성되어 있는지를 개발자와 브라우저에게 알려준다.
간단한 예로 '<div>'는 non-semantic 태그라 볼 수 있고 '<table>, <header>, <footer>' 등은 semantic 태그라고 볼 수 있다.

<br />
 
## 웹 표준과 시멘틱 마크업 작성의 장점은, 
 1. 앞서 설명했듯이 스크린리더기,  휴대폰 PDA, 장애인 지원용 프로그램 등 보조공학 기기로 사이트를 읽어 왔을 때 접근성이 좋아진다.
 2. SEO(Search Engine Optimization) : 검색 엔진이 웹사이트를 크롤링할 때는 웹페이지가 담고 있는 데이터에 주목한다. 하지만, 모든 컨텐트가 <div> 태그로만 마크업이 되어 있다면, 검색 엔진이 효과적으로 해당 웹페이지를 분석하기 어렵다. 따라서 검색 엔진 최적화, 즉 SEO 측면에서 시멘틱 태그를 적지적소에 사용하는 것은 매우 중요한 부분이다. 
 3. 코드 가독성이 좋아지고 소스의 통일화로 코드와 데이터의 재사용성이 높아진다. 또한 논리적이고 효율적으로 작성된 웹 문서는 코드의 양이 줄어 파일 크기가 줄고 서버부담의 감소로 이어질 수 있다. 웹표준을 지키면 CSS와 HTML이 분리되어 유지보수에 들어가는 시간이 단축되고, 불필요한 마크업이 최소화되어 페이지 로딩속도가 향상된다.
 4. 오래된 브라우저에서도 컨텐츠가 적절하게 표시되면서 크로싱브라우징 시 호환성과 운용성이 확보된다. 
<br /><br />

출처:  
[[웹표준]HTML5 시맨틱(Semantic) 마크업](https://yeoninim.tistory.com/20), <br />
[웹표준의 이해(웹 표준이란?)](https://goddaehee.tistory.com/244),<br />
[[HTML] 시멘틱 마크업](https://www.daleseo.com/html-semantic-markup/)
