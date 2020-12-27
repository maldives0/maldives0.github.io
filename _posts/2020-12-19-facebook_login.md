---
title: facebook OAuth redirect url 설정하기
author: juyoung
date: 2020-12-19 22:08:00 +0800
categories: [project, api]
tags: [api]
---

# 0. Facebook for Developers에 접속해 내 앱 만들기
<br />

[Facebook for Developers](https://developers.facebook.com/?locale=ko_KR)에 접속해서 '내 앱'을 이미 만들었다는 가정 아래 시작합니다!
<br />


# 1. 앱 도메인에 OAuth로 로그인 요청을 보낼 url 주소를 입력하기  

내 앱 > 설정 > 기본 설정으로 들어가 '앱 도메인'과 그 밖의 정보를 입력해준다.

앱 도메인
ymillonga.xyz

개인정보처리방침 URL
https://ymillonga.xyz/?mode=privacy  

서비스 약관 URL
https://ymillonga.xyz/?mode=policy  


![facebook_config](/assets/img/facebook_config.jpg)
<br />  

아래로 스크롤을 내리면 '플랫폼 추가' 버튼이 있는데 내 앱의 기본 '사이트 URL' 값을 넣을 수 있다.  

아래의 예시에서 나는 이곳에 프론트서버의 url 즉, 
OAuth로 로그인을 요청 보낼 url 주소인 'https://ymillonga.xyz'를 입력했다. 

<br />

![facebook_platform](/assets/img/facebook_platform.jpg)
<br />  
<br /> 
* 프론트엔드 서버에서 백엔드 서버로 로그인 요청을 보낼 버튼은 아래와 같다. 버튼을 클릭하면 백엔드 서버의 'https://ymillonga.xyz/user/facebook' 주소로 넘어가고,  
백엔드 서버에서 facebook으로 클라이언트의 정보를 요청하는 'GET /facebook'을 보내게 된다. 
<br /> 

front > components > FacebookLoginBtn.js
```javascript
import React, { useCallback } from 'react';
import { FacebookLoginButton } from 'react-social-login-buttons';
import { useRouter } from 'next/router';
import { backUrl } from '../config/config';
const FacebookLoginBtn = () => {

    const router = useRouter();
    const onClickFacebookLogin = useCallback(() => {
        router.push(`${backUrl}/user/facebook`);
    });
    return (

        <FacebookLoginButton
            onClick={onClickFacebookLogin}
            align="center"
            size="40px"
            text="Facebook"
            style={{ width: '150px' }}
        />

    );
};

export default FacebookLoginBtn;
```  
<br />

passport > routes > user.js 

```javascript
const express = require('express');
const router = express.Router();
const passport = require('passport');
const prod = process.env.NODE_ENV === 'production';
const frontUrl = prod ? "https://ymillonga.xyz" : "http://localhost:3050";

router.get('/facebook', passport.authenticate('facebook', { scope: ['public_profile', 'email'] }));

router.get('/facebook/callback', passport.authenticate('facebook', {
    failureRedirect: '/',
}), async (req, res, next) => {
    return res.status(200).redirect(frontUrl);

});
module.exports = router;

```
<br />

# 2. 유효한 OAuth 리디렉션 URI에 callback url값 넣어주기
<br />

'내 앱 > 제품 + > Facebook 로그인 > 설정' 으로 들어가면 다음과 같이 '유효한 OAuth 리디렉션 URI'을 넣을 수 있는 창이 있다.  
페이스북 로그인 전략에서 클라이언트의 로그인 정보를 받기로 한 callbackURL을 이곳에 넣어주면 된다.   
아래의 예시의 경우 백엔드 서버 주소인 'https://ymillonga.xyz/user/facebook/callback'을 넣어주었다.  

![facebook_redirectionUrl](/assets/img/facebook_redirectionUrl.jpg)


<br />

passport > facebook.js
```javascript
const passport = require('passport');
const dotenv = require('dotenv');
const bcrypt = require('bcrypt');

//페이스북 로그인 전략
dotenv.config();
const { User } = require('../models');
const FacebookStrategy = require('passport-facebook').Strategy;

module.exports = () => {
    passport.use(new FacebookStrategy({
        clientID: process.env.FACEBOOK_APP_ID,
        clientSecret: process.env.FACEBOOK_APP_SECRET,
        callbackURL: '/user/facebook/callback',
    },
        async (accessToken, refreshToken, profile, done) => {
            console.log('profile', profile);
            try {
                const exUser = await User.findOne({
                    where: {
                        email: profile.displayName,
                        provider: 'facebook',
                    },

                });

                if (exUser) {
                    return done(null, exUser);
                }
                else {
                    const hashedPassword = await bcrypt.hash(profile.displayName, 11);
                    if (profile.emails) {
                        const newUser = await User.create({
                            email: profile.emails[0].value,
                            password: hashedPassword,
                            nickname: profile.displayName,
                            snsId: profile.id,
                            provider: 'facebook',
                        });
                        done(null, newUser);
                    } else {
                        const newUser = await User.create({
                            email: profile.displayName,
                            password: hashedPassword,
                            nickname: profile.displayName,
                            snsId: profile.id,
                            provider: 'facebook',
                        });
                        done(null, newUser);
                    }

                }
            }
            catch (err) {
                console.error(err);
                done(err);
            }

        }
    ));
};
```
<br />


<br />


참고:    
  
[Facebook for Developers](https://developers.facebook.com/?locale=ko_KR)  

[passportjs 공식 홈페이지](http://www.passportjs.org/docs/google/)    

[인프런 react nodebird 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C%EB%B2%84%EB%93%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%A6%AC%EB%89%B4%EC%96%BC)  

[zerocho node-js](https://github.com/ZeroCho/nodejs-book/tree/master/ch9/9.5/nodebird)  

