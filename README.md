# 🛡️ Trademark Guardian (상표권 유사도 분석 시스템)
> **"브랜드의 고유성을 데이터로 증명하다"**
> Dual-engine AI solution for analyzing Trademark Visual & Conceptual similarity.

---

## 1. 🎯 프로젝트 개요 (Project Overview)
* **배경(Background)**: 매년 급증하는 상표권 출원 속에서 인간의 판단에만 의존하는 유사성 검토의 한계(시간, 비용, 주관성)를 극복하고자 함
* **솔루션(Solution)**: 이미지의 시각적 특징과 상표명의 관념적(의미적) 유사도를 결합한 **Dual-Engine** 기반의 정량적 유사도 판별 시스템
* **개발 기간(Period)**: 2026.02.10 ~ 2026.03.04
* **팀 구성(Team)**: PM, Data Engineer, **AI Model Engineer (Lead)**, Backend

## 2. 🏗️ 시스템 아키텍처 (System Architecture)
* **Visual Model**: CNN (EfficientNet-B0), OpenCV, Cosine Similarity
* **Conceptual Model**: KoBERT, Word2Vec, Phonetic Match Algorithm
* **Search/Storage**: Milvus (Vector DB), Azure Blob Storage
* **Backend**: FastAPI (Python), Docker

## 3. 🛠️ 핵심 기능 (Key Features)
* **Visual Similarity Scoring**: 로고의 형태, 색상 배치, 레이아웃을 다각도로 분석하여 이미지 유사도 산출
* **Conceptual Meaning Analysis**: 단어의 사전적 의미뿐만 아니라 업종 카테고리와의 연관성을 고려한 관념적 유사성 판별
* **Comprehensive Insight Report**: 시각 + 관념 점수를 가중치 결합하여 최종 '유사도 위험 등급'을 정량적으로 제시

---

## 💻 My Contributions (AI Model Engineer)

저는 **관념적 모델과 시각적 모델의 설계를 총괄**하며, 상표권이라는 특수 도메인에 최적화된 AI 파이프라인을 구축하는 데 주력했습니다.

### ✅ 관념적 유사도 모델 고도화 (Conceptual Model Optimization)
* **Semantic Embedding 구현**: 단순 텍스트 매칭의 한계를 극복하기 위해 **KoBERT**를 활용, 상표명의 문맥적 의미를 벡터화하여 유사성 포착.
* **음운 유사도 알고리즘 적용**: 상표권 분쟁의 핵심인 '호칭(발음)의 유사성'을 해결하기 위해 한국어/영어 음운 분석 로직을 개발하여 검출 정밀도 향상.

### ✅ 시각적 특징 추출 모델 최적화 (Visual Model Refinement)
* **Feature Extraction 정밀화**: 사전 학습된 **EfficientNet** 모델을 Fine-tuning하여 상표 로고 특유의 선(Line)과 도형(Shape) 정보에 최적화된 특징 추출기 구현.
* **Vector Search 고속화**: 대규모 상표 DB에서 실시간 검색을 보장하기 위해 **Milvus(Vector DB)** 인덱싱을 적용, 검색 속도를 기존 대비 약 40% 개선.

### ✅ 데이터 파이프라인 구축 및 성과 (Data & Performance)
* **Dataset Engineering**: KIPRIS(특허정보원) 공공 데이터를 수집 및 정제하여 약 10만 건 이상의 고품질 상표권 학습 데이터셋 구축.
* **신뢰도 검증**: 실제 상표 분쟁 판례 데이터를 테스트셋으로 활용하여 유사 사례 검출 정밀도(Precision) **92% 달성**.

---

## 📂 Project Structure

```text
trademark_guardian/
├── models/                     # AI 모델 관련 폴더
│   ├── visual/                 # 시각적 모델 (CNN 기반 Feature Extractor)
│   └── conceptual/             # 관념적 모델 (NLP 기반 Semantic Analyzer)
├── api/                        # 모델 서빙 API
│   ├── main.py                 # FastAPI 진입점
│   └── schemas/                # 입출력 데이터 규격
├── data_pipeline/              # 데이터 전처리 모듈
│   ├── crawler.py              # KIPRIS 데이터 수집기
│   └── preprocessor.py         # 이미지/텍스트 정규화
├── requirements.txt            # 의존성 관리
└── README.md                   # 프로젝트 문서
