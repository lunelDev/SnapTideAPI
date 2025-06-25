# 🌊 snapTide API

**snapTide**는 사진 기반 SNS 애플리케이션을 위한 Spring Boot 기반의 RESTful API 백엔드입니다. 회원 관리부터 피드, 리뷰, 사진 업로드 기능까지 통합 제공하는 소셜 플랫폼 서버입니다.

---

## 🛠️ 기술 스택

- **Java 17**
- **Spring Boot 3.x**
- **Spring Data JPA**
- **Spring Web / REST**
- **Spring Validation**
- **Lombok**
- **MySQL (또는 H2)**
- **Swagger (Springdoc OpenAPI)**
- **Maven / Gradle**

---

## 🚀 주요 기능

| 기능 구분       | 설명 |
|----------------|------|
| 🧑‍🤝‍🧑 회원 관리   | 회원 가입, 정보 조회, 권한 관리 (`Members`, `MembersRole`) |
| 📰 피드 관리     | 피드 생성/수정/삭제/조회, 페이징 처리 지원 |
| 📝 리뷰 기능     | 피드나 사진에 대한 리뷰 CRUD |
| 🖼️ 사진 업로드   | Multipart 기반 사진 업로드 및 URL 반환 |
| 🔐 인증 처리     | 로그인 및 인증 컨트롤러 (`AuthController`) |
| ⚠️ 예외 처리     | REST 예외 전역 처리 (`ControllerExceptionAdvice`) |
| 📄 공통 페이징   | `PageRequestDTO`, `PageResultDTO` 기반 페이징 API |

---

## 🧱 ERD 다이어그램 (Entity 관계도)

```text
Members (회원)
 ├── id (PK)
 ├── email
 ├── password
 ├── nickname
 └── roles (ManyToMany) ──▶ MembersRole

Feeds (피드)
 ├── id (PK)
 ├── title
 ├── content
 ├── member_id (FK) ──▶ Members

Reviews (리뷰)
 ├── id (PK)
 ├── content
 ├── member_id (FK) ──▶ Members
 └── feed_id (FK) ──▶ Feeds

Photos (사진)
 ├── id (PK)
 ├── uuid
 ├── fileName
 ├── uploadPath
 └── review_id (nullable, FK) ──▶ Reviews
````

> 참고: `BasicEntity`는 모든 엔티티에서 상속되며 `regDate`, `modDate` 필드를 포함합니다.

---

## 📘 Swagger 사용법

이 프로젝트는 [springdoc-openapi](https://springdoc.org/) 기반 Swagger UI를 제공합니다.

### 실행 후 접속:

```
http://localhost:8080/swagger-ui/index.html
```

### 주요 경로 예시:

* `POST /api/members` – 회원 등록
* `GET /api/feeds` – 피드 목록 조회 (페이징 지원)
* `POST /api/reviews` – 리뷰 등록
* `POST /upload` – 사진 업로드

---

## ⚙️ 실행 방법

### 1. 의존성 설치

```bash
./mvnw clean install
# 또는
./gradlew build
```

### 2. 애플리케이션 실행

```bash
./mvnw spring-boot:run
# 또는
./gradlew bootRun
```

### 3. 로컬 테스트

```bash
http://localhost:8080
```

---

## 🚢 CI/CD 파이프라인 (예시)

> GitHub Actions, Jenkins 또는 GitLab CI 등을 사용할 수 있습니다.

### GitHub Actions 예시 (`.github/workflows/deploy.yml`)

```yaml
name: Deploy snapTide API

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK
        uses: actions/setup-java@v3
        with:
          java-version: '17'
      - name: Build with Maven
        run: mvn clean install

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Server
        run: ssh $USER@$HOST 'docker-compose up -d --build'
```

> 🔐 참고: `USER`, `HOST`, `SSH_KEY`는 GitHub Secrets에 등록 필요

---

## 🌐 배포 주소

| 환경       | 주소                        |
| -------- | ------------------------- |
| ✅ 개발 서버  | `https://dev.snaptide.io` |
| 🚀 운영 서버 | `https://snaptide.io`     |

> 배포 서버가 없을 경우, 위 주소는 예시입니다. 향후 실제 IP나 도메인으로 교체하세요.

---

## 🙌 기여 방법

1. 이슈 확인 및 작업 브랜치 생성
2. 기능 개발 및 PR 작성
3. 코드 리뷰 및 Merge

```
