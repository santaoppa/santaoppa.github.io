---
title: "[Java] Lombok이란?"

categories:
  - Java
tags:
  - [Java, Lombok]

toc: true
toc_sticky: true

date: 2025-04-07
last_modified_at: 2025-04-07
---

### Lombok이란?
Lombok(롬복)은 반복되는 메소드를 Annotation을 사용해서 자동으로 작성해주는 Java 라이브러리이다.

보통 DTO나 Model, Entity와 같은 도메인 클래스 등이 가지는 프로퍼티에 대해서 Getter/Setter, 생성자 등의 메소드를 자동으로 만들어주는 기능을 한다.  



## Lombok getter/setter 어노테이션 사용 전
```java
public class UserDTO {

    private String id;
    private String name;

    public String getId(){
        return id;
    }

    public void setId(String id){
        this.id = id;
    }

    public String getName(){
        return name;
    }

    public void setName(String name){
        this.name = name;
    }

}
```  

## Lombok getter/setter 어노테이션 사용 후
```java
@Getter
@Setter
public class UserDTO {

    private String id;
    private String name;

}
```  

이러한 부분을 자동으로 만들어주고 Lombok을 이용해서 작성한 코드는 컴파일 과정에서 Annotation을 이용해서 코드를 생성하고 .class에 자동 컴파일 된다.  


## 자주 사용되는 Lombok 어노테이션

### `@NonNull`  
메소드나 생성자의 매개변수에 사용하면 null check를 한다.  

### `@AllArgsConstructor`  
모든 필드 값을 매개변수로 받는 생성자를 생성한다.  

```java
@Getter
@AllArgsConstructor
public class UserDTO {

    private String id;
    private String name;

    //public UserDto(String id, String name) {
    //    this.id = id;
    //    this.name = name;
    //}

}

```

### `@NoArgsConstructor`  
매개변수가 없는 생성자를 생성한다.  

```java
@Getter
@NoArgsConstructor
public class UserDTO {

    private String id;
    private String name;

    //public UserDto() {}

}

```

### `@RequiredArgsConstructor`  
`final`, `@NonNull`이 있는 필드를 포함한 생성자를 생성한다.  

```java
@Getter
@RequiredArgsConstructor
public class UserDTO {

    @NonNull
    private String id;
    private final String name;
    private String password;

    //public UserDto(String id, String name) {
    //    this.id = id;
    //    this.name = name;
    //}
}

```  

### `@EqualsAndHashCode`  
사용 객체에 대해서 `equals()`, `hashCode()` 메서드를 생성한다.  

```java
@RequiredArgsConstructor
@EqualsAndHashCode(of = {"id", "emailId"}) //Id와 emailId가 동일하면 같은 객체로 인식
public class UserDTO {

    @NonNull
    private String id;
    @NonNull
    private String emailId;
    private String name;

    //public UserDto(String id, String name) {
    //    this.id = id;
    //    this.name = name;
    //}

}

```  

### `@Data`  
모든 필드에 대해 `@ToString`, `@EqualsAndHashCode`, `@Getter`를, 모든 non-final 필드에 대해 `@Setter`를 설정하고 `@RequiredArgsConstructor`를 생성해주는 단축 Annotation
