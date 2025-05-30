# 모바일/임베디드 배포 — Paddle Lite

Paddle Lite는 Paddle-Mobile의 업그레이드 버전으로, 모바일 기기를 포함한 더 다양한 시나리오를 지원하는 경량 고효율 예측 엔진입니다. 더 폭넓은 하드웨어와 플랫폼을 지원하며, 고성능·경량화된 딥러닝 추론 엔진입니다. PaddlePaddle과의 완전한 호환을 유지하면서도, 타 프레임워크에서 학습된 모델도 지원합니다.

전체 사용 설명서는 [Paddle-Lite 문서](https://www.paddlepaddle.org.cn/lite)에서 확인할 수 있습니다.

## 주요 특성

### 경량화
- 실행 단계와 연산 최적화 단계를 잘 분리하여 구현하였으며, 실행 단계만으로 모바일 기기에서 직접 배포 가능하고 제3자 의존성이 없습니다.
- 80개 OP + 85개 커널을 포함한 동적 라이브러리 크기는 ARMV7에서 800K, ARMV8에서 1.3M 수준이며, 더 작게도 축소 가능합니다.
- 애플리케이션 배포 시 모델을 불러오기만 하면 바로 예측 가능하며, 별도의 분석·최적화 과정이 필요 없습니다.

### 고성능
- ARM CPU에 대해 극한의 성능 최적화가 이루어졌으며, 각 마이크로 아키텍처에 맞춘 커널 설계를 통해 계산 성능을 최대한 끌어올렸습니다.
- [PaddleSlim 모델 압축 도구](https://github.com/PaddlePaddle/models/tree/v1.5/PaddleSlim)의 양자화 기능과 결합하여 높은 정확도와 성능을 동시에 제공합니다.
- Huawei NPU 및 FPGA에서도 우수한 성능을 보입니다.

최신 성능 데이터는 [Benchmark 문서](https://www.paddlepaddle.org.cn/lite/develop/performance/benchmark.html)에서 확인할 수 있습니다.

### 범용성
- 하드웨어: ARM CPU, Mali GPU, Adreno GPU 외에도 Huawei NPU, FPGA 등 다양한 엣지 디바이스를 지원하며, 향후 Cambricon(寒武纪), Bitmain(比特大陆) 등의 AI 칩도 지원할 예정입니다.
- 모델: PaddlePaddle과 연산자(OP) 호환성을 유지하여 더 많은 모델을 지원합니다. 현재까지 18개 모델, 85개 OP의 정확도와 성능을 검증하였으며, 분류, 감지, 위치 추정 등 다양한 비전 모델을 지원하고 OCR 모델도 포함됩니다.
- 프레임워크: PaddlePaddle 외에도 Caffe, TensorFlow에서 학습된 모델을 [X2Paddle](https://github.com/PaddlePaddle/X2Paddle) 도구로 변환해 사용할 수 있으며, 향후 ONNX 등 다른 포맷도 지원할 계획입니다.

## 아키텍처

Paddle Lite는 다양한 하드웨어와 플랫폼을 지원할 수 있도록 설계되었으며, 여러 하드웨어가 하나의 모델 내에서 혼합 실행될 수 있도록 지원합니다. 또한 다층적인 성능 최적화와 경량화된 엔드 디바이스 실행 환경을 고려하여 설계되었습니다.

![](https://github.com/Superjomn/_tmp_images/raw/master/images/paddle-lite-architecture.png)

- `Analysis Phase`: MIR(Machine IR) 관련 모듈이 포함되어 있으며, 하드웨어에 따라 연산자 통합, 계산 축소 등의 최적화를 수행합니다.
- `Execution Phase`: 커널 실행만을 포함하며, 독립적으로 배포되어 초경량 배포가 가능합니다.

## Paddle-Mobile → Paddle-Lite 전환 안내

Paddle-Mobile은 임베디드 플랫폼을 위한 예측 엔진으로, ARM CPU, Mali GPU, Adreno GPU, Apple Metal GPU, ZU5/ZU9 FPGA 보드, 라즈베리파이 등 다양한 하드웨어를 지원하며, 내부적으로 폭넓은 비즈니스 적용이 이루어졌습니다.  
설계 문서는 [mobile/README](https://github.com/PaddlePaddle/Paddle-Lite/blob/develop/README.md) 참조.

Paddle-Mobile은 전면 리팩터링되어 Paddle-Lite로 전환되었으며, 기존 기능은 대부분 [신규 아키텍처](https://github.com/PaddlePaddle/Paddle-Lite/tree/develop/lite)에 통합되었습니다. 과도기적으로 기존 `paddle-mobile` 코드도 유지되며, `mobile/` 디렉터리에 있습니다. 향후 모든 기능은 신 아키텍처로 이전되고 유지·개발됩니다.

Metal 및 Web 모듈은 독립적으로 유지되며, 각각 `./metal`, `./web` 디렉터리에서 개발되고 있습니다. Apple GPU 및 Web 프론트엔드 예측이 필요할 경우 해당 디렉터리를 참고하세요.

## 감사의 말

Paddle Lite는 다음의 오픈소스 프로젝트에서 영감을 받거나 이를 통합하였습니다:

- [ARM compute library](https://github.com/ARM-software/ComputeLibrary)
- [Anakin](https://github.com/PaddlePaddle/Anakin): 고성능 예측 프레임워크이며, Paddle Lite에 주요 최적화 기능이 통합되었습니다. Anakin은 더 이상 독립적으로 업데이트되지 않으며 Paddle Lite에 통합되었습니다.

## 소통 및 피드백
- GitHub Issues를 통해 문제나 제안을 제출해 주세요.
- 공식 위챗 계정: 飞桨 PaddlePaddle  
- 공식 QQ 커뮤니티: 696965088

<p align="center"><img width="200" height="200"  src="https://user-images.githubusercontent.com/45189361/64117959-1969de80-cdc9-11e9-84f7-e1c2849a004c.jpeg"/>&#8194;&#8194;&#8194;&#8194;&#8194;<img width="200" height="200" margin="500" src="https://user-images.githubusercontent.com/45189361/64117844-cb54db00-cdc8-11e9-8c08-24bbe594608e.jpeg"/></p>
<p align="center">  &#8194;&#8194;&#8194;공식 위챗 계정&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;공식 기술 교류 QQ 그룹</p>

- **포럼**: [PaddlePaddle 포럼](https://ai.baidu.com/forum/topic/list/168)에서 Paddle 사용 중의 문제나 경험을 공유해 주세요. 활발한 커뮤니티 분위기 조성을 환영합니다.
