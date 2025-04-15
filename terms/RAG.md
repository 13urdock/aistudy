RAG는 LLM에 추가 정보를 학습시켜 최신정보를 제공할 수 있도록 하는 프레임워크야.

## Generation이란?

- 사용자 쿼리(프롬프트)에 대한 응답으로 텍스트를 생성하는 것
- 예시) "가장 많은 위성을 가진 행성은?"
    - LLM: "목성(나의 기억으로)", 출처 없음
    - 문제점: 출처가 없고, 오래된 정보일 수 있음 (= LLM의 문제점)

## Retrieval-Augmented의 의미

- 데이터들을 저장해두고, LLM이 대답하기 전에 사용자 쿼리와 관련 있는 정보를 가져온 후 응답
- 데이터를 확장할수록 LLM은 데이터에 기반한 더 정확한 정보를 제공할 수 있음
- LLM이 학습하며 얻은 정보에만 의존하지 않게 됨
- 모른다는 답을 할 수 있음 (정보가 없을 때 답을 지어내지 않음)
## 진행과정
문서로드
-> 문서를 청크로 분할
-> 청크를 벡터로 변환해 벡터 데이터베이스에 저장
-> user query와 관련된 문서 검색
-> 검색해온 것과 질문을 결합해 LLM을 위한 프롬프트 생성
-> LLM으로 응답 생성

## RAG vs Fine Tuning

### LLM의 과제

- 너무 구체적인 정보는 학습하지 않았고, 최신 정보나 정확한 정답을 제공하지 못함

### RAG

- 모델의 기능을 확장하기 위해 외부의 최신 정보를 검색하여 원래 프롬프트를 보강한 후 답변 생성
- retriever가 정보를 찾아 LLM에 전달하고, 다시 출력하는 방식
- 모델 재훈련이 필요 없음

### Fine Tuning

- 특정 데이터셋을 더 학습시켜 특정 분야에서 답변을 잘할 수 있도록 하는 것
- 특정 도메인/영역에서 LLM을 특화시킴
- 이 데이터들은 LLM에 직접 포함됨

### 각각의 장단점

#### RAG

- 장점:
    - 동적 데이터 소스(database, data repository)에 좋음
    - 데이터가 지속적으로 업데이트되어야 하는 상황에 적합
- 단점:
    - 데이터를 올바르게 가져오는 방식에 대한 고민이 필요함

#### Fine Tuning

- 장점:
    - 데이터에 대한 맥락을 제공
    - 모델의 가중치를 직접 조정하므로 추론 속도가 빠르고 비용이 적게 듦
    - 프롬프트를 적게 작성해도 모델이 적절한 답변을 생성함
    - 특정 태스크에 더 강해질 수 있음
- 단점:
    - 최신 정보 반영이 어려움

### 언제 무엇을 사용해야 할까?

#### RAG

- 지속적으로 업데이트되는 상황 (예: documentation 챗봇)
- 출처가 필요한 정보를 제공하는 경우

#### Fine Tuning

- 글쓰기 스타일, 용어 등에 미묘한 차이가 있는 특정 분야
- 과거 데이터에 대한 분석이나 그것을 기반으로 한 상황 이해가 필요할 때
## 용어

### Use Case
사용자가 시스템을 어떻게 사용하는지를 나타낸 시나리오

### retriever - search type
similarity
쿼리 벡터와 문서 벡터의 유사도를 기반으로 순위 산정
mmr
similarity_score_threshold

source: https://drfirst.tistory.com/entry/langchain%EA%B3%B5%EB%B6%80-Retriever-%EA%B8%B4-%EB%AC%B8%EC%84%9C%EC%97%90%EC%84%9C-%EC%9B%90%ED%95%98%EB%8A%94-%EB%8B%B5%EB%B3%80-%EC%B0%BE%EA%B8%B03-feat-similarity-mmr-similarityscorethresholdhybrid


### hybrid search

- **Lexical Search** 
	전통적인 검색 엔진에서 주로 사용되는 방법BM25?와 같은 희소 벡터 알고리즘을 통해 키워드 기반 매칭을 진행한다. 특정 도메인 용어를 검색하기에 용이하지만 오타 및 동의어에 취약하다.
- **Semantic Search**
	키워드가 일치하지 않더라도 의미론적으로 유사한 검색 결과를 반환

 각 검색 방법의 장점을 추려서 사용된다. 특정 도메인 용어나 제품 용어가 포함된 쿼리로 검색했을 때도 Lexical Search의 검색 결과를 통해 보조한다. 의미가 유사한 동의어 검색의 경우 및 오타가 일부 포함되더라도 Semantic Search가 벡터 기반으로 가장 가까운 내용을 반환하기 때문에 보다 정확도를 높일 수 있다.
```python
from langchain.retrievers import EnsembleRetriever

from langchain_community.vectorstores import OpenSearchVectorSearch

  
# source: https://github.com/aws-samples/aws-ai-ml-workshop-kr/blob/master/genai/aws-gen-ai-kr/10_advanced_question_answering/03_2_rag_opensearch_hybrid_ensemble_retriever_kr.ipynb
vector_db = OpenSearchVectorSearch(

            index_name=index_name,

            opensearch_url=oss_endpoint,

            embedding_function=embeddings,

            http_auth=http_auth,

            is_aoss =False,

            engine="faiss",

            space_type="l2"

        )

  

semantic_retriever = vectorstore.as_retriever(

    search_type="similarity",

    search_kwargs={"k": 3}

)

semantic_result = semantic_retriever.get_relevant_documents(user_msg)

  

lecical_retriever = OpenSearchLexicalSearchRetriver(

  os_client=os_client,

  index_name=index_name

)

lexical_retriever.update_search_params(

  k=10, # 10개의 결과값 반환

  minimum_should_match=0

)

lexical_result = lexical_retriever.get_relevant_documents(user_msg)

  

# 두 검색결과 합해서 검색

ensemble_retriever = EnsembleRetriever(

  retrievers=[lexical_retriever, semantic_retriever],

  weights=[0.5, 0.5]

)

  

hybrid_docs = ensemble_retriever.get_relevant_documents(user_msg)

```
source: https://medium.com/@nuatmochoi/%ED%95%98%EC%9D%B4%EB%B8%8C%EB%A6%AC%EB%93%9C-%EA%B2%80%EC%83%89-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-feat-ensembleretriever-knowledge-bases-for-amazon-bedrock-d6ef1a0daaf1

### 주피터 노트북 velog 로 옮기는 방법
주피터->마크다운->벨로그
https://velog.io/@ganadara/Jupyter-Notebook-%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4%EB%B3%80%ED%99%98%ED%95%98%EC%97%AC-%EB%B2%A8%EB%A1%9C%EA%B7%B8-%EC%97%85%EB%A1%9C%EB%93%9C%EB%B0%A9%EB%B2%95

## retriever 비교
https://medium.com/@shravankoninti/mastering-rag-a-deep-dive-into-retriever-2ac7957106b7

## FAISS
vector db developed by facebook

## unstructured loader
