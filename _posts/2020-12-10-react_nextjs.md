---
title: react와 nextjs를 쓰는 이유
author: juyoung
date: 2020-12-10 04:00:00 +0800
categories: [diary, project]
tags: [diary]
---

페이지가 index, error뿐인 포트폴리오를 만드는데 nextjs를 굳이 사용해야 할까?

데이터를 서버에서 받아오는 것도 아니고 화면만 렌더링하면 되는데 굳이 오랜 시간 걸리며 react와 nextjs로 포트폴리오를 만들어야 하는 이유가 뭘까?  
이런 질문이 포트폴리오는 만드는 내내 따라다닌다.

5일동안 react-full-page package를 적용해 스크롤링하는데 하루, animation 효과 적용하는데 하루, next.config.js로 css와 less 동시에 적용하는 방법 찾는데 하루... 이런 식으로 그냥 vanillajs로 구현하는 것보다 배는 시간을 잡아먹는데, 그리고 사실 간단한 js로만으로 페이지를 구현할 수 있는데 굳이 react를 써야하나, 또 next 프레임워크를 써야하나 싶어졌다.

그러다가 내가 클론코딩한 방식이 react+nextjs이기 때문에, 그래서 다른 방식으로는 어떻게 만들어야 할지 막막하기 때문에도 이유에 포함된다는 사실을 깨닫고 포트폴리오를 만드는데 react+nextjs를 사용하는 것은 오히려 비효율적인 일이 아닐까에 대해 고민하게 되었다.

그럼 왜 react를 쓰고, nextjs를 쓰는지에 대한 답을 찾고 싶었다.

### react를 쓰는 이유는

### 1. web에서 app과 같은 사용자 경험을 위해서다. 한마디로 UI(user interaction)을 향상시켜준다

### 2. 컴포넌트 시스템을 구현했기 때문에 매우 효율적으로 코딩할 수 있다. component를 재사용할 수 있고 유지보수가 쉬워진다.

### 3. 데이터를 받아올 때 업데이트 빈도가 높아도 그렇게 무리가 가지 않는데 DOM 조작이 브라우저에 엄청 무리를 주기 때문에 가상 DOM을 만들어 달라진 부분만 비교해 업데이트한다.

### nextjs를 쓰는 가장 큰 이유는 SSR때문이다.

<br />
사실 react는 데이터가 수시로 바뀌는 경우에 필요하고 그렇지 않다면 굳이 쓸 필요가 없다.  
component같이 개발할 때 편한 부분도 있으나 이를 위한 제약사항도 많기 때문이다. 나 같은 경우는 react와 nextjs를 더 익숙하게 다루고 싶어 portpolio를 만들 때 사용했으나 보여주기식 페이지를 만들 때는 jequery를 사용해도 충분할 것이다.

또한 아주 간단한 페이지를 구현하면서 nextjs가 주는 이런 고급스런 기능들을 굳이 사용하지 않는데 그렇다면 더 간단하게 react를 사용할 수 있는 방법을 찾다보니 전에 zeroCho님의 webgame강좌에서 react 문법들을 설명하던 것들을 보며 내가 기본 개념들- npm package, webpack 기본 설정, jsx, antd design library, prerendering 등등을 처음 강의를 들었을 때는 이해하지 못하고 넘어갔다는 것을, 이제 전반적인 프로세스를 알자 이런 것들이 어떤 의미를 가지는지가 다시 다가왔다.

5일동안 정말 중간에 쉬지않고 포트폴리오를 만들었더니 컴퓨터를 안하는 시간에는 머리가 아프고 눈 주위에 통증이 심해진다. 무엇보다 더이상은 하기 싫다는 생각이 드는 것이 가장 큰 문제다. 왜 이렇게까지 이 일이 하고 싶은지를 모르겠다는 의문이 들었다. 그 질문에 대한 답이 절실했다.

내 삶을 요약하면서 느낀 건 내가 무언가를 만들고자 하는 욕구가 있다는 것이다.
미야자키 하야오의 애니메이션들에서 보았던 비행기 설계하는 사람들, 언어의 정원에 나왔던 구두수선공의 삶을 떠올리면 가슴이 뛴다. 그런 생각을 하니 다시 힘이 난다. 그저 빠른 시간 안에 만들어야 할 귀찮은 포트폴리오가 내가 새로운 기술을 익히고 이미 알던 것을 다시 확인해보는 기회라는 생각을 할 수 있게 되었다.

에러를 통해 내가 이 개념을 놓치고 있었다는 것을 알게 된다. next에서는 window에 바로 접근할 수 없기때문에 useEffect안에 넣어주어야 한다는 해결방법을 이해하지 못하는 나를 보며 내가 next가 무엇을 지원하고 무엇을 지원하지 않는지 궁금해진 것이 한 예이다.

package하나 쓰는데도 하루 종일이 걸리면서 깨달은 것은 설명서에 나온 한 기능을 제대로 이해하려면 몇시간이고 뻘짓을 해야하는데 그게 설명서를 꼼꼼히 살피고 demo를 먼저 확인했다면 훨씬 빠르게 익힐수도 있었던 게 아닐까 싶더라는 것이다.  
급할수록 호흡에 집중하며 일부러 천천히 일을 진행한다는 정목스님처럼 기본부터 차근히 밟아나가야 결국 돌아가지 않는다는 것 배운다.
