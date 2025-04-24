---
title: "[Web] 버블링 & 캡쳐링"
excerpt: ""

categories:
  - Web
tags:
  - [Web, JavaScript]

toc: true
toc_sticky: true

date: 2025-04-23
last_modified_at: 2025-04-23
---

![Image](https://github.com/user-attachments/assets/5c14cafe-0908-4410-8da7-a9414d48973d)


## 이벤트 흐름

브라우저의 이벤트는 두 단계를 거쳐 흐름

- **버블링 단계(Bubbling Phase)**: 이벤트가 발생한 요소에서 다시 최상위 요소까지 올라감
- **캡처링 단계(Capturing Phase)**: 최상위 요소에서 이벤트가 발생한 요소까지 내려감

기본적으로 `addEventListener`는 **버블링** 방식으로 동작함
  

##  버블링
`addEventListener`는 버블링으로 동작하기 때문에 `span` 태그 클릭하면 `body` 태그 영역까지 이벤트가 실행됨

```js
  const $body = document.querySelector('body');
  const $main = document.querySelector('main');
  const $div = document.querySelector('div');
  const $p = document.querySelector('p');
  const $span = document.querySelector('span');

  $span.addEventListener('click', function(){
    console.log('span 태그');
  })
  $p.addEventListener('click', function(){
    console.log('p 태그');
  })
  $div.addEventListener('click', function(){
    console.log('div 태그');
  })
  $main.addEventListener('click', function(){
    console.log('main 태그');
  })
  $body.addEventListener('click', function(){
    console.log('body 태그');
  })
```

## ✅ 클릭 시 결과
```
span 태그
p 태그
div 태그
main 태그
body 태그
```

# 캡쳐링
캡처링을 사용하려면 `addEventListener`의 세 번째 인자에 true를 전달

```js
  $span.addEventListener('click', function(){
    console.log('capturing span 태그');
  }, true)
  $p.addEventListener('click', function(){
    console.log('capturing p 태그');
  }, true)
  $div.addEventListener('click', function(){
    console.log('capturing div 태그');
  }, true)
  $main.addEventListener('click', function(){
    console.log('capturing main 태그');
  }, true)
  $body.addEventListener('click', function(){
    console.log('capturing body 태그');
  }, true)
  $span.addEventListener('click', function(){
    console.log('bubbling span 태그');
  })
  $p.addEventListener('click', function(){
    console.log('bubbling p 태그');
  })
  $div.addEventListener('click', function(){
    console.log('bubbling div 태그');
  })
  $main.addEventListener('click', function(){
    console.log('bubbling main 태그');
  })
  $body.addEventListener('click', function(){
    console.log('bubbling body 태그');
  })
```

## ✅ 클릭 시 결과
```
capturing body 태그
capturing main 태그
capturing div 태그
capturing p 태그
capturing span 태그
bubbling span 태그
bubbling p 태그
bubbling div 태그
bubbling main 태그
bubbling body 태그
```

# 이벤트 막기
## 1. `stopPropagation()`
이벤트 전파(캡처링 & 버블링)을 중단함
```js
  $span.addEventListener('click', function(){
    console.log('capturing span 태그');
  }, true)
  $p.addEventListener('click', function(){
    event.stopPropagation();
    console.log('capturing p 태그');
  }, true)
  $div.addEventListener('click', function(){
    console.log('capturing div 태그');
  }, true)
  $main.addEventListener('click', function(){
    console.log('capturing main 태그');
  }, true)
  $body.addEventListener('click', function(){
    console.log('capturing body 태그');
  }, true)
  $span.addEventListener('click', function(){
    console.log('bubbling span 태그');
  })
  $p.addEventListener('click', function(){
    console.log('bubbling p 태그');
  })
  $div.addEventListener('click', function(){
    console.log('bubbling div 태그');
  })
  $main.addEventListener('click', function(){
    console.log('bubbling main 태그');
  })
  $body.addEventListener('click', function(){
    console.log('bubbling body 태그');
  })
```

### ✅ 클릭 시 결과
```
capturing body 태그
capturing main 태그
capturing div 태그
capturing p 태그
```

## 2.`Event.preventDefault()`
기본 동작 방지
```js
  <a href="http://naver.com">네이버 이동</a>

  <script>
    const $a = document.querySelector('a')
    $a.addEventListener('click', function(event){
      event.preventDefault();
    })
  </script>
```