# Shapley Sector Portfolio

S&P 500을 구성하는 **11개 섹터 ETF를 협력게임(cooperative game)의 플레이어**로 보고, 각 섹터가 포트폴리오의 위험 감소와 평균-분산 효용에 얼마나 기여하는지를 **Shapley value**로 정량 분해하는 프로젝트입니다.

전체 이론·유도·결과는 [report/Trading_report.pdf](./report/Trading_report.pdf)에 정리되어 있고, 이 노트북이 그 구현체입니다.

## 방법론

- **플레이어**: 11개 섹터 ETF (XLK, XLF, XLV, XLY, XLI, XLE, XLC, XLU, XLB, XLRE, XBI)
- **두 가치함수**
  - `v_risk(S)` — 개별 분산의 합에서 연합 S의 최소분산 포트폴리오 분산을 뺀 값 (위험 감소 기여)
  - `v_mv(S)` — 연합 S 자산만으로 달성 가능한 최대 평균-분산 효용 `μᵀw − λ·wᵀΣw`
- **Shapley value 근사**: 플레이어 수 11 → 연합 2¹¹ 완전열거 대신 **몬테카를로 순열 샘플링**(`joblib` 병렬)
- **동적 분석**: 36개월 롤링 윈도로 시간에 따른 기여도 변화 추적
- **코어(core)**: `scipy.optimize.linprog`로 코어 방향 샘플링

## 실행

```bash
pip install -r requirements.txt
jupyter lab shapley_sector_portfolio.ipynb
```

노트북은 실행 디렉터리에 `report/figure/`를 만들어 그림을 저장합니다.

## 데이터 & 재현성

- **데이터 출처**: Yahoo Finance(`yfinance`)에서 섹터 ETF 월말 종가를 **실시간 다운로드**합니다. 별도 데이터 파일이 없어 인터넷만 있으면 바로 돌아갑니다.
- **시드**: Shapley 근사의 난수(`random_state`, `rng_seed`)는 고정되어 있어, **동일한 입력 데이터에 대해서는 결과가 재현**됩니다.
- **주의 — 데이터 드리프트**: 다운로드에 종료일(`end`)이 없어 매달 새 데이터가 붙습니다. 따라서 Shapley 값의 **절대 수치는 실행 시점에 따라 조금씩 변합니다**(정성적 결론 — XLE·XBI가 위험 완화 주도, XLK가 효용 주도 — 은 안정적).
- **논문 수치 정확 재현**: 노트북 안 `yf.download(...)` 호출에 종료일을 고정하세요. 예)
  ```python
  yf.download(tickers, start="2010-01-01", end="2024-12-31", interval="1mo")["Close"]
  ```
- **폰트**: 그림 코드가 `Apple SD Gothic Neo`(macOS 한글 폰트)를 지정합니다. 다른 OS에서는 `plt.rcParams["font.family"]`를 설치된 폰트로 바꾸세요.

## 파일

```
shapley-sector-portfolio/
├── shapley_sector_portfolio.ipynb   # 구현
├── requirements.txt
└── report/
    └── Trading_report.pdf           # 이론·결과 리포트 (11p)
```
