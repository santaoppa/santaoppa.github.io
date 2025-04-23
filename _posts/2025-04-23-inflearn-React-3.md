---
title: "[React] 조건부/리스트 렌더링"
excerpt: ""

categories:
  - React
tags:
  - [Web, React]

toc: true
toc_sticky: true

date: 2025-04-23
last_modified_at: 2025-04-23
---

## 조건부 렌더링
### if문
```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          isPacked={true}
          name="Space suit"
        />
        <Item
          isPacked={true}
          name="Helmet with a golden leaf"
        />
        <Item
          isPacked={false}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}

```

### 삼항 조건 연산자
```js
return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
```

### 논리 AND 연산자 
```js
return (
  <li className="item">
    {name} {isPacked && '✅'}
  </li>
);
```
  
## 리스트 렌더링
JavaScript 객체와 배열에 저장하고 map()과 filter() 같은 메서드를 사용하여 해당 객체에서 컴포넌트 리스트를 렌더링할 수 있음

```js
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```
  
> 🚨 **주의사항**<br/>
> map() 호출 내부의 JSX 엘리먼트에는 항상 **key**가 필요함. 따라서 리스트 렌더링 시 각 배열 항목에 다른 항목 중에서 고유하게 식별할 수 있는 문자열 또는 숫자를 key로 지정해야 한다!
```js
<li key={person.id}>...</li>
```
  
### 리스트 항목 필터링하기
1. 필터링한 새로운 배열 생성하기
```js
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```

2. 배열 매핑하기
```js
const listItems = chemists.map(person =>
  <li>
     <img
       src={getImageUrl(person)}
       alt={person.name}
     />
     <p>
       <b>{person.name}:</b>
       {' ' + person.profession + ' '}
       known for {person.accomplishment}
     </p>
  </li>
);
```

3. 컴포넌트에서 반환하기
```js
return <ul>{listItems}</ul>;
```
  
### 각 리스트 항목에 대해 여러 DOM 노드 표시하기
각 항목이 하나가 아닌 여러 개의 DOM 노드를 렌더링해야하는 경우
`<Fragment>` 문법을 사용해야 한다!
  
```js
import { Fragment } from 'react';

// ...

function CourseListCard({ title, items }) {

 const lastIndex = items.length - 1;

  return (
    <Card title={title}>
      <div className='courses'>
        {items.map((item, index) => (
          <Fragment key={item.id}>
            <CourseItem {...item} />
            {index !== lastIndex && <hr className="divider" />}
          </Fragment>
        ))}
      </div>
    </Card>
  );
}
```