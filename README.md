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
**소스 코드 → 빌드(JAR) → 아카이빙** 까지 자동화 수행.  

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
| 🟦 **Checkout** | GitHub 저장소(`main` 브랜치)에서 최신 코드를 가져오고, JDK 환경(`JAVA_HOME`) 확인. |
| 🟩 **Build JAR** | Gradle Wrapper(`gradlew`)를 실행하여 `bootJar` 또는 `build` 태스크로 JAR 파일을 생성. <br> *(테스트는 `-x test` 옵션으로 제외)* |
| 🟨 **Archive**  | 빌드된 JAR(`build/libs/*.jar`)을 Jenkins 워크스페이스에 저장하고, `fingerprint`로 아티팩트를 추적. |

<br>

## 결과 화면
![Jenkins Pipeline Flow](https://i.postimg.cc/qR9zpFqn/mermaid-jenkins-Flow.png)

<br>

## 4. Pipeline Stage 설명

| Stage 이름   | 주요 동작                                                                 | 비고 |
|--------------|-------------------------------------------------------------------------|------|
| **Checkout** | - GitHub 저장소에서 `main` 브랜치 소스 코드 가져오기<br>- Java 환경 변수 확인 (`java -version`) | Git 연동 테스트 |
| **Build JAR** | - Gradle Wrapper 실행 (`./gradlew`)<br>- Clean 빌드 후 `bootJar` 생성<br>- Test 스킵 옵션 적용 (`-x test`) | 빌드 실패 시 fallback으로 `build` 실행 |
| **Archive**  | - `build/libs/*.jar` 산출물을 Jenkins 아티팩트로 보관<br>- Fingerprint 생성하여 추적 가능 | JAR 버전 관리 |

<br>

# 5. 실행 결과 (Pipeline Result)

### 5.1 Stage View
Jenkins 파이프라인이 **Checkout → Build JAR → Archive** 단계까지 정상적으로 동작했으며,  
빌드 결과물 JAR 파일이 성공적으로 아카이빙됨.

<img src="https://i.postimg.cc/2SPJFF1G/image.png" alt="Jenkins Pipeline Stage View" width="700">

<br>

### 5.2 산출물 경로 (Artifact)
빌드된 JAR 파일은 Jenkins 워크스페이스에 생성:

```
~/jenkis-test/workspace/step03_teamArt/build/libs/step05_GradleBuild-0.0.1-SNAPSHOT.jar
```
<br>

### 5.3 실행 (Run on Ubuntu)
> 8080 포트가 이미 점유 중이었으므로 **8081** 포트로 우회 실행

```bash
cd ~/jenkis-test/workspace/step03_teamArt/build/libs
```

### 5.4 실행 화면 (Running App)

> Jenkins 빌드 산출물인 Jar 실행

<img src="https://i.postimg.cc/50qtLSmQ/image.png" alt="App running on 8081" width="700">

<br>

# 6. Troubleshooting (트러블슈팅)

## 6.1 JDK 버전 불일치
- **문제**: Jenkins 컨테이너 기본 JDK는 `21`, Gradle Toolchain은 `17`을 요구 → 빌드 실패 발생  
- **해결**: Jenkins 환경 변수 `JAVA_HOME`을 OpenJDK 17 경로로 지정 → 정상 빌드 완료  

✅ **교훈**: CI/CD 환경에서는 JDK 버전 호환성 반드시 체크 필요

<br>

## 6.2 포트 충돌
- **문제**: 기존 프로세스가 `8080` 포트를 점유 → 빌드된 JAR 실행 불가  
- **해결**: 실행 포트를 `8081`로 변경 → 정상 구동 확인  

✅ **교훈**: 포트 충돌은 자주 발생하는 이슈 → **프로세스 관리·모니터링 자동화 필요**

<br>

---

<br>

# 7. Next Steps (향후 확장)

- **아티팩트 마운트 최적화**  
  - 현재는 Jenkins의 전체 워크스페이스가 호스트와 공유되어 불필요한 파일까지 노출됨  
  - 추후 개선 시, **JAR 파일만 Host에 마운트**되도록 경로를 세분화할 계획  

<br>

- **무중단 배포(Zero-Downtime) 실습**  
  - Shell 스크립트 기반 트리거와 프로세스 교체 방식을 활용해  
    배포 시 서비스 중단 없이 새로운 버전으로 전환하는 CI/CD 파이프라인 구성 예정  

