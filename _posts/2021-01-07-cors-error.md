---
title: cors 에러 해결하기
author: juyoung
date: 2021-01-07 11:58:00 +0800
categories: [project, error]
tags: [project]
---

# 1. CORS error

 CORS error는 브라우저에서 다른 도메인(서버)으로 요청을 보낼 때 이것이 브라우저 보안에 위협이 될 수 있다고 판단한 브라우저가 브라우저 보안정책에 따라 해당 요청이 보안에 위협이 될 수 있다고 보내는 에러이다.
 백엔드와 프론트엔드의 도메인 주소가 다른 경우 이런 문제가 발생할 수 있다.

![cors-error](/assets/img/cors-error.jpg)



## 여기서 포인트는 <font color="orange">'이를 해결하려면 요청을 보낸 브라우저가 아닌 요청을 받는 back서버 쪽에서 'Access-Control-Allow-Origin' header를 설정해주어야 한다'</font>는 점이다.

# 2. 해결방법   

 우선 front 서버에서 back 서버로 axios 요청을 보낼 때 쿠키가 전송되는 것을 허락하는 `withCredentials = true` 설정을 함께 보내주어야 한다.  
  아래의 예는 axios 전역 설정으로 모든 axios 요청을 보낼 때 이 설정이 적용된다.

front > saga > index.js
```javascript
import axios from 'axios';

axios.defaults.baseURL = backURL;
axios.defaults.withCredentials = true;
```

 back서버에서는 아래 예시의 경우 nodejs를 서버로 사용했는데 응답헤더에  `Access-Control-Allow-Credentials = true`, `Access-Control-Allow-Origin= (응답을 보낼 front url)`를 설정해주면 된다.

back > app.js
```javascript
res.writeHead(200, { 'Access-Control-Allow-Origin': 'frontURL' });
res.writeHead(200, { 'Access-Control-Allow-Credentials': 'true' });

또는

res.setHeader('Access-Control-Allow-Origin', 'frontURL');
res.setHeader('Access-Control-Allow-Credentials', 'true'); 

```
 
  나는 프로젝트를 할 때 back서버에서 cors라는 미들웨어를 사용해 다음과 같이 `origin:` 설정에 요청에 대한 응답을 보낼 url주소를 적어주었다. `origin:*`로 설정하면 모든 도메인을 허용한다. 이는 개발모드에서 사용하고 배포할 때는 해당 front url주소를 정확히 명시해주도록 한다.   `origin:true`로 설정하면 해당 front url주소를 자동으로 넣어준다. 

back > app.js
```javascript
const express = require('express');
const cors = require('cors');
const app = express();
    app.use(cors({
        origin: frontURL,
        credentials: true,
    }))
```

<br />
<br />
<br />

참고:  
[MDN: CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)  
[MDN: Credentials](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/withCredentials)  
[zerocho blog: Access-Control-Allow-Origin(CORS)](https://www.zerocho.com/category/NodeJS/post/5a6c347382ee09001b91fb6a)