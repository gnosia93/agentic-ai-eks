TRT-LLM 엔진 빌드 삽질 일지

```
1. build.py 인자 변경

구버전 방식으로 --model_dir, --dtype, --tp_size를 build.py에 직접 넣음 → 에러
v0.9 이후 2단계로 변경됨: 체크포인트 변환 → 엔진 빌드

2. convert_checkpoint.py 경로 찾기

examples/qwen/ 경로가 없음 → 최신 TRT-LLM에서 디렉토리 구조 변경
find로 찾아보니 
convert_checkpoint.py
로 이동됨

3. Python 패키지 의존성 지옥

ModuleNotFoundError: No module named 'torch' → torch 설치
ModuleNotFoundError: No module named 'transformers' → transformers 설치
ModuleNotFoundError: No module named 'tensorrt_llm.bindings' → pip으로 해결 불가
하나씩 설치하다가 이미지 원본 환경을 망가뜨림

4. torch 버전 충돌

pip으로 torch를 새로 설치 → 이미지에 있던 TRT-LLM 바인딩과 심볼 불일치
undefined symbol: _Z16THPVariable_WrapN2at10TensorBaseE 에러
해결: pip에서 torch 제거, 이미지 원본 torch 유지

5. transformers 버전 충돌

pip으로 최신 transformers(4.57.6) 설치됨 → 이미지의 torch 2.6과 호환 안 됨
cannot import name 'TransformGetItemToIndex' 에러
해결: pip에서 transformers도 제거, 이미지 원본 유지

6. TRT-LLM 소스 버전 불일치

git clone으로 최신 소스 받음 → 이미지의 TRT-LLM 0.16.0과 안 맞음
ModuleNotFoundError: No module named 'tensorrt_llm._deprecation' 에러
해결: 이미지를 26.02(TRT-LLM 1.1.0)로 업그레이드

7. 이미지 내 스크립트 발견

클론 없이 이미지 안에 이미 
convert_checkpoint.py
 존재
find / -name "convert_checkpoint.py"로 확인

8. Python 3.13 호환성

로컬 환경에서 pip install tensorrt_llm 시도 → Python 3.13 미지원 (3.10, 3.12만 지원)
conda로 3.12 환경 만들려 했으나 약관 동의 필요

핵심 교훈

NVIDIA 공식 이미지에 이미 다 들어있으니 pip으로 건드리지 말 것
이미지 안에 뭐가 있는지 먼저 확인 (pip show, find)
소스 클론 시 이미지 TRT-LLM 버전과 태그 맞출 것
최종 pip은 "huggingface_hub<1.0" awscli만 필요
```
