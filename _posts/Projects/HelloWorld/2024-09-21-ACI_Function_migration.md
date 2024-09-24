---
title: "[HelloWorld 개발일지] ACI(Azure Container Instance)에서 Azure Function으로 마이그레이션"
author: BambooStreet
date: 2024-09-21 00:00:00 +0800
categories: []
tags: []
image: assets/img/posts/20240921/logo.png
---


## ACI -> Function 마이그레이션 이유

기존 HelloWorld 모델 서버를 배포할 때, ACI(Azure Container Instance)를 사용했는데, 몇가지 문제가 발생

<br>

1. 변동 IP로 백엔드와 연동 불안정
2. 고정 비용 발생

<br>

Azure Function을 활용하면 고정된 url로 연동 안정성이 생기고 사용량에 따른 요금 청구로 비용을 절감할 수 있겠다 싶어서 마이그레이션을 결심했습니다.


<br>

## 방법

### VS코드에서 Azure Functions 프로젝트 생성

1. VS Code 확장 설치:
- VS Code를 열고 좌측 사이드바의 확장(Extensions) 아이콘을 클릭합니다.
- 검색창에 "Azure Functions"를 입력합니다.
- "Azure Functions" 확장을 찾아 설치합니다.
- 추가로 "Python" 확장도 설치되어 있는지 확인합니다.

2. Azure Functions Core Tools 설치:
- Windows의 경우:
  - 관리자 권한으로 PowerShell을 실행합니다.
  - 다음 명령어를 입력합니다:
    ```
    npm install -g azure-functions-core-tools@4 --unsafe-perm true
    ```
- macOS/Linux의 경우:
  - 터미널을 열고 다음 명령어를 입력합니다:
    ```
    npm install -g azure-functions-core-tools@4 --unsafe-perm true
    ```

3. Python 설치 확인:
- VS Code의 터미널에서 다음 명령어로 Python 버전을 확인합니다:
  ```
  python --version
  ```
- Python이 설치되어 있지 않다면, python.org에서 다운로드하여 설치합니다.

4. VS Code에서 프로젝트 생성:
- VS Code를 실행합니다.
- `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`)를 눌러 명령 팔레트를 엽니다.
- "Azure Functions: Create New Project"를 입력하고 선택합니다.
- 프로젝트를 저장할 폴더를 선택합니다.
- 언어로 "Python"을 선택합니다.
- Python 인터프리터를 선택합니다.
- 첫 번째 함수의 템플릿으로 "HTTP trigger"를 선택합니다.
- 함수 이름을 입력합니다 (예: "HttpTrigger").
- 인증 수준을 선택합니다 (예: "Function").

5. 가상 환경 설정:
- VS Code의 통합 터미널에서 다음 명령어를 실행합니다:
  ```
  python -m venv .venv
  ```
- 가상 환경을 활성화합니다:
  - Windows: `.venv\Scripts\Activate`
  - macOS/Linux: `source .venv/bin/activate`

6. 필요한 라이브러리 설치:
- `requirements.txt` 파일을 열고 필요한 라이브러리를 추가합니다.
- 터미널에서 다음 명령어를 실행합니다:
  ```
  pip install -r requirements.txt
  ```



## 로컬에서 테스트

로컬에서 Azure Functions를 테스트하는 방법을 단계별로 설명해 드리겠습니다:

1. 가상 환경 활성화:
   먼저 프로젝트의 가상 환경이 활성화되어 있는지 확인합니다.
   ```
   # Windows
   .venv\Scripts\activate
   
   # macOS/Linux
   source .venv/bin/activate
   ```

2. 의존성 설치 확인:
   필요한 모든 라이브러리가 설치되어 있는지 확인합니다.
   ```
   pip install -r requirements.txt
   ```

3. 로컬 설정 파일 확인:
   `local.settings.json` 파일에 필요한 모든 환경 변수가 설정되어 있는지 확인합니다. 예:
   ```json
   {
     "IsEncrypted": false,
     "Values": {
       "AzureWebJobsStorage": "",
       "FUNCTIONS_WORKER_RUNTIME": "python",
       "ES_CLOUD_ID": "your_elastic_cloud_id",
       "ES_API_KEY": "your_elastic_api_key",
       "OPENAI_KEY": "your_openai_api_key"
     }
   }
   ```

4. 함수 앱 실행:
   VS Code의 통합 터미널에서 다음 명령을 실행합니다:

   ```
   func start
   ```

   이 명령은 로컬 개발 서버를 시작하고 함수의 URL을 표시합니다.

5. 함수 테스트:   
- Postman 사용:
- 새 POST 요청 생성
- URL 입력: `http://localhost:7071/api/question`
- Headers 탭에서 `Content-Type: application/json` 추가
- Body 탭에서 raw 선택 후 JSON 형식으로 다음 입력:
  ```json
  {
    "Conversation": [
      {
        "speaker": "human",
        "utterance": "한국에서 외국인 근로자로 일하기 위한 비자 요건은 무엇인가요?"
      }
    ]
  }
  ```


  ### Azure 포털에서 배포
  