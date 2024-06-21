RAG 시스템에 대해 Study한 과정을 기록했습니다.

몇가지 문제점이 있으나, 현황으로는 개인적인 사정으로 인해 보류중이라 추후 기회가 나면 더 확장된 버전으로 개선할 예정입니다.



### 문제점 및 개선사항 : 
- query : 사용자 질문은 매우 다양함, 예를 들어 오늘, 어제, 언제까지 등등.. 사용자 intend를 판별할 수 있는 방안 필요 (본 프로젝트에서는 미진행), 또한 접근방식에 따라 키워드/시멘틱을 혼합한 방법, Hyde 방법을 고려해야함 
- preprocessing : pdf와 같은 경우 규격이 특별히 정해진게 없어서 html, json, lxml과 달리 상당히 어려울 것으로 예상 (layout parser와 같은 경우 고려했으나, 시간상 보류)
- retriever : 검색 단계로, 단순히 임베딩 벡터를 넘어서 reranker 모델도 필요함. 하고 안하고 차이가 심함 (현재는 오픈소스로 했으나 도메인에 따라 파인튜닝할 필요가 있음 -> 유료 모델로는 cohere 사의 모델이 용이)
- vectorDB : 무료로써 chromadb를 활용했으나 (faiss는 현재 환경에서 파이썬 버전에 걸려서 스킵), input 제한이 있고, (41633 records) 실제 서비스로는 Pinecone, AWS, Elasticsearch 등 유료 VDB고려하는 것이 보안/속도/기능 면에서 중시할만함
- Embedding model : 보통은 유료 모델은 gpt-ada2 (또는 3)가 성능이 좋으나, 오픈소스 기반으로 중국의 다국적 모델 "BAAI"가 가장 좋다고 함(한글 기준).  BAAI 에서 reranker 모델도 공유하나, 영/중 만 되고 이를 기반으로 파인튜닝한 한글 오픈소스 모델을 활용 : 개선 관련해서는 추가 파인튜닝 필요
- LLM : 본문에서는 최소치 성능을 위해 초기 llama 3 8b instruction 모델을 사용했으나, 용도에 따라서 직접 파인 튜닝 또는 최근 업로드한 오픈소스를 활용하는 것이 좋아보임 (올거나이즈, 야놀자 등)
- Generation : 인용을 활용, 평가 모델을 위한 발판이 될수 있음. 뿐만 아니라 CoT 를 위한 x-shot 질/답 데이터셋을 구성해서 RAG를 위한 파인튜닝 모델 및 시스템 구축 가능 (RAFT)
- LLM CPU : llama.cpp 를 기반으로  gguf 양자화 모델 지원, 이것으로 cpu 가동이 가능함. 그러나 실서비스에선 COM power를 논해야하고, 이보다 gpu로도 가동 가능하니 그쪽으로 고려




### 결과물 예시 : 
![image](https://github.com/oosij/rag/assets/94098546/79a65871-0bbc-4edc-8dd8-897278b76aac)





### 참고하면 좋은 논문 :
  [1] RAFT: Adapting Language Model to Domain Specific RAG (https://arxiv.org/abs/2403.10131)
  
  [2] Corrective Retrieval Augmented Generation (https://arxiv.org/abs/2401.15884)
  
  [3] Precise Zero-Shot Dense Retrieval without Relevance Labels (https://arxiv.org/abs/2212.10496)
  
  [4] Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (https://arxiv.org/abs/2005.11401)
