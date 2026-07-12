# ML Asset Pricing

Gu, Kelly & Xiu(2020) *"Empirical Asset Pricing via Machine Learning"* 을 축소 재현한 프로젝트입니다. CRSP 미국 주식 데이터에서 **17개 팩터**를 만들고, **LASSO / XGBoost**로 다음 달 횡단면 수익률을 예측한 뒤 **롱숏(H-L) 포트폴리오**를 백테스트합니다.

## 파이프라인

1. **전처리** — $5 미만 종목 제거(기간 중 한 번이라도 저가였던 종목 전체 배제), 월별 시가총액 하위 10% 제거
2. **팩터 엔지니어링 (17개)** — size, mom1m/3m/6m/9m/12m, volatility, Amihud 비유동성, turnover·turnover_vol, ret_lag/skew/mean, price_vs_avg, drawdown 등
3. **타깃** — 회귀: 횡단면 정규화 수익률(`n_target_ret`), 분류: 수익률 십분위(`target_cls`)
4. **모델** — `LASSO`(GridSearchCV) / `XGBRegressor` / `XGBClassifier`
5. **백테스트** — 예측 상·하위 10%로 롱숏 구성, 등가중/시총가중, Sharpe·MDD·누적수익 산출
6. **분할** — train: `date < 2010-01-01`, test: `date >= 2010-01-01` (시간순 분할, 룩어헤드 방지)

## 실행

```bash
pip install -r requirements.txt
export CRSP_PATH=/path/to/crspm.csv   # 아래 '데이터' 참고
jupyter lab ml_asset_pricing.ipynb
```

## 데이터 & 재현성

- **데이터**: WRDS **CRSP 월별(crspm)** 데이터가 필요합니다. **저작권 자료라 저장소에 포함하지 않습니다.** 로컬에 `crspm.csv`를 준비한 뒤 환경변수 `CRSP_PATH`로 경로를 지정하거나 노트북의 `DATA_PATH`를 수정하세요. 필요 컬럼: `permno, date, prc, shrout, vol, ret`.
- **시드**: 노트북 상단에서 `SEED = 42`로 `random`·`numpy` 시드를 고정합니다. 데이터 셔플(`sklearn.utils.shuffle`)은 시드를 사용합니다.
- **XGBoost 비결정성(주의)**: 현재 `XGBRegressor/XGBClassifier`에 `random_state`가 지정되어 있지 않아 트리 학습에 약간의 비결정성이 남습니다. 완전 재현을 원하면 두 모델 생성부에 `random_state=SEED, n_jobs=1`을 추가하세요(멀티스레드도 미세한 부동소수점 차이를 만듭니다).

## 알려진 한계 (포트폴리오 리뷰용 메모)

- **XGB 분류기 10회 반복이 마감 시간 제약으로 완주하지 못했습니다** (원본 노트 마크다운에 기록). 재실행 시 시간(수십 분)을 확보하거나 `n_estimators`·`cv`를 줄이세요.
- 리포트에 "수익률이 믿기 힘들 만큼 강했다"는 언급이 있습니다. **거래비용 미반영**, 시총가중 시 **동시점 mcap 사용**, 십분위 경계 설정 등은 룩어헤드/과대성과 여지가 있으므로, 포트폴리오로 제출 전 거래비용·현실적 제약을 넣어 재검증할 것을 권장합니다.
- `use_label_encoder=False`는 최신 XGBoost에서 제거되었습니다. 버전에 따라 해당 인자를 빼야 할 수 있습니다.
