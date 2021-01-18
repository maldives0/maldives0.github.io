---
title: Algorithm-삽입 정렬
author: juyoung
date: 2021-01-18 20:11:00 +0800
categories: [algorithm, tutorial]
tags: [algorithm]
---

[zeroCho blog: 삽입 정렬](https://www.zerocho.com/category/Algorithm/post/57e39fca76a7850015e6944a)의 예시를 공부하며 쓴 블로그 글입니다.
<br /><br />
삽입 정렬은 첫 숫자는 놔두고, 두 번째 자리 숫자부터 뽑아서 그 숫자가 첫 숫자보다 크면 첫 숫자 오른쪽에, 작으면 왼쪽에 넣는다. 이러한 과정을 반복해 숫자를 오름차순으로 정렬한다.

```javascript
const insertionSort = function (array) {
  let i = 1,
    j,
    temp;
  for (i; i < array.length; i++) {
    temp = array[i];

    for (j = i - 1; j >= 0 && temp < array[j]; j--) {
      array[j + 1] = array[j];
    }
    array[j + 1] = temp;
  }
  return array;
};
console.log(insertionSort([5, 6, 1, 2, 4, 3]));
```

1. temp = 6일 때(한 칸 뒤의 숫자인 `temp`가 한 칸 앞의 숫자인 `array[j]`보다 클 때)<br />
   for문의 `temp < array[j]`(6<5)가 성립하지 않으므로<br />
   바로 `array[j + 1]` = `temp`수식으로 가서 `array[1]`의 자리에 6을 넣어준다.
   <br /><br />
2. temp = 1일 때(한 칸 뒤의 숫자인 `temp`가 한 칸 앞의 숫자인 `array[j]`보다 작을 때)<br />
   for문의 `temp` < `array[j]`(1<6)가 성립하므로<br />
   `array[j + 1]` = `array[j]` 수식으로 가서 `array[2]`의 자리(현재 temp인 1의 자리)에 6을 넣어준다.<br />
   for문을 한번 거쳐서 `array[j + 1] = temp `수식으로 가기때문에 j값이 -1이 되어 `array[1]`의 자리에 현재 temp인 1을 넣어준다(원래 6의 자리)

<br />
이 두 가지 경우를 반복하면 결과값은 [1, 2, 3, 4, 5, 6]이 return된다.
<br />

출처:<br />

[zeroCho blog: 삽입 정렬](https://www.zerocho.com/category/Algorithm/post/57e39fca76a7850015e6944a)
