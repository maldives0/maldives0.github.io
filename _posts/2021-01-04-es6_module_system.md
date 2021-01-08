---
title: Node 모듈 시스템과 ES6 모듈 시스템
author: juyoung
date: 2021-01-04 20:55:00 +0800
categories: [javascript, ES6]
tags: [javascript]
---

# <font color=blue>모듈 시스템</font> 
코딩을 하다보면 한 파일의 길이가 길어질 수 있는데 이를 여러 파일로 쪼개서 관리하면 가독성도 높이고 재사용성도 얻을 수 있다. 모듈 시스템은 해당 웹페이지에 필요한 리소스 파일들을 관리하고, 현재 파일이 어떤 패키지를 필요로 하는지 보여주는 의존성을 관리하기 위해 도입되었다. 

모듈은 export 방식의 차이에 따라 다르게 import해주어야 한다.

## (1) Node 모듈 시스템
Node 모듈 시스템은 commonjs방식을 쓴다.  

file1.js
```javascript
exports.apple = 'red';
exports.banana = true;

또는

module.exports = {
    apple: 'red',
    banana: true,
};
```

file2.js
```javascript
 const {apple, banana} = require('./file1');

또는 

import * as fruit from './file1';//commonjs를 es6문법을 사용해 가져올 때
```

`module.exports`는 한 파일에 한번만 써야된다. `module.exports`가 위의 `exports.apple`, `exports.banana`를 덮어씌워버리므로 둘 중 하나만 쓰도록 해야한다.
es6에서는 이러한 문제를 해결하기위해 `default`라는 개념을 도입한다. commonjs가 정적(static)타입이라면 es6는 동적(dynamic)타입인 점에서 차이가 난다.  
  
  위의 예시를 es6 모듈 시스템으로 표현하면 다음과 같다.  

file1.js
```javascript
const apple = 'red';
const banana = true;

export { apple };
export { banana };

또는 

export const apple = 'red';
export const banana = true;

export default function(){
    //fruit
};
```  

file2.js
```javascript
 import fruit, { apple ,banana } from ('./file1');

```

* 정적타입은 자료형을 컴파일 시에 결정하는 것이고, 변수에 들어갈 값의 형태에 따라 자료형을 지정해주어야 한다.(예: C, C#, C++, Java, nodejs, typescript 등) 
> let a:string = 'a';
  

    동적타입은 컴파일 시 자료형을 정하는 것이 아니고 실행 시에 결정한다. 타입 없이 변수만 선언하여 값을 지정할 수 있다. (예: JavaScript, Ruby, Python 등)
    > let a = 'a'


## (2) ES6 모듈 시스템


export default로 내보낸 경우는 변수명을 바꿔서 import할 수 있고, 그 밖에 export로 보낸 것들은 {}로 감싸서 import해야한다.  

file1.js
```javascript
    const apple = 'red';
    export { apple };// {}로 감싸서 import하기

    const banana = 'yellow';
    export const coconut = 'white';// {}로 감싸서 import하기

    export default banana;//변수명을 바꿔서 import할 수 있음
```
   
file2.js
```javascript
    import donut, { apple, coconut as eat } from './file1';
    console.log(apple, donut, eat); // red, yellow, white

또는

    import * as food from './file1';
    console.log(food); // { apple: 'red', coconut: 'white', default: 'yellow' }
```

* 모듈은 아직 대다수의 브라우저에서 지원하지 않기 때문에 BabelJS 같은 컴파일러를 사용해서 ES5 스타일로 바꿔줘야 한다.

출처:
[zerocho blog: ES2015(ES6) 모듈 시스템](https://www.zerocho.com/category/ECMAScript/post/579dca4054bae71500727ab9)  

[zerocho blog: Node 모듈 시스템](https://www.zerocho.com/category/NodeJS/post/5835b500373b5b0018a81a10)  

[모던 JavaScript 튜토리얼: 모듈](https://ko.javascript.info/modules-intro)

[정적언어(타입)과 동적언어(타입)](https://itmining.tistory.com/65)