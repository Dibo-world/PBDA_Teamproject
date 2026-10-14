# Model

추천 시스템 코드를 논문 원본과 revision으로 나눠 둡니다.

| 폴더 | 내용 | 출처 |
|------|------|------|
| [`paper/`](paper/) | 제출·GitHub에 올라간 원본 모델 코드 | [Nekerworld/PBDA_Teamproject](https://github.com/Nekerworld/PBDA_Teamproject.git) `Model/` |
| [`paper_revision/`](paper_revision/) | IEEE Access 리비전용 재현·공정 프로토콜·paper_v2 실험 코드 | 현재 워크스페이스 개량본 |

## 실행

원본 파이프라인:

```powershell
cd Model/paper
python model.py
```

리비전 실험·시각화:

```powershell
cd Model/paper_revision
python experiments.py --output-dir ../../data/research_revision_paper --device cuda --isolation-forest on
python viz.py --experiment-dir ../../data/research_revision_paper --output-dir ../../data/research_revision_paper/figures
python -m pytest test_revision.py -q
```

리비전 실험은 기본적으로 학습 데이터에 Isolation Forest를 적용합니다. 끄려면 별도 출력 폴더를 사용해 `python experiments.py --output-dir ../../data/research_revision_paper_if_off --device cuda --isolation-forest off`로 실행하세요. 두 설정은 서로 다른 출력 디렉터리에서 실행해야 하며, 실험 계획에는 선택된 설정이 기록됩니다. `--with-ablations`를 추가하면 반대 IF 조건도 비교 실험에 포함됩니다.

실행 폴더에는 `table1_dataset_statistics.csv`, `table_s1_event_breakdown_reference.csv`, `ablation_model_comparison_8020.csv`가 생성됩니다. Gamma 검증은 K=10, 20, 30, 40, 50의 NDCG/Recall을 `runs/<condition>__main__s<seed>/validation/gamma_validation.csv`에 저장합니다. 그림은 선택한 K로 만들 수 있습니다:

```powershell
python viz.py --top-k 50
```

기본 experiment 출력 폴더(`data/research_revision_paper`)를 사용합니다. 다른 폴더를 사용했다면 `--experiment-dir`를 지정하세요. 이 명령은 `data/research_revision_paper/figures/gamma_validation_k50.pdf`와 PNG를 포함한 figure set을 생성합니다. `--output-dir`를 지정하면 그림 저장 위치를 바꿀 수 있습니다.

데이터는 프로젝트 루트의 `data/`를 사용합니다 (`paper_revision`의 `data_protocol.ROOT` 기준).
