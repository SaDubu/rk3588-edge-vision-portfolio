# 포트폴리오 검토 가이드

이 저장소는 다음 순서로 보면 약 10분 안에 프로젝트의 문제, 구현, 결과와 공개 범위를 확인할 수 있다.

## 1. 전체 이야기 확인 — 3분

먼저 [`README.md`](README.md)를 Markdown preview로 연다.

확인할 항목:

- 문제 정의가 상품 인식이 아니라 지정 영역 통과 여부까지 포함하는가
- 대표 결과 `2,683 / 2,981 = 0.9000`의 의미가 명확한가
- EdgeMOT 선행 실험에서 custom tracker로 전환한 이유가 이어지는가
- 회사나 협업사 이름 없이 기술 내용만 이해할 수 있는가

## 2. 실제 구현과 문서 대조 — 3분

다음 파일을 나란히 열어 문서의 설명이 코드와 대응하는지 확인한다.

| 확인 내용 | 문서 | 코드 |
|---|---|---|
| 9-worker pipeline | [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | [`src/cpp/run.cpp`](src/cpp/run.cpp) |
| motion ROI | architecture의 움직임 영역 생성 | [`src/cpp/LaborManager.cpp`](src/cpp/LaborManager.cpp) |
| tracking rule | architecture의 추적 규칙 | [`src/cpp/define.h`](src/cpp/define.h) |
| C++/Python IPC | architecture의 IPC 계약 | [`src/cpp/SharedMemoryManager.hpp`](src/cpp/SharedMemoryManager.hpp), [`src/python/ipc_python.py`](src/python/ipc_python.py) |
| RKNN 후처리 | [`results/model_benchmark.md`](results/model_benchmark.md) | [`src/python/yolo8_rknn.py`](src/python/yolo8_rknn.py) |

특히 `run.cpp`의 `main() → run_2()`와 `std::thread t1`부터 `t9`까지를 보면 최신 기본 경로를 빠르게 확인할 수 있다.

## 3. 결과의 계산 정의 확인 — 2분

[`results/product_test_summary.md`](results/product_test_summary.md)에서 다음을 확인한다.

- 결과가 mAP가 아니라 timestamp별 최고-confidence 정답 class 비율로 정의됐는가
- 상품별 correct count와 전체 frame 수가 공개됐는가
- 합계가 `2,683 / 2,981 = 0.900034...`와 맞는가
- 낮은 두 class의 편차도 숨기지 않았는가

FPS, 메모리와 인식률 비교는 [`results/model_benchmark.md`](results/model_benchmark.md)와 [`results/memory_benchmark.md`](results/memory_benchmark.md)에서 당시 기록으로 구분했는지 본다.

## 4. 원본성과 공개 범위 확인 — 2분

- [`SOURCE_MANIFEST.md`](SOURCE_MANIFEST.md): 원본 커밋, 경로 매핑, SHA-256
- [`docs/PRIVACY_AND_ATTRIBUTION.md`](docs/PRIVACY_AND_ATTRIBUTION.md): 회사명·개인정보·내부 데이터 처리 기준
- [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md): RKNN 예제, SPSC queue와 참고 자료 출처

핵심 소스는 기존 SMOF에서 내용 수정 없이 복사했으며, 문서와 중립 config만 새로 작성했다.

## 터미널에서 확인하는 방법

```bash
cd /Users/dubumacbook/Documents/my_project/first_test/rk3588-edge-vision-portfolio
```

저장소 구성과 크기:

```bash
find . -type f | sort
du -sh .
```

공개 소스 해시:

```bash
shasum -a 256 src/cpp/* src/python/*.py src/python/py_utils/*.py
```

회사·협업사 이름과 일반적인 비밀정보 패턴 확인:

```bash
rg -n -i -g '!REVIEW_GUIDE.md' '회사명|협업사명|password|passwd|api[_-]?key|secret[_-]?key|access[_-]?token' .
```

제외하기로 한 대형 산출물 확인:

```bash
find . -type f \( -name '*.rknn' -o -name '*.onnx' -o -name '*.pt' -o -name '*.pyc' -o -name '*.so' \)
```

마지막 두 명령은 아무것도 출력되지 않는 것이 정상이다. 첫 번째 `rg` 명령의 `회사명|협업사명`은 실제 확인하려는 명칭으로 바꾸어 사용할 수 있다.

## 문장 검토 기준

포트폴리오를 읽고 다음 질문에 모두 답할 수 있으면 공개 문서의 목적을 충족한다.

1. 어떤 문제를 해결했는가?
2. 왜 기존 SORT/DeepSORT 경로를 그대로 사용하지 않았는가?
3. C++과 Python을 왜 나눴는가?
4. motion ROI와 YOLO detection을 어떻게 결합했는가?
5. 대표 수치는 어떤 단위로 계산됐는가?
6. 원본 코드와 포트폴리오 문서의 경계를 어떻게 확인하는가?
