# MNIST Hyperparameter Search

MNIST 분류용 완전연결 신경망(FCN)에 대해 **4가지 하이퍼파라미터 탐색 전략**(Grid / Random / Bayesian / Hyperband)을 비교하고, `joblib` 병렬 실험 파이프라인으로 학습·평가·분석하는 프로젝트입니다. 이미지 분류 자체보다 **모델 선택·하이퍼파라미터 최적화 방법론**을 보여주는 데 초점이 있습니다.

## 파이프라인

1. `build_model` — 하이퍼파라미터에 따라 은닉층을 동적으로 구성 (L2 정규화 + 드롭아웃 + Softmax 10-class)
2. 4가지 탐색 전략으로 하이퍼파라미터 탐색
   - **Grid** — 사전 정의 조합 전수 탐색
   - **Random** — 무작위 샘플링
   - **Bayesian** — 확률 모델 기반 (`keras-tuner`)
   - **Hyperband** — 자원 배분 조기 종료 (`keras-tuner`)
3. `train_and_evaluate_model` — Adam + EarlyStopping 학습, train/val/test 평가
4. `run_search` — `joblib.Parallel`로 CPU 코어 병렬 학습, 결과를 `results/<strategy>.csv`에 저장
5. **Data Analysis** 셀 — `results/`의 CSV를 취합해 상위 모델·전략 비교·정규화/드롭아웃 효과·직렬 vs 병렬 속도·과적합 곡선 분석

전체 흐름: [data/workflow.png](./data/workflow.png)

## 실행

```bash
pip install -r requirements.txt
jupyter lab mnist_hyperparameter_search.ipynb
```

- 데이터는 Keras 내장 MNIST(`tf.keras.datasets.mnist`)라 별도 다운로드가 필요 없습니다.
- 새 탐색을 돌리려면 마지막 실행 셀에서 `search_type`을 골라 실행하면 `results/<strategy>.csv`에 append 됩니다.
- 이미 담긴 `results/*.csv`만으로 **Data Analysis** 셀은 바로 실행됩니다.

## 재현성

- 상단 import 셀에서 `SEED = 42`로 `random`·`numpy`·`tensorflow` 시드를 고정합니다.
- 단, `joblib` 다중 프로세스 + GPU/멀티스레드 환경에서는 부동소수점 누적 순서 차이로 결과가 비트 단위까지 완전히 일치하지는 않을 수 있습니다(경향은 재현).

## 결과 요약

전략 비교 결과 **Grid Search**가 최고 성능. 선택 모델: 은닉층 `(256,)`, swish, adam, batch 64, dropout 0.5, L2 `reg_value=0.0`.

| 데이터셋 | Loss | Accuracy |
|----------|------|----------|
| Train | 0.0163 | 0.9957 |
| Validation | 0.0638 | 0.9821 |
| Test | 0.0585 | **0.9834** |

## 파일

```
mnist-hp-search/
├── mnist_hyperparameter_search.ipynb
├── requirements.txt
├── data/workflow.png          # 파이프라인 다이어그램
└── results/                   # 전략별 탐색 결과 CSV
    ├── grid.csv
    ├── random.csv
    ├── bayesian.csv
    └── hyperband.csv
```
