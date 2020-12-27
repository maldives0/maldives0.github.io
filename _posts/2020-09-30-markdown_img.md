---
title: markdown local img url 경로 설정방법
author: juyoung
date: 2020-09-30 22:55:00 +0800
categories: [diary, github.io]
tags: [github.io]
pin: 
---


![](https://dthumb-phinf.pstatic.net/?src=%22https%3A%2F%2Fdbscthumb-phinf.pstatic.net%2F2382_000_1%2F20130219201256465_Y2A8J7HZR.jpg%2Fib12_71_i1.jpg%3Ftype%3Dw690_2%26wm%3DY%22&twidth=690&theight=438&opts=17)


![avatar.jpg](/assets/img/sample/avatar.jpg)


- - -

'_config.yml' file에 profile 이미지 경로에서 힌트를 얻어 
로컬저장소에서 이미지 경로 url를 설정하는 법을 알아냈다
 
> '/assets/img/sample/avatar.jpg'


알고 보니 내가 선택한 jekyll-theme-chirpy에도 이미지 포스팅하는 법이 자세히 나와 있었다~


## Images

### Preview image

If you want to add an image to the top of the post contents, specify the url for the image by:

```yaml
---
image: /path/to/image-file
---
```

### Image caption

Add italics to the next line of an image，then it will become the caption and appear at the bottom of the image:

```markdown
![img-description](/path/to/image)
_Image Caption_
```

### Image size

You can specify the width (and height) of a image with `width`:

```markdown
![Desktop View](/assets/img/sample/mockup.png){: width="400"}
```

### Image position

By default, the image is centered, but you can specify the position by using one of class `normal` , `left` and `right`. For example:

- **Normal position**

  Image will be left aligned in below sample:

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: width="350" .normal}
  ```

- **Float to the left**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: width="240" .left}
  ```

- **Float to the right**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: width="240" .right}
  ```

> **Limitation**: Once you specify the position of an image, it is forbidden to add the image caption.

출처:  
[jekyll-theme-chirpy 이미지 포스트 방법](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/_posts/2019-08-08-write-a-new-post.md#images)