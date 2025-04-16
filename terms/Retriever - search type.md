---
tags: 
link: 
source: https://drfirst.tistory.com/entry/langchain%EA%B3%B5%EB%B6%80-Retriever-%EA%B8%B4-%EB%AC%B8%EC%84%9C%EC%97%90%EC%84%9C-%EC%9B%90%ED%95%98%EB%8A%94-%EB%8B%B5%EB%B3%80-%EC%B0%BE%EA%B8%B03-feat-similarity-mmr-similarityscorethresholdhybrid
---
쿼리와 문서를 비교하는 다양한 검색 방식:

- **Similarity**: 쿼리 벡터와 문서 벡터의 유사도를 기반으로 순위 산정
- **MMR**: Maximum Marginal Relevance, 유사도와 다양성을 모두 고려한 검색 방식
- **Similarity Score Threshold**: 특정 유사도 점수 이상인 문서만 반환