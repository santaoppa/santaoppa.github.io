---
title: "[Spring] @PathVariable란?"

categories:
  - Spring
tags:
  - [Spring]

toc: true
toc_sticky: true

date: 2025-04-10
last_modified_at: 2025-04-10
---

## `@PathVariable`란?
`@PathVariable`은 URL 경로에 포함된 변수를 추출하기 위해 메서드의 매개변수에 사용하는 어노테이션으로 요청된 URL 경로의 특정 값을 변수로 받아와 컨트롤러 메서드에서 사용 가능  

아래 URL에서 밑줄 친 부분을 @PathVariable로 처리 가능  
localhost:3000/api/posts/<U>3</U>  


💡 기본적으로 경로 변수는 반드시 값을 가져야 하며, 값이 없는 경우 404 오류가 발생한다.