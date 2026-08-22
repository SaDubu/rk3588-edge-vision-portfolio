# Source manifest

이 저장소는 포트폴리오 공개를 위해 원본 저장소의 최신 구현에서 핵심 파일만 선별한 스냅샷이다. 모델, 데이터셋, 실행 바이너리와 전체 Git 이력은 포함하지 않았다.

## 기준점

- 원본 코드 저장소: [SaDubu/SMOF](https://github.com/SaDubu/SMOF)
- 기준 브랜치: `main`
- 기준 커밋: `b020a0a4c894c2a62848b708598b29bf47db28ce`
- 기준 커밋 날짜: 2026-03-26
- 선별 및 검증일: 2026-08-22
- 복제 원칙: 아래 소스 파일은 경로만 재구성했으며 내용은 수정하지 않았다.

## 원본 경로 매핑 및 SHA-256

| 포트폴리오 경로 | 원본 경로 | SHA-256 |
|---|---|---|
| `src/cpp/run.cpp` | `test_object_rule/run.cpp` | `6a6233ea8d273a7cd922191a08b1fb0ff03f52b79ae5b420c581c7784e121289` |
| `src/cpp/LaborManager.cpp` | `test_object_rule/LaborManager.cpp` | `dc953115f9d95055710661ad9ac71c1789313b36361e644e83ebb56057047f07` |
| `src/cpp/LaborManager.hpp` | `test_object_rule/LaborManager.hpp` | `1c1251c620a2a3f3df608abcac792c59d256570407dee858e3aecef897a02641` |
| `src/cpp/MotionDetector.cpp` | `test_object_rule/MotionDetector.cpp` | `1cbf4fa7d1ffe8dba263bd75f0e93d81b8946e1166622ae255de5fbacebe8af8` |
| `src/cpp/MotionDetector.hpp` | `test_object_rule/MotionDetector.hpp` | `2696a7fb7197239b3ae9441e270e17b3b9142ae6b0f521fb5aa7651f081ae50a` |
| `src/cpp/SharedMemoryManager.hpp` | `test_object_rule/SharedMemoryManager.hpp` | `00e342006f7454d3f7349aa8c2f0e861ddfec516954b41848d7f3a86c134641f` |
| `src/cpp/LFQSPSC.h` | `test_object_rule/LFQSPSC.h` | `892c1a0091d98885081e1ac9a9960fe2eb4ebfe65a064f8f938127f684962545` |
| `src/cpp/define.cpp` | `test_object_rule/define.cpp` | `389681af504bd505e2809c162446b21549da9b9bb4dfcf57f8992bc9dee6c585` |
| `src/cpp/define.h` | `test_object_rule/define.h` | `3b6427031763a02e0fa740058192b8d7fce23af22bafd1f38f885212b8b9335c` |
| `src/python/ipc_python.py` | `ipc_python.py` | `15442b0e7180ab21648a60ea1f04eeef949817be85f6ca0e84c70f000eca0f52` |
| `src/python/yolo8_rknn.py` | `yolo8_rknn.py` | `082e1245511ad294ec0562aa4563534fdc1e0ff306899f17f2c0a5624979627a` |
| `src/python/a_log.py` | `a_log.py` | `a50fe4a97872944c9ab7e58d24dd1e53c23f9d4bee72d2097170ff4ae24d8ed3` |
| `src/python/py_utils/__init__.py` | `py_utils/__init__.py` | `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` |
| `src/python/py_utils/coco_utils.py` | `py_utils/coco_utils.py` | `cfe558435f43469e9c3bc4b0b6a526f42585dbf3040c5b69af933e8a2c21f96c` |
| `src/python/py_utils/onnx_executor.py` | `py_utils/onnx_executor.py` | `c4c4a6b8b8e05fdfaee5aa1ef9601112ba409891f16fa31f48be3f1539a0ba28` |
| `src/python/py_utils/pytorch_executor.py` | `py_utils/pytorch_executor.py` | `2dd92e5d5c42d45196da3848dd1a9b518c7752cc6b0781b1720ef92a68a9bd57` |
| `src/python/py_utils/rknn_executor.py` | `py_utils/rknn_executor.py` | `ec9a3af4e30c670b58a72969a9bc92cee47432db1db696b20c47d5153122210f` |

## 공개 결과 이미지

추적 이미지는 선행 실험 저장소의 포트폴리오 스냅샷에서 복사했다. 원본 이미지의 픽셀 데이터는 변경하지 않고 파일명만 용도 중심으로 바꿨다.

| 경로 | SHA-256 |
|---|---|
| `assets/tracking/cup-tracking-1.jpg` | `be35308680669d43c17cf173977a9c3941ac70c761055b55aee35dafb77d3ef5` |
| `assets/tracking/cup-tracking-2.jpg` | `6ddf71ca8f66db4ef97b3a91f282bc14aa0a5c55211a2eae4bece4a39155108f` |
| `assets/tracking/bottle-tracking-1.jpg` | `d1f7c174fd5e0ab0a27cb24b6342a5902d48026ef38c34b6a68eb1b4f7500f47` |
| `assets/tracking/bottle-tracking-2.jpg` | `3a788c720fb115228a4cb99f85eb632567893e100aa4b46d59402bf7bed52448` |

## 의도적으로 제외한 항목

- RKNN/ONNX/PyTorch 모델 파일과 변환 산출물
- 원천·가공 이미지 데이터셋, 영상, 프레임별 원시 로그
- 제품·제조사 전체 클래스 목록과 로컬 경로가 포함된 학습 설정
- 컴파일된 ELF 바이너리, 오브젝트 파일, Python 바이트코드
- IDE 설정, 복구 파일, 압축 백업
- 개인 이메일과 개발 장비 경로가 남을 수 있는 Git 이력
- 검증 근거가 없는 목표 수치를 성과로 오해하게 만드는 자료

## 해시 재검증

저장소 루트에서 다음 명령으로 공개 소스 해시를 다시 계산할 수 있다.

```bash
shasum -a 256 src/cpp/* src/python/*.py src/python/py_utils/*.py assets/tracking/*.jpg
```
