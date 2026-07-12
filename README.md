# 2025 Asset Pricing

![last commit](https://img.shields.io/github/last-commit/hoseunggg/2025_asset_pricing)
![repo size](https://img.shields.io/github/repo-size/hoseunggg/2025_asset_pricing)

계량 금융과 머신러닝을 주제로 진행한 프로젝트 세 개입니다. 폴더마다 따로 돌아가며, 각각 README와 requirements.txt를 두었습니다.

## 구성

**1. Shapley Sector Portfolio** — `shapley-sector-portfolio/`

S&P 500 섹터 ETF 11개를 협력게임의 플레이어로 놓고, 각 섹터가 포트폴리오 위험을 줄이고 평균-분산 효용을 키우는 데 기여한 몫을 Shapley 값으로 계산합니다. 플레이어가 11명이면 연합이 2¹¹개까지 늘어나는데, 이걸 전부 계산하는 대신 몬테카를로 순열 샘플링으로 근사했습니다. 36개월 롤링 윈도를 돌려 기여도가 시기별로 어떻게 달라지는지도 살펴봤습니다. 데이터는 yfinance로 받아오므로 따로 준비할 것 없이 실행됩니다. 이론과 수식 유도는 [report/Trading_report.pdf](./shapley-sector-portfolio/report/Trading_report.pdf)에 정리해 두었습니다.

사용: NumPy, SciPy(SLSQP·linprog), joblib, yfinance

**2. ML Asset Pricing** — `ml-asset-pricing/`

Gu·Kelly·Xiu(2020)의 *Empirical Asset Pricing via Machine Learning* 을 축소해 재현했습니다. 팩터 17개로 다음 달 횡단면 수익률을 예측하고, 예측 상·하위 종목을 묶어 롱숏 포트폴리오를 백테스트합니다. 학습과 테스트는 시점 기준으로 잘라 미래 정보가 새지 않게 했고, LASSO와 XGBoost를 같이 돌려 비교했습니다. CRSP 월별 데이터가 필요한데 저작권 때문에 저장소에는 넣지 않았습니다. 실행 방법은 폴더 안 README를 참고하세요.

사용: pandas, scikit-learn, XGBoost

**3. MNIST Hyperparameter Search** — `mnist-hp-search/`

완전연결 신경망을 대상으로 하이퍼파라미터 탐색 전략 네 가지(Grid, Random, Bayesian, Hyperband)를 비교합니다. joblib으로 실험을 병렬로 돌리고, 정규화·드롭아웃·과적합을 나눠서 분석했습니다. MNIST는 Keras에 들어 있어 바로 실행됩니다.

사용: TensorFlow/Keras, Keras Tuner, joblib

## 폴더 구조

```
2025_asset_pricing/
├── shapley-sector-portfolio/
│   ├── shapley_sector_portfolio.ipynb
│   ├── report/Trading_report.pdf
│   ├── README.md
│   └── requirements.txt
├── ml-asset-pricing/
│   ├── ml_asset_pricing.ipynb
│   ├── README.md
│   └── requirements.txt
├── mnist-hp-search/
│   ├── mnist_hyperparameter_search.ipynb
│   ├── data/workflow.png
│   ├── results/
│   ├── README.md
│   └── requirements.txt
├── README.md
└── .gitignore
```

## 실행

```bash
git clone https://github.com/hoseunggg/2025_asset_pricing.git
cd 2025_asset_pricing/<폴더>     # 예: shapley-sector-portfolio
pip install -r requirements.txt
jupyter lab
```

노트북은 자기 폴더를 작업 디렉터리로 두고 상대경로로 동작합니다. 1·3번은 데이터 없이 바로 돌아가고, 2번은 CRSP 데이터가 있어야 합니다.

## 재현성

세 프로젝트 모두 시드를 42로 고정했습니다. yfinance는 실시간 데이터라 받는 시점에 따라 수치가 조금씩 달라질 수 있습니다. 버전을 정확히 맞추려면 첫 실행 뒤 `pip freeze > requirements.lock.txt` 로 고정해 두면 됩니다.
