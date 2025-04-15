---
title: "[React] 게시판 프로젝트 - (5) JWT 인증 구현(Spring)"

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## 의존성 추가
```xml
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```   

## 애플리케이션 구성 설정
`application.properties`
```
token.secretKey=secret_key
token.access_expiration_time=3600000
token.refresh_expiration_time=86400000
```  


## `WebSecurityConfig.java`
Spring Security의 보안 설정을 정의  

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http
        .cors(Customizer.withDefaults())  // 1) 
        .csrf(csrf -> csrf.disable()) // 2)
        .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        ) // 3)
        .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers(HttpMethod.GET, "/api/post/**").permitAll()
                .requestMatchers("/api/**").authenticated()
        ) // 4)
        .addFilterBefore(jwtTokenFilter, UsernamePasswordAuthenticationFilter.class) //5)
        .build();
}
```  
1. 프론트엔드와 백엔드가 분리돼 있을 떄 CORS 오류를 방지하기 위해 설정  
2. CSRF(사이트 간 요청 위조) 보호 비활성화  
<details>
    <summary>CSRF(Cross-site Request Forgery)란?</summary>
    <div markdown="1">
       로그인된 사용자의 권한을 도용해서 원치 않는 요청을 서버에 보내는 공격으로 세션 기반 인증일 때 가능함. <br>
       하지만 JWT는 헤더로 직접 인증정보를 보내기 때문에 CSRF 공격이 불가능함!
    </div>
</details>  

3. JWT 기반 인증에서는 서버가 세션을 만들지 않기 떄문에 비활성화  
<details>
    <summary>세션 기반 인증이란?</summary>
    <div markdown="1">
        1) 로그인 시, 서버가 DB에서 사용자 정보 조회<br>
        2) 세션ID와 사용자 정보를 세션 저장소에 저장<br>
        3) 이후 요청마다 클라이언트는 쿠키에 담긴 JSESSIONID 자동 전송<br>
        4) 서버는 JSESSIONID로 세션 저장소에서 사용자 정보를 꺼냄<br>
        ✅ 서버는 쿠키 내의 세션 ID를 키로 해서 서버가 직접 메모리에 사용자의 로그인 상태와 정보(예: 이름, 권한 등)를 기억하고 있음 <br>
        ➡️ 서버가 상태(state)를 가짐<br><br>
        반면, JWT 기반 인증이란?<br>
        1) 로그인 시, 서버가 JWT 토큰 생성해서 클라이언트에 전달<br>
        2) 이후 요청마다 클라이언트는 Authorization 헤더에 토큰을 붙여서 요청<br>
        3) 서버는 그때그때 토큰 자체만 검증<br>
        ✅ 서버는 사용자의 로그인 상태 모름. 정보 모름. 그냥 토큰이 유효한지 검증함<br>
        ➡️ 서버는 상태를 기억하지 않음 → stateless
    </div>
</details>  

4. 어떤 API에 어떤 사용자를 허가할 것인지 설정  
5. JWT 인증을 처리할 커스텀 필터(`JwtTokenFilter`)를 Spring Security의 기본 로그인 필터(`UsernamePasswordAuthenticationFilter`) 앞에 끼워 넣는다는 의미
<details>
    <summary>Spring Security 기본 인증 흐름</summary>
    <div markdown="1">
        1) UsernamePasswordAuthenticationFilter가 폼 로그인 처리 담당<br>
        2) POST 요청에서 ID/PW 꺼내서 인증함<br>
        3) 인증 성공 시 → 세션 만들어서 SecurityContext에 저장함<br><br>
        ✔️ SecurityContext란? 현재 사용자의 인증(Authentication) 정보를 담고 있는 Spring Security의 저장소
    </div>
</details>

<details>
    <summary>Spring Security 6.1부터 변경된 보안 설정 방식</summary>
    <div markdown="1">
        - 인증 설정 메서드 : authorizeRequests() ➡️ authorizeHttpRequests()<br>
        - URL 패턴 지정 : antMatchers ➡️ requestMatchers<br>
        - 설정 방식 : 체인형(.) ➡️ 람다식
    </div>
</details>  



## 인증 필터 처리 흐름 순서
>[POST `/api/auth/login`]<br>
>      ↓<br>
>[컨트롤러] → [서비스] → DB 사용자 조회 & 토큰 생성 → 응답 헤더에 JWT 세팅<br>
>      ↓<br>
>[클라이언트] 응답 헤더에서 accessToken, refreshToken 저장<br>
>      ↓<br>
>[이후 요청] Authorization: Bearer accessToken<br>
>      ↓<br>
>[`JwtTokenFilter`] → 토큰 추출 → 검증 → 인증 객체 생성 → `SecurityContext `등록<br>  
  

1. 클라이언트가 로그인 요청 (`/api/auth/login`)
```
POST /api/auth/login
Content-Type: application/json

{
  "userId": "testuser",
  "password": "test1234"
}
```

2. 해당 URL 매핑된 `UserController`의 `login()` 메서드 실행
```java
@PostMapping("/api/auth/login")
public ResponseEntity<?> login(HttpServletResponse response, @RequestBody UserRequestDto requestDto){
    UserResponseDto responseDto = userService.login(response, requestDto);
    return ResponseEntity.ok(responseDto);
}
```

3. `UserService`의 `login()` 메서드 실행
```java
@Transactional
public UserResponseDto login(HttpServletResponse response, UserRequestDto userRequestDto) {
    // 1. DB에서 유저 조회
    User user = this.userRepository.findByUserId(userRequestDto.getUserId()).orElseThrow(...);

    // 2. 비밀번호 비교
    if (!passwordEncoder.matches(userRequestDto.getPassword(), user.getPassword())) {
        throw new IllegalArgumentException("Wrong password");
    }

    // 3. JWT 토큰 생성
    String accessToken = jwtUtil.generateToken(..., "access");
    String refreshToken = jwtUtil.generateToken(..., "refresh");

    // 4. DB에 refresh token 저장
    userRepository.save(user.updateRefreshToken(refreshToken));

    // 5. 토큰을 응답 헤더에 세팅
    response.addHeader("Authorization", accessToken);
    response.addHeader("Refresh-Token", refreshToken);

    return new UserResponseDto(user);
}
```

4. 프론트는 응답 헤더에서 토큰 저장  
```js
const accessToken = response.headers['authorization'];
const refreshToken = response.headers['refresh-token'];

localStorage.setItem("accessToken", accessToken);
localStorage.setItem("refreshToken", refreshToken);

// 이후 요청에 헤더 설정
axios.defaults.headers.common['Authorization'] = `Bearer ${accessToken}`;
```

5. 이후 요청시 `JwtTokenFilter`의 `doFilterInternal()` 동작  
```java
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) {
    // 1. 헤더에서 토큰 추출
    String accessToken = getTokenFromRequest(request); // "Bearer xxx..." → substring(7)

    // 2. 토큰 유효성 검사
    if (accessToken != null && jwtUtil.validateToken(accessToken)) {
        // 3. 토큰에서 사용자 정보 꺼내 인증 객체 생성
        UsernamePasswordAuthenticationToken authentication = getAuthenticationFromToken(accessToken);

        // 4. SecurityContext 에 인증 정보 저장
        SecurityContextHolder.getContext().setAuthentication(authentication);
    }

    // 5. 다음 필터 체인으로 넘김
    filterChain.doFilter(request, response);
}
```
