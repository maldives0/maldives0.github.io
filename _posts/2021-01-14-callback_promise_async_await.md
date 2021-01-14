---
title: callback과 promise 그리고 async/await
author: juyoung
date: 2021-01-14 12:46:00 +0800
categories: [html, concept]
tags: [html]
---

# 1. callback 함수

<br />
콜백 함수는 나중에 호출할 함수를 의미하며 비동기 동작을 처리하기 위해 쓰인다.

<br />

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  script.onerroror = () =>
    callback(new Erroror(`${src}를 불러오는 도중에 에러가 발생했습니다.`));
  document.body.append(script);
}

loadScript(
  'https://dapi.kakao.com/v2/maps/sdk.js?appkey=(API-KEY입력하기)',
  (script) => {
    console.log(`${script.src} load`);
    let script2 = document.createElement('script');
    script.src = 'map.js';
    document.body.append(script2);
  },
);
```

꼬리에 꼬리를 무는 비동기 동작이 많아지면 소위 ‘콜백 지옥(callback hell)’ 혹은 '멸망의 피라미드(pyramid of doom)'라고 불리는 중첩 문제가 발생한다.
<br />
<br />

# 2. Promise 객체

<br />
callback의 중첩 문제를 해결하기위해 나온 것이 Promise이다.

```javascript
let promise = new Promise(function (resolve, reject) {
   try {
    ...비동기 작업
    resolve(value);//value가 then의 매개변수 result로 전달된다
  } catch (err) {
    reject(err);//err가 catch의 매개변수 error로 전달된다
  }
});
```

new Promise에 전달되는 함수는 `executor(실행자, 실행 함수)` 라고 부른다. executor는 new Promise가 만들어질 때 자동으로 실행되는 executor의 인수 `resolve`와 `reject`는 자바스크립트가 자체적으로 제공하는 콜백이다. executor는 resolve나 reject 중 하나를 반드시 호출해야 한다.

```javascript
promise.then(
  function (result) {
    /* 결과(result)를 다룹니다 */
  },
  function (error) {
    /* 에러(erroror)를 다룹니다 */
  },
);

promise.catch(error);
```

`.then`은 프라미스에서 가장 중요하고 기본이 되는 메서드이다. Promise의 결과값을 받아온다. 에러가 발생한 경우만 다루고 싶다면 `.then(null, function)`같이 null을 첫 번째 인수로 전달하거나 `.catch()` 메서드를 사용한다. `.catch(function)`는 `promise.then(null, function)`과 동일하게 작동한다.

```javascript
function loadScriptPromise(src) {
  return new Promise((resolve, reject) => {
    let script = document.createElement('script');
    script.src = src;
    script.onload = () => resolve(script);
    script.onerror = () =>
      reject(new Error(`${src}를 불러오는 도중에 에러가 발생했습니다.`));
    document.body.append(script);
  });
}

loadScriptPromise(
  'https://dapi.kakao.com/v2/maps/sdk.js?appkey=(API-KEY입력하기)',
)
  .catch((error) => console.log(`sdk load 중 ${error}가 발생하였습니다!`))
  .then(() => loadScriptPromise('map.js'))
  .catch((error) => console.log(`map.js를 load 중 ${error}가 발생하였습니다!`))
  .then(() => map())
  .catch((error) =>
    console.log(`map.js의 function map을 실행 중 ${error}가 발생하였습니다!`),
  );
```

.then 핸들러를 원하는 만큼 사용하다 마지막에 .catch 하나만 붙이면, .then 핸들러에서 발생한 모든 에러를 처리할 수 있다. 어디서 에러가 발생했는지 확인하고 싶다면 각각의 .then 핸들러 바로 뒤나 return하는 Promise 객체에 .catch를 붙인다.
<br />
<br />

# 3. async/await 문법

function 앞에 `async`를 붙이면 해당 함수는 반드시 프라미스를 반환하고, 프라미스가 아닌 것은 프라미스로 감싸 반환한다. 자바스크립트는 `await` 키워드를 만나면 프라미스가 처리(settled)될 때까지 기다린다. 결과는 그 이후 반환된다. <br />

```javascript
let loadScriptPromise = (src) => {
  return new Promise((resolve, reject) => {
    let script = document.createElement('script');
    script.src = src;
    script.onload = () => resolve(script);
    script.onerror = () =>
      reject(new Error(`${src}를 불러오는 도중에 에러가 발생했습니다.`));
    document.body.append(script);
  });
};

async function showMap(src) {
  try {
    await loadScriptPromise(src);
    return loadScriptPromise('map.js');
  } catch (error) {
    console.log(`${error}가 발생하였습니다!`);
  }
}
showMap('https://dapi.kakao.com/v2/maps/sdk.js?appkey=(API-KEY입력하기)');
```

await는 promise.then보다 좀 더 세련되게 프라미스의 result 값을 얻을 수 있도록 해주는 문법이다. promise.then보다 가독성 좋고 쓰기도 쉽다. await가 던진 에러는 throw가 던진 에러를 잡을 때처럼 `try..catch`문법을 사용해 잡을 수 있다.
<br /><br />

참고:<br />

[ko.javascript :프라미스와 async, await](https://ko.javascript.info/async)
