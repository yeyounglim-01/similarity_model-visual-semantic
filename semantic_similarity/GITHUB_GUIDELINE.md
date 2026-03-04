# GitHub 업로드 가이드

## 📋 분석 결과

### ✅ GitHub에 올려도 되는 파일

| 파일명 | 설명 | 상태 |
|--------|------|------|
| `config.py` | 환경변수 기반 설정 관리 | 안전함 ✓ |
| `compare.py` | 유사도 비교 로직 | 안전함 ✓ |
| `trademark_analysis.py` | 상표 분석 메인 로직 | 안전함 ✓ |
| `requirements.txt` | 의존성 패키지 | 안전함 ✓ |
| `.env.example` | 환경변수 템플릿 | 안전함 ✓ |
| `README.md` | 프로젝트 설명서 | 개선 필요 |

### ❌ GitHub에 올리면 안 되는 파일

| 파일명 | 이유 | 조치 |
|--------|------|------|
| `.env` | **API 키 등 민감정보 포함** | `.gitignore` 추가 |
| `data.jsonl` | 실행 결과 데이터 (2.8MB, 재생성 가능) | `.gitignore` 추가 |
| `targeresult_TARGET_logo.csv` | 실행 결과 데이터 (재생성 가능) | `.gitignore` 추가 |
| `*.png` (visualization) | 테스트 결과 이미지 (필요시 재생성) | `.gitignore` 추가 |
| `semanticmodel_final0210.zip` | 대용량 압축파일 (불필요한 파일 포함 가능) | `.gitignore` 추가 |

---

## 🎯 권장 폴더 구조

```
semanticmodel/
├── .env.example              # 환경변수 템플릿
├── .gitignore                # Git 제외 파일 목록
├── README.md                 # 프로젝트 설명
├── requirements.txt          # 의존성
├──
│── src/
│   ├── config.py            # 설정 관리
│   ├── trademark_analysis.py # 상표 분석 메인
│   └── compare.py           # 유사도 비교
│
├── test_images/             # 테스트 이미지 (선택)
│   ├── target/              # 기준 상표 이미지
│   └── candidates/          # 비교 상표 이미지
│
├── outputs/                 # 실행 결과 (생성됨)
│   ├── data.jsonl           # 분석 결과 데이터
│   └── results.csv          # 비교 결과
│
└── docs/                    # 추가 문서
    └── setup-guide.md       # 설치 가이드
```

---

## 🔧 설정 단계

### 1. .gitignore 파일 생성
프로젝트 루트에 `.gitignore` 파일을 생성하여 다음 항목 추가:

```
# 환경설정
.env
.env.local
*.pyc
__pycache__/

# 실행 결과
data.jsonl
*.csv
outputs/

# 테스트 이미지 (대용량)
test_images/

# IDE
.vscode/
.idea/
*.swp

# 불필요한 파일
*.zip
*.png
.DS_Store
```

### 2. 로컬 설정 (.env 파일 생성)
`.env.example` 복사하여 `.env` 생성 후 실제 API 키 입력:

```bash
cp .env.example .env
```

그리고 `.env` 파일에 다음 정보 입력:
```
AZURE_API_KEY=your_actual_api_key
AZURE_API_VERSION=2024-05-01-preview
AZURE_ENDPOINT=https://your-resource-name.openai.azure.com/
MODEL_NAME=gpt-5.1-chat
EMBEDDING_MODEL=text-embedding-3-large
TARGET_DIR=./test_images/target
CANDIDATE_DIR=./test_images/candidates
OUTPUT_FILE=data.jsonl
SLEEP_TIME=0.5
```

### 3. Python 파일 정리 (선택사항)
현재 구조가 깔끔하므로 필요시 `src/` 폴더로 이동:

```bash
mkdir src
mv config.py compare.py trademark_analysis.py src/
```

이 경우 Python 실행 시 경로 수정 필요:
```python
from src.config import ...
```

---

## 📤 GitHub 업로드 절차

### 1. Git 초기화
```bash
cd semanticmodel_share
git init
git config user.email "your-email@example.com"
git config user.name "Your Name"
```

### 2. 파일 추가 (`.gitignore` 먼저 생성 필수!)
```bash
git add .
git status  # 확인: .env, data.jsonl, *.csv 등이 제외되어야 함
git commit -m "Initial commit: Trademark analysis with semantic model"
```

### 3. 원격 저장소 연결
```bash
git remote add origin https://github.com/your-username/semanticmodel.git
git branch -M main
git push -u origin main
```

---

## 📝 README 개선 사항

현재 README는 한글이 깨져있습니다. 다음과 같이 개선 권장:

- 프로젝트 개요 명확히 작성
- 설치 방법 단계별 표시
- 사용 방법 코드 예제 포함
- 요구사항 명시
- 라이선스 정보 추가
- 기여 방법 작성

---

## ✨ 보안 체크리스트

- [x] API 키는 `.env` 파일에만 저장
- [x] `.env.example` 파일에는 실제 키값 없음
- [x] 모든 Python 파일에서 `os.getenv()` 사용 (하드코딩 없음)
- [x] 민감 정보 데이터 없음
- [ ] `.gitignore` 파일 생성 필요
- [ ] README 개선 필요

---

## 📚 추가 권장사항

1. **라이선스**: LICENSE 파일 추가 (MIT, Apache 2.0 등)
2. **설치 가이드**: docs/ 폴더에 상세 설정 가이드
3. **예제**: examples/ 폴더에 사용 예제 추가
4. **테스트**: 간단한 테스트 코드 포함
5. **GitHub Actions**: CI/CD 설정 (선택사항)
