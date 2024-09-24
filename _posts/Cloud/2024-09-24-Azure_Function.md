---
title: "[클라우드] Azure Function이란?"
author: BambooStreet
date: 2024-09-24 00:00:00 +0800
categories: [study,cloud,azure]
tags: [favicon]
image: assets/img/thumbnail/azure_function.png
---


## Azure Function이란?

Azure Function이란 Microsoft Azure 클라우드 플랫폼에서 제공하는 서버리스 컴퓨팅 서비스입니다.
주요특징은 다음과 같습니다.

1. 서버리스 컴퓨팅: "서버리스"란 실제로 서버가 없다는 뜻이 아니라, 개발자가 서버 관리에 대해 걱정할 필요가 없다는 것을 의미

2. 이벤트 기반: HTT 요청, 타이머, 메시지 큐 등 다양한 이벤트에 반응해 실행

3. 자동 확장: 트래픽에 따라 자동으로 확장되어 리소스를 효율적으로 사용

4. 다양한 언어 지원: C#, JavaScript, Python, Java 등 여러 프로그래밍 언어를 지원

5. 마이크로서비스 아키텍처: 작은 독립적인 기능 단위로 애플리케이션 구축 가능



## 구성 요소/저장

서버리스 컴퓨팅에서 애플리케이션은 개별 함수들로 구성되며, 각 함수는 클라우드 제공업체의 저장소에 저장됩니다. 예를 들어, AWS Lambda의 경우 S3 버킷, 지금 설명하고 있는 Azure Functions의 경우 Azure Storage에 저장됩니다. Azure 포털에 들어가 업로드한 function app을 확인하면 다음과 같은 파일들이 업로드 된 것을 확인할 수 있습니다.

![img](assets/img/posts/20240924/app_files.png)


각 파일을 살펴보면,

1. function_app.py 
   
   개발자가 작성한 코드 함수

2. host.json 


    Azure Function의 모든 함수에 적용되는 설정을 정의합니다. 개별 함수가 아닌 전체 앱의 동작을 제어하며, 제가 설정한 주요 항목은 다음과 같습니다.
    ```
    {
        "version": "2.0",
        "logging": {
            "applicationInsights": {
            "samplingSettings": {
                "isEnabled": true,
                "excludedTypes": "Request"
            }
          }
        },
        "extensionBundle": {
            "id": "Microsoft.Azure.Functions.ExtensionBundle",
            "version": "[4.*, 5.0.0)"
        }
    }
    ```
    * version: Functions 런타임 버전 지정, 2.0은 크로스 플랫폼 지원을 제공하고 확장된 언어 지원과 향상된 성능을 지원
    * functionTimeout: 함수의 최대 실행 시간을 설정
    * logging: 로깅 관련 설정
      * isEnabled: Application Insights의 샘플링 기능 활성화
      * excludedTypes: "Request"요청 유형의 텔레메트리는 샘플링에서 제외, 모든 HTTP 요청 데이터를 누락 없이 수집하고자 할 때 유용
    * extensions: 다양한 바인딩 및 트리거에 대한 설정 관리
      * "id":"Microsoft.Azure.Functions.ExtensionBundle"는 사용 확장 번들의 ID를 지정하며, 확장 번들은 여러 바인딩 확장을 하나의 패키지로 제공
      * "version":"[4.*,5.0.0)"는 사용할 확장 번들(Microsoft.Azure.Functions.ExtensionBundle)의 버전 범위를 지정, 4.x대의 최신 버전을 사용하되, 5.0.0 미만의 버전만 사용하도록 제한함으로써 주요 버전 업그레이드로 인한 호환성 문제를 방지

3. oryx-manifast.toml
   
   
   ORYX-MANIFEST.TOML 파일은 Azure App Service와 Azure Functions에서 사용되는 빌드 및 배포 시스템인 Oryx와 관련된 메타데이터, 구성 정보를 포함하는 문서 파일입니다.
   파이썬 버전과, 파이썬 패키지들이 설치된 디렉토리 경로, 특정 빌드/배포 작업의 고유 식별자, 소스코드 디렉토리 경로, 애플리케이션 플랫폼 또는 런타임 지정 등의 정보를 포함합니다.

4. requirements.txt


   파이썬 의존성을 명시한 파일입니다.


## 배포 프로세스

1. **코드 압축**: VS Code가 프로젝트 파일들을 압축합니다.
   
2. **Azure Storage 업로드**: 
   - 압축된 코드가 Function App과 연결된 Azure Storage 계정에 업로드됩니다.
   - 이 스토리지 계정에는 `scm-releases`라는 특별한 컨테이너가 있어, 여기에 코드가 저장됩니다.

3. **Kudu 서비스 활성화**:
   - Kudu는 Azure App Service의 배포 및 관리 엔진입니다.
   - Kudu가 새로운 배포를 감지하고 배포 프로세스를 시작합니다.

4. **코드 추출 및 설치**:
   - Kudu가 업로드된 압축 파일을 `D:\home\site\wwwroot`에 추출합니다.
   - `requirements.txt`에 명시된 Python 패키지들이 설치됩니다.

5. **환경 구성**:
   - `local.settings.json`의 설정들이 App Settings로 변환됩니다.
   - 환경 변수가 설정됩니다.

6. **Function App 재시작**:
   - 새 코드와 설정을 적용하기 위해 Function App이 재시작됩니다.

7. **웜업 요청**:
   - Azure가 자동으로 각 함수에 웜업 요청을 보내 초기화합니다.

8. **로그 및 모니터링 활성화**:
   - Application Insights가 구성되어 있다면, 로깅 및 모니터링이 시작됩니다.

<br>
<br>
<br>
배포가 완료되면 HTTP 트리거를 사용하는 함수들은 특정 URL로 요청을 받아 작동하게 됩니다. 


![img](assets/img/posts/20240924/azure-function-url-diagram.png)

클라이언트가 특정 URL 중 하나로 HTTP 요청을 보내면, Azure Function 런타임이 요청을 받아 해당 함수를 트리거 하고, 함수가 실행되어 로직을 처리하고 응답을 반환하는 프로세스입니다.


이처럼 클라이언트는 단순히 적절한 URL로 요청을 보내는 것만으로 서버리스 함수의 기능을 활용할 수 있게 됩니다!