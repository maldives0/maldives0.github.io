---
title: programmers_Level1_javascript 네 번째
author: juyoung
date: 2020-11-17 19:06:00 +0800
categories: [algorithm, Level1]
tags: [algorithm]
---



[프로그래머스에서 문제 확인하기](https://programmers.co.kr/learn/challenges?selected_part_id=12079)

# 31. 약수의 합


```javascript
//sol1
function solution(n) {
    console.log(Array(Math.floor(n / 2)).fill().map((v, i) => i + 1).reduce((s, c) => n % c ? s : s + c) + n);

}

//sol2
function solution(n) {
    console.log(Array(n).fill().map((v, i) => i + 1).reduce((s, c) => n % c ? s : s + c));

}

//sol3
function solution(n, a = 0, b = 0) {
    console.log(n % a);
    return n <= a / 2 ? b : solution(n, a + 1, b += n % a ? 0 : a);
}

//sol4
function solution(n) {

    let a = 0;
    for (let i = 0; i <= n / 2; i++) {
        if (n % i === 0) a += i;
    }
    console.log(a);

    return a + n;
}
console.log(solution(12));

```


# 32. 체육복


```javascript
//sol1
function solution(n, lost, reserve) {
    let answer = n;
    let student = Array(n).fill(1);
    for (let i = 0; i < student.length; i++) {
        if (lost.includes(i + 1)) {
            student[i] -= 1;
        }
        if (reserve.includes(i + 1)) {
            student[i] += 1;
        }
    }
    for (let i in student) {
        //i=0~4
        if (student[i] == 2 && student[i - 1] == 0) {
            student[i] -= 1;
            student[i - 1] += 1;
        }
        if (student[i] == 0 && student[i - 1] == 2) {
            student[i] += 1;
            student[i - 1] -= 1;
        }
    }
    for (let s of student) { // s=[1,1,1,1,2]
        if (s == 0) answer--;
    }
    return answer;
};

//sol2
function solution(n, lost, reserve) {
    console.log(lost.filter(a => {
        const b = reserve.find(r => {

            return Math.abs(r - a) <= 1
        })
        console.log(b);
        if (!b) return true

        reserve = reserve.filter(r => r !== b)
    }).length)

    return n - lost.filter(a => {
        const b = reserve.find(r => Math.abs(r - a) <= 1)
        if (!b) return true
        reserve = reserve.filter(r => r !== b)
    }).length
}

//sol3
function solution(n, lost, reserve) {      
    return n - lost.filter(a => {
        const b = reserve.find(r => Math.abs(r-a) <= 1)
        if(!b) return true
        reserve = reserve.filter(r => r !== b)
    }).length
}

//sol4
function solution(x, lost, reserve) {
    const students = Array(x).fill(1);
    for (let i of lost) {
        students[i - 1] -= 1;
    }
    for (let i of reserve) {
        students[i - 1] += 1;
    }
    for (let i in students) {
        if (students[i] === 2 && students[i + 1] === 0) {
            students[i] -= 1;
            students[i + 1] += 1;
        }
        if (students[i - 1] === 0 && students[i] === 2) {
            students[i] -= 1;
            students[i - 1] += 1;
        }
    }
    let answer = 0;
    for (let s of students) {
        if (s > 0) {
            answer += 1;
        };
    }
    return answer;
}
console.log(solution(3, [3], [1]));

```  
 

# 33. 완주하지 못한 선수

```javascript
//sol1
function solution(participant, completion) {
    completion.sort().map((c, j) => {
        participant.sort().map((p, i) => {
            if (p == c) {
                participant.splice(i, 1);
            }
        })
    })
    let answer = participant[0];
    return answer;
}

//sol2
function solution(participant, completion) {
    participant = participant.sort();
    completion = completion.sort();
    for (let i in participant) {
        if (participant[i] !== completion[i]) {
            return participant[i];
        }
    }
}

//sol3
function solution(participant, completion) {
    var dic = completion.reduce((obj, t) => {

        return obj[t] = obj[t] ? obj[t] + 1 : 1, obj
    }
    );
    return participant.find(t => {
        console.log(t);
        if (dic[t])
            dic[t] = dic[t] - 1;
        else
            return true;
    });
}

//sol4
const solution = (p, c) => {
    p.sort()
    c.sort()
    while (p.length) {
        let pp = p.pop()

        if (pp !== c.pop()) return pp
    }
}
console.log(solution(["a", "b", "c", "a"], ["b", "a", "c"]));

```    

# 34. K번째 수

```javascript
//sol1
function solution(a, c) {
    let arr = [], i = 0, j = 0, k = 0, b = 0;
    c.forEach((e, h) => {
        i = e[0], j = e[1], k = e[2];
        b = a.slice(i - 1, j).sort((a, b) => a - b)[k - 1];
        arr.push(b);
    });
    return arr;
}


//sol2
function solution(g, h) {
    return h.map(([i, j, k]) => {
        return g.slice(i - 1, j).sort((a, b) => a - b)[k - 1];
    })
}


//sol3
function solution(g, h) {
    let arr = [];
    for (let v of h) {
        arr.push(g.slice(v[0] - 1, v[1]).sort((a, b) => a - b)[v[2] - 1]);
    }
    return arr;
}
console.log(solution([1, 5, 2, 6, 3, 7, 4], [[2, 5, 3], [4, 4, 1], [1, 7, 3]]));

```    

# 35. 모의고사

```javascript
function solution(n) {
    let answer = [];
    let a = [1, 2, 3, 4, 5], a1 = 0;
    let b = [2, 1, 2, 3, 2, 4, 2, 5], b1 = 0;
    let c = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5], c1 = 0;


    for (let i in n) {
        console.log([i % b.length]);
        if (n[i] === a[i % a.length]) a1++;
        if (n[i] === b[i % b.length]) b1++;
        if (n[i] === c[i % c.length]) c1++;
    }
    let max = Math.max(a1, b1, c1);
    if (max === a1) answer.push(1);
    if (max === b1) answer.push(2);
    if (max === c1) answer.push(3);
    return answer;
}

console.log(solution([1, 3, 2, 4, 2, 1, 3, 2, 4, 2]));

```  

# 36. 자릿수 더하기   


```javascript
//sol1
function solution(n) {

    return (n + "").split("").reduce((acc, c) => acc + parseInt(c), 0);

}

//sol2
function solution(n) {
    let a = 0;
    n.toString().split('').map(v => a += parseInt(v));
    console.log(a);

}
console.log(solution(987));

```    
  
  # 37. 내적
    
    reduce()를 알고는 있는데 이런 쉬운 문제에서도 막상 쓰려면 막히는 걸 보니 reduce가 어렵나보다..ㅜㅜ
   > reduce(function(accumulator, currentValue, currentIndex, array) {}
    sol2를 보고 reduce의 콜백에서 currentValue를 '_'로 쓰면 빈값으로 놔두는 걸 알게 됐다.  

```javascript
//sol1
function solution(a, b) {
    let sum = 0;
    for (let i = 0; i < a.length; i++) {
        sum += a[i] * b[i];
    }
    return sum;
}

//sol2
function solution(a, b) {
    return a.reduce((acc, _, i) => acc += a[i] * b[i], 0);
}

//sol3
var solution = (a, b) => a.reduce((a, c, i) => a + c * b[i], 0);
console.log(solution([-1, 0, 1], [1, 0, -1]));

```    

# 38. 예산  
    
문제를 풀 때 규칙을 우선 발견하는 것이 중요하다는 것을 다시 한 번 깨달은 문제였다.
부서별 신청 금액이 담긴 d 배열을 오름차순으로 정리해서 가장 큰 숫자를 하나씩 빼나가면 최대 지원해줄 수 있는 부서의 수를 구할 수 있다.


```javascript
//sol 1
function solution(d, budget) {
    while (d.sort((p, c) => p - c).reduce((a, c) => (a + c), 0) > budget) d.pop();
    return d.length;
}
//sol2
function solution(d, budget) {
    let count = 0;
    let sum = 0;
    d.sort((p, c) => p - c);
    for (let i = 0; i < d.length; i++) {
        count++;
        sum += d[i];
        console.log(count, sum);
        if (sum > budget) {
            count--;
            break;
        }
    }
    return count;
}
//sol3
function solution(d, budget) {
    let count = 0;
    let sum = 0;
    d.sort((p, c) => p - c);
    for (let i = 0; i < d.length; i++) {
        sum += d[i];
        count = d.length;
        console.log(sum, i);
        if (sum > budget) {
            count = i;
            break;
        }
    }
    return count;
}
console.log(solution([1, 3, 2, 5, 4], 9));
```


참고:#10. 예산 (프로그래머스) - JavaScript <https://velog.io/@qwerzxcvss/10.-%EC%98%88%EC%82%B0-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-JavaScript>