# Hi there, I'm devers2 👋

🌐 **English** | [한국어](README.ko.md)

Full-Stack Software Engineer & Open-Source Creator. Passionate about high-performance Java architectures, eliminating developer friction, and developer ergonomics.

---

## 🌟 Flagship Project: [s2-util](https://github.com/devers2/s2-util) & [s2-validator](https://github.com/devers2/s2-util/tree/main/s2-validator)

[![Maven Central](https://img.shields.io/maven-central/v/io.github.devers2/s2-validator?color=brightgreen&label=Maven%20Central)](https://central.sonatype.com/artifact/io.github.devers2/s2-validator)
[![Java 17+](https://img.shields.io/badge/Java-17%2B-blue?logo=openjdk)](https://openjdk.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-orange.svg)](https://github.com/devers2/s2-util/blob/main/LICENSE)

> **"Tired of writing validation rules twice in Java and JavaScript? Tired of annotation hell for conditional fields?"**
> **s2-validator** is a next-generation dynamic validation framework that lets you **Write Once, Validate Anywhere** — from server-side Java DTOs to native browser HTML with **zero frontend JavaScript code**.

### 💡 Why s2-validator over Standard Bean Validation?

| Pain Points in Real-World Enterprise            | Standard Bean Validation (JSR-380)                               | ⭐ s2-validator                                                                       |
| :---------------------------------------------- | :--------------------------------------------------------------- | :------------------------------------------------------------------------------------ |
| **Conditional Fields** _(If A then B required)_ | Verbose `@GroupSequenceProvider` or custom annotation classes ❌ | Expressive in 2 lines: `.when("type", "VIP").rule(REQUIRED)` ✅                       |
| **Cross-Field Comparison** _(pw == confirmPw)_  | Class-level annotations; errors bound to root object ❌          | Directly bound to the target field: `.rule(EQUALS_FIELD, "pw")` ✅                    |
| **Browser / Frontend Sync**                     | Server-only. Must duplicate identical regex/rules in JS/TS ❌    | **Zero frontend code**: `getRulesJson()` + native tooltip auto-focus ✅               |
| **Korean Particle Grammar** 🇰🇷                  | Complex custom `MessageInterpolator` required ❌                 | Built-in smart postpositions (`{0\|은/는}`, `{0\|이/가}`) ✅                          |
| **Field Typo / Refactoring Safety**             | String-based property paths fail silently at runtime ❌          | **`s2-validator-plugin`** catches typos at compile time via AST static analysis 🛡️ ✅ |

### 🚀 30-Second Taste

#### 1. Instant Backend Validation (No frontend sync needed)

Validate DTOs, VOs, or Maps in a single fluent line. Omitted rules default to `REQUIRED`:

```java
// Fail-fast: throws S2RuntimeException on first error
S2Validator.of(command)
    .field("name", "Name") // Omitting rule auto-applies REQUIRED
    .field("email", "Email").rule(S2RuleType.EMAIL) // With rules, REQUIRED is NOT auto-applied
    .field("birthDate", "Birth Date").rule(S2RuleType.REQUIRED).rule(S2RuleType.DATE)
    .validate();

// Or collect all errors without throwing:
List<S2ValidationError> errors = new ArrayList<>();
boolean isValid = S2Validator.of(command)
    .field("name", "Name")
    .field("email", "Email").rule(S2RuleType.EMAIL)
    .validate(errors::add);
```

#### 2. Full-Stack Auto-Sync (0 Lines of Frontend JS)

Need browser-side validation? Send the server rules directly to HTML:

```java
// Server: Validate with Spring BindingResult & expose rules JSON
S2BindValidator.context("signUp", this::signUpRules).validate(command, result);
model.addAttribute("rules", S2BindValidator.context("signUp", this::signUpRules).getRulesJson());
```

```html
<!-- Client: Native browser tooltips & auto-focus with a single HTML attribute! -->
<form th:data-s2-rules="${rules}"></form>
```

🔗 **Dive Deeper into the Ecosystem:**

- 🏛️ **[s2-util Suite Repository →](https://github.com/devers2/s2-util)**: Explore the complete utility toolkit (`s2-core`, `s2-validator`, `s2-jpa`) and quick start guide
- 📖 **[s2-validator Deep Dive & Guide →](https://github.com/devers2/s2-util/tree/main/s2-validator)**: Complete rule reference, Spring MVC binding, and Thymeleaf/HTML integration

---

## 📦 S2 Project Suite

| Project / Module                                                                              | Description                                                                                            |                            Repository                             |
| :-------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------: |
| **[`s2-validator`](https://github.com/devers2/s2-util/tree/main/s2-validator)**               | ⭐ Unified dynamic cross-platform validation engine & Spring binding integration                       |          [`s2-util`](https://github.com/devers2/s2-util)          |
| **[`s2-validator-plugin`](https://github.com/devers2/s2-util/tree/main/s2-validator-plugin)** | Gradle static analysis plugin — catches DTO field typos and incomplete chains at compile time          |          [`s2-util`](https://github.com/devers2/s2-util)          |
| **[`s2-core`](https://github.com/devers2/s2-util/tree/main/s2-core)**                         | High-performance Java utility toolkit (MethodHandle Reflection, W-TinyLFU Cache, Dates, Strings)       |          [`s2-util`](https://github.com/devers2/s2-util)          |
| **[`s2-jpa`](https://github.com/devers2/s2-util/tree/main/s2-jpa)**                           | Fluent dynamic JPQL query builder with SQL injection prevention                                        |          [`s2-util`](https://github.com/devers2/s2-util)          |
| **[`s2-support`](https://github.com/devers2/s2-support)**                                     | Opinionated companion library for application-level workflows (Pagination, SFTP/Files, Spring Context) |       [`s2-support`](https://github.com/devers2/s2-support)       |
| **[`s2-build-support`](https://github.com/devers2/s2-build-support)**                         | Convention Gradle plugin for automated licensing, copyright, and publication                           | [`s2-build-support`](https://github.com/devers2/s2-build-support) |

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
