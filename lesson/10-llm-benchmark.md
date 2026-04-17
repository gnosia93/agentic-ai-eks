## 추론 성능 비교 (versus vLLM) ##

python 3.12 로 된 conda 환경을 생성한다.
```
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r

conda create -n py312 python=3.12 -y
conda activate py312

pip install genai-perf
```

테스트 한다.
```
genai-perf profile \
    --model qwen \
    --endpoint-type chat \
    --url http://<서비스주소>:8000 \
    --num-prompts 100 \
    --concurrency 10
```
#### 측정 항목: ####
* TTFT (Time To First Token): 첫 토큰까지 걸리는 시간
* ITL (Inter-Token Latency): 토큰 간 지연
* Throughput: 초당 생성 토큰 수
* Request Latency: 요청당 전체 응답 시간

### 측정 결과 ###
* vLLM
  
* TensorRT-LLM


