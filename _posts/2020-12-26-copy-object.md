---
title: javascript - 객체의 복사(copy object) 
author: juyoung
date: 2020-12-26 20:40:00 +0800
categories: [javascript, syntax]
tags: [javascript]
---
  
  * 이 글은 [zerocho blog: 객체의 복사 ](https://www.zerocho.com/category/JavaScript/post/5750d384b73ae5152792188d)의 내용을 정리하며 복습하기위해 작성되었습니다.

# 1. 문자열, 숫자, 불린의 경우 값이 바로 복사된다.

```javascript
let string = 'string';
let copy = string;
console.log(copy); // 'string'
copy = 'str';
console.log(string); // 'string'
```  
copy를 바꾼다고 string까지 바뀌지 않는다.
<br />
<br />

# 2. 배열, 일반 객체, 함수는 참조(reference)를 복사한다.

```javascript
let array = ['arr', 'obj', 'func'];
let ref = array;
ref[0] = 'dir';
console.log(array); // ['dir', 'obj', 'func']
```
ref의 값을 바꿨는데 array의 값까지 변한다. 왜냐하면 ref와 array가 공통으로 `['arr', 'obj', 'func']`라는 메모리의 주소를 가리키기 때문이다.

> 문자열, 숫자, 불린을 제외한 객체는 다른 변수에 대입할 때 값을 복사하는 게 아니라 참조(메모리의 주소)를 복사한다.
<br />
<br />

# 3. 복사(copy)하기  
<br />

이를 방지하려면 `Array.prototype.slice.call(array)`로 메모리의 주소를 하나 더 만들어준다.  

```javascript
let array = ['arr', 'obj', 'func'];
let copy = Array.prototype.slice.call(array);
copy[0] = 'dir';
console.log(array); // ['arr', 'obj', 'func']
```
<br />
<br />

## (1) 얕은 복사(shallow copy) : 가장 상위 객체만 새로 생성되고 내부 객체들은 참조 관계인 경우

<br />

```javascript
let array = [{ name: 'arr' }, { name: 'obj' }, { name: 'func' }];
let shallow = Array.prototype.slice.call(array);
shallow[0].name = 'dir';
shallow[1] = 'else';
console.log(array); // [{ name: 'dir' }, { name: 'obj' }, { name: 'func' }]
``` 
array의 내부객체들은 참조관계이기 때문에 `shallow[0].name`의 값은 변하지만, 상위객체인 `shallow[1]`은 변하지 않는다.
<br />


## (2) 깊은 복사(deep copy) : 내부 객체까지 모두 새로 생성된 경우

<br />

### - 일반 객체를 깊은 복사하기
  
  상속이 없는 일반 객체만 해당되기 때문에 `hasOwnProperty` 메소드를 사용한다. 아래는 obj2가 obj의 값을 변경시키지 못하도록 깊은 복사를 한 예이다.

```javascript

function copyObj(obj) {
    let copy = {};
    if (Array.isArray(obj)) {
        //obj = [ { d: null, e: 'f' } ];
        copy = obj.slice().map((v) => {
            //v = { d: null, e: 'f' }
            return copyObj(v);
        });
    }
    else if (typeof obj === 'object' && obj !== null) {
        for (let attr in obj) {
            //attr = a, b, c, d, e
            if (obj.hasOwnProperty(attr)) {
                // obj, obj.hasOwnProperty(attr)
                //{ a: 1, b: 2, c: [ { d: null, e: 'f' } ] } true
                // { a: 1, b: 2, c: [ { d: null, e: 'f' } ] } true
                // { a: 1, b: 2, c: [ { d: null, e: 'f' } ] } true
                // { d: null, e: 'f' } true
                // { d: null, e: 'f' } true

                copy[attr] = copyObj(obj[attr]);
                //console.log(copy[attr])
                // 1
                // 2
                // null
                // f
                // [ { d: null, e: 'f' } ]
            }
        }
    }
    else {
        copy = obj;
    }
    return copy;
}
let obj = { a: 1, b: 2, c: [{ d: null, e: 'f' }] };
let obj2 = copyObj(obj);
obj2.a = 3;
obj2.c[0].d = true;
console.log(obj.a); // 1
console.log(obj.c[0].d); // null
```
copy[attr]의 값이 a, b, d, e, c 순으로 찍히는 이유는 c가 배열이므로 `copy = obj.slice().map((v) => copyObj(v));`이 부분이 먼저 실행되기 때문이다.


* `객체.hasOwnProperty`: 객체의 속성이 자신의 속성이면 true, 부모의 속성이거나 아예 속성이 아니면 false를 반환한다.

```javascript
let obj = {
  example: 'yes',
};
obj.example; // yes
obj.hasOwnProperty('example'); // true
obj.toString; // function toString() { [native code] }
obj.hasOwnProperty('toString'); // false
```
 <br />

### - 함수를 깊은 복사하기  
   <br />
 `bind`를 이용해 this를 기존 함수와 같게 하면 똑같게 함수가 복사된다.  

 ```javascript
let func = function () {
  alert('hi');
};
func2 = func.bind(this);
func2(); // 'hi'
```
 