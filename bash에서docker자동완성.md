IT 전문가 강사님, \*\*Bash 환경(Git Bash, WSL 등)\*\*에서는 PowerShell보다 **훨씬 간단하고 표준적인 방법**으로 도커 명령어 자동 완성 기능을 설정할 수 있습니다. 🐚

리눅스 및 유닉스 계열 셸(Shell)인 Bash는 **`source`** 명령과 **`~/.bashrc`** 파일을 이용해 설정을 관리하며, 도커(Docker) 또한 이에 맞춰 자동 완성 스크립트를 제공합니다.

-----

## 🛠️ Bash에서 도커 자동 완성 설정 방법

Bash 환경에서 도커 자동 완성을 활성화하려면 **도커가 제공하는 자동 완성 스크립트**를 Bash 프로필 파일에 등록해야 합니다.

### 1\. 도커 자동 완성 스크립트 경로 확인

Docker Desktop 설치 시, 도커는 Bash용 자동 완성 스크립트를 다음 위치에 설치합니다.

```bash
/usr/share/bash-completion/completions/docker
```

Git Bash나 WSL 환경에서 이 경로의 스크립트를 로드해야 합니다.

### 2\. 프로필 파일 (`.bashrc`) 편집

Bash를 시작할 때마다 이 스크립트가 자동으로 로드되도록 사용자 프로필 파일인 `~/.bashrc`에 명령을 추가해야 합니다.

#### 단계 1: `.bashrc` 파일 열기

Bash 터미널을 열고 다음 명령어를 사용하여 프로필 파일을 편집합니다.

```bash
nano ~/.bashrc   # nano 에디터 사용
# 또는
vi ~/.bashrc     # vi/vim 에디터 사용
```

#### 단계 2: 자동 완성 설정 명령어 추가

파일의 맨 아래에 다음 내용을 추가하고 저장합니다. 이 코드는 \*\*자동 완성 패키지(bash-completion)\*\*가 설치되어 있을 때 도커 자동 완성 스크립트를 로드하도록 지시합니다.

```bash
# 도커 명령어 자동 완성 활성화 설정
if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
fi

if [ -f /usr/share/bash-completion/completions/docker ]; then
    . /usr/share/bash-completion/completions/docker
fi
```

> **참고:** Git Bash 환경에서는 `bash_completion`이 기본적으로 활성화되어 있을 수 있습니다. 핵심은 `completions/docker` 파일을 `source`(`.` 명령)하는 것입니다.

#### 단계 3: 변경 사항 적용 또는 터미널 재시작

파일을 저장하고 닫은 후, 다음 명령어를 실행하여 변경된 설정을 **현재 세션에 즉시 적용**합니다.

```bash
source ~/.bashrc
```

### 3\. 테스트

이제 터미널에서 다음 명령어를 입력하고 **Tab 키**를 눌러 자동 완성이 정상적으로 작동하는지 확인합니다.

```bash
docker r<Tab>  # 'run' 등으로 자동 완성
docker run -<Tab> # -d, -p, --name 등 옵션 자동 완성
```

**Bash 환경**은 명령어 환경이 **표준화**되어 있어 PowerShell보다 자동 완성 설정을 훨씬 직관적으로 처리할 수 있다는 점을 수강생들에게 강조해 주시면 좋습니다. 👍
