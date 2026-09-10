# Hi there, I'm devers2 👋

🌐 **English** | [한국어](README.ko.md)

Full-Stack Software Engineer & Open-Source Creator. Passionate about high-performance Java architectures, eliminating developer friction, and developer ergonomics.

---

## 🌟 Flagship Project: [S2Util](https://github.com/devers2/s2-util) & [s2-validator](https://github.com/devers2/s2-util/tree/main/s2-validator)

[![Maven Central](https://img.shields.io/maven-central/v/io.github.devers2/s2-validator?color=brightgreen&label=Maven%20Central)](https://central.sonatype.com/artifact/io.github.devers2/s2-validator)
[![Java 17+](https://img.shields.io/badge/Java-17%2B-blue?logo=openjdk)](https://openjdk.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-orange.svg)](https://github.com/devers2/s2-util/blob/main/LICENSE)

> **"Tired of writing validation rules twice in Java and JavaScript? Tired of annotation hell for conditional fields?"**
> **S2Validator** is a next-generation dynamic validation framework that lets you **Write Once, Validate Anywhere** — from server-side Java DTOs to native browser HTML with **zero frontend JavaScript code**.

### 💡 Why S2Validator over Standard Bean Validation?

| Pain Points in Real-World Enterprise | Standard Bean Validation (JSR-380) | ⭐ S2Validator |
| :--- | :--- | :--- |
| **Conditional Fields** *(If A then B required)* | Verbose `@GroupSequenceProvider` or custom annotation classes ❌ | Expressive in 2 lines: `.when("type", "VIP").rule(REQUIRED)` ✅ |
| **Cross-Field Comparison** *(pw == confirmPw)* | Class-level annotations; errors bound to root object ❌ | Directly bound to the target field: `.rule(EQUALS_FIELD, "pw")` ✅ |
| **Browser / Frontend Sync** | Server-only. Must duplicate identical regex/rules in JS/TS ❌ | **Zero frontend code**: `getRulesJson()` + native tooltip auto-focus ✅ |
| **Korean Particle Grammar** 🇰🇷 | Complex custom `MessageInterpolator` required ❌ | Built-in smart postpositions (`{0\|은/는}`, `{0\|이/가}`) ✅ |
| **Typo Protection** | Misspelled field names fail silently until runtime ❌ | Compile-time AST check via `s2-validator-plugin` 🛡️ ✅ |

### 🚀 30-Second Quick Taste

#### 1. Instant Backend Validation (No frontend setup required)
Validate any DTO, VO, or Map in place with zero boilerplate. Omit `.rule()` to enforce `REQUIRED` by default:

```java
// Throws S2RuntimeException immediately on the first failure (Fail-Fast)
S2Validator.of(command)
    .field("name", "Name") // Rule omitted -> REQUIRED by default
    .field("email", "Email").rule(S2RuleType.EMAIL) // Specifying rules disables default REQUIRED (optional); add REQUIRED explicitly if needed
    .field("birthDate", "Birth Date").rule(S2RuleType.REQUIRED).rule(S2RuleType.DATE)
    .validate();

// Or collect all errors without throwing:
List<S2ValidationError> errors = new ArrayList<>();
boolean isValid = S2Validator.of(command)
    .field("name", "Name")
    .field("email", "Email").rule(S2RuleType.EMAIL)
    .validate(errors::add);
```

#### 2. Full-Stack Sync (Zero frontend JavaScript code)
When client-side UI validation is needed, define rules once and sync them directly to the browser:

```java
// Server: Validate with Spring BindingResult and pass rules JSON to view
S2BindValidator.context("signUp", this::signUpRules).validate(command, result);
model.addAttribute("rules", S2BindValidator.context("signUp", this::signUpRules).getRulesJson());
```

```html
<!-- Client: Native browser tooltips & auto-focus work automatically! -->
<form th:data-s2-rules="${rules}">
```

🔗 **Dive Deeper into the Ecosystem:**
- 🏛️ **[S2Util Suite Repository →](https://github.com/devers2/s2-util)**: Explore the complete utility toolkit (`s2-core`, `s2-validator`, `s2-jpa`) and quick start guide
- 📖 **[s2-validator Deep Dive & Guide →](https://github.com/devers2/s2-util/tree/main/s2-validator)**: Complete rule reference, Spring MVC binding, and Thymeleaf/HTML integration

---

## 📦 S2 Project Suite

| Project / Module | Description | Repository |
| :--- | :--- | :---: |
| **[`s2-validator`](https://github.com/devers2/s2-util/tree/main/s2-validator)** | ⭐ Unified dynamic cross-platform validation engine & Spring binding integration | [s2-util](https://github.com/devers2/s2-util) |
| **[`s2-validator-plugin`](https://github.com/devers2/s2-util/tree/main/s2-validator-plugin)** | Gradle static analysis plugin — catches DTO field typos at compile time | [s2-util](https://github.com/devers2/s2-util) |
| **[`s2-core`](https://github.com/devers2/s2-util/tree/main/s2-core)** | High-performance Java utility toolkit (MethodHandle Reflection, W-TinyLFU Cache, Dates, Strings) | [s2-util](https://github.com/devers2/s2-util) |
| **[`s2-jpa`](https://github.com/devers2/s2-util/tree/main/s2-jpa)** | Fluent dynamic JPQL query builder with SQL injection prevention | [s2-util](https://github.com/devers2/s2-util) |
| **[`s2-support`](https://github.com/devers2/s2-support)** | Opinionated companion library for application-level workflows (Pagination, SFTP/Files, Spring Context) | [s2-support](https://github.com/devers2/s2-support) |
| **[`s2-build-support`](https://github.com/devers2/s2-build-support)** | Convention Gradle plugin for automated licensing, copyright, and publication | [s2-build-support](https://github.com/devers2/s2-build-support) |

---

## 🛠️ Tech Stack & Skills

- **Languages:** Java (17 / 21+), Kotlin, SQL, JavaScript
- **Frameworks & Tech:** Spring Boot, Spring MVC, Spring Data JPA, Hibernate, Caffeine Cache
- **Build & CI/CD:** Gradle, Maven Central Publishing, GitHub Actions, Docker
- **Architecture:** Clean Architecture, High-Performance I/O & Concurrency, AST Static Analysis

---

## 📫 Connect

- **GitHub:** [@devers2](https://github.com/devers2)
- **Email:** [eseungsu.dev@gmail.com](mailto:eseungsu.dev@gmail.com)
