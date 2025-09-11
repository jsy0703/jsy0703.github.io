---
title: "MCPO 서버 설치 및 인증 가이드"
date: 2025-01-27
author: "jsy0703"
category: "기술"
tags: ["MCPO", "설치", "인증", "MCP", "서버"]
---

# MCPO 서버 설치 및 인증 가이드

이 문서는 MCPO 서버 설치를 두 가지 독립 흐름으로 안내합니다. 원하시는 방법의 섹션만 따라 하시면 됩니다.
- Python 직접 설치 흐름: `setup_mcpo_python.bat`
- Docker 컨테이너 설치 흐름: `setup_mcpo_docker.bat`

### 설치 방식 비교
| 구분                | Python 직접 설치                                 | Docker 환경 설치                                 |
|---------------------|--------------------------------------------------|--------------------------------------------------|
| **권장 환경**       | - Docker 사용 불가(회사 정책, 가상화 미지원 등)<br>- Python/Node.js 환경 충돌 우려 없음 | - Python/Node.js가 이미 설치되어 있고<br>  다른 프로젝트와 충돌 우려가 있을 때<br>- 여러 프로젝트 환경을 분리하고 싶을 때 |
| **장점**            | - 별도 가상화/컨테이너 필요 없음<br>- 설치 직관적 | - 로컬 환경과 완전 분리<br>- 환경 충돌 방지<br>- 손쉬운 초기화/재설치 |
| **단점/제약**       | - 기존 Python/Node.js와 충돌 가능<br>- 권한 문제 발생 가능 | - Docker Desktop 필수<br>- 가상화 미지원 PC 사용 불가<br>- 리소스 추가 사용 |
| **설치 방법**       | `setup_mcpo_python.bat` 실행                     | `setup_mcpo_docker.bat` 실행                     |

> 중요: 다른 프로젝트가 Python/Node.js의 특정 버전에 고정되어 있다면, 버전 충돌을 피하기 위해 **Docker 환경을 강력히 권장**합니다.
> 권장 버전: **Python 3.11 이상**, **Node.js 20 이상**

---

## Python 설치 흐름 (로컬 Python에 직접 설치)

- **권장 대상**: Docker 사용이 어렵거나, 기존 로컬 개발 환경에 직접 설치하고 싶은 경우
- **특징**: 별도 컨테이너 없이 바로 설치, 기존 Python/Node 환경과 충돌 가능성 있음

### 사전 준비
- 운영체제: Windows 10/11
- 패키지 관리자: `winget` 사용 가능 권장 (자동 설치에 활용)
- 권장 버전: Python 3.11 이상, Node.js 20 이상

### 설치
1. `setup_mcpo_python.bat` 더블클릭 또는 관리자 PowerShell에서 실행
2. 스크립트가 자동으로 수행합니다.
   - Python 미설치/구버전 시 최신 안정 버전(3.10+) 자동 설치 및 PATH 보강
   - `C:\nally_mcpo` 폴더 및 `config.json` 생성
   - `mcpo`, `uv` 등 Python 패키지 설치
   - `npx` 사용을 위한 Node.js(LTS) 설치
3. 완료 메시지(설치가 완료되었습니다)가 나오면 성공

### 서버 시작
- `start_mcpo_server.bat` 실행

### 서버 종료
- 실행 중인 터미널에서 `Ctrl + C` 입력

### 인증 (공통)
- 설치 후 `authenticate_mcps_python.bat` 실행하여 Atlassian, Teams 등 인증 진행

---

## Docker 설치 흐름 (컨테이너 격리 환경)

- **권장 대상**: 로컬 환경과 격리해 충돌을 피하고 싶을 때, 여러 프로젝트를 병행할 때
- **특징**: Docker Desktop 필요, 환경 격리와 손쉬운 초기화/재설치 장점

### 사전 준비
- 운영체제: Windows 10/11
- Docker Desktop 설치 및 실행 (가상화 기능 활성화 필요)

### 설치
1. `setup_mcpo_docker.bat` 더블클릭 또는 PowerShell에서 실행
2. 스크립트가 자동으로 수행합니다.
   - Docker 연결 확인 및 빌드 컨텍스트 준비
   - `ncurity-mcpo` 이미지 빌드
   - `mcpo` 컨테이너 생성
3. 완료 메시지(Docker setup and container creation complete) 확인

### 서버 시작
- `start_mcpo_docker.bat` 실행

### 서버 종료
- 스크립트 안내에 따라 아무 키나 눌러 컨테이너 중지
- 또는 수동으로 `docker stop mcpo` 실행

### 인증 (공통)
- 설치 후 `authenticate_mcps_docker.bat` 실행하여 Atlassian, Teams 등 인증 진행

---

## 2. 설치 파일 개요

이 가이드에서는 다음 5개의 `.bat` 스크립트 파일을 사용합니다.

1.  `setup_mcpo_python.bat`: **(방법 1)** Python 환경에 `mcpo` 서버를 직접 설치합니다.
2.  `setup_mcpo_docker.bat`: **(방법 2)** Docker를 사용하여 `mcpo` 서버를 컨테이너로 설치합니다.
3.  `authenticate_mcps.bat`: Atlassian, Teams 등 MCP 서비스 사용을 위해 인증을 수행합니다. (설치 후 1회 실행)
4.  `start_mcpo_server.bat`: Python 방식으로 설치된 서버를 시작합니다.
5.  `start_mcpo_docker.bat`: Docker 방식으로 설치된 서버 컨테이너를 시작합니다.

---

## 3. MCP 인증

설치가 끝났다면 Atlassian, Teams 등 MCP 서비스 사용을 위해 인증을 진행하세요. (설치 방식과 무관하게 필수)

- `authenticate_mcps.bat`를 실행합니다.
- 스크립트가 순서대로 인증을 시작합니다.
  - Atlassian 인증: 새 터미널 창이 열리고 브라우저가 자동 실행됩니다. 로그인 후 접근을 허용하세요. 인증 후에도 이 터미널 창은 닫지 마세요.
  - Teams 인증: 원래 창에서 안내에 따라 로그인 코드를 입력하거나 브라우저로 Microsoft 계정 로그인 진행.
- Atlassian 인증 후 Teams 단계로 넘어가지 않으면 창을 닫고 `authenticate_mcps.bat`를 다시 실행하세요.

Teams MCP 디바이스 코드 인증 안내:
- 예시 메시지
```
📱 Please complete authentication:
🌐 Visit: https://microsoft.com/devicelogin
🔑 Enter code: ANYZZSCQH
⏳ Waiting for you to complete authentication...
```
- 방법: 터미널 메시지의 링크(https://microsoft.com/devicelogin)를 Ctrl 키를 누른 상태로 클릭해 접속한 뒤, 표시된 코드를 그대로 입력하면 됩니다.

참고: Atlassian MCP 인증 중 아래 로그가 보이면 이미 인증이 완료된 상태입니다. 바로 **Teams MCP 인증**으로 넘어가도 됩니다.
```
[22552] Connecting to remote server: https://mcp.atlassian.com/v1/sse
[22552] Connected to remote server using SSEClientTransport
[22552] Local STDIO server running
[22552] Proxy established successfully between local STDIO and remote SSEClientTransport
[22552] Press Ctrl+C to exit
```

---

## Open WebUI에서 MCP 연결 방법 안내

MCP를 추가 설정하기 전에, Open WebUI에서 다음과 같이 연결을 진행해야 합니다.

1. Open WebUI 화면의 설정 > 도구에서 **+ (연결 추가)** 버튼을 클릭합니다.
2. **API 기본 URL**에 기본 베이스 주소로 `http://localhost:8000`을 입력한 뒤, 뒤에 연동할 MCP 이름을 붙여줍니다.  
   예시:  
   - Atlassian MCP의 경우: `http://localhost:8000/atlassian`
   - Teams MCP의 경우: `http://localhost:8000/teams-mcp`
   - N8N MCP의 경우: `http://localhost:8000/n8n-mcp`
3. **활성화** 버튼 옆의 **새로고침(연결확인)** 버튼을 클릭합니다.
4. 화면 오른쪽 상단에 **"연결 성공"**이라는 문구가 뜨면 연동이 정상적으로 완료된 것입니다.

## 4. MCP 추가 설정

새로운 MCP를 추가하고 싶으면 `C:\nally_mcpo` 폴더 안에 있는 `config.json` 파일을 직접 편집하면 됩니다.

`mcpServers` 객체 안에 새로운 MCP 설정을 추가합니다. 
예를 들어, Salesforce를 추가하려면 다음과 같이 작성할 수 있습니다.

```json
{
  "mcpServers": {
    "fetch": { ... },
    "teams-mcp": { ... },
    "atlassian": { ... },
    "salesforce": {
      "command": "uvx",
      "args": [
        "--from",
        "mcp-salesforce-connector",
        "salesforce"
      ],
      "env": {
        "SALESFORCE_ACCESS_TOKEN": "YOUR_ACCESS_TOKEN",
        "SALESFORCE_INSTANCE_URL": "YOUR_INSTANCE_URL",
        "SALESFORCE_DOMAIN": "YOUR_DOMAIN"
      }
    }
  }
}
```

> **참고**: `config.json` 파일을 수정한 후에는 `mcpo` 서버를 재시작해야 변경 사항이 적용됩니다.

---

## 5. 문제 해결

-   **Docker 관련 오류 발생 시**:
    -   `setup_mcpo_docker.bat` 실행 중 Docker 연결 오류가 발생하면, Docker Desktop이 완전히 시작될 때까지 기다린 후 다시 시도해 보세요. 시스템 트레이의 고래 아이콘 애니메이션이 멈추면 시작이 완료된 것입니다.
-   **인증이 안될 경우**:
    -   `authenticate_mcps.bat` 실행 시 브라우저가 자동으로 열리지 않으면, 터미널에 출력되는 URL을 직접 복사하여 브라우저 주소창에 붙여넣어 시도해 보세요.