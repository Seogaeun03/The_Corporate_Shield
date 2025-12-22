# Project Ideation: The Corporate Shield (AI 직장인 생존 전략가)

> "우리는 기술적 부채와 모호한 기획서 뒤에 숨는다."
> **Upstage AI Agent Engineer 포지션 지원을 위한 Agentic AI 프로젝트**

## 1. 프로젝트 개요 (Overview)
* **프로젝트명:** The Corporate Shield (직장인 방어 기제 자동화 시스템)
* **목표:** 상사의 불가능한 업무 지시나 모호한 수정 요청에 대해, 복합 문서와 일정을 분석하여 **'가장 논리적이고 정중한 거절 사유'**와 **'일정 연기 핑계'**를 설계해주는 AI 에이전트 개발.
* **핵심 가치:** B급 유머를 표방하지만, 기술적으로는 **Upstage가 요구하는 Agent Engineering 역량(LangGraph, RAG, VLM)**을 고도화하여 구현함.
* **개발 기간:** 4주 (예상)

## 2. 배경 및 문제 정의 (Problem Statement)
### 2.1. 현업의 고충 (Pain Point)
1.  **비정형 데이터의 홍수:** 기획서는 텍스트뿐만 아니라 화이트보드 사진, 엑셀 표, 구두 지시사항 등이 뒤섞여 있어 파악이 힘들다.
2.  **거절의 어려움:** "안 됩니다"라고 말하면 무능해 보인다. "현재 레거시 시스템의 구조적 한계와 보안 규정으로 인해..."라고 말해야 전문가처럼 보인다.
3.  **대응의 비효율:** 핑계를 대기 위해 과거 회의록을 뒤지고 캘린더를 확인하는 데 너무 많은 시간이 소요된다.

### 2.2. 해결책 (Solution)
* **Upstage Solar LLM**의 뛰어난 한국어 처리 능력과 **Document Parse** 기술을 활용하여 문서를 해독.
* **LangGraph**를 통해 '상황 분석 → 증거 수집 → 전략 수립 → 핑계 생성'의 사고 과정(Reasoning)을 자동화.

## 3. 핵심 기능 (Key Features)
### 🛠️ 1. The Messy Spec Decoder (난해한 기획서 해독)
* 손글씨, 스크린샷, 표가 섞인 PDF/이미지를 업로드하면 **Upstage Document Parse API**를 통해 구조화된 데이터(HTML/Markdown)로 변환.
* **VLM(Vision Language Model)**을 활용하여 "이 화살표가 가리키는 버튼"과 같은 시각적 맥락 이해.

### 🧠 2. The Excuse Planner (핑계 전략 수립 - LangGraph)
* 단순 답변 생성이 아닌, **Stateful한 에이전트 워크플로우** 구현.
* **전략 분기(Conditional Edge):**
    * *Time Strategy:* 일정 부족 강조 (캘린더 연동)
    * *Tech Strategy:* 기술적 난이도/레거시 문제 강조 (과거 기술문서 RAG)
    * *Process Strategy:* 절차 및 승인 이슈 강조 (사규 검색)

### 🕵️ 3. The Evidence Collector (증거 수집 - Tools)
* **Calendar Check:** "이번 주는 전체 회의와 인터뷰로 인해 가용 시간이 2시간뿐입니다"라는 팩트 확보.
* **Regulation Search (RAG):** "보안 지침 3조 2항에 의거하여..."와 같은 강력한 근거 검색.

### 🗣️ 4. Polite Refusal Generator (급여체 변환)
* 사용자의 속마음("이걸 지금 어떻게 해")을 고품격 비즈니스 언어로 변환.
* **Evaluation:** 생성된 핑계의 '논리적 완결성'과 '정중함'을 자체 평가(Self-Reflection).

## 4. 기술 스택 (Tech Stack)
> **컨셉:** 최소 비용(Zero Cost), 유지보수 제로(Serverless), 하지만 확실한 기술력 어필.

| 구분 | 기술(Technology) | 선정 이유 |
| :--- | :--- | :--- |
| **Language** | **Python 3.10+** | AI 엔지니어링 표준 언어 |
| **Orchestration** | **LangGraph** | 순환(Loop) 및 상태 관리(State Management)가 가능한 에이전트 워크플로우 구현 (채용 공고 필수 요건) |
| **Framework** | **LangChain** | LLM 애플리케이션 개발 프레임워크 |
| **LLM Model** | **Upstage Solar API** | 한국어 뉘앙스 처리에 탁월하며, 지원 기업의 핵심 제품 활용 (우대 사항 공략) |
| **Doc Understanding**| **Upstage Document Parse** | PDF 및 이미지 내의 복잡한 표(Table) 구조 인식 |
| **Vector DB** | **ChromaDB (Local)** | 별도 서버 구축 없이 인메모리/로컬 파일로 벡터 저장 (비용 절감) |
| **Web Search** | **Tavily API** | LLM에 최적화된 검색 결과 제공 (무료 티어 활용) |
| **UI & Hosting** | **Streamlit Community Cloud** | Python만으로 UI 구현 및 무료 배포/호스팅 가능 |

## 5. 시스템 아키텍처 (Architecture)

```mermaid
graph TD
    User[User (Frustrated Employee)] -->|Upload Spec & Query| UI[Streamlit UI]
    
    subgraph "Serverless Container"
        UI --> DocParser[Upstage Document Parse]
        DocParser --> VectorDB[(ChromaDB - Local)]
        
        UI --> Agent[LangGraph Agent]
        
        Agent -->|Plan| Planner((Planner Node))
        Planner -->|Need Info?| Tools
        
        subgraph "Tools & APIs"
            Tools -->|Search| Tavily[Tavily Search API]
            Tools -->|Check Schedule| Cal[Google Calendar API]
            Tools -->|Retrieve Docs| VectorDB
        end
        
        Tools -->|Context| Generator((Generator Node))
        Generator -->|Draft| Evaluator((Evaluator Node))
        
        Evaluator -- "Not Good (Score < 0.7)" --> Planner
        Evaluator -- "Good" --> Final[Polite Email Draft]
    end
    
    Final --> UI