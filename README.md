# VoiceRAG: Azure AI Search기반 RAG와 GPT-4o Realtime API를 사용한 음성 애플리케이션 패턴
(VoiceRAG: An Application Pattern for RAG + Voice Using Azure AI Search and the GPT-4o Realtime API for Audio

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=brightgreen&logo=github)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&skip_quickstart=true&machine=basicLinux32gb&repo=860141324&devcontainer_path=.devcontainer%2Fdevcontainer.json&geo=WestUs2)
[![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/Azure-Samples/aisearch-openai-rag-audio)

이 저장소는 음성을 사용자 인터페이스로 사용하는 애플리케이션에서 RAG 지원을 구현하는 방법의 예제를 포함하고 있으며, GPT-4o 실시간 API를 통해 제공됩니다. 우리는 이러한 패턴을 좀 더 자세히 [이 블로그 게시물](https://aka.ms/voicerag)에서 설명하며, [이 비디오](https://youtu.be/vXJka8xZ9Ko)에서 이 샘플 앱의 작동 모습을 볼 수 있습니다.

* [기능](#Features)
* [아키텍처 다이어그램](#architecture-diagram)
* [시작하기](#getting-started)
  * [GitHub Codespaces](#github-codespaces)
  * [VS Code Dev Containers](#vs-code-dev-containers)
  * [로컬 환경](#local-environment)
* [앱 배포](#deploying-the-app)
* [개발 서버](#development-server)

## 기능

* **음성 인터페이스**: 이 앱은 브라우저의 마이크를 사용하여 음성 입력을 캡처하고, 이를 백엔드로 전송하여 Azure OpenAI GPT-4o 실시간 API에 의해 처리합니다.
* **RAG (Retrieval Augmented Generation)**: 이 앱은 Azure AI Search 서비스를 사용하여 지식 기반에 대한 질문에 답하며, 검색된 문서를 GPT-4o 실시간 API에 전송하여 응답을 생성합니다.
* **Audio output**: 이 앱은 브라우저의 오디오 기능을 사용하여 GPT-4o 실시간 API의 응답을 오디오로 재생합니다.
* **Citations**: 이 앱은 응답 생성에 사용된 검색 결과를 보여줍니다.

### 아키텍처 다이어그램

프론트엔드의 RTClient는 오디오 입력을 수신하고, 이를 Python 백엔드로 전송합니다. 백엔드는 RTMiddleTier 객체를 사용하여 Azure OpenAI 실시간 API와 인터페이스하며, Azure AI Search 검색 도구를 포함합니다.

![Real-Time API RAG pattern 다이어그램](docs/RTMTPattern.png)

이 저장소에는 인프라 코드와 Dockerfile이 포함되어 있어 앱을 Azure Container Apps에 배포할 수 있지만, Azure AI Search와 Azure OpenAI 서비스가 설정되면 로컬에서도 실행할 수 있습니다.

### 시작하기
이 템플릿을 시작하는 몇 가지 옵션이 있습니다. 가장 빠르게 시작할 수 있는 방법은 이 [GitHub Codespaces](#github-codespaces)를 사용하여 모든 도구를 설정하는 것입니다. 하지만 [로컬 환경](#local-environment)에서 설정하는 방법을 사용할 수도 있습니다. 아니면 [VS Code dev container](#vs-code-dev-containers)를 사용할 수도 있습니다.

### GitHub Codespaces

GitHub Codespaces를 사용하여 이 리포지토리를 가상으로 실행할 수 있으며, 이는 브라우저에서 웹 기반의 VS Code를 열 것입니다:

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=brightgreen&logo=github)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&skip_quickstart=true&machine=basicLinux32gb&repo=860141324&devcontainer_path=.devcontainer%2Fdevcontainer.json&geo=WestUs2)

Once the codespace opens (this may take several minutes), open a new terminal and proceed to [deploy the app](#deploying-the-app).

### VS Code Dev Containers

[Dev Containers 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)을 사용하여 로컬 VS Code Dev Container에서 프로젝트를 실행할 수 있습니다:

1. Docker Desktop 시작 (설치되지 않은 경우 설치)
2. 프로젝트 열기:

    [![Dev Containers에서 열기](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/azure-samples/aisearch-openai-rag-audio)
3. VS Code 창에서 프로젝트 파일이 나타나면 (몇 분이 걸릴 수 있습니다), 새 터미널을 열고 [앱 배포](#deploying-the-app) 섹션의 절차를 따르세요.

### Local environment

1. 다음 도구를 설치하세요:
   * [Azure Developer CLI](https://aka.ms/azure-dev/install)
   * [Node.js](https://nodejs.org/)
   * [Python >=3.11](https://www.python.org/downloads/)
      * **Important**: Python and the pip package manager must be in the path in Windows for the setup scripts to work.
      * **Important**: Ensure you can run `python --version` from console. On Ubuntu, you might need to run `sudo apt install python-is-python3` to link `python` to `python3`.
   * [Git](https://git-scm.com/downloads)
   * [Powershell](https://learn.microsoft.com/powershell/scripting/install/installing-powershell) - For Windows users only.

2. 리포지토리 클론(Clon) (`git clone https://github.com/Azure-Samples/aisearch-openai-rag-audio`)
3. 다음 섹션으로 이동하여 [앱 배포](#deploying-the-app) 절차를 따르세요.

## Deploying the app

다음 단계는 Azure 리소스를 프로비저닝하고 Azure Container Apps에 애플리케이션 코드를 배포합니다.

1. Azure 계정에 로그인:

    ```shell
    azd auth login
    ```

    GitHub Codespaces 사용자의 경우 이전 명령이 실패하면 다음 명령을 시도하세요:

   ```shell
    azd auth login --use-device-code
    ```

1. 새 azd 환경 생성:

    ```shell
    azd env new
    ```

    리소스 그룹에 사용될 이름을 입력하세요.
    이는 `.azure` 폴더에 새 폴더를 생성하고, 앞으로 `azd` 호출에 대해 활성 환경으로 설정합니다.

1. (선택 사항) 기존 리소스를 재사용하려면, [이 지침](docs/existing_services.md)을 따라 적절한 `azd` 환경 변수를 설정하세요.

1. 다음 단일 명령을 실행하여 리소스를 프로비저닝하고, 코드를 배포하며, 샘플 데이터의 통합 벡터화를 설정하세요:

   ```shell
   azd up
   ````

   * **중요**: 이 명령으로 생성된 리소스는 즉시 비용이 발생할 수 있으며, 주로 AI Search 리소스에서 발생합니다. 이 명령이 완전히 실행되기 전에 중단하더라도 비용이 발생할 수 있습니다. 불필요한 지출을 피하려면 `azd down`을 실행하거나 리소스를 수동으로 삭제하세요.
   * 두 위치를 선택하라는 메시지가 표시됩니다. 하나는 대부분의 리소스를 위한 위치이고, 다른 하나는 OpenAI 리소스를 위한 위치입니다. 이 위치 목록은 [OpenAI 모델 가용성 표](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#global-standard-model-availability)를 기반으로 하며, 가용성이 변경됨에 따라 업데이트될 수 있습니다.

1. 애플리케이션이 성공적으로 배포되면 콘솔에 URL이 출력됩니다. 이 URL로 이동하여 브라우저에서 앱과 상호작용하세요. 앱을 사용해보려면 "Start conversation" 버튼을 클릭하고, "Hello"라고 말한 다음 "What is the whistleblower policy for Contoso electronics?"와 같은 데이터에 대한 질문을 하세요. 이제 [다음 섹션](#development-server)의 지침에 따라 로컬에서 앱을 실행할 수도 있습니다.

## 개발 서버

Azure 서비스를 프로비저닝한 상태에서 이 앱을 로컬로 실행할 수 있으며, [배포 지침](#deploying-the-app)을 따르거나, [기존 서비스를](docs/existing_services.md) 가리킴으로써 가능합니다.

1. `azd up` 명령으로 배포한 경우 `app/backend/.env` 파일에 필요한 환경 변수가 포함되어 있어야 합니다.

2. `azd up`을 사용하지 않은 경우, 다음 환경 변수를 포함하는 `app/backend/.env` 파일을 작성해야 합니다:

   ```shell
   AZURE_OPENAI_ENDPOINT=wss://<your instance name>.openai.azure.com
   AZURE_OPENAI_REALTIME_DEPLOYMENT=gpt-4o-realtime-preview
   AZURE_OPENAI_API_KEY=<your api key>
   AZURE_SEARCH_ENDPOINT=https://<your service name>.search.windows.net
   AZURE_SEARCH_INDEX=<your index name>
   AZURE_SEARCH_API_KEY=<your api key>
   ```

   Entra ID(로컬 실행 시 사용자, 배포 시 관리 ID)를 사용하려면 키를 설정하지 마세요.

4. 다음 명령을 실행하여 앱을 시작하세요:

   Windows:

   ```pwsh
   pwsh .\scripts\start.ps1
   ```

   Linux/Mac:

   ```bash
   ./scripts/start.sh
   ```

5. 앱은 [http://localhost:8765](http://localhost:8765)에서 이용할 수 있습니다.

   앱이 실행되면 위 URL로 이동하면 앱의 시작 화면을 볼 수 있습니다:
   ![앱 스크린샷](docs/talktoyourdataapp.png)

   앱을 사용해보려면 "Start conversation" 버튼을 클릭하고, "Hello"라고 말한 다음 "What is the whistleblower policy for Contoso electronics" 같은 데이터에 대한 질문을 하세요.

## Guidance

### 비용

리전과 사용량에 따라 가격이 다르므로, 사용량에 따른 정확한 비용을 예측할 수 없습니다.
그러나 아래 리소스에 대한 [Azure 가격 계산기](https://azure.com/e/a87a169b256e43c089015fda8182ca87)를 시도해볼 수 있습니다.

* Azure Container Apps: 1 CPU 코어, 2.0 GB RAM을 사용하는 소비 계획. 사용량 기반 요금제(Pay-as-You-Go). [가격](https://azure.microsoft.com/pricing/details/container-apps/)
* Azure OpenAI: 표준 등급, gpt-4o-realtime 및 text-embedding-3-large 모델. 사용된 1K 토큰 당 가격. [가격](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)
* Azure AI Search: 표준 등급, 1 replica, 무료 수준의 시맨틱 검색. 시간당 가격. [가격](https://azure.microsoft.com/pricing/details/search/)
* Azure Blob Storage: ZRS(Zone-redundant storage)를 갖춘 표준 등급. 저장 및 읽기 작업당 가격. [가격](https://azure.microsoft.com/pricing/details/storage/blobs/)
* Azure Monitor: 사용량 기준 요금제(Pay-as-You-Go). 데이터 수집량에 따른 비용 발생. [가격](https://azure.microsoft.com/pricing/details/monitor/)

비용을 줄이려면 다양한 서비스의 무료 SKU로 전환할 수 있지만, 해당 SKU에는 제한이 있습니다.

⚠️ 불필요한 비용을 피하기 위해 사용하지 않는 앱은 정지(Down)하거나, 포털에서 리소스 그룹을 삭제하거나 `azd down`을 실행하세요.

### Security

이 템플릿은 [Managed ID](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview)를 사용하여 개발자가 자격 증명을 관리할 필요를 생략할 수 있습니다. 애플리케이션은 Managed ID를 사용하여 자격 증명을 관리할 필요 없이 Microsoft Entra 토큰을 얻을 수 있습니다. 리포지토리에서 최선의 관행을 유지하기 위해 템플릿 기반의 솔루션을 만드는 모든 사람은 [Github secret scanning](https://docs.github.com/code-security/secret-scanning/about-secret-scanning) 설정이 리포지토리에서 활성화되어 있는지 확인하는 것을 권장합니다.


### Notes

>샘플 데이터: 이 데모에서 사용된 PDF 문서는 언어 모델(Azure OpenAI 서비스)을 사용하여 생성된 정보입니다. 이러한 문서에 포함된 정보는 시연 목적을 위한 것이며 Microsoft의 의견이나 신념을 반영하지 않습니다. Microsoft는 이 문서에 포함된 정보의 완전성, 정확성, 신뢰성, 적합성 또는 가용성에 대해 어떠한 종류의 표현이나 보증도 명시적 또는 묵시적으로 제공하지 않습니다. 모든 권리는 Microsoft에게 있습니다.
