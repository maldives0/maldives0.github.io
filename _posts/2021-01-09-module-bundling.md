---
title: ymillonga-map 프로젝트 모듈시스템 적용하기
author: juyoung
date: 2021-01-09 19:58:00 +0800
categories: [project, resolution]
tags: [webpack]
---
  
 현재 active.js와 map.js 두 개의 파일로 나누어진 두 파일이 공통으로 사용하는 함수를 모듈로 만들어 재사용성을 높이고 무엇보다 코드량을 줄여 가독성을 높이기 위해 모듈시스템을 적용하기로 했다.  

# 1. webpack 설정하기
<br />
 여러 modules로 쪼개진 js파일들을 합치기 위해 webpack으로 bundling해줄 필요가 있어서 다음과 같이 webpack설정을 했다.

webpack.config.js
```javascript
const webpack = require('webpack');
const path = require('path');
const prod = process.env.NODE_ENV === 'production';

module.exports = {
    mode: prod ? 'production' : 'development',
    devtool: prod ? 'eval' : 'hidden-source-map',
    resolve: {
        extensions: ['.js'],
    },
    entry: {
        active: [path.join(__dirname, './js/active.js')],
        map: [path.join(__dirname, './js/map.js')],
    },
    module: {
        rules: [
            {
                test: /\.js?$/,
                loader: 'babel-loader',
                options: {
                    presets: [
                        [
                            '@babel/preset-env', {
                                modules: false,
                                targets: '> 0.25%, not dead',
                            }
                        ],

                    ],
                },
                exclude: ['/node_modules'],
            },           
        ],
    },

    plugins: [
        new webpack.LoaderOptionsPlugin({
            minimize: true,
        }),
      ],
    devServer: {
        historyApiFallback: true,
        publicPath: '/dist',
    },
    output: {
        filename: '[name].js',
        path: path.join(__dirname, 'dist'),
        publicPath: '/dist',
    },
};
```  

원래 js폴더에 있는 active.js와 map.js파일을 bundling해서 dist폴더에 넣어주도록 `entry`와 `output`에 파일 경로를 설정한다. html에서는 아래와 같이 `<script>`의 src를 변경해준다.

index.html
```html
<head>
   <script src="dist/active.js"></script>
</head>
```  
<br />


# 2. js파일을 여러개의 module로 나누기  
<br />
 여러번의 시도 끝에 총 4개의 파일로 나누었다. entry파일로 active.js와 map.js를 두고 이 두 파일에서 중복해서 사용되는 함수인 mapSearch와 dragList를 따로 모듈로 만들어 import하기로 했다. 
 mapSearch파일에 mapSearch, markerEvent, getFindMe, regionNow라는 총 4개의 함수가 들어있는데 각 함수별로 모듈을 분리하려고 했으나 카카오에서 지도를 생성하는 변수 map을 인자로 전달하면 getFindMe함수를 실행할 때 tileAnimation error가 발생했다.
 그래서 최종적으로 아래와 같은 구조를 만들고 다음과 같이 매개변수를 통해 인자를 전달해주니 오류가 해결되었다.  

 js > mapSearch.js
```javascript
import dragList from './dragList';
const map = new kakao.maps.Map(mapContainer, mapOption);

function mapSearch(moveX, listLen, idxList, posChoice, findMeBtn, ulEle) {
  ...
}

function markerEvent(posChoice, idxList, moveX, ulEle) {
  ...
}

export function getFindMe(e, posChoice) {
  ...
}

function regionNow(map) {
  ...
}

export default mapSearch;

```

  input창에 새로운 검색어가 입력될 때 getFindMe함수를 호출할 필요가 있어 getFindMe는 따로 export를 해주어 아래와 같이 active파일에서 가져다 쓸 수 있도록 만들었다. 

js > active.js
```javascript
import mapSearch from './mapSearch';
import { getFindMe } from './mapSearch';
import dragList from './dragList';

```

 각 함수를 모듈로 나누니 서로 공유하는 변수를 위와 같이 인자로 전달해줘야하는 필요가 생겼다. 이때 getFindMe의 경우처럼 addEventListener의 콜백함수는 클로저를 이용하거나 bind메소드를 이용해 인자를 전달해야 했다.  
 왜냐하면 아래와 같이 findMeBtn이라는 element에 addEventListener를 연결하면 이벤트에 연결한 콜백함수는 첫번째 매개변수 자리가 이벤트 객체(아래 예시에서는 e의 자리)이기 때문이다.   

js > mapSearch.js
```javascript
const findMeBtn = document.querySelector('#findMe');
  findMeBtn.addEventListener('click', (function (posChoice) {
      return function (e) { getFindMe(e, posChoice) }
            })(posChoice), false); 
export function getFindMe(e, posChoice) {
  ...
}

```

<br />

# 번외. addEventListener의 콜백함수에 인자 전달하기

<br />  
 getFindMe에 인자를 전달하는 방법을 찾다가 아래와 같이 클로저와 this, 화살표함수를 다시 집어볼 수 있었다.   


## (1) 클로저 이용하기
```javascript
  let params= 'global';
  const button = document.querySelector('button');

  button.addEventListener('click',(function(params){
      return function(e){
          otherFunc(e,params)
          }   
      })(params));

  function otherFunc(e,params){
    console.log(params);//global
  }

  또는 

  button.addEventListener('click',eventHandler(params))

  function eventHandler(params){
      return function(e){
          otherFunc(e,params)
      }      
  }
  function otherFunc(e,params){
      console.log(params);//global
  }


```   
<br />  

## (2) bind 이용하기
```javascript
  let params= 'global';
  const button = document.querySelector('button');

button.addEventListener('click',function(params,e){
    console.log(params);//global
}.bind(button, params));

또는 


function onClick(params, func){
    button.addEventListener('click', func.bind(button, params));
}

const otherFunc = (params,e)=>{
    console.log(params, e, this);//global, MouseEvent, Window
};

onClick(params, otherFunc);

```  
<br />  

## (3) 화살표함수 사용하기
```javascript
  let params= 'global';
  const button = document.querySelector('button');

  
const eventHandler = (e)=> otherFunc(e,params);

button.addEventListener('click', eventHandler);

const otherFunc = (e,params)=>{
    console.log(params);//global
};

또는 

button.addEventListener('click', (e)=> ((params)=>otherFunc(e,params))(params));

const otherFunc = (e,params)=>{
    console.log(params);//global
};

또는 

button.addEventListener('click', (e)=> ((params)=>otherFunc(e,params))(params));

const otherFunc = (e,params)=>{
    console.log(params);//global
};
```     
      
<br />
<br />
<br />

참고:  
[zerocho blog: 웹팩5(Webpack) 설정하기](https://www.zerocho.com/category/Webpack/post/58aa916d745ca90018e5301d)  
[ko.javascript.info: 이벤트 객체](https://ko.javascript.info/introduction-browser-events#ref-3256)  
[매개 변수가있는 자바 스크립트 이벤트 핸들러](https://lottogame.tistory.com/7698)