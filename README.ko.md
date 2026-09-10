# 안녕하세요, devers2입니다 👋

🌐 [English](README.md) | **한국어**

풀스택 소프트웨어 엔지니어 & 오픈소스 개발자입니다. 고성능 Java/Spring 아키텍처, 개발자 생산성 향상, 그리고 실무의 고통을 덜어주는 도구 개발에 집중하고 있습니다.

---

## 🌟 대표 프로젝트: [s2-util](https://github.com/devers2/s2-util) & [s2-validator](https://github.com/devers2/s2-util/tree/main/s2-validator)

[![Maven Central](https://img.shields.io/maven-central/v/io.github.devers2/s2-validator?color=brightgreen&label=Maven%20Central)](https://central.sonatype.com/artifact/io.github.devers2/s2-validator)
[![Java 17+](https://img.shields.io/badge/Java-17%2B-blue?logo=openjdk)](https://openjdk.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-orange.svg)](https://github.com/devers2/s2-util/blob/main/LICENSE)

> **"백엔드 DTO에도 정규식 쓰고, 프론트엔드 JS에도 똑같은 검증 코드를 또 짜고 계신가요?"**
> **"비밀번호 확인이나 조건부 필수값 때문에 복잡한 어노테이션을 만드느라 지치셨나요?"**
>
> **s2-validator**는 **"한 번 작성하고, 어디서든 검증한다(Write Once, Validate Anywhere)"**는 철학으로 만들어진 차세대 동적 검증 프레임워크입니다. Java에서 단 한 번 정의하면 서버 검증은 물론, **프론트엔드 JavaScript 코드 0줄로 브라우저 네이티브 툴팁 검증까지 완벽하게 자동 동기화**됩니다.

### 💡 표준 Bean Validation 대비 차별점 (Why s2-validator?)

| 실무 개발의 고질적인 통곡의 벽 | 표준 Bean Validation (JSR-380) | ⭐ s2-validator |
| :--- | :--- | :--- |
| **동적 조건부 검증**<br>*(카드 결제일 때만 카드번호 필수)* | 커스텀 어노테이션 작성 또는 악명 높은 `@GroupSequenceProvider` 필요 (코드 급증) ❌ | 직관적인 체이닝 단 2줄로 해결:<br>`.when("payMethod", "CARD").rule(REQUIRED)` ✅ |
| **크로스 필드 비교**<br>*(비밀번호 일치, 기간 전후 관계)* | 클래스 레벨 어노테이션 작성 필요; 루트(Global) 에러로 박혀 필드별 표시 곤란 ❌ | 해당 필드에 에러가 정확히 바인딩됨:<br>`.rule(EQUALS_FIELD, "password")` ✅ |
| **클라이언트(브라우저) 연동** | 서버 전용. 프론트엔드에서 JS/TS(Zod 등)로 **동일 규칙 중복 코딩** 필수 ❌ | **프론트엔드 코드 0줄**: `getRulesJson()` 전달 시 브라우저 네이티브 툴팁/포커스 자동 처리 ✅ |
| **자연스러운 한국어 조사** 🇰🇷 | 기본 미지원. 받침 유무에 따른 커스텀 `MessageInterpolator` 직접 구현 ❌ | `{0\|은/는}`, `{0\|이/가}` 등 **받침에 따른 조사 자동 보정 기본 내장** ✅ |
| **필드 오타 / 리팩토링 안전성** | 문자열 기반 바인딩 실수 시 런타임에 에러 발생 위험 ❌ | **`s2-validator-plugin`**이 **컴파일 시점 AST 정적 분석으로 빌드 사전 차단** 🛡️ ✅ |

### 🚀 30초 코드 맛보기

#### 1. 즉시 백엔드 검증 (프론트엔드 연동 불필요 시)
DTO, VO, Map 상관없이 단 한 줄의 체이닝으로 즉시 검증합니다. 규칙을 생략하면 기본 필수값(`REQUIRED`)으로 자동 처리됩니다:

```java
// 실패 시 즉시 S2RuntimeException 발생 (Fail-Fast 모드)
S2Validator.of(command)
    .field("name", "이름") // 규칙 생략 시 기본 REQUIRED 자동 적용
    .field("email", "이메일").rule(S2RuleType.EMAIL) // 규칙 지정 시 기본 REQUIRED 미적용(선택 입력), 필수 체크 필요 시 명시적 추가 필요
    .field("birthDate", "생년월일").rule(S2RuleType.REQUIRED).rule(S2RuleType.DATE)
    .validate();

// 또는 예외 대신 전체 에러 목록을 수집:
List<S2ValidationError> errors = new ArrayList<>();
boolean isValid = S2Validator.of(command)
    .field("name", "이름")
    .field("email", "이메일").rule(S2RuleType.EMAIL)
    .validate(errors::add);
```

#### 2. 풀스택 자동 동기화 (프론트엔드 JavaScript 0줄)
클라이언트 입력 폼 검증이 필요할 때는 서버 규칙을 그대로 브라우저에 전달합니다:

```java
// Server: Spring BindingResult 검증 및 규칙 JSON 전달
S2BindValidator.context("signUp", this::signUpRules).validate(command, result);
model.addAttribute("rules", S2BindValidator.context("signUp", this::signUpRules).getRulesJson());
```

```html
<!-- Client: HTML 속성 하나로 브라우저 네이티브 툴팁 및 자동 포커스 동작! -->
<form th:data-s2-rules="${rules}">
```

🔗 **단계별로 더 깊이 살펴보기:**
- 🏛️ **[s2-util 통합 저장소 방문하기 →](https://github.com/devers2/s2-util)**: 전체 유틸리티 제품군(`s2-core`, `s2-validator`, `s2-jpa`) 개요 및 통합 시작 가이드
- 📖 **[s2-validator 상세 가이드 보기 →](https://github.com/devers2/s2-util/tree/main/s2-validator)**: 30여 종 전체 규칙 목록, Spring MVC 연동, Thymeleaf/HTML 프론트엔드 연동 튜토리얼

---

## 📦 S2 프로젝트 전체 구성

| 프로젝트 / 모듈 | 설명 | 저장소 |
| :--- | :--- | :---: |
| **[`s2-validator`](https://github.com/devers2/s2-util/tree/main/s2-validator)** | ⭐ 서버·클라이언트 통합 유효성 검증 엔진 & Spring 바인딩 통합 | [`s2-util`](https://github.com/devers2/s2-util) |
| **[`s2-validator-plugin`](https://github.com/devers2/s2-util/tree/main/s2-validator-plugin)** | Gradle 정적 분석 플러그인 — DTO 필드 오타 및 체이닝 누락(죽은 코드) 감지 | [`s2-util`](https://github.com/devers2/s2-util) |
| **[`s2-core`](https://github.com/devers2/s2-util/tree/main/s2-core)** | 고성능 Java 유틸리티 툴킷 (MethodHandle 리플렉션, W-TinyLFU 캐시, 날짜, 문자열) | [`s2-util`](https://github.com/devers2/s2-util) |
| **[`s2-jpa`](https://github.com/devers2/s2-util/tree/main/s2-jpa)** | SQL 인젝션 방어형 플루언트 동적 JPQL 쿼리 빌더 | [`s2-util`](https://github.com/devers2/s2-util) |
| **[`s2-support`](https://github.com/devers2/s2-support)** | 실무 애플리케이션 보조 라이브러리 (페이징, 파일/SFTP 관리, Spring 컨텍스트 유틸) | [`s2-support`](https://github.com/devers2/s2-support) |
| **[`s2-build-support`](https://github.com/devers2/s2-build-support)** | 라이선스·저작권·배포 자동화를 위한 Gradle 컨벤션 플러그인 | [`s2-build-support`](https://github.com/devers2/s2-build-support) |

---

## 🛠️ 기술 스택

- **언어:** Java (17 / 21+), Kotlin, SQL, JavaScript
- **프레임워크 & 기술:** Spring Boot, Spring MVC, Spring Data JPA, Hibernate, Caffeine Cache
- **빌드 & CI/CD:** Gradle, Maven Central 배포, GitHub Actions, Docker
- **아키텍처:** 클린 아키텍처, 고성능 I/O & 동시성, AST 정적 코드 분석

---

## 📫 연락처

- **GitHub:** [@devers2](https://github.com/devers2)
- **이메일:** [eseungsu.dev@gmail.com](mailto:eseungsu.dev@gmail.com)
