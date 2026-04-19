
### 아키텍처 ###
```
사용자
  ↓ HTTP
Open WebUI (프론트엔드)
  ↓ OpenAI-compatible API (/v1/chat/completions)
FastAPI (OpenAI API 호환 레이어)
  ↓ invoke()
LangGraph Agent (워크플로우)
  ├─ Tool: RAGSearch (Milvus)
  ├─ Tool: web_fetch
  ├─ Tool: ...
  └─ Bedrock (LLM)
```
#### 필수 구현해야 할 3개 엔드포인트 ####
* GET  /v1/models              → 사용 가능한 모델 목록
* POST /v1/chat/completions    → 실제 채팅 (stream / non-stream 모두)
* GET  /health                 → 헬스체크 (선택이지만 권장)

