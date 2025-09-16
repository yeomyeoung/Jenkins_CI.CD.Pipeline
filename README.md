# Jenkins CI/CD Pipeline Exploration

## 0. 팀원 소개
| 박여명 | 신기범 |
|:------:|:------:|
| <img src="https://avatars.githubusercontent.com/u/166470537?v=4" alt="박여명" width="150"> | <img src="https://avatars.githubusercontent.com/u/112679148?v=4" alt="신기범" width="150"> |
| [GitHub](https://github.com/yeomyeoung) | [GitHub](https://github.com/shin-kibeom)) |

---

## 1. 프로젝트 개요
이 저장소는 **Jenkins의 주요 기능 탐구**를 목적으로 합니다.  
단순한 빌드 자동화가 아닌, **CI/CD 파이프라인의 핵심 흐름**을 직접 구성하고 테스트하면서  
실무 환경에서 Jenkins가 어떻게 활용되는지 실습한 결과를 담고 있습니다.  

주요 목표는 다음과 같습니다:
- GitHub와 Jenkins 연동을 통한 **자동 소스 코드 반영**
- Gradle 기반 **JAR 빌드 및 아카이빙**
- **컨테이너 환경(Ubuntu, Docker)** 에서 Jenkins 동작 검증
- 외부 접속 및 관리 도구(Ngrok, MobaxTerm)를 활용한 운영 실습

---

## 2. 기술 스택
- **OS**: Ubuntu 20.04 (Docker 기반 Jenkins 컨테이너 실행)
- **Language / Build Tool**: Java 17, Gradle
- **CI/CD**: Jenkins (Pipeline Script 기반)
- **Version Control**: Git / GitHub
- **Utility Tools**: MobaxTerm (SSH 관리), Ngrok (외부 접근)
