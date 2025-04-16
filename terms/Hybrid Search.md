---
tags: 
link: 
source: https://medium.com/@nuatmochoi/%ED%95%98%EC%9D%B4%EB%B8%8C%EB%A6%AC%EB%93%9C-%EA%B2%80%EC%83%89-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-feat-ensembleretriever-knowledge-bases-for-amazon-bedrock-d6ef1a0daaf1
---
[[Lexical search]] 와 [[Semantic search]]를 결합한 방식

각 검색 방법의 장점을 추려서 사용됨. 
특정 도메인 용어나 제품 용어가 포함된 쿼리로 검색했을 때도 Lexical Search의 검색 결과를 통해 보조함. 
의미가 유사한 동의어 검색의 경우 및 오타가 일부 포함되더라도 Semantic Search가 벡터 기반으로 가장 가까운 내용을 반환하기 때문에 보다 정확도를 높일 수 있음.
[[EnsembleRetriever]]로 검색방법 합쳐서 사용

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
