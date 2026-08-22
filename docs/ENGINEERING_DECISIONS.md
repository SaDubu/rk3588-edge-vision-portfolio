# 엔지니어링 의사결정

## 1. 범용 MOT 조합에서 도메인 규칙 기반 tracker로 전환

선행 프로젝트에서는 SORT에 appearance embedding과 cosine similarity를 추가했다. SORT는 실시간에 가까웠지만 ID switch가 잦았고, DeepSORT 계열은 엣지 CPU에서 처리량이 크게 떨어졌다. 이후 시스템이 요구하는 것은 임의의 보행자 재식별이 아니라 제한된 화면에서 상품이 지정 영역을 통과했는지를 판단하는 일이었다.

최종 SMOF 경로에서는 중심 거리, IoU, box 크기 비율, class history 중 두 개 이상을 만족시키는 custom association을 사용했다. 범용성은 줄지만 별도 embedding model과 고차원 feature 비교를 제거하고, 판정에 필요한 상태를 직접 보존할 수 있다는 선택이었다.

## 2. detector와 motion detector의 역할 분리

frame difference만 사용하면 상품 종류를 알 수 없고, detector만 사용하면 정지 물체와 배경 검출이 tracker 입력에 계속 남는다. 두 결과를 다음처럼 분리했다.

- motion path: 실제로 변화가 발생한 ROI를 찾는다.
- YOLO path: 상품 class와 confidence를 찾는다.
- filter path: ROI center와 가장 가까운 detection만 tracker에 전달한다.

이 결합은 detector 추론을 생략하는 gating이 아니라, detector 출력의 공간적 후보를 줄이는 방식이다.

## 3. C++ 제어부와 Python NPU 추론부 분리

실시간 frame/queue/tracker 제어는 C++로, RKNN 예제와 모델 후처리는 Python으로 유지했다. 단일 언어로 합치면 호출 경계는 줄지만, 모델 변환 실험과 후처리 수정이 제어부 빌드에 강하게 묶인다.

POSIX shared memory와 semaphore를 사용해 두 프로세스 사이의 계약을 고정했다. frame은 RGB byte array, 결과는 float32 6개로 구성된 detection array다. 이 선택 덕분에 모델 런타임 코드를 교체해도 C++ tracker의 입력 구조는 유지할 수 있다.

## 4. YOLOv5s의 처리량보다 YOLOv8 계열의 통합 경로 유지

당시 처리량 기록에서는 YOLOv5s가 YOLOv8s보다 빠른 경향을 보였다. 반면 상품 인식 비교와 이후 모델 분할 실험, RKNN 후처리 수정은 YOLOv8 계열을 중심으로 축적됐다. 최종 구현은 최고 FPS 하나가 아니라 다음을 함께 고려했다.

- 상품 class 인식 기록
- RKNN 변환 및 출력 branch 처리 경험
- nano/small 모델과 class split 구성 비교
- C++ IPC detection contract와의 연결

결과적으로 YOLOv8 경로를 유지하되, 수치 문서에서는 v5s의 처리량 우세도 숨기지 않았다.

## 5. 다중 모델보다 단일 기본 경로 선택

두 개의 class-split YOLOv8s는 전체 모델 두 개보다 메모리와 처리량이 유리한 기록을 보였다. 그러나 세 모델 구성은 중단되었고, 두 모델도 lifecycle과 결과 병합 복잡도를 높였다. 최신 기본 소스는 단일 모델을 실행하며 다중 모델 코드는 실험 흔적으로 남겨 두었다.

## 6. 움직임 episode를 구간 단위로 처리

최신 경로는 움직임이 있는 frame과 ROI를 기록하고, 약 90 frame 동안 움직임이 없으면 해당 episode 목록을 추론 경로로 넘긴다. 계속 흐르는 frame과 event 단위를 분리해 한 번의 상품 이동을 묶어 처리하려는 설계다.

대신 디스크 I/O와 episode 종료 지연이 생긴다. 후속 구현에서는 동일한 경계를 유지하되 memory ring buffer로 바꾸고, peak memory와 end-to-event latency를 함께 비교하는 것이 적절하다.

## 7. 원본 코드 보존과 공개 저장소 정리의 분리

기존 저장소는 실험 이력, 모델, 데이터, 바이너리가 뒤섞여 있어 포트폴리오의 설명 단위와 맞지 않았다. 원본을 정리한다는 이유로 구현을 다시 쓰지 않고, 기준 커밋에서 최신 핵심 파일을 그대로 복제했다.

파일별 SHA-256은 `SOURCE_MANIFEST.md`에 기록했고, 공개 문서·중립 config·출처 고지만 새로 작성했다. 이 방식은 포트폴리오 문장과 실제 구현 사이의 거리를 줄이면서 원본 연구 기록도 보존한다.
