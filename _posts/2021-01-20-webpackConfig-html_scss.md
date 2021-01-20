---
title: webpack-html과 scss 그리고 code spliting 적용하기
author: juyoung
date: 2021-01-20 19:11:00 +0800
categories: [webpack, html]
tags: [webpack]
---

webpack으로 scss와 code spliting, 이미지 가져오기를 적용해보았다.
가장 문제가 되었던 것이 webpack-dev-server와 build한 파일의 `publicPath`가 일치하지 않은 것이었다.
<br />

production mode일 때, `publicPath`에 파일들이 위치할 서버 상의 경로를 넣어주면 빌드 시에는 번들한 파일 이름 앞에 해당 설정값이 포함되어 html에 script 태그로 자동 입력된다.
나의 경우 로컬의 파일구조 `/webpack/dist/`를 설정값에 적어주면 html에 /webpack/dist/(번들된 파일)로 자동으로 입력되었다.
<br />

```html
<script
  defer="defer"
  src="/webpack/dist/vendor.0ef3ea92ffc39d3f0fbb.js"
></script>
```

<br />

그러나 development mode일 경우, webpack-dev-server로 가상의 서버를 띄웠을 때는 `/`로 설정해야 웹팩 데브 서버가 번들한 결과물이 위치하는 경로를 제대로 읽어낼 수 있었다.
<br />

```html
<script defer="defer" src="/vendor.0ef3ea92ffc39d3f0fbb.js"></script>
```

<br />
webpack/webpack.config.js

```javascript
const path = require('path');
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const { WebpackManifestPlugin } = require('webpack-manifest-plugin');

module.exports = (env, options) => {
  const prod = options.mode === 'production';
  console.log(prod);
  const config = {
    mode: prod ? 'production' : 'development',
    devtool: prod ? 'eval' : 'hidden-source-map',
    resolve: {
      extensions: ['.js'],
    },
    entry: {
      index: ['./src/index.js'],
    },
    output: {
      path: path.join(__dirname, './dist'), //output으로 나올 파일이 저장될 경로
      publicPath: prod ? '/webpack/dist/' : '/', //파일들이 위치할 서버 상의 경로(빌드 시 내 로컬 파일 구조)
      filename: '[name].[chunkhash].js', //청크별로 해시값이 부여
    },
    plugins: [
      new CleanWebpackPlugin(), //빌드할 때 전에 빌드할 때 미리 만들어져있는 파일이 있다면 삭제하기(기본적으로 변경되는 부분만 바뀐다, 단 npm run dev를 하면 전체 삭제됨)
      new HtmlWebpackPlugin({
        template: './src/index.html', //정의되어있는 기본 template을 기준으로 번들링 시에 새로운 html 파일을 생성해 준다(js, css에 수정사항이 있을 시)
      }),
      new MiniCssExtractPlugin({
        filename: '[name].[contenthash].css',
        chunkFilename: '[id].[contenthash].css',
      }), //여러 css 파일을 하나의 파일로 묶어서 번들링함
      new webpack.LoaderOptionsPlugin({
        minimize: true,
      }), //loader들의 옵션을 설정할 수 있다
      new WebpackManifestPlugin(), //manifest파일을 자동으로 생성한다, 별다른 설정없이도 빌드할 때마다 manifest의 파일경로(src)가 바뀌는데 이 바뀌는 경로들이 자동으로 html파일에 적용된다.
    ],
    module: {
      rules: [
        {
          test: /\.scss$/,
          use: [
            prod ? MiniCssExtractPlugin.loader : 'style-loader',
            'css-loader',
            'sass-loader',
          ],
        },
        {
          test: /\.js?$/,
          loader: 'babel-loader',
          options: {
            plugins: ['@babel/plugin-syntax-dynamic-import'], //코드 스플리팅을 적용한다
            presets: [
              [
                '@babel/preset-env',
                {
                  modules: false,
                  targets: '> 0.25%, not dead',
                  useBuiltIns: 'entry',
                  corejs: '3.0.0',
                },
              ],
            ],
          },
          exclude: ['/node_modules'],
        },
        {
          test: /\.(ico|png|jpg|jpeg|gif|svg|woff|woff2|ttf|eot)(\?v=[0-9]\.[0-9]\.[0-9])?$/,
          loader: 'url-loader',
          options: {
            name: '[hash].[ext]', //[hash]는 웹팩 빌드를 할 때마다 고유값이 설정됨
            limit: 10000,
            esModule: false,
          },
        },
      ],
    },
    devServer: {
      historyApiFallback: true,
      publicPath: '/', // 웹팩 데브 서버가 번들한 결과물이 위치하는 경로(보통 output에 적어주는 publicPath와 동일한 위치를 표시),
      contentBase: path.join(__dirname, './dist'), //콘텐츠를 제공할 경로지정(정적파일을 제공하려는 경우에만 필요),
    },
    optimization: {
      minimize: true,
      splitChunks: {
        cacheGroups: {
          vendor: {
            // 청크들간 공통적으로 사용하는 모듈들을 모아둔 것
            chunks: 'initial',
            name: 'vendor',
            enforce: true,
          },
        },
      },
      concatenateModules: true,
    },
  };

  return config;
};
```

<br />

webpack/src/index.js

```javascript
import './style/main.scss';
import 'core-js/stable';
import 'regenerator-runtime/runtime';
//다음과 같이 import하면 웹팩에서 알아서 해당 코드가 필요할 때마다 비동기적으로 로딩해준다, import한 값은 Promise객체이다
import(/* webpackChunkName: "module" */ './module')
  .then(function (res) {
    const p = document.createElement('p');
    p.textContent = res.default;
    document.body.append(p);
  })
  .catch(function (err) {
    console.error('module error', err);
  });

const div = document.createElement('div');
document.body.append(div);
```

결과물은 다음과 같다.<br />

webpack/dist/index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" lang="ko" />
    <title>hello</title>
    <script
      defer="defer"
      src="/webpack/dist/vendor.0ef3ea92ffc39d3f0fbb.js"
    ></script>
    <script
      defer="defer"
      src="/webpack/dist/index.f41f95a8ac593bbf808a.js"
    ></script>
    <link
      href="/webpack/dist/vendor.30ab4e262a68d4200550.css"
      rel="stylesheet"
    />
  </head>
  <body>
    <div>hello</div>
  </body>
</html>
```

![webpack_html](/assets/img/webpack_html.jpg)<br />
<br />

전체 폴더 구조는 [깃헙](https://github.com/maldives0/webpack)에 있습니다
<br />

출처:<br />

[[BuildSystem][ModuleBundler][Webpack]sass(scss)를 css로 바꾸기(3)](https://kamang-it.tistory.com/entry/BuildSystemModuleBundlerWebpacksassscss%EB%A5%BC-css%EB%A1%9C-%EB%B0%94%EA%BE%B8%EA%B8%B03)
<br />

[Webpack 완전정복하기!!](https://kdydesign.github.io/2017/11/04/webpack-tutorial/)
<br />

[zeroCho blog: Webpack](https://www.zerocho.com/category/Webpack)
