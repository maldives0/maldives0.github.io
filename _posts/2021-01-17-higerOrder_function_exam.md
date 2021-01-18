---
title: 고차함수 응용하기, add(2, 3)와 add(2)(3)
author: juyoung
date: 2021-01-17 16:11:00 +0800
categories: [project, resolution]
tags: [project]
---

add(2, 3)와 add(2)(3)값이 모두 5가 나올 수 있는 함수를 만드는 방법이다.

고차함수를 이용하면 이 문제를 해결할 수 있다.

```javascript
function add(a) {
  return function (b) {
    return a + b;
  };
}
또는;

const add = (a) => (b) => a + b;

add(2)(3); // 5
```

```javascript
function add(a, b) {
  const isB = function (b) {
    return a + b;
  };

  if (typeof b == 'undefined') {
    return isB;
  } else {
    return isB(b);
  }
}

// console.log(add(2, 5));
console.log(add(2)(5));
```

<br /><br />

참고:<br />

[stackoverflow:How can I make var a = add(2)(3); //5 work?](https://stackoverflow.com/questions/2272902/how-can-i-make-var-a-add23-5-work)
