# Jenkins CI/CD Pipeline Exploration
Jenkins를 활용해 **CI/CD 파이프라인**을 직접 구성하고 테스트한 실습 프로젝트입니다.  
GitHub 저장소 변경 사항을 자동으로 감지하고, **Gradle 빌드 → JAR 생성 → 아카이빙**까지  
자동화되는 과정을 통해 Jenkins의 핵심 기능을 탐구했습니다.

<br>

# 0. Getting Started

### 수행 환경
- **운영 환경**: Ubuntu 20.04+ (Docker 기반 Jenkins 컨테이너)
- **필수 패키지**: Docker, Git, Java 17
- **외부 연동**: Ngrok (원격 접속), GitHub Webhook
- **구현 방식**: Jenkins Pipeline Script 기반 CI/CD

<br>

# 1. Project Overview (프로젝트 개요)

- **프로젝트 이름**: Jenkins CI/CD Pipeline Exploration
- **프로젝트 설명**:
  - GitHub 저장소 변경 사항 자동 감지 및 Pull
  - Gradle 기반 JAR 빌드 및 Jenkins 아카이빙
  - Jenkins 컨테이너 환경에서 CI/CD 동작 검증
  - Ngrok, MobaXterm 등을 활용한 외부 접속 및 운영 실습

<br>

# 2. Technology Stack (기술 스택)

## 2.1 OS & Tools
| Ubuntu | Jenkins | MobaxTerm |
|--------|---------|-----------|
| <img src="https://cdn.simpleicons.org/ubuntu" alt="Ubuntu" width="70"> | <img src="https://cdn.simpleicons.org/jenkins" alt="Jenkins" width="70"> | <img src="https://github.com/user-attachments/assets/b9ae3f4a-9b01-4e12-9806-12dc33bdbe8e" alt="MobaXterm" width="70"> |


## 2.2 Language & Build
| Java 17 | Gradle |
|---------|--------|
| <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original.svg" width="70"/> | <img src="https://cdn.simpleicons.org/gradle/02303A" width="70"/> |


## 2.3 Cooperation
| GitHub | Ngrok |
|--------|-------|
| <img src="https://avatars.githubusercontent.com/u/22289824?s=200&v=4" width="70"/> | <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/ngrok.svg" width="70"/> |


