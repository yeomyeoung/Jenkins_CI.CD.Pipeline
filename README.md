# Jenkins CI/CD Pipeline Lab
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

<br>

## 3. CI/CD Pipeline Flow (파이프라인 흐름)

본 프로젝트에서는 Jenkins Declarative Pipeline을 사용하여  
**소스 코드 → 빌드(JAR) → 아카이빙** 까지 자동화했습니다.  

### 📜 Jenkinsfile (Pipeline Script)

```groovy
pipeline {
  agent any
  environment {
    JAVA_HOME = '/opt/java/openjdk'
    PATH = "${JAVA_HOME}/bin:${env.PATH}"
    GITHUB_REPO  = 'https://github.com/yeomyeoung/jenkins.git'
    BRANCH_NAME  = 'main'
    PROJECT_PATH = '.'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: "${BRANCH_NAME}", url: "${GITHUB_REPO}"
        sh 'java -version && echo JAVA_HOME=$JAVA_HOME'
      }
    }

    stage('Build JAR') {
      steps {
        sh '''
          chmod +x gradlew || true
          ./gradlew --version
          ./gradlew clean bootJar -x test || ./gradlew clean build -x test
        '''
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
      }
    }
  }
}

```
<br>

### 🔎 Stage 설명

| Stage 이름      | 기능 설명 |
|-----------------|-----------|
| 🟦 **Checkout** | GitHub 저장소(`main` 브랜치)에서 최신 코드를 가져오고, JDK 환경(`JAVA_HOME`)을 확인합니다. |
| 🟩 **Build JAR** | Gradle Wrapper(`gradlew`)를 실행하여 `bootJar` 또는 `build` 태스크로 JAR 파일을 생성합니다. <br> *(테스트는 `-x test` 옵션으로 제외)* |
| 🟨 **Archive**  | 빌드된 JAR(`build/libs/*.jar`)을 Jenkins 워크스페이스에 저장하고, `fingerprint`로 아티팩트를 추적합니다. |


## 결과 화면
![Jenkins Pipeline Flow](https://i.ibb.co/4wL3K5cn/mermaid-jenkins-Flow.png)


