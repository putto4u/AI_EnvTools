

## 🛠️ 도커 자동 완성 기능 설정 방법

PowerShell에서 도커 자동 완성을 활성화하려면 **도커 자동 완성 스크립트**를 찾아서, PowerShell 시작 시 자동으로 로드되도록 **`profile.ps1`** 파일에 등록해야 합니다.

### 1\. 도커 자동 완성 스크립트 확인

Docker Desktop 설치 시, 도커는 보통 자동 완성을 위한 모듈을 특정 경로에 설치합니다.

  * **스크립트 파일 이름:** `docker.ps1` 또는 이와 유사한 이름.

### 2\. PowerShell 프로필 파일 (`profile.ps1`) 편집

`profile.ps1`은 PowerShell이 실행될 때마다 자동으로 로드되는 사용자 설정 파일입니다. 이 파일에 도커 자동 완성 스크립트를 로드하는 명령을 추가해야 합니다.

#### 단계 1: 프로필 파일 경로 확인

다음 명령어를 PowerShell에 입력하여 프로필 파일의 정확한 경로를 확인합니다.

```powershell
$profile
```

(경로는 보통 `C:\Users\사용자이름\Documents\WindowsPowerShell\profile.ps1`입니다.)

#### 단계 2: 프로필 파일 열기 (없으면 생성)

다음 명령어로 프로필 파일을 텍스트 편집기(예: 메모장)로 열거나, 파일이 없으면 새로 생성합니다.

```powershell
notepad $profile
```

#### 단계 3: 도커 자동 완성 명령 추가

파일 내용 중 마지막 줄에 다음 명령을 추가하여 도커의 자동 완성 모듈을 가져옵니다.

> **주의:** PowerShell 환경에 따라 `docker.ps1`의 정확한 위치는 다를 수 있습니다. 만약 아래 경로에서 파일을 찾을 수 없다면, 파일 탐색기로 `C:\Program Files\Docker\Docker` 폴더 내에서 `*.ps1` 파일을 검색해 보세요.

```powershell
# Docker 자동 완성을 위한 모듈 로드 (Docker Desktop 기준)
Import-Module 'C:\Program Files\Docker\Docker\resources\dockerd\docker.ps1' -Force
```

위의 경로가 맞다면, `docker.ps1` 파일을 로드하는 명령을 **`profile.ps1`** 파일에 추가하고 저장합니다.

### 3\. PowerShell 재시작 및 테스트

1.  PowerShell 창을 **닫았다가 다시 시작**합니다. (변경된 `profile.ps1` 파일을 로드하기 위함)

2.  다음 명령어를 입력하고 **Tab 키**를 눌러봅니다.

    ```powershell
    docker run -<Tab>
    docker ps -<Tab>
    ```

성공적으로 설정되었다면, **`-d`, `-p`, `--name`** 등 도커 명령어의 **파라미터**와 실행 중인 컨테이너의 **ID**가 자동 완성 목록에 나타나야 합니다.

-----

## 💡 IT 전문가 팁: 모듈 사용의 원리

이 과정은 `Import-Module` 명령을 통해 PowerShell에게 \*\*"이제부터 도커와 관련된 명령어들이 들어오면, 이 스크립트(`docker.ps1`)에 정의된 규칙에 따라 자동 완성을 도와라"\*\*라고 지시하는 것과 같습니다. 이 과정을 거쳐야 PowerShell이 단순한 과거 기록을 넘어 도커의 문맥을 이해하고 전문적인 자동 완성을 제공하게 됩니다.

## 1\. 해결책: 시스템 전체에서 `docker.ps1` 검색하기

파일이 표준 경로가 아닌 다른 곳에 설치되었을 가능성이 가장 높습니다. **파일 탐색기**를 이용해 시스템 전체에서 파일을 검색해 보세요.

1.  **파일 탐색기 열기:** Windows 키 + E를 눌러 파일 탐색기를 엽니다.
2.  **`C:` 드라이브 선택:** 왼쪽 탐색 창에서 `C:` 드라이브를 선택합니다.
3.  **검색:** 오른쪽 상단 검색창에 \*\*`docker.ps1`\*\*을 입력하고 검색을 시작합니다.

파일이 발견되면, 해당 파일의 **전체 경로**를 확인하고, 이 경로를 아까 알려드렸던 **`profile.ps1`** 파일에 정확하게 입력해 주시면 됩니다.

```powershell
# 예시: 만약 다른 경로에서 찾았다면
Import-Module '새로운\전체\경로\docker.ps1' -Force
```

-----

## 2\. 해결책: PowerShell 갤러리에서 스크립트 직접 가져오기 (고급)

만약 파일 검색으로도 `docker.ps1`을 찾지 못했다면, 도커는 PowerShell 갤러리([PowerShell Gallery](https://www.powershellgallery.com/))를 통해 자동 완성 스크립트를 **모듈 형태로 제공**하고 있습니다. 이를 설치하는 것이 가장 확실한 방법입니다.

이 방법은 **PowerShell 갤러리에서 모듈을 설치**하고, 이를 `profile.ps1`에 등록하는 과정입니다.

### 단계 1: DockerCompletion 모듈 설치

관리자 권한이 필요할 수 있습니다. 이미 관리자 권한 PowerShell에서 작업 중이라면 그대로 진행합니다.

```powershell
# PSGallery가 신뢰할 수 있는 저장소인지 확인 후, DockerCompletion 모듈 설치
Install-Module -Name DockerCompletion -Repository PSGallery -Scope CurrentUser
```

  * **`-Scope CurrentUser`**: 시스템 전체가 아닌 현재 사용자에게만 모듈을 설치하여 권한 문제를 피합니다.

### 단계 2: 프로필 파일에 로드 명령 추가

이제 설치한 모듈을 PowerShell 시작 시 자동으로 로드하도록 `profile.ps1` 파일에 추가합니다.

1.  **`notepad $profile`** 명령으로 `profile.ps1`을 다시 엽니다.

2.  파일의 마지막 줄에 다음 명령어를 추가하고 저장합니다.

    ```powershell
    # DockerCompletion 모듈 로드
    Import-Module DockerCompletion
    ```

### 단계 3: PowerShell 재시작 및 테스트

PowerShell 창을 닫고 다시 실행한 후, `docker` 명령어와 **Tab 키**를 눌러 자동 완성이 정상적으로 작동하는지 확인합니다.

이 방법은 파일을 직접 찾아 헤맬 필요 없이 **PowerShell 표준 방식**으로 자동 완성 기능을 활성화하는, IT 전문가다운 깔끔한 해결책입니다. 👍
