# 🔧 설정 및 실행 가이드

## 1️⃣ 초기 설정

### Step 1: 저장소 클론
```bash
git clone https://github.com/your-username/semanticmodel.git
cd semanticmodel
```

### Step 2: 가상 환경 생성
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: 패키지 설치
```bash
pip install -r requirements.txt
```

### Step 4: 환경변수 설정
```bash
# .env.example 파일을 .env로 복사
cp .env.example .env
```

그 후 `.env` 파일을 텍스트 에디터로 열어서 다음 정보를 입력합니다:

```env
# Azure OpenAI 설정
AZURE_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxx  # 실제 API 키
AZURE_API_VERSION=2024-05-01-preview
AZURE_ENDPOINT=https://your-resource-name.openai.azure.com/

# 모델 설정
MODEL_NAME=gpt-5.1-chat
EMBEDDING_MODEL=text-embedding-3-large

# 경로 설정
TARGET_DIR=./test_images/target
CANDIDATE_DIR=./test_images/candidates
OUTPUT_FILE=data.jsonl

# API 호출 간격 (초)
SLEEP_TIME=0.5
```

---

## 2️⃣ Azure OpenAI 설정

### Azure 계정 생성 및 API 키 발급

1. [Azure Portal](https://portal.azure.com) 접속
2. "리소스 만들기" → "Azure OpenAI" 검색
3. Azure OpenAI 리소스 생성
4. 모델 배포 (gpt-5.1-chat, text-embedding-3-large)
5. 리소스 키 및 엔드포인트 복사

### API 키 확인 방법
```
Azure Portal → 해당 OpenAI 리소스 → "키 및 끝점"
- AZURE_API_KEY: 키 1 또는 키 2
- AZURE_ENDPOINT: 끝점 URL
```

---

## 3️⃣ 이미지 파일 준비

### 폴더 구조 생성
```bash
mkdir -p test_images/target
mkdir -p test_images/candidates
```

### 이미지 추가
- **target 폴더**: 기준이 될 상표 이미지 (1개 또는 여러 개 가능)
  - 예: `TARGET_logo.jpg`
  
- **candidates 폴더**: 비교할 상표 이미지들
  - 예: `candidate_1.jpg`, `candidate_2.png`, ...

### 지원 이미지 형식
- JPG, JPEG, PNG, GIF, BMP, WebP

---

## 4️⃣ 프로그램 실행

### 방법 1: 상표 분석 (필수 첫 실행)
```bash
# 이미지 분석 및 임베딩 생성
python src/trademark_analysis.py
```

**결과물**: `data.jsonl` 파일 생성
```json
{"file_name": "TARGET_logo.jpg", "description": "...", "embedding": [...]}
{"file_name": "candidate_1.jpg", "description": "...", "embedding": [...]}
...
```

### 방법 2: 유사도 비교
```bash
# Python 대화형 환경에서
python

>>> from src.compare import analyze_similarity
>>> results = analyze_similarity("data.jsonl")
>>> print(results)

# 결과 예시:
# {
#   "TARGET_logo.jpg": [
#       ("candidate_1.jpg", 0.92),
#       ("candidate_2.jpg", 0.78),
#       ("candidate_3.jpg", 0.65),
#   ]
# }
```

### 방법 3: CSV 리포트 생성
```bash
# Python 대화형 환경에서
python

>>> from src.compare import analyze_similarity, save_results_to_csv
>>> results = analyze_similarity("data.jsonl")
>>> save_results_to_csv(results, "analysis_results.csv")
```

---

## 5️⃣ 출력 결과

### data.jsonl
각 라인이 하나의 JSON 객체:
```json
{
  "file_name": "TARGET_logo.jpg",
  "description": "분석된 상표의 특징 설명",
  "embedding": [0.123, -0.456, 0.789, ...],
  "is_target": true
}
```

### CSV 리포트 예시
| 보호상표 | 유사상표 | 유사도 |
|--------|---------|-------|
| TARGET_logo.jpg | candidate_1.jpg | 0.92 |
| TARGET_logo.jpg | candidate_2.jpg | 0.78 |
| TARGET_logo.jpg | candidate_3.jpg | 0.65 |

---

## 🔍 실행 과정 상세 설명

### 1단계: 이미지 인코딩
- 각 이미지를 base64로 변환
- Azure OpenAI API로 전송

### 2단계: 이미지 분석 (Vision)
- GPT-5.1이 이미지 분석
- 상표의 특징, 색상, 디자인 등 설명 생성
- 한글 300자 이내

### 3단계: 임베딩 생성
- 설명문을 vector로 변환
- 3,072 차원의 임베딩 생성
- 의미적 유사성 계산을 위해 저장

### 4단계: 유사도 계산
- Cosine Similarity 사용
- 0 ~ 1 사이의 값 (1에 가까울수록 유사)
- 자동으로 높은 순서대로 정렬

---

## ⚙️ 고급 설정

### SLEEP_TIME 조정
```env
# 빠른 처리 원할 경우 (API 한도 있으면 에러 가능)
SLEEP_TIME=0.1

# 안정적 처리 (권장)
SLEEP_TIME=0.5

# 매우 안전한 처리 (느림)
SLEEP_TIME=2.0
```

### 출력 파일 경로 변경
```env
OUTPUT_FILE=./results/output_20260304.jsonl
OUTPUT_CSV=./results/analysis_20260304.csv
```

### 배치 처리
매우 많은 이미지 처리 시:
```env
SLEEP_TIME=1.0  # 더 오래 대기
```

---

## 🐛 문제 해결

### 1. "ModuleNotFoundError: No module named 'openai'"
```bash
# 해결책: 패키지 재설치
pip install --upgrade -r requirements.txt
```

### 2. "AZURE_API_KEY was not configured properly"
```
해결책:
1. .env 파일이 프로젝트 루트에 있는지 확인
2. AZURE_API_KEY 값이 올바른지 확인
3. 공백 및 특수 문자 확인
```

### 3. "Image file not found"
```
해결책:
1. TARGET_DIR과 CANDIDATE_DIR 경로 확인
2. 이미지 파일이 해당 폴더에 있는지 확인
3. 파일명에 한글이 포함되면 제거
```

### 4. API 연결 실패
```
해결책:
1. 인터넷 연결 확인
2. Azure OpenAI 리소스가 배포되었는지 확인
3. API 키가 유효한지 확인 (Portal에서 재발급)
4. 엔드포인트 URL이 올바른지 확인
```

### 5. 메모리 부족 (매우 많은 이미지)
```
해결책:
1. 이미지를 작은 배치로 나누어 처리
2. 큰 이미지는 리사이징
3. 메모리 확인: python -c "import psutil; print(psutil.virtual_memory())"
```

---

## 📊 성능 최적화

### 권장 설정
```env
# 표준 배치 (100개 이미지)
SLEEP_TIME=0.5

# 소규모 (10개 이미지 이하)
SLEEP_TIME=0.3

# 대규모 (1000개 이상)
SLEEP_TIME=1.0
```

### API 비용 추정
- 이미지 분석: 약 10-20 USD/1000 images
- 임베딩 생성: 약 0.02 USD/1000 requests
- 총 비용: 프로젝트 규모에 따라 결정

---

## 💡 팁

1. **작은 테스트부터**: 먼저 1-2개 이미지로 작동 확인
2. **로그 저장**: 처리 과정을 파일로 기록하면 디버깅 용이
3. **정기적 백업**: 분석 결과 JSON/CSV 파일 정기 백업
4. **API 모니터링**: Azure Portal에서 API 사용량 확인

---

## 📞 지원

문제 발생 시:
1. `.env` 파일 다시 확인
2. 이미지 파일 경로 재확인
3. GitHub Issues에 로그 함께 기재
