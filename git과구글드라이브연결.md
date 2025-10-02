강사님, 대용량 파일 관리의 가장 현명하고 안전한 방법인 **"코드는 GitHub, 데이터는 클라우드(Google Drive)"** 전략을 위한 구체적인 워크플로우와 실전 코드를 안내해 드리겠습니다.

### \---

## 1\. GitHub에 대용량 파일 경로만 남기는 방법 (모범 사례)

Git의 저장소 크기 제한(특히 Git LFS 무료 용량)을 우회하고, 수강생들에게 \*\*Git의 목적(코드 관리)\*\*을 명확히 교육하는 데 적합한 방법입니다.

### 단계 1: 대용량 파일은 Drive에 저장하고 `.gitignore` 처리

먼저, AI 모델 파일(`*.h5`, `*.pt`), 압축된 데이터셋(`*.zip`), 대형 로그 파일 등 Git으로 관리할 필요가 없는 파일은 Google Drive의 **전용 폴더**에 저장합니다.

이 파일들이 실수로 Git에 커밋되는 것을 막기 위해 프로젝트 루트 디렉토리의 **`.gitignore`** 파일에 추가합니다.

```bash
# .gitignore 파일 내용 예시

# 모델 파일 (.pt, .h5, .pth 등) 무시
*.pt
*.h5
*.pth
/models/*.h5
 
# 대용량 데이터셋 파일 무시
data/raw/*.zip
 
# 학습 로그 파일 무시
/logs/
```

### 단계 2: GitHub에 **경로 참조 파일** 커밋하기

실제 코드에서 데이터를 찾을 수 있도록, 해당 파일의 **경로를 담은 작은 텍스트 파일이나 설정 파일**을 Git에 커밋합니다.

| 파일 유형 | 설명 | 예시 |
| :--- | :--- | :--- |
| **TEXT 파일** | 파일의 Drive 경로를 한 줄로 기록 | `data/path_to_dataset.txt` |
| **JSON/YAML 설정** | 데이터 경로와 메타데이터를 함께 기록 | `config/data_paths.json` |

**`config/data_paths.json` 예시:**

```json
{
    "raw_data_path": "/content/drive/MyDrive/AI_Project_Data/original_dataset.zip",
    "model_save_dir": "/content/drive/MyDrive/AI_Project_Models/"
}
```

이제 GitHub 저장소에는 수백 GB의 데이터 대신, **단 몇 KB의 경로 정보가 담긴 설정 파일만 남게 됩니다.**

### \---

## 2\. 실제 코드에서 Google Drive 파일 업로드/호출 방법 (Colab 예제)

강의록에 포함하기 좋은, Colab 환경에서 가장 많이 쓰이는 마운트 및 파일 입출력(I/O) 코드 예제입니다.

### A. Google Drive 마운트 (필수)

Colab 세션이 시작될 때마다 Google Drive를 가상 파일 시스템으로 연결하는 첫 번째 단계입니다.

```python
from google.colab import drive

# Google Drive 마운트. 인증 코드를 입력해야 합니다.
print("Google Drive를 마운트합니다.")
drive.mount('/content/drive')

# 마운트된 Drive 경로 확인 (주로 이 경로를 사용)
DRIVE_ROOT = '/content/drive/MyDrive' 
print(f"Drive 루트 경로: {DRIVE_ROOT}")
```

### B. 대용량 데이터 파일 **호출** (Read)

위에서 GitHub에 커밋한 **경로 참조 파일**을 읽어 실제 Google Drive의 대용량 파일에 접근합니다.

```python
import pandas as pd
import json
import os

# 1. GitHub에 커밋된 설정 파일 읽기
# (my-project는 Git Clone한 프로젝트 디렉토리라고 가정)
config_path = 'my-project/config/data_paths.json' 
with open(config_path, 'r', encoding='utf-8') as f:
    config = json.load(f)

# 2. Drive 경로에 DRIVE_ROOT를 붙여 실제 경로를 완성
# (참조: /content/drive/MyDrive/AI_Project_Data/original_dataset.zip)

# Colab에서 Drive가 /content/drive/에 마운트되므로,
# 설정 파일의 경로를 Drive 마운트 경로에 맞게 조정해야 합니다.
# (이 예제에서는 json에 Full Path를 기록했다고 가정합니다)
DATA_PATH = config['raw_data_path'] 

# 3. 실제 Drive의 대용량 파일 로드 (예: CSV 파일)
try:
    # 예시: Drive 경로에 있는 대용량 CSV 파일을 로드
    df = pd.read_csv(os.path.join(DATA_PATH, 'large_data.csv'))
    print(f"데이터셋 로드 성공! 크기: {len(df)} rows")
except FileNotFoundError:
    print(f"오류: Drive 경로에 파일이 없습니다. 경로를 확인하세요: {DATA_PATH}")
```

### C. 학습된 모델 또는 결과물 **저장** (Write)

Colab 인스턴스의 휘발성 메모리에서 작업한 결과(예: 학습된 AI 모델)를 세션이 종료된 후에도 보존하기 위해 Google Drive에 저장합니다.

```python
import torch # PyTorch 모델 저장 예시
import os

# 1. 저장할 Drive 경로 지정 (JSON 설정에서 가져오거나 직접 지정)
MODEL_SAVE_DIR = '/content/drive/MyDrive/AI_Project_Models/Trained_Models/'
MODEL_NAME = 'best_model_epoch_10.pt'

# 디렉토리가 없으면 생성 (실수 방지)
os.makedirs(MODEL_SAVE_DIR, exist_ok=True)

# 2. 모델 저장 (대용량 파일 쓰기)
# model은 Colab에서 학습시킨 torch.nn.Module 인스턴스라고 가정
# torch.save(model.state_dict(), os.path.join(MODEL_SAVE_DIR, MODEL_NAME))

print(f"모델 저장 완료: {os.path.join(MODEL_SAVE_DIR, MODEL_NAME)}")
```

이 워크플로우를 통해 수강생들은 **Git의 무결성을 유지**하면서, Colab의 **고성능 환경을 활용**하고, **Google Drive의 영속적인 저장 공간**까지 활용하는 전문가 수준의 파이프라인을 구축할 수 있습니다.
