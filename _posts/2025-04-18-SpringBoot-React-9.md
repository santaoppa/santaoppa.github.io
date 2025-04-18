---
title: "[React] 게시판 프로젝트 - (9) React 상태 관리 최적화"
excerpt: ""

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-18
last_modified_at: 2025-04-18
---

## ‼️ 문제점

입력값에 따라 각 상태를 개별적으로 관리하고, `changeHandler`를 각각 선언하는 방식으로 작성했을 때  
상태가 많아질수록 코드가 복잡해지고 각 입력 필드에 대해 상태와 핸들러를 별도로 관리해야함  


## 기존 코드
```js
const [currentPassword, setCurrentPassword] = useState(null);
const [newPassword, setNewPassword] = useState(null);
const [confirmPassword, setConfirmPassword] = useState(null);

/* 이하 생략 */

<Form.Group controlId="currentPassword">
    <Form.Label>현재 비밀번호</Form.Label>
    <Form.Control
        type="password"
        placeholder="현재 비밀번호"
        value={currentPassword}
        onChange={(e) => setCurrentPassword(e.target.value)}
    />
</Form.Group>

<Form.Group controlId="newPassword">
    <Form.Label>새 비밀번호</Form.Label>
    <Form.Control
        type="password"
        placeholder="새 비밀번호"
        value={newPassword}
        onChange={(e) => setNewPassword(e.target.value)}
    />
</Form.Group>

<Form.Group controlId="confirmPassword">
    <Form.Label>새 비밀번호 확인</Form.Label>
    <Form.Control
        type="password"
        placeholder="새 비밀번호 확인"
        value={confirmPassword}
        onChange={(e) => setConfirmPassword(e.target.value)}
    />
</Form.Group>
```

## 바뀐 코드
상태를 그룹화하여 form 객체를 하나의 상태로 관리하고, `onChange` 핸들러를 하나로 통합하여 중복 코드를 줄이고, 상태 관리를 더 깔끔하게 할 수 있음  
이렇게 하면 추후 상태를 추가하거나 수정하기 쉬워짐 👍  

```js
const [form, setForm] = useState ({
    currentPassword: '',
    newPassword: '',
    confirmPassword: ''
});

/* 이하 생략 */

<Form.Group controlId="currentPassword">
    <Form.Label>현재 비밀번호</Form.Label>
    <Form.Control
        type="password"
        placeholder="현재 비밀번호"
        value={form.currentPassword}
        onChange={(e) => setForm({...form, currentPassword: e.target.value})}
        required
    />
</Form.Group>

<Form.Group controlId="newPassword">
    <Form.Label>새 비밀번호</Form.Label>
    <Form.Control
        type="password"
        placeholder="새 비밀번호"
        value={form.newPassword}
        onChange={(e) => setForm({...form, currentPassword: e.target.value})}
        required
    />
</Form.Group>

<Form.Group controlId="confirmPassword">
    <Form.Label>새 비밀번호 확인</Form.Label>
    <Form.Control
        type="password"
        placeholder="새 비밀번호 확인"
        value={form.confirmPassword}
        onChange={(e) => setForm({...form, currentPassword: e.target.value})}
        required
    />
</Form.Group>
```
