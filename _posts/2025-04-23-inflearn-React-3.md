---
title: "[React] 조건부/리스트 렌더링"
excerpt: "조건에 따라 컴포넌트를 렌더링하거나 배열을 기반으로 반복 렌더링하는 방법"

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
### 1. `if`문 사용
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

### 2. 삼항 연산자 사용
```js
return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
```

### 3. 논리 AND (&&) 연산자 사용
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
> map()을 사용할 때는 반드시 고유한 key 값을 각 요소에 지정해야 함!
```js
<li key={person.id}>...</li>
```
  
### 리스트 항목 필터링
1. .filter()로 새 배열 만들기
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```

2. .map()으로 컴포넌트 생성
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

3. 컴포넌트에서 반환
```js
return <ul>{listItems}</ul>;
```
  
### 하나의 컴포넌트에 중첩된 리스트 
```js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe =>
        <div key={recipe.id}>
          <h2>{recipe.name}</h2>
          <ul>
            {recipe.ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
            )}
          </ul>
        </div>
      )}
    </div>
  );
}
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

### ⚠️ forEach 함수
React에서 `forEach`는 `map()`과 헷갈리는 경우가 많음.
하지만 `forEach`는 배열의 요소를 순회만 할 뿐 컴포넌트를 반환해 주는 메서드가 아님

```js
배열.forEach((요소, 인덱스, 배열) => {
  // 실행할 코드
});  
```