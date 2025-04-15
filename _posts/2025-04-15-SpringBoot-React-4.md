---
title: "[React] 게시판 프로젝트 - (4) JWT 인증 방식(Spring)"

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-15
last_modified_at: 2025-04-15
---

## JWT란?
JSON 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web 토큰  

### JWT 구조
![Image](https://github.com/user-attachments/assets/87df6430-6291-44ac-9588-98b1b24f3770)
JWT는 Header, Payload, Signature 세가지로 구성됨  
- `Header` : `Signature`를 해싱하기 위한 알고리즘 정보
- `Payload` : 서버와 클라이언트가 서로 주고받을 실질적인 정보
- `Signature` : 토큰의 유효성 검증을 위한 문자열  

## 기본 인증 방식
1. `UsernamePasswordAuthenticationFilter`가 폼 로그인 처리 담당
2. POST 요청에서 ID/PW 꺼내서 인증함
3. 인증 성공 시 → 세션 만들어서 `SecurityContext`에 저장  

> `SecurityContext`란? 현재 사용자의 인증(Authentication) 정보를 담고 있는 Spring Security의 저장소  


## JWT 인증 방식
1. 로그인 성공 시 → Access Token + Refresh Token 발급
2. Refresh Token은 DB에 저장하고, Access Token은 클라이언트에게 응답
3. 클라이언트가 API 요청 시, Access Token이 만료되었으면 서버는 DB에 저장된 Refresh Token과 비교해 유효성 검증
5. 새 Access Token 발급 (필요시 Refresh Token도 갱신)
> 에러 핸들러에서 Refresh Token과 함께께 토큰을 새로 발급받는 페이지로 요청을 전송하고, 서버는 해당 Refresh Token을 검증 후 새로운 토큰을 발급함  

### Access Token
사용자 인증 정보를 담고 있는 짧은 수명의 토큰으로
클라이언트 측(브라우저 또는 앱)에서 로컬 스토리지, 세션 스토리지, 메모리, 쿠키 등에 저장함

✅ 요청 시 Authorization: Bearer {AccessToken} 헤더로 전송  

### Refresh Token
Access Token이 만료되었을 때, 새로운 Access Token을 발급받기 위한 토큰으로
보통 서버에서 DB에 저장하고, 클라이언트는 HttpOnly 쿠키로 저장함

✅ 토큰 탈취에 대비해서 사용자별로 서버 측에서 상태를 갖고 관리  


### `JwtUtil.java`
Token의 생성, 분석, 유효성 검사 수행  

```java
// 토큰 생성
public String generateToken(Authentication authentication, String type) {
    CustomUserDetails customUserDetails = (CustomUserDetails) authentication.getPrincipal();
    String expirationTime = type.equals("access") ? TOKEN_ACCESS_TIME : TOKEN_REFRESH_TIME;
    Date expirationDate = new Date(System.currentTimeMillis() + Long.parseLong(expirationTime));
    return Jwts.builder()
            .setSubject(customUserDetails.getUserId())
            .setIssuedAt(new Date())
            .setExpiration(expirationDate)
            .signWith(SignatureAlgorithm.HS512, TOKEN_SECRET)
            .compact();
}

// 토큰 분석
public String getUserIdFromToken(String token) {
    return Jwts.parserBuilder().setSigningKey(TOKEN_SECRET).build().parseClaimsJws(token).getBody().getSubject();
}

// 토큰 유효성 검사
public Boolean validateToken(String token) {
    try {
        Jwts.parser().setSigningKey(TOKEN_SECRET).parseClaimsJws(token);
        return true;
    } catch (SignatureException e) {
        System.out.println("[JWT] 서명 오류: 위조된 JWT 서명입니다.");
    } catch (MalformedJwtException e) {
        System.out.println("[JWT] 구조 오류: 잘못된 JWT 형식입니다.");
    } catch (ExpiredJwtException e) {
        System.out.println("[JWT] 만료됨: JWT의 유효 기간이 지났습니다.");
    } catch (UnsupportedJwtException e) {
        System.out.println("[JWT] 지원되지 않음: 이 JWT는 지원되지 않습니다.");
    } catch (IllegalArgumentException e) {
        System.out.println("[JWT] 비어 있음: JWT 클레임이 비어있습니다.");
    }

    return false;
}
```

### `JwtTokenFilter.java`
JWT 인증을 처리할 커스텀 필터로 클라이언트가 보낸 HTTP 요청에 포함된 JWT 토큰을 꺼내서, 유효하면 사용자 인증 정보를 만들어 Spring Security의 `SecurityContext`에 등록하는 필터 

```java
// JWT 인증 처리
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
      String accessToken = getTokenFromRequest(request);
      if (accessToken != null && jwtUtil.validateToken(accessToken)) {
          UsernamePasswordAuthenticationToken authentication = getAuthenticationFromToken(accessToken);
          authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
          SecurityContextHolder.getContext().setAuthentication(authentication);
      }
      filterChain.doFilter(request, response);
  }

  // 토큰 추출
  private String getTokenFromRequest(HttpServletRequest request) {
      String bearerToken = request.getHeader("Authorization");
      if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
          return bearerToken.substring(7);
      }
      return null;
  }

  // 토큰으로 유저 정보 조회
  private UsernamePasswordAuthenticationToken getAuthenticationFromToken(String token) {
      String userId = jwtUtil.getUserIdFromToken(token);
      UserDetails userDetails = customUserDetailsService.loadUserByUserId(userId);
      return new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
  }
```  

> `UsernamePasswordAuthenticationToken`란? Spring Security에서 사용되는 인증을 나타내는 클래스로 주로 사용자명과 비밀번호를 담고 인증을 수행하는 데 활용됨
