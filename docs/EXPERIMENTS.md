# 실험 기록과 해석

## 실험 A — 엣지 환경의 tracker 선택

선행 실험에서 SORT는 약 18 FPS로 동작했지만 ID가 자주 바뀌었고, DeepSORT는 CPU 부하로 약 2 FPS에 머물렀다. appearance feature를 RK3588 NPU로 분리한 변형도 구현했으며, MOT15 전체 평가에서 IDF1 33.1%, MOTA 24.8%를 기록했다.

이 결과는 범용 MOT 성능을 더 밀어붙이기보다, 상품 이동이라는 좁은 문제에서 필요한 규칙을 직접 설계하는 방향으로 이어졌다. SMOF의 custom tracker 결과와 MOT15 수치는 목적과 데이터가 다르므로 하나의 개선율로 비교하지 않는다.

## 실험 B — YOLO 모델 계열 비교

24,643장 커스텀 이미지와 118,287장 COCO 이미지에 대한 당시 기록에서 YOLOv5s는 22–24 FPS, YOLOv8s는 12–13 FPS 범위를 보였다. 처리량만으로는 v5s가 유리했지만, 이후 상품 인식과 RKNN 모델 분할 작업은 v8 계열을 중심으로 진행했다.

세부 표와 수치 해석은 [`results/model_benchmark.md`](../results/model_benchmark.md)에 분리했다.

## 실험 C — RKNN 출력 후처리 교정

YOLOv8s RKNN 변환 직후에는 24,643개 입력 중 200개만 검출 기록이 생겼다. 모델 자체의 실패로 단정하지 않고 출력 tensor branch와 후처리 연결을 점검했고, branch별 box/class 출력을 복원·병합하도록 바꾼 뒤 검출이 정상화됐다.

이 경험 때문에 포트폴리오 코드에서도 `sigmoid_post_process`, branch decode, NMS와 C++ 전송 형식을 함께 보존했다.

## 실험 D — 다중 모델 메모리 한계

단일 YOLOv8n/s, 전체 모델 두 개, 817/822 class 분할 모델 두 개, 세 모델 구성을 비교했다. class-split 두 모델은 2.8 GB와 15.x FPS 기록을 보였지만, 세 모델은 실행이 중단됐다.

세부 구성은 [`results/memory_benchmark.md`](../results/memory_benchmark.md)에 정리했다. 결론은 “모델을 더 많이 올리는 것”이 아니라, class 분할이 메모리·처리량·병합 복잡도에 미치는 영향을 함께 보아야 한다는 것이었다.

## 실험 E — 10개 상품 class 집계

timestamp마다 가장 confidence가 높은 class를 하나 선택하는 규칙으로 2,981개 frame 시도를 집계했다. 그중 2,683개가 정답 class로 선택되어 가중 비율 0.9000을 기록했다.

이 수치는 mAP가 아니라 frame-level correct-class ratio다. 가장 낮은 두 class는 0.75와 0.78로, 평균값만 제시할 때 가려지는 취약 구간을 보여준다. 전체 표는 [`results/product_test_summary.md`](../results/product_test_summary.md)에 있다.

## 실험 F — 시각화 문자열 처리

OpenCV 기본 텍스트 렌더링에서 한글이 깨지는 문제는 PIL 기반 렌더링을 사용해 해결한 기록이 있다. 최종 C++ 스냅샷의 런타임 label은 영문 중심이지만, 사용자 화면과 디버그 화면의 렌더링 계층을 분리해야 한다는 판단으로 이어졌다.

## 근거 수준

| 수준 | 이 포트폴리오에서의 의미 | 해당 자료 |
|---|---|---|
| A | 공개 코드와 집계 파일로 계산 정의 확인 가능 | 10개 상품 집계 로직과 class별 결과 |
| B | 당시 표와 문서에 조건·수치가 남음 | FPS, 메모리, NPU 사용률, 인식률 비교 |
| C | 탐색 과정의 근사 관찰 | 초기 SORT MOTA 약 30%, 약 180 ms/5 FPS |

README의 대표 결과는 A 수준 자료를 우선하고, B·C 수준은 의사결정 맥락에서만 사용한다.
