---
title: "[React] 프로퍼티(props)란?"
excerpt: "React에서 컴포넌트 간 데이터를 전달하는 방법"

categories:
  - React
tags:
  - [Web, React]

toc: true
toc_sticky: true

date: 2025-04-23
last_modified_at: 2025-04-23
---

## props란?
`props`는 JSX 태그를 통해 컴포넌트에 전달하는 **데이터**로 
React에서는 컴포넌트 간에 정보를 주고받을 때 `props`를 활용함

- 부모 컴포넌트는 `props`를 통해 자식 컴포넌트에 **데이터 전달** 가능
- 자식 컴포넌트는 전달받은 `props`를 읽기 전용으로 사용* 
  
## props 전달하기
```js
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```
  
```js
function Avatar({ person, size }) {
  // person과 size는 이곳에서 사용 가능
}
```

### 1️⃣ 객체 전달하기 객체 형태로 전달하기 (Spread 문법)
반복되는 props는 **spread 연산자(...)**를 활용하여 간결하게 전달할 수 있음

```js
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

```js
function Avatar(props) {
  let person = props.person;
  let imageId = props.imageId;
  // ...
}
```
  
### 2️⃣ 배열 전달하기
배열 형태의 데이터를 전달하고, 내부에서 구조 분해 할당으로 처리할 수 있음
```js
export default function Profile() {

  const items = [
    {
      person: '홍길동',
      imageId: '1bX5QH6'
    },
    {
      person: '에디슨',
      imageId: '1bX5QH6'
    }
  ]
  return (
    <Avatar items = {items}/>
  );
}
```
  
```js
function Avatar( {items}) {
  const [avatar1, avatar2, avatar3] = items;
}
```
  
### 3️⃣ 콘텐츠로 컴포넌트 전달하기
컴포넌트 태그 사이에 내용을 작성하면, 해당 내용은 `props.children`으로 자식 컴포넌트에 전달됨

```js
export default function Profile() {
  return (
    <div>
      <Card>
        <h1>Photo</h1>
        <img
          className="avatar"
          src="https://i.imgur.com/OKS67lhm.jpg"
          alt="Aklilu Lemma"
          width={100}
          height={100}
        />
      </Card>
      <Card>
        <h1>About</h1>
        <p>Aklilu Lemma was a distinguished Ethiopian scientist who discovered a natural treatment to schistosomiasis.</p>
      </Card>
    </div>
  );
}
```
  
`<Card>...</Card>` 내부의 요소들은 Card 컴포넌트의 children으로 전달되어 렌더링됨


```js
function Card(children) {
  return (
    <div className="card">
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}
```
  
## 실습
![Image](https://github.com/user-attachments/assets/f76dfc1d-e3a8-4dbd-9fdb-486de00b6c8d)
### 변경 전
```js
import { getImageUrl } from './utils.js';

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <section className="profile">
        <h2>Maria Skłodowska-Curie</h2>
        <img
          className="avatar"
          src={getImageUrl('szV5sdG')}
          alt="Maria Skłodowska-Curie"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Profession: </b>
            physicist and chemist
          </li>
          <li>
            <b>Awards: 4 </b>
            (Nobel Prize in Physics, Nobel Prize in Chemistry, Davy Medal, Matteucci Medal)
          </li>
          <li>
            <b>Discovered: </b>
            polonium (chemical element)
          </li>
        </ul>
      </section>
      <section className="profile">
        <h2>Katsuko Saruhashi</h2>
        <img
          className="avatar"
          src={getImageUrl('YfeOqp2')}
          alt="Katsuko Saruhashi"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Profession: </b>
            geochemist
          </li>
          <li>
            <b>Awards: 2 </b>
            (Miyake Prize for geochemistry, Tanaka Prize)
          </li>
          <li>
            <b>Discovered: </b>
            a method for measuring carbon dioxide in seawater
          </li>
        </ul>
      </section>
    </div>
  );
}
```

### ➡️ 변경 후

```js
import { getImageUrl } from './utils.js';

function Profile({ person, imageSize = 70 }) {
  const imageSrc = getImageUrl(person)

  return (
    <section className="profile">
      <h2>{person.name}</h2>
      <img
        className="avatar"
        src={imageSrc}
        alt={person.name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li>
          <b>Profession:</b> {person.profession}
        </li>
        <li>
          <b>Awards: {person.awards.length} </b>
          ({person.awards.join(', ')})
        </li>
        <li>
          <b>Discovered: </b>
          {person.discovery}
        </li>
      </ul>
    </section>
  )
}

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <Profile person={{
        imageId: 'szV5sdG',
        name: 'Maria Skłodowska-Curie',
        profession: 'physicist and chemist',
        discovery: 'polonium (chemical element)',
        awards: [
          'Nobel Prize in Physics',
          'Nobel Prize in Chemistry',
          'Davy Medal',
          'Matteucci Medal'
        ],
      }} />
      <Profile person={{
        imageId: 'YfeOqp2',
        name: 'Katsuko Saruhashi',
        profession: 'geochemist',
        discovery: 'a method for measuring carbon dioxide in seawater',
        awards: [
          'Miyake Prize for geochemistry',
          'Tanaka Prize'
        ],
      }} />
    </div>
  );
}
```