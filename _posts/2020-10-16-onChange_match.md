---
title: input의 onchange 그리고 match로 검색창 만들기
author: juyoung
date: 2020-10-16 18:56:00 +0800
categories: [project, resolution]
tags: [project]
---


## 1. 구현순서
1. input창을 enter키를 누르는 것만으로 value값을 받는다.
2. input값과 json data와 비교한다.
3. input값이 해당 json data에 있을 때 지도 아래 리스트 목록으로 넣어준다. (ulEle에 결과값을 넣어주기)
4. input값이 바뀌면 match로 얻은 값을 새로고침한다.
  

  

## 2. <form> 테그 만들기  

 먼저 html에 `<form>` 테그를 만들고 addEventListener로 submit 콜백함수를 불러온다. 이때 e.preventDefault()로 input값을 넣고 enter할 때마다 페이지가 새로고침되지 않도록 막아야 input의 value를 얻을 수 있다.  
  
  ```html
    <form>
            <div class="inputarea f_b">
                <input type="text" placeholder="밀롱가 이름으로 검색해보세요" 
                >
                <button style="display:none"></button>
                <div>
                    <a href="#">
                        <img src="img/ic_search.png" alt="">
                    </a>
                </div>
            </div>
        </form>
```
    
## 3. match method로 json data 검색하기  

 검색하고자 하는 밀롱가의 영어, 또는 한국어 이름과 input의 value를 비교한다. 대소문자 구분 없이 비교하려면 정규표현식으로 비교하려는 값 뒤에 `/gi`를 넣어 `(input value)/gi`와 같이 설정을 하면 된다고 하는데... `en.match(inputVal)`해도 대소문자 구분없이 비교가 되긴 했다.  
  

이러한 과정을 거치면 match했을 때 같은 단어가 들어간 이름의 목록이 object형식으로 주어진다.

```console
 ["탱고", index: 0, input: "탱고 오나다", groups: undefined]  

0: "탱고" 
groups: undefined
index: 0
input: "탱고 오나다"
length: 1
__proto__: Array(0)
```

   그런데 `inputVal`에 바로 input.value의 값을 할당하면 페이지가 새로고침될 때까지 input에 검색어를 바꿔주더라도 계속 ulEle에 목록이 쌓이는 문제가 발생했다. 이를 해결하기위해서는 input의 `onChange`함수가 필요하다. 
  

## 4. input의 `onChange`함수로 검색결과 새로고침하기
<br> `inputVal = e.target.value`와 같이 target의 value를 할당하면 input에 검색어가 달라지면 enter를 쳤을 때 change 콜백함수가 실행되며 전 value값이 삭제된다. 
이로써 ulEle에 쌓이는 목록이 검색어가 달라질 때마다 매번 새로워진다.  


* input창에 입력을 마치고 enter를 치면 input창에 커서가 focus되고 빈칸으로 바뀌도록 할 때도 `inputVal=''` 이라고 하면 검색한 값이 계속 남아있는데 반해 `input.value=''`라고 선언하면 빈칸이 된다. input의 매개변수인 `value`를 사용해야만 기능한다는 사실을 드디어 깨달았다...

```javascript

 form.addEventListener('submit',dataFun);
      
     function dataFun(e) {
    e.preventDefault();
        response = JSON.parse(data.responseText);
        
        ulEle.innerHTML = '';
        input.addEventListener('change',function(e){
            inputVal = e.target.value;           
        }); 
     
        response.millonga.forEach(function (el, idx) {
            thumb = el.thumb;
            url = el.url;
            en = el.en;
            address = el.address;
            ko = el.ko;
                     
            let a = en.match(inputVal);
            let b = ko.match(inputVal);
                                          
       if (a || b) {                
                    liEle = "<li class='item item" + idx + " f_b'>";
                    liEle += "<div class='con f_b'> <div class='leftsec'><div class='thumb'><a class='linkA link" + idx + "' href='" + url + "'><img src='" + thumb + "' alt='" + en + "'></a></div></div>";
                    liEle += " <div class='rightsec'> <div class='f_b'><h4 class='f_b'>" + en + "</h4><span>거리m</span></div><h6>" + ko + "</h6>";
                    liEle += " <p class='address'>" + address + "</p></div> </div>";
                    liEle += " <div class='appraisal'><span class='like'>371</span><span class='write'>39</span> </div></li>";

                    ulEle.innerHTML += liEle;                
                    input.value = '';
                    input.focus();  
                  }else if(!a || !b){
                    input.value = '';
                    input.focus();  
                  }
            });           
    }//datafun

```


