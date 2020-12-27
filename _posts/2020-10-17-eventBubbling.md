---
title: eventBubbling 문제 해결하기
author: juyoung
date: 2020-10-16 18:56:00 +0800
categories: [project, resolution]
tags: [project]
---

# 검색결과 목록 드레그하기
<br />
  ```html
    <div class="slider">
        <div class="listbox">
            <ul class="items">
<!-- <li class="item" id="0">
    </li>                 -->
            </ul>
        </div>
    </div>
```  

## 문제상황  

검색결과에 따라 'items'안에 'item'이 만들어진다. 그 값을 drag의 인자로 보내서 사용자가 'item'에서 mouseup을 할 때 좌표값을 구한다.

 현재 드레그를 위해 필요한 4가지 이벤트의 target을 item으로 두었더니 mouseup이벤트가 3번 발생하는 이벤트 버블링 현상이 발생했다.
   
<br />
<br />

## 해결방법
  
  나머지 mousedown, mousemove, mouseleave 이벤트가 발생하는 영역을 item의 부모인 items를 감싸는(증조부?!) listbox로 범위를 확대해 적용하면 문제가 해결된다.
  

```javascript
  function mapSearch() {
        const item = document.querySelectorAll('.item');
        drag(item);
        //...
        }

        //drag
    function drag(item) {
        const listBox = document.querySelector('.listbox');
        listLen = item.length;

        let isDown = false;
        let startX;
        let endX;

        listBox.addEventListener('mousedown', (e) => {
            isDown = true;
            listBox.classList.add('active');
            startX = e.pageX - listBox.offsetLeft;
            scrollLeft = ulEle.scrollLeft;
        });

        listBox.addEventListener('mousemove', (e) => {
            endX = e.pageX - listBox.offsetLeft;

            if (!isDown) return endX;
            e.preventDefault();
        });

        listBox.addEventListener('mouseleave', (e) => {
            isDown = false;
            listBox.classList.remove('active');
        });

        item.forEach(function (el) {
            el.addEventListener('mouseup', (e) => {
                // console.log(e.currentTarget);
                isDown = false;
                listBox.classList.remove('active');
                endPos();
            });
      });
                     function endPos() {
            if (startX > endX) {
                //next
                if (idxList < listLen - 1) idxList++;
            } else {
                //prev
                if (idxList != 0) idxList--;
            }
            setTimeout(function () { ulEle.style = "transform:translateX(" + (-420 * idxList) + "px);"; }, 100);
        };

```  
<br />
 알아보니 자바스크립트 앱을 만들 때 자주 사용되는 패턴 중 책임 연쇄 패턴에서 자주 마주칠 수 있는 현상이 <font color=purple>이벤트 버블링</font> 현상이다.
  

## <font color=purple>이벤트 버블링</font> 현상이란,

 > DOM에 연결한 이벤트는 버블링이 일어납니다. 어떤 태그에 이벤트가 발생하면 그 이벤트는 해당 태그의 부모나 조상에게도 순서대로 발생합니다. 
 ```
 <div id="first">
   <div id="second">
    <div id="third">
    </div>
   </div>
 </div>
 ```
 위와 같은 구조가 있을 때 div#third를 클릭한 경우, 부모와 조상 태그인 second, first 순으로 같은 클릭 이벤트가 발생합니다. 
 이 때 사용자가 이벤트 리스너를 달아서 어떤 태그에서 이벤트를 처리할지, 또는 다음 태그로 이벤트 처리 수행을 넘길지 결정할 수 있습니다. 이와 같이 동작의 처리를 자신이 할지 다음으로 넘길지 결정하는 패턴이 책임 연쇄입니다.
<br />
<br />
참고  
zeroCho blog : <https://www.zerocho.com/category/JavaScript/>