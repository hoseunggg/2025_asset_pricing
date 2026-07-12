# Quantitative Finance & ML Portfolio

> 정량 금융과 머신러닝을 아우르는 세 개의 독립 프로젝트 — **협력게임이론 기반 포트폴리오 기여도 분해**, **팩터 ML 자산가격**, **하이퍼파라미터 탐색 방법론 벤치마크**.

각 프로젝트는 자체 `README.md`·`requirements.txt`를 갖춘 독립 폴더입니다. 관심 주제부터 바로 열어보세요.

---

## 프로젝트

### 1. [Shapley Sector Portfolio](./shapley-sector-portfolio) &nbsp;·&nbsp; `실행 가능 ✅`
S&P 500 **11개 섹터 ETF를 협력게임의 플레이어**로 보고, 각 섹터가 포트폴리오의 위험 감소·평균-분산 효용에 얼마나 기여하는지를 **Shapley value**로 분해합니다.
- **보여주는 것** — 게임이론을 실증 금융에 적용, 몬테카를로 근사로 지수적 계산량($2^{11}$) 회피, 롤링 윈도로 시간에 따른 기여도 변화 추적
- **스택** — NumPy · SciPy(SLSQP·linprog) · joblib · yfinance
- 이론·유도·결과는 [리포트 PDF](./shapley-sector-portfolio/report/Trading_report.pdf)에 정리

### 2. [ML Asset Pricing](./ml-asset-pricing) &nbsp;·&nbsp; `데이터 필요 🔑`
Gu·Kelly·Xiu(2020) *"Empirical Asset Pricing via Machine Learning"* 을 축소 재현. **17개 팩터**로 다음 달 횡단면 수익률을 예측하고 **롱숏(H-L) 포트폴리오**를 백테스트합니다.
- **보여주는 것** — 팩터 엔지니어링, 시간순 train/test 분할(룩어헤드 방지), LASSO vs XGBoost 비교, Sharpe·MDD 기반 성과 평가
- **스택** — pandas · scikit-learn(LASSO) · XGBoost
- CRSP 월별 데이터가 필요하며 저작권상 저장소에 미포함 (실행법은 프로젝트 README 참고)

### 3. [MNIST Hyperparameter Search](./mnist-hp-search) &nbsp;·&nbsp; `실행 가능 ✅`
완전연결 신경망에 대해 **4가지 하이퍼파라미터 탐색 전략**(Grid / Random / Bayesian / Hyperband)을 비교하고 `joblib` 병렬 실험 파이프라인으로 평가합니다.
- **보여주는 것** — 모델 선택·하이퍼파라미터 최적화 방법론, 병렬 실험 설계, 정규화/드롭아웃/과적합 분석
- **스택** — TensorFlow/Keras · Keras Tuner · joblib

---

## 빠른 시작

```bash
cd <project-folder>            # 예: shapley-sector-portfolio
pip install -r requirements.txt
jupyter lab                    # 노트북 실행
```

- 각 노트북은 **해당 폴더를 작업 디렉터리(cwd)로** 삼아 상대경로로 동작합니다.
- 데이터 없이 바로 돌아가는 건 **프로젝트 1·3** (yfinance 실시간 / Keras 내장 MNIST). 프로젝트 2는 CRSP 데이터가 있어야 합니다.

## 재현성

- 세 프로젝트 모두 난수 시드 고정(`SEED = 42`).
- 외부 데이터 의존성(Yahoo Finance 실시간, CRSP)은 각 README에 명시. yfinance는 실시간이라 절대 수치가 드리프트할 수 있습니다.
- 첫 실행 성공 후 `pip freeze > requirements.lock.txt`로 정확한 버전 고정 권장.
