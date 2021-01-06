---
title: non-boolean attribute warning와 css prop 사용하기
author: juyoung
date: 2021-01-06 11:18:00 +0800
categories: [project, resolution]
tags: [project]
---

# 1. non-boolean attribute warning

> Warning: Received `false` for a non-boolean attribute `prefetch`.
    If you want to write it to the DOM, pass a string instead: prefetch="false" or prefetch={value.toString()}.
    If you used to conditionally omit it with prefetch={condition && value}, pass prefetch={condition ? value : undefined} instead.
  

![non-boolean_attribute](/assets/img/non-boolean_attribute.jpg)


'prefetch는 boolean속성이 아닌데 boolean 값으로 false를 받았다'는 에러이다.
nextjs에서 제공하는 `<Link>`가 아닌 antd에서 제공하는 `<Link>`에 다음과 같이 `prefetch={false}`를 적어놓아 생긴 warning이었다.
  

```javascript
import { Link } from 'antd';

    <Link href="https://maldives0.github.io" target="_blank" prefetch={false}>
        https://maldives0.github.io/
    </Link>
```

# 2. 해결방법 

```javascript
import { Link } from 'antd';

    <Link href="https://maldives0.github.io" target="_blank" prefetch="false">
        https://maldives0.github.io/
    </Link>
```
  
 검색하는 과정에서 이 오류가 style에 prop을 전달할 때 자주 발생하는 문제라는 것도 알았다.
styled-components는 5.1 버전부터 prefix 로 "$" 를 사용하게 되면, props 가 실제 DOM 요소에 전달되는 것을 막는다.

emotion에서는 Customizing prop forwarding기능을 가진 shouldForwardProp를 제공한다.


참고:  
[마이구미의 HelloWorld: Warning Received `true` for non-boolean attribute](https://mygumi.tistory.com/382)  
[emotion: customizing-prop-forwarding](https://emotion.sh/docs/styled#customizing-prop-forwarding)