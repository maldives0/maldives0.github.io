---
title: programmers_Level1_javascript - 카카오 크레인 인형뽑기
author: juyoung
date: 2020-12-27 18:06:00 +0800
categories: [algorithm, Level1]
tags: [algorithm]
---


[프로그래머스에서 문제 확인하기](https://programmers.co.kr/learn/challenges?selected_part_id=12079)

# 1. board에서 선택된 인형들을 result 배열에 담기 

count = 0 이면 board의 첫번째 열의 값들을 b[m - 1]로 찾을 수 있다.
  
   b[m - 1] = 0,0,0,4,3

result에 0이 아닌 값을 가진 첫번째 인형 [4] 가 담기면  
b[m - 1] = 0로 board에서 선택된 인형의 값 [4]를 [0]으로 만들어준다.  

```console
  [
  [ 0, 0, 0, 0, 0 ],
  [ 0, 0, 1, 0, 3 ],
  [ 0, 2, 5, 0, 1 ],
  [ 4, 2, 4, 4, 2 ],
  [ 3, 5, 1, 3, 1 ]
]

// b[m - 1] = 0 를 하면
[
  [ 0, 0, 0, 0, 0 ],
  [ 0, 0, 1, 0, 3 ],
  [ 0, 2, 5, 0, 1 ],
  [ 0, 2, 4, 4, 2 ],
  [ 3, 5, 1, 3, 1 ]
]
```
```javascript
function solution(board, moves) {
    const result = [];
    let count = 0;
    let point = 0;
    moves.map((m, j) => {
        board.map((b, i) => {
            if (b[m - 1] !== 0 && count === j) {
                count++;
                result.push(b[m - 1]);              
                 b[m - 1] = 0;
                  // console.log(result)
               // [ 4, 3, 1, 1, 3, 2 ]
            }
           // console.log(board)          
         
        })
    })
   }

solution([[0, 0, 0, 0, 0], [0, 0, 1, 0, 3], [0, 2, 5, 0, 1], [4, 2, 4, 4, 2], [3, 5, 1, 3, 1]], [1, 5, 3, 5, 1, 2, 1, 4]);


```

# 2. 선택된 인형들 중 연속해서 같은 값이 담기면 result에서 제거하기  

전에 선택된 인형 result[result.length - 1] 와 현재 선택한 인형의 값 b[m - 1] 을 비교해서 같으면 result 배열에서 지우고 point를 2씩 올려준다. 

```javascript
function solution(board, moves) {
    const result = [];
    let count = 0;
    let point = 0;
    moves.map((m, j) => {
        board.map((b, i) => {

            if (b[m - 1] !== 0 && count === j) {
                count++;
                if (result[result.length - 1] === b[m - 1]) {
                    result.pop();
                    point += 2;
                } else result.push(b[m - 1]);
                b[m - 1] = 0;               
            }           
        })
    })
    return point;

}

solution([[0, 0, 0, 0, 0], [0, 0, 1, 0, 3], [0, 2, 5, 0, 1], [4, 2, 4, 4, 2], [3, 5, 1, 3, 1]], [1, 5, 3, 5, 1, 2, 1, 4]);

```





참고:  
[알고리즘 카카오 크레인 인형뽑기-JavaScript](https://velog.io/@wooder2050/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%B9%B4%EC%B9%B4%EC%98%A4-%ED%81%AC%EB%A0%88%EC%9D%B8-%EC%9D%B8%ED%98%95%EB%BD%91%EA%B8%B0-JavaScript)