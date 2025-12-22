# 프로젝트 작업 목록 (Tasks): The Corporate Shield

## 🗓️ 전체 일정 (4주 목표)
*   **1주차:** 환경 설정 및 기본 구조 잡기 (LangGraph, Pinecone Setup)
*   **2주차:** 입력 처리 및 문서 분석기 구현 (Upstage Document Parse 집중)
*   **3주차:** 에이전트 핵심 로직 및 도구 연동 (**Tavily 실습 포함**)
*   **4주차:** UI 구현 (Streamlit) 및 최종 배포

---

## 🛠️ 1주차: 프로젝트 셋업 (Environment Setup)
### 1.1 저장소 및 가상환경 구성
- [ ] GitHub Repository 생성 및 Clone
- [ ] Python 3.10+ 가상환경 생성 (`venv`)
- [ ] 필수 라이브러리 설치 (`langchain`, `langgraph`, `streamlit`, `python-dotenv`)
- [ ] `.env` 파일 생성 및 API Key 관리 (Upstage Solar/Embedding, Pinecone, Tavily, Google Cal)

### 1.2 Pinecone (Vector DB) 구축
- [ ] Pinecone 회원가입 및 API Key 발급
- [ ] **Upstage Embedding Model** (`solar-embedding-1-large` 등) 설정 확인
- [ ] Index 생성 (Dimensions: Upstage Embedding 모델 스펙 확인 필요, 보통 4096)
- [ ] 가상 사규/기술 문서 데이터 준비 (Markdown 파일)
- [ ] 문서 Embedding 및 Pinecone 업로드 스크립트 작성

---

## � 2주차: 입력 처리 모듈 (The Spec Decoder)
> 복잡한 문서 양식을 **Upstage Document Parse**로 깔끔하게 구조화합니다.

### 2.1 Upstage Document Parse 연동
- [ ] Upstage Console에서 Document Parse API Key 확인
- [ ] **Document Parse 유틸리티 작성:**
    - PDF/이미지 파일을 API로 전송하고 결과를 받는 함수 구현
    - 결과 JSON에서 HTML/Markdown 내용 추출
- [ ] **단위 테스트:**
    - 복잡한 표가 포함된 기획서(PDF)를 파싱하여 표 구조(`<table>`)가 잘 보존되는지 확인

---

## 🧠 3주차: 에이전트 두뇌 및 도구 (The Excuse Planner)
> LangGraph를 사용하여 상황을 판단하고, 적절한 핑계를 찾기 위해 **Tavily**를 사용합니다.

### 3.1 🔍 심층 탐구 & 실습: Tavily Search API
Tavily는 **AI 에이전트를 위해 만들어진 검색 엔진**입니다. 광고나 불필요한 HTML 태그 없이 **LLM이 바로 읽을 수 있는 요약된 텍스트**를 제공합니다. "외부 핑계(React 지원 종료일 등)"를 찾을 때 필수적입니다.

#### 실습: Tavily 맛보기 (Hello World)
- [ ] **실습 스크립트 작성 (`practice_tavily.py`):**
    ```python
    from tavily import TavilyClient
    import os
    
    # 1. 클라이언트 초기화
    client = TavilyClient(api_key=os.environ["TAVILY_API_KEY"])
    
    # 2. 질문 던지기
    response = client.search("Python 3.14 출시 예정일")
    
    # 3. 결과 확인
    for result in response['results']:
        print(f"제목: {result['title']}")
        print(f"내용: {result['content']}\n")
    ```
- [ ] 위 스크립트를 실행하여 깔끔한 텍스트 결과가 나오는지 직접 눈으로 확인.

### 3.2 Tavily 및 도구 연동 작업 목록
- [ ] **Tavily 가입 및 설정:**
    - [tavily.com](https://tavily.com) 가입 후 API Key 발급
    - `.env` 파일에 `TAVILY_API_KEY` 추가
- [ ] **검색 도구 (Search Tool) 구현:**
    - `langchain_community.tools.tavily_search` 모듈 활용
    - **검색 함수 작성:** `search_tech_issue(query)` -> 기술적 난관이나 버전 호환성 문제 검색
- [ ] **LangGraph 노드 구성:**
    - **Planner Node:** 입력된 요구사항을 보고 '시간 부족', '기술 문제', '규정 위반' 중 어떤 전략을 쓸지 결정
    - **Tool Node:** 결정된 전략에 따라 Calendar API, Pinecone(사규), Tavily(웹검색) 중 필요한 도구 호출
    - **Generator Node:** 수집된 정보를 바탕으로 핑계 생성

---

## 🎨 4주차: UI 구현 및 통합 (Final Polish)
### 4.1 Streamlit 인터페이스
- [ ] 파일 업로더 위젯 (이미지/PDF) 배치
- [ ] 채팅창 스타일의 결과 화면 (User Request vs AI Excuse)
- [ ] 사이드바에 설정(API Key 입력 등) 메뉴 구성

### 4.2 통합 테스트
- [ ] 전체 파이프라인 테스트: 문서 업로드 -> 문서 파싱 -> 전략 수립 -> 검색/검증 -> 최종 핑계 생성
- [ ] 생성된 핑계의 퀄리티 평가 및 프롬프트 튜닝
