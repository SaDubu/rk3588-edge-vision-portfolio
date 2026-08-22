# Third-party notices

이 문서는 포트폴리오 소스 스냅샷에 포함되거나 구현 과정에서 참고한 제3자 자료를 구분한다. 프로젝트 자체 코드에 대한 일괄 라이선스 선언은 아니다.

## Rockchip RKNN Model Zoo

- Upstream project: [airockchip/rknn_model_zoo](https://github.com/airockchip/rknn_model_zoo)
- YOLOv8 example: [examples/yolov8/python/yolov8.py](https://github.com/airockchip/rknn_model_zoo/blob/main/examples/yolov8/python/yolov8.py)
- Utility modules: [py_utils](https://github.com/airockchip/rknn_model_zoo/tree/main/py_utils)
- Upstream license: [Apache License 2.0](https://github.com/airockchip/rknn_model_zoo/blob/main/LICENSE)
- Local license copy: [`licenses/Apache-2.0.txt`](licenses/Apache-2.0.txt)

다음 공개 파일은 RKNN Model Zoo의 YOLOv8 예제 또는 공통 utility를 기반으로 한다.

- `src/python/yolo8_rknn.py`
- `src/python/py_utils/coco_utils.py`
- `src/python/py_utils/onnx_executor.py`
- `src/python/py_utils/pytorch_executor.py`
- `src/python/py_utils/rknn_executor.py`

프로젝트 버전에는 custom YAML class loading, confidence 설정, 여러 RKNN 출력 형태를 위한 후처리, PIL 기반 label rendering, 평가/로그 경로가 추가되거나 변경됐다. `yolo8_rknn.py`는 upstream 예제의 수정본으로 취급한다.

Apache-2.0에 따라 upstream 출처와 라이선스 사본을 함께 보존하며, 재배포 시 수정 사실과 기존 저작권·특허·상표·귀속 고지를 유지해야 한다.

## 코드에 남아 있는 구현 참고 링크

다음 자료는 소스에 참고 URL만 남아 있으며 해당 페이지의 코드나 콘텐츠를 별도 파일로 배포하지 않는다.

- 움직임 bounding box 병합 아이디어: `src/cpp/LaborManager.cpp`에 기록된 기술 블로그 URL
- Orange Pi camera/GStreamer 조사: `src/cpp/define.cpp`에 기록된 Stack Overflow URL

## `LFQSPSC.h`

- 파일: `src/cpp/LFQSPSC.h`
- 설계 참고 출처: [C++ Thread — Lock Free Queue](https://narakit.tistory.com/198)
- 사용 범위: single-producer/single-consumer queue의 dummy-node, head/tail 구조
- 프로젝트 상태: 재배포 가능 확인

포트폴리오에 포함된 파일은 `std::unique_ptr`, atomic `Next`, acquire/release memory ordering과 `Push(T)`/`Pop(T&)` interface를 사용하는 프로젝트 버전이다. 기술 블로그의 SPSC queue 설명을 참고한 구현임을 표시하고, 실제 공개 파일은 `SOURCE_MANIFEST.md`의 SHA-256으로 고정했다.

## 라이브러리와 모델 자산

OpenCV, NumPy, Pillow, PyYAML, PyTorch, ONNX Runtime, pycocotools, POSIX IPC Python binding, RKNN Toolkit/Runtime은 import 또는 link 대상으로만 참조하며 이 저장소에 해당 패키지 소스를 vendoring하지 않는다. 각 패키지를 설치·배포할 때는 해당 버전의 라이선스를 별도로 확인해야 한다.

YOLO/RKNN 모델 가중치와 학습 데이터는 이 저장소에 포함하지 않는다. 모델의 취득 경로와 데이터 권리는 소스 코드 라이선스와 별개의 검토 대상이다.

## 프로젝트 라이선스 상태

이 포트폴리오 스냅샷에는 프로젝트 자체 코드에 대한 `LICENSE`를 아직 추가하지 않았다. `LFQSPSC.h`의 재배포 가능 여부와 참고 출처는 확인했지만, 저장소 전체에 적용할 허가 범위는 별도의 프로젝트 라이선스로 결정해야 한다.
