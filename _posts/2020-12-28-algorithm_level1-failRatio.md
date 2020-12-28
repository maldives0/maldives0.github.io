---
title: programmers_Level1_javascript - 카카오 실패율
author: juyoung
date: 2020-12-28 19:24:00 +0800
categories: [algorithm, Level1]
tags: [algorithm]
---


[프로그래머스에서 문제 확인하기](https://programmers.co.kr/learn/challenges?selected_part_id=12079)

# 1. filter를 이용해 각 단계의 실패율을 배열 arr에 담아준다


```javascript
function solution(N, stages) {
    let totalNum = stages.length;
    let arr = [];

    for (let i = 1; i <= N; i++) {
        let stageNum = stages.filter(e => e === i).length;
        let failRatio = 0;
        if (stageNum === 0) {
            failRatio = 0
        } else {
            failRatio = stageNum / totalNum;
        }
        totalNum -= stageNum;
        arr.push({ id: i, ratio: failRatio });

    }
    // console.log(arr);
    // [
    //     { id: 1, ratio: 0.125 },
    //     { id: 2, ratio: 0.42857142857142855 },
    //     { id: 3, ratio: 0.5 },
    //     { id: 4, ratio: 0.5 },
    //     { id: 5, ratio: 0 }
    //   ]
  

}
console.log(solution(5, [2, 1, 2, 6, 2, 4, 3, 3]));

```

# 2. sort를 이용해 실패율을 내림차순으로 정렬한다 

sort 할 때 '-1'이면 순서가 유지되고, '1'이면 순서가 서로 바뀐다.

```javascript

  const sortNum1 = [1, 2].sort((a, b) => {
        return a - b
        //a-b < 0 이므로 [1,2]로 순서가 유지된다. 

    });
    const sortNum2 = [2, 1].sort((a, b) => {
        return a - b
        //a-b > 0 이므로 [1,2]로 순서가 바뀐다.

    });

const sortNum3 = [1, 3, 2].sort((a, b) => {
    if (a - b > 0) {//[3, 2] 일 때 1이면 순서가 바뀌므로 [2, 3]가 될 것이다
        return -1;//오름차순으로 바꿔준다 [ 3, 2 ]
    }else if (a - b < 0) {//[1, 3] 일 때 -1이면 순서가 유지되므로 [1, 3]가 될 것이다
            return 1;//오름차순으로 바꿔준다 [ 3, 1 ]
        }
});


```

```javascript
function solution(N, stages) {
    let totalNum = stages.length;
    let arr = [];

    for (let i = 1; i <= N; i++) {
        let stageNum = stages.filter(e => e === i).length;
        let failRatio = 0;
        if (stageNum === 0) {
            failRatio = 0
        } else {
            failRatio = stageNum / totalNum;
        }
        totalNum -= stageNum;
        arr.push({ id: i, ratio: failRatio });

    }
  
    arr.sort((p, c) => {
        if (p.ratio > c.ratio) {
            return -1;//유지
        } else if (p.ratio < c.ratio) {
            return 1;//바꾸기
        } else {
            if (p.id > c.id) {
                return 1;//바꾸기
            } else {
                return -1;//유지
            }
        }
    })

}
console.log(solution(5, [2, 1, 2, 6, 2, 4, 3, 3]));

```





참고:  
[[프로그래머스] 실패율 (JavaScript)](https://jongbeom-dev.tistory.com/144)