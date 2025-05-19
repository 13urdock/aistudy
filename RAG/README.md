# 🔍 RAG(Retrieval-Augmented Generation) 챗봇 프로젝트

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![LangChain](https://img.shields.io/badge/LangChain-0.1.0-green.svg)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-red.svg)
![ChromaDB](https://img.shields.io/badge/ChromaDB-0.4.0-purple.svg)

## 📝 프로젝트 개요

검색 증강 생성(Retrieval-Augmented Generation, RAG) 기술을 활용한 챗봇 시스템 구현 프로젝트입니다. 다양한 리트리버(Retriever) 모델을 비교 분석하고, 최적의 RAG 파이프라인을 구축했습니다. 특히 '코딩 공모전 수상작 정보'를 분석하는 실제 사례를 통해 리트리버 성능을 비교했습니다.

## 🎯 주요 기능

- 다양한 리트리버(Similarity, MMR, Contextual Compression, Self-Query) 구현 및 성능 비교
- 벡터 데이터베이스(ChromaDB)를 활용한 효율적인 정보 검색
- 웹 데이터 크롤링 및 전처리
- 검색된 정보를 활용한 LLM 응답 생성

## 📈 리트리버 성능 비교

| 리트리버 유형 | 장점 | 단점 | 적합한 상황 |
|-------------|------|------|------------|
| Similarity | 구현이 단순하고 직관적 | 단순 유사도만 고려하여 중복 정보 가능 | 간단한 정보 검색 |
| MMR | 관련성과 다양성을 모두 고려 | 조정 매개변수 설정이 필요 | 중복 정보를 피하고 다양한 관점 필요 |
| Contextual Compression | 핵심 내용만 추출하여 효율적 | LLM 추가 비용 발생 | 장문 문서 처리 시 |
| Self-Query | 구조화된 정보 검색에 강점 | 메타데이터 정의 작업 필요 | 복잡한 쿼리와 구조화된 데이터 |

자세한 비교 내용은 [리트리버 비교 블로그](https://velog.io/@l3urdock/RAG-%EB%A6%AC%ED%8A%B8%EB%A6%AC%EB%B2%84-%EB%B9%84%EA%B5%90%ED%95%98%EA%B8%B0)에서 확인할 수 있습니다.

## 🏆 실험 결과

실제 'ALL-in 코딩 공모전' 수상작 정보를 분석한 결과, Self-Query 리트리버가 가장 효과적인 성능을 보였습니다:

- **구조화된 정보 검색**: 메타데이터 필드를 명확히 정의하여 특정 정보를 정확히 추출
- **포괄적인 정보 제공**: 다른 리트리버들이 놓친 입선작까지 포함한 완전한 정보 검색
- **체계적인 정보 추출**: 기술 스택, 서비스 기능, 문제 해결 방식 등 구조화된 정보 제공

## 🚀 결론

다양한 리트리버 방식을 비교한 결과, 정보의 특성과 필요에 따라 적절한 리트리버를 선택하는 것이 중요합니다:

- **단순 정보 검색**: Similarity 리트리버 활용
- **다양한 관점 필요**: MMR 리트리버 활용
- **장문 문서 압축**: Contextual Compression 리트리버 활용
- **구조화된 정보 검색**: Self-Query 리트리버 활용

특히 메타데이터가 잘 정의된 환경에서는 Self-Query 리트리버가 가장 우수한 성능을 보여주었습니다.
