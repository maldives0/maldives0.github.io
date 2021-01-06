---
title: ES6 - 모듈 시스템
author: juyoung
date: 2021-01-04 20:55:00 +0800
categories: [javascript, ES6]
tags: [javascript]
---

# <font color=skyblue>ES6 모듈 시스템</font> 
해당 웹페이지에 필요한 리소스 파일들을 관리하고, 현재 파일이 어떤 패키지를 필요로 하는지 보여주는 의존성을 관리하기 위해 도입되었다.

export 방식의 차이에 따라 다르게 import해주어야 한다.


> file1.js

    const apple = 'red';
    export { apple };// {}로 감싸서 import하기

    const banana = 'yellow';
    export const coconut = 'white';// {}로 감싸서 import하기

    export default banana;// 변수명을 바꿔서 import할 수 있음

   
> file2.js

    import donut, { apple, coconut as eat } from './file1';
    console.log(apple, donut, eat); // red, yellow, white

또는

    import * as food from './file1';
    console.log(food); // { apple: 'red', coconut: 'white', default: 'yellow' }


* 모듈은 아직 대다수의 브라우저에서 지원하지 않기 때문에 BabelJS 같은 컴파일러를 사용해서 ES5 스타일로 바꿔줘야 한다.

출처:
[zerocho blog: HTML&DOM](https://www.zerocho.com/category/ECMAScript/)

