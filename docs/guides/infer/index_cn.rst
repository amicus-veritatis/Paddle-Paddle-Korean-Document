########
Inference 배포
########

PaddlePaddle Inference 소개
==================

PaddlePaddle 생태계의 중요한 부분으로서, PaddlePaddle은 여러 가지 추론 제품을 제공하여 딥러닝 모델 애플리케이션의 마지막 단계를 완전히 지원합니다.

전체적으로 추론 제품은 다음과 같은 하위 제품들로 구성되어 있습니다.

.. csv-table::
    :header: "이름", "영문 명칭", "적용 시나리오"
    :widths: 10, 10, 30

    "PaddlePaddle 기본 추론 라이브러리", "`Paddle Inference <http://paddleinference.paddlepaddle.org.cn/introduction/summary.html>`_ ", "고성능 서버 및 클라우드 추론"
    "PaddlePaddle 서비스형 추론 프레임워크", "Paddle Serving", "자동화 서비스, 모델 관리 등의 고급 기능"
    "PaddlePaddle 경량화 추론 엔진", "`Paddle Lite <http://paddlelite.paddlepaddle.org.cn/introduction/tech_highlights.html>`_ ", "모바일, IoT 등"
    "PaddlePaddle 프론트엔드 추론 엔진", "Paddle.js", "브라우저 내 추론, 미니 프로그램 등"

각 제품은 추론 생태계 내에서 다음과 같은 관계를 가집니다:


.. code-block:: text

    ┌────────────┐       ┌────────────┐
    │PaddlePaddle│─────▶ │   모델     │
    │  개발+학습   │       │            │
    └────────────┘       └────┬───────┘
                              │
                              ▼
                       ┌────────────┐
                       │ PaddleSlim │
                       │ 압축/양자화/  │
                       │ 가지치기     │
                       └────┬───────┘
                            │
                            ▼
              ┌─────────────┼────────────────┬──────────────┐
              ▼             ▼                ▼              ▼
       ┌────────────┐ ┌────────────┐  ┌────────────┐  ┌────────────┐
       │Paddle      │ │Paddle Lite │  │Paddle.js   │  │Paddle      │
       │Inference   │ │모바일/엣지    │  │웹 프론트엔드  │  │Serving     │
       │서버 추론      │ │추론        │  │추론         │  │서비스형 배포  │
       └────────────┘ └────────────┘  └────────────┘  └────────────┘

                              ▲
                              │
                       ┌──────┴───────┐
                       │ X2Paddle     │
                       │ 모델 변환 도구│
                       └──────┬───────┘
                              │
                              ▼
                       ┌────────────┐
                       │TensorFlow/ │
                       │ONNX 등 외부│
                       │ 프레임워크 │
                       └────────────┘

    ───────────────────────────────────────────────────────────────
                    설치 및 환경 구성


**사용자가 PaddlePaddle 추론 제품을 사용하는 워크플로우는 다음과 같습니다:**

1. PaddlePaddle 추론 모델을 획득합니다. 다음 두 가지 방법이 있습니다:
   
    1. PaddlePaddle을 이용해 학습을 수행하여 추론 모델을 얻음
    2. X2Paddle 도구를 사용해 TensorFlow 또는 Caffe 등의 타 프레임워크에서 생성된 모델을 변환

2. (선택 사항) 모델을 추가로 최적화합니다. `PaddleSlim` 도구를 통해 모델 압축, 양자화, 가지치기 등을 수행하여 모델 실행 속도와 성능을 크게 향상시키고 자원 소모를 줄일 수 있습니다.

3. 모델을 특정 추론 제품에 배포합니다.

..  toctree::
    :hidden:

    inference/inference_cn.rst
    mobile/mobile_index_cn.md
    paddleslim/paddle_slim_cn.md
