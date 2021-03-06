---
title: scroll 속성 집고 넘어가기!
author: juyoung
date: 2020-11-23 22:33:00 +0800
categories: [javascript, syntax]
tags: [javascript]
---

프로젝트에서 react로 infinite scrolling을 구현할 때 document와 window의 scroll 값을 구하는 window properties인 scrollY와  
 Element properties인 clientHeight, scrollHeight의 차이가 헷갈려서 정리해본다.

```javascript
  useEffect(() => {
        function onScroll() {
            if (window.scrollY + document.documentElement.clientHeight > document.documentElement.scrollHeight - 300) {
                if (hasMorePosts && !loadPostsLoading) {
                    const lastId = mainPosts[mainPosts.length - 1]?.id;
                    dispatch({
                        type: LOAD_POSTS_REQUEST,
                        lastId,
                    });
                }
            }
        }
        window.addEventListener('scroll', onScroll);
        return () => {
            //쌓여있는 이벤트 메모리를 제거해주기
            window.removeEventListener('scroll', onScroll);
        };
    }, [mainPosts, hasMorePosts, loadPostsLoading]);
```
* 참고로 리엑트에서 window객체에 접근하려면 useEffect를 사용해야 한다.

# 1. window.scrollY: 움직인 스크롤 Y좌표값
- 얼마나 스크롤을 내렸는지를 알려주며, 스크롤되면서 현재는 사용자 눈에 보이지 않는 부분의 값까지 포함한다.  

# 2. document.documentElement.clientHeight: 엘리먼트의 내부 높이 
- 현재 브라우저창으로 보여지는 로딩된 문서 길이(스크롤바 영역을 제외하며, 브라우저창 크기에 따라 달라진다)  

![clientHeight](https://miro.medium.com/max/419/1*L-QquYrgfWfNNB8YP3K2eA.png)

# 3. document.documentElement.scrollHeight: 로딩된 문서 전체의 스크롤 길이  
  
  * clientHeight가 800이고, scrollHeight가 2800이라고 가정했을 때,  
  scrollY를 2000까지 내리면 로딩된 문서의 마지막을 보게 된다.  
    
      
 ![scrollHeight](https://miro.medium.com/max/333/1*IjO5mKXNyTO5moRHlj4m1A.png)
 
출처: [Jaisa Ram Patel
](https://medium.com/@jbbpatel94/difference-between-offsetheight-clientheight-and-scrollheight-cfea5c196937)