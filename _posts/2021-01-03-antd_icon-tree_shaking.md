---
title: antd-icon 용량 줄이기
author: juyoung
date: 2021-01-03 12:18:00 +0800
categories: [project, resolution]
tags: [project]
---

# 1. EC2 서버에서 JavaScript heap out of memory 문제 발생
  
  ![heap-error](/assets/img/heap-error.jpg)
AWS에 배포하는 과정에서 용량 때문에 heap이 터지는 문제가 발생했다.
 > FATAL ERROR: MarkCompactCollector: young object promotion failed Allocation failed - JavaScript heap out of memory

아무래도 front 용량이 크기때문인 것 같아 확인해보니 antd-icon에서 많은 용량을 차지하고 있었다.  

![antd-icon_over-chunk](/assets/img/antd-icon_over-chunk.jpg)

<br />
<br />
<br />

# 2. 해결방법   
<br />

## (1) antd-icon tree shaking  

[Github: ant-design issue](https://github.com/ant-design/ant-design/issues/12011)에 이에 대한 다양한 해결방법들이 소개되어 있었다.  
제일 먼저 webpack설정에 손을 대는 것을 시도했으나 달라지는 게 없어서...  
 결국 가장 간단한 방법을 선택해 다음과 같이 필요한 icon만 @ant-design/icons 파일에서 가져다 쓰는 방식을 택했다.
 ![antd_icon_solve](/assets/img/antd_icon_solve.jpg)  

front > saga > index.js
```javascript
import {
    default as HomeOutlined,
} from '@ant-design/icons/HomeOutlined';
import {
    default as LoginOutlined,
} from '@ant-design/icons/LoginOutlined';

 ...
```

 그랬더니 성공~!  

  ![antd_icon-solve_chunk](/assets/img/antd_icon-solve_chunk.jpg)  

그러나 여전히 heap은 터졌고..  
<br />
<br />

## (2) build 파일을 github에 올리기  
<br />

 빌드를 해보면 아래와 같은 정도의 용량이었는데,
 
```console
Page
     Size     First Load JS
┌ λ /
     2.94 kB        1.19 MB
├   /_app
     0 B             532 kB
├ ○ /404
     274 B           532 kB
├ λ /hashtag/[tag]
     808 B          1.18 MB
├ λ /login
     8.85 kB        1.17 MB
├ λ /post/[id]
     714 B          1.18 MB
├ λ /posts/related
     2.67 kB        1.18 MB
├ λ /profile
     7.78 kB        1.17 MB
├ λ /signup
     1.33 kB        1.17 MB
└ λ /user/[id]
     1.28 kB        1.18 MB
+ First Load JS shared by all
     532 kB
  ├ chunks/033f869c0cc364627d93bd7d05534baade1e7634.68b4d0.js  4.92 kB
  ├ chunks/6b7903cd2497917111f687055581f790035a2aa9.5aa3b1.js  15.6 kB
  ├ chunks/777c2cca.558465.js
     77 B
  ├ chunks/a48d0dfd1ba72b4ca1f15f44979b0d2398b47bf8.0786c6.js  428 kB
  ├ chunks/b6451bfa71415e1eb6b699247070fee6c4d97f38.fee3f6.js  11.5 kB
  ├ chunks/commons.7aac71.js
     8.79 kB
  ├ chunks/framework.9d2e16.js
     42.1 kB
  ├ chunks/main.5b6e30.js
     7.06 kB
  ├ chunks/pages/_app.e4826c.js
     13 kB
  ├ chunks/styles.f4dce8.js
     99 B
  ├ chunks/webpack.470e21.js
     752 B
  └ css/777c2cca.81a3d2c1.chunk.css
     65.7 kB

λ  (Server)  server-side renders at runtime (uses getInitialProps or getServerSideProps)
○  (Static)  automatically rendered as static HTML (uses no initial props)
●  (SSG)     automatically generated as static HTML + JSON (uses getStaticProps)        
   (ISR)     incremental static regeneration (uses revalidate in getStaticProps)        

[==  ] info  - Generating static pages (0/1)
```  
  
   [인프런](https://www.inflearn.com/questions/105490)에 질문을 했더니 zeroCho님이 ec2가 t2.micro라서 메모리 용량이 부족하기 때문이라고 build된 파일을 통째로 github에 올려 EC2 서버에서 내려받는 방법을 조언해주셨다.  
    멀고 먼 길을 돌아 드디어 EC2 front 서버가 돌아갔답니다~  




 
