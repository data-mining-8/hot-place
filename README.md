# 서울시 핫플레이스 분석 및 예측 프로젝트

# 주요 파일
- [`clustering.ipynb`](https://github.com/data-mining-8/hot-place/blob/main/clustering.ipynb) : 클러스터링 분석 코드
- [`spvised_hp_pred.ipynb`](https://github.com/data-mining-8/hot-place/blob/main/spvised_hp_pred.ipynb) : 핫플레이스 예측 모델 개발 코드

## 데이터 출처
- 서울시 상권 데이터 : https://data.seoul.go.kr/dataList/OA-15547/S/1/datasetView.do
- 서울시 유동인구 데이터 : https://data.seoul.go.kr/dataList/OA-15547/S/1/datasetView.do

## 📌 프로젝트 개요
본 프로젝트는 서울시 상권 데이터를 활용하여 지역별 특성을 분석하고 핫플레이스를 예측하는 데이터마이닝 프로젝트입니다. 클러스터링 기법을 통한 지역 특성 분류와 머신러닝을 이용한 핫플레이스 예측 모델 개발을 수행하였습니다.

## 📁 데이터 구성
| 파일명 | 설명 | 크기 |
|--------|------|------|
| `floating_population.csv` | 시간대/연령대별 유동인구 데이터 | 2.3MB |
| `sales.csv` | 상세 매출 정보 | 29MB |
| `store.csv` | 점포 운영 현황 | 12MB |
| `label.xlsx` | 핫플레이스 라벨 데이터 | 13KB |

## 🔍 주요 분석 내용

### 1. 지역 특성 클러스터링 (`clustering.ipynb`)
- **분석 방법**: K-means 클러스터링 (k=10)
- **주요 변수**:
  ```python
  selected_df["총_유동인구_수"] = merged["총_유동인구_수"]
  selected_df["청년층_유동인구_비율"] = (연령대 20+30 유동인구)/총유동인구
  selected_df["점포당_월매출액"] = 당월_매출_금액/유사_업종_점포_수
  ```
- **분석 절차**:
  1. 이상치 제거 (IQR 기반)
  2. 표준화(StandardScaler) 및 PCA 차원 축소
  3. 실루엣 점수 기반 최적 클러스터 탐색

### 2. 핫플레이스 예측 모델 (`spvised_hp_pred.ipynb`)
- **모델 특징**:
  - 과거 4년간(2020-2023) 데이터 활용
  - CAGR(연평균성장률) 기반 성장 지표 반영
  ```python
  # 성장률 계산 예시
  metrics['매출액_연평균증가율_4년전_작년'] = 
    (current_year_total_sales / sales_2020_val) ** (1/3) - 1
  ```
- **주요 피처**:
  - 작년 대비 순증가점포수
  - 프랜차이즈 점포 비율
  - 주말 매출 비율
  - 연령대 20-30 매출 비중

## 📊 분석 결과
- **클러스터링 결과**:
  - 10개 그룹으로 분류된 지역 특성
  - 예시: 
    - Cluster 2: 핫플레이스 밀집지역
    - Cluster 3: 고객단가 높은 프랜차이즈 밀집지역
    - Cluster 5: 야간 유동인구 많은 젊은 층 밀집지역

- **예측 모델 성능**:
  - 임계값 0.52 기준 예측 정확도: 82%
  - 주요 영향 요인: 점포당 매출액 > 청년층 비율 > 개업율

## 🗂️ 파일 구조

 hot-place/
 <pre>
<code>
├── data/
│   ├── raw/                             # 원본 데이터
│   └── processed/                       # 처리된 데이터
├── notebooks/
│   ├── clustering.ipynb                 # 클러스터링 분석
│   └── spvised_hp_pred.ipynb           # 예측 모델 개발
├── results/
│   ├── prediction_results.csv           # 전체 예측 결과
│   └── prediction_results_threshold_0.52.csv  # 최종 필터링 결과
</code>
</pre>


결과

## 🛠️ 기술 스택
- **Python 3.10**
- 주요 라이브러리: Pandas, NumPy, Scikit-learn, Matplotlib
- 분석 도구: Jupyter Notebook

## 📝 결론 및 활용 방안
본 분석을 통해 도출된 인사이트는 다음과 같이 활용 가능합니다:
1. **입지 선정**: 클러스터 특성에 맞는 점포 유형 추천
2. **투자 의사결정**: 성장 잠재력이 높은 지역 식별
3. **정책 수립**: 유동인구 특성에 맞는 지역 개발 계획 수립
