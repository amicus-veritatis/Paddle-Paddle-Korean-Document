# 서버 배포 — Paddle Inference

Paddle Inference는 PaddlePaddle의 네이티브 추론 라이브러리로, 서버 및 클라우드 환경에서 고성능 추론 기능을 제공합니다.

Paddle Inference는 PaddlePaddle의 학습 연산자를 기반으로 동작하기 때문에, Paddle에서 학습한 모든 모델을 범용적으로 지원할 수 있습니다.

풍부한 기능과 뛰어난 성능을 갖춘 Paddle Inference는 다양한 플랫폼과 애플리케이션 시나리오에 최적화되어 높은 처리량과 낮은 지연 시간을 보장하며, 서버에서 모델을 학습 직후 바로 사용할 수 있도록 빠른 배포를 지원합니다.

자주 사용하는 문서 링크:
- 전체 사용 설명서: [Paddle Inference 문서](https://www.paddlepaddle.org.cn/inference/product_introduction/inference_intro.html)
- 코드 예제: [inference demo](https://github.com/PaddlePaddle/Paddle-Inference-Demo)
- [Linux 예측 라이브러리 다운로드](https://www.paddlepaddle.org.cn/inference/master/guides/install/download_lib.html#linux)
- [Windows 예측 라이브러리 다운로드](https://www.paddlepaddle.org.cn/inference/master/guides/install/download_lib.html#windows)

## Model.predict와의 차이점

Paddle Inference는 고성능 추론 라이브러리이며, 서버 및 클라우드에 적합합니다. 반면, Model 객체의 `Model.predict`는 학습·테스트·추론이 모두 가능한 네트워크 모델입니다. Paddle Inference는 MKLDNN, CUDNN, TensorRT를 사용하여 예측 성능을 가속화할 수 있으며, X2Paddle 도구를 통해 TensorFlow, PyTorch, Caffe 등에서 변환된 모델도 사용할 수 있습니다. 또한, PaddleSlim과 연동되어 양자화, 가지치기, 지식 증류된 모델도 배포할 수 있습니다.  
`Model.predict`는 간단한 예측 용도에 적합하며, `Paddle Inference`는 고성능 및 범용성이 요구되는 환경에서 더 적합합니다.

## 추론 프로세스 흐름도

![](./images/inference.png)

## 고성능 구현

### 메모리/VRAM 재사용을 통한 서비스 처리량 향상

초기화 단계에서 OP의 출력 Tensor 간의 의존성을 분석하여, 서로 독립적인 Tensor는 동일한 메모리/VRAM 공간을 공유하여 병렬 처리를 확대하고 처리량을 높입니다.

### 세밀한 OP 통합으로 계산량 감소

초기화 시 여러 OP를 사전에 정의된 방식으로 하나의 OP로 융합하여, 계산량과 Kernel 실행 횟수를 줄여 추론 성능을 향상시킵니다. 현재 수십 가지 이상의 융합 방식이 지원됩니다.

### 고성능 CPU/GPU Kernel 내장

Intel, Nvidia와 공동 개발한 고성능 Kernel을 내장하여 고성능 추론을 보장합니다.

## 다양한 기능 통합

### TensorRT 통합으로 GPU 추론 가속

Paddle Inference는 서브그래프 형태로 TensorRT를 통합하여 GPU 추론 속도를 높입니다. OP 통합, 불필요한 OP 제거, 최적 Kernel 선택 등의 최적화를 통해 속도를 향상시킵니다.

### oneDNN CPU 추론 가속 엔진 통합

한 줄의 코드로 oneDNN 가속을 시작할 수 있어 간편하고 효율적입니다.

### PaddleSlim 양자화 압축 모델 배포 지원

PaddleSlim은 모델 압축 도구로, Paddle Inference와 연동하여 양자화, 가지치기, 증류된 모델을 지원합니다.  
양자화된 모델의 경우 X86 CPU에서 단일 스레드 성능이 최대 3배 향상되며, ERNIE 모델은 최대 2.68배 향상됩니다.

### X2Paddle 변환 모델 지원

Paddle에서 학습한 모델 외에도 TensorFlow, PyTorch, Caffe 등의 모델을 X2Paddle 도구로 변환하여 지원합니다.

## 다양한 시나리오에 적응

### 주요 하드웨어 및 소프트웨어 환경 지원

X86 CPU, NVIDIA GPU를 지원하며, Linux/Mac/Windows뿐 아니라 중국산 칩(Feiteng, Kunpeng, Sugon, KylinX)까지 지원합니다. Paddle 모델을 바로 학습하고 바로 배포할 수 있습니다.

### 주요 및 국산 운영체제 호환

Linux, Windows, macOS는 물론, Kylin OS, Tongxin OS, Puhua OS, Zhongke Fangde 등도 지원합니다.

### 다양한 언어 API 지원

C++, Python, C, Go, Java, R 언어 API를 제공하며, 기타 언어는 안정적인 C API를 통해 사용 가능합니다. 튜토리얼, API 문서, 예제도 함께 제공됩니다.

## 소통 및 피드백

- GitHub Issues를 통해 문제 및 제안 사항을 접수해 주세요.
- 공식 위챗 계정: 飞桨 PaddlePaddle  
- 위챗 커뮤니티: 배포 관련 기술 그룹

<p align="center"><img width="200" height="200"  src="https://user-images.githubusercontent.com/45189361/64117959-1969de80-cdc9-11e9-84f7-e1c2849a004c.jpeg"/>&#8194;&#8194;&#8194;&#8194;&#8194;<img width="200" height="200" margin="500" src="https://github.com/PaddlePaddle/FluidDoc/blob/develop/doc/paddle/guides/05_inference_deployment/inference/images/wechat.png?raw=true"/></p>
<p align="center">  &#8194;&#8194;&#8194;공식 위챗 계정&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;&#8194;공식 기술 교류 위챗 그룹</p>
