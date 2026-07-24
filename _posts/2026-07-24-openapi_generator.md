---
title: "[AI] OpenAPI Generator"
excerpt: ""

categories:
  - AI
tags:
  - [AI]

toc: true
toc_sticky: true

date: 2026-07-24
last_modified_at: 2026-07-24
---

이전 프로젝트에서 OpenAPI Generator로 백엔드의 Swagger 스펙을 읽어서   
TypeScript 클라이언트 코드(DTO 인터페이스 + API 클래스)를 자동 생성해줬음
<br>
<br>

# 전체 흐름
```
백엔드 (Swagger 어노테이션)
   ↓
OpenAPI 스펙 JSON (예: /v3/api-docs)
   ↓ (openapi-generator-cli)
프론트 TS 코드 자동 생성 (DTO + Api 클래스)
   ↓
직접 만든 Service 레이어에서 감싸서 사용
```
이렇게 하면 백엔드 DTO가 바뀔 때마다 프론트에서 맞춰줄 필요 없이,   
명령어 한 번으로 DTO/API 함수가 자동으로 떨어짐
<br>
<br>

# 백엔드 
## `springdoc-openapi` 추가
`build.gradle`에 의존성 추가:
```
implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:3.0.3'
```
추가하면 별다른 설정 없이도 아래 경로에서 OpenAPI 3 스펙(JSON)이 자동으로 노출됨

```
http://localhost:{port}/v3/api-docs
```
<br>

# 프론트엔드
## `openapi-generator-cli` 설치
```bash
npm install -D @openapitools/openapi-generator-cli
```
## 생성 스크립트 등록 (package.json)
```json
{
  "scripts": {
    "api-portal:local": "openapi-generator-cli generate -i http://localhost:8002/portal-service/v3/api-docs -g typescript-axios -o src/api/generated --skip-validate-spec"
  }
}
```
- `-i` : OpenAPI 스펙 URL (백엔드 실제 context-path 포함해서 정확히 넣어야 함)
- `-g typescript-axios` : axios 기반 TS 클라이언트 생성 템플릿
- `-o` : 생성 결과 저장 폴더
- `--skip-validate-spec` : 스펙 검증 스킵 (백엔드 스펙이 완벽하지 않아도 생성 진행)

## 실행
```bash
npm run api:local
```

### 생성 결과물
```
src/api/generated/
├── api.ts          # API 클래스 + DTO 인터페이스
├── base.ts         # BASE_PATH, Configuration 기반 클래스
├── common.ts
├── configuration.ts
└── index.ts        # re-export
```
`api.ts`에 Swagger 태그별로 API 클래스가 생성됨 (@Tag로 지정한 이름 기준)

이렇게 세팅해두면 백엔드 DTO/엔드포인트가 바뀔 때마다
npm run api:local 한 번으로 프론트 타입과 API 함수가 동기화된다! 🤓