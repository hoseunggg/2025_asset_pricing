# Quantitative Finance & ML Portfolio

정량 금융(quant)·머신러닝 관련 세 개 프로젝트 모음입니다. 각 프로젝트는 독립 폴더로 분리되어 있고, 자체 `README.md`와 `requirements.txt`를 갖습니다.

| # | 프로젝트 | 한 줄 요약 | 핵심 기법 |
|---|----------|-----------|-----------|
| 1 | [shapley-sector-portfolio](./shapley-sector-portfolio) | S&P 500 섹터 ETF의 위험·효용 기여도를 협력게임이론으로 분해 | Shapley value, Monte-Carlo 근사, 롤링 윈도, MPT |
| 2 | [ml-asset-pricing](./ml-asset-pricing) | 팩터 기반 횡단면 수익률 예측 + 롱숏 포트폴리오 백테스트 | LASSO, XGBoost, 팩터 엔지니어링, H-L 백테스트 |
| 3 | [mnist-hp-search](./mnist-hp-search) | 4가지 하이퍼파라미터 탐색 전략 벤치마크 + 병렬 실험 파이프라인 | Grid/Random/Bayesian/Hyperband, joblib 병렬화 |

프로젝트 1·2가 정량 금융 도메인에 직접 해당하며, 3은 모델 선택·하이퍼파라미터 최적화 방법론을 다룹니다.

## 실행

각 프로젝트 폴더로 이동해 의존성을 설치한 뒤 노트북을 실행합니다.

```bash
cd <project-folder>
pip install -r requirements.txt
jupyter lab   # 또는 jupyter notebook
```

노트북은 각자의 폴더를 작업 디렉터리(cwd)로 삼아 실행하는 것을 전제로 상대경로를 사용합니다.

## 재현성 원칙

- 각 프로젝트에 난수 시드를 고정(`SEED = 42`)했습니다.
- 외부 데이터 의존성(Yahoo Finance 실시간 다운로드, CRSP 데이터 등)은 각 프로젝트 README에 명시했습니다.
- 첫 실행이 성공하면 `pip freeze > requirements.lock.txt`로 정확한 버전을 고정할 것을 권장합니다.
