# 제품 요구사항 문서 (PRD): The Corporate Shield

## 1. 프로젝트 개요 (Project Overview)
*   **프로젝트명:** The Corporate Shield (직장인 방어 기제 자동화 시스템)
*   **버전:** 1.0.0
*   **상태:** 초안 (Draft)
*   **최종 수정일:** 2025-12-22

"Corporate Shield"는 직장인들이 무리한 업무 지시나 모호한 수정 요청에 대응할 수 있도록 돕는 AI 에이전트입니다. 복잡한 기획서와 일정을 분석하여 가장 논리적이고 정중한 거절 사유나 일정 연기 핑계를 설계해주며, 사용자의 "방패" 역할을 수행합니다. B급 유머를 지향하지만, 기술적으로는 Upstage AI 포지션에 걸맞은 Agent Engineering 역량(LangGraph, RAG, VLM)을 보여주는 것을 목표로 합니다.

## 2. 문제 정의 (Problem Statement)
### 2.1 배경 (Background)
현대 직장 생활에서 직원들은 종종 다음과 같은 어려움에 직면합니다.
*   **비정형 지시:** 업무 지시가 구두, 화이트보드 사진, 엑셀, 정리되지 않은 PDF 등 다양한 형태로 전달되어 요구사항 파악이 어렵습니다.
*   **거절의 어려움:** 무리한 요청을 직설적으로 거절하면 무능하거나 비협조적인 사람으로 비춰질까 두려워합니다. "전문가다운" 핑계가 필요합니다.
*   **대응의 비효율:** 그럴듯한 핑계를 만들기 위해 캘린더, 과거 회의록, 사규 등을 확인하는 데 많은 시간이 소요됩니다.

### 2.2 목표 (Goals)
*   비정형 입력(이미지, 텍스트, PDF)의 분석 자동화.
*   상황에 맞는 논리적이고 정중한 거절/연기 전략 생성.
*   Upstage의 기술 스택을 활용한 고급 AI 엔지니어링 역량 입증.

## 3. 사용자 스토리 (User Stories)
| 행위자 | 스토리 | 인수 기준 |
| :--- | :--- | :--- |
| **사용자 (직원)** | 정리되지 않은 기획서(이미지/PDF)를 업로드하여 내용을 다 읽지 않고도 핵심을 파악하고 싶다. | 시스템이 텍스트, 표, 이미지를 파싱하여 구조화된 요약을 제공한다. |
| **사용자 (직원)** | 내 캘린더를 확인하여 시간이 없다는 타당한 핑계를 찾고 싶다. | 시스템이 캘린더 API와 연동하여 바쁜 일정을 식별하고 이를 근거로 제시한다. |
| **사용자 (직원)** | 정중하면서도 단호하게 거절하는 "비즈니스 매너"를 갖춘 이메일 초안을 받고 싶다. | 시스템이 수집된 근거를 바탕으로 "정중한 회사원 말투"의 이메일 초안을 생성한다. |
| **사용자 (직원)** | 사규나 과거 기술적 제약을 핑계 거리로 찾고 싶다. | 시스템이 내부 문서를 검색(RAG)하여 관련 조항이나 기술적 난관을 찾아낸다. |

## 4. 기능 요구사항 (Functional Requirements)

### 4.1 The Messy Spec Decoder (입력 처리)
*   **입력:** PDF, 이미지(JPG, PNG), 텍스트 지원.
*   **처리:**
    *   **Upstage Document Parse API**를 사용하여 텍스트와 표 추출.
    *   **Upstage Document Parse API**를 사용하여 텍스트와 표 추출.
    *   (삭제됨) VLM은 텍스트 위주의 문서 처리에 집중하기 위해 제외. Upstage Document Parse의 강력한 레이아웃 분석 기능을 활용.
*   **출력:** 입력 내용의 구조화된 Markdown/HTML 표현.

### 4.2 The Excuse Planner (핵심 로직 - LangGraph)
*   **워크플로우:** 상태 기반 에이전트 워크플로우 구현: `분석` -> `증거 수집` -> `전략 선택` -> `초안 작성`.
*   **전략:**
    *   *Time Strategy:* 일정 충돌 및 시간 부족 강조.
    *   *Tech Strategy:* 기술적 부채나 레거시 시스템의 한계 강조.
    *   *Process Strategy:* 규정 준수 및 승인 절차의 복잡성 강조.

### 4.3 The Evidence Collector (도구)
*   **Calendar Check:** Google Calendar API를 통해 가용성 조회.
*   **Regulation Search (RAG):** 가상의 사규 및 기술 문서가 저장된 벡터 DB(Pinecone) 검색.
*   **Web Search:** Tavily API를 사용하여 외부 검증(예: "이 기술 스택이 deprecated 되었는가?") 수행.

### 4.4 Polite Refusal Generator (출력)
*   **초안 작성:** **Upstage Solar LLM**을 사용하여 최종 답변 생성.
*   **톤 앤 매너 조정:** 사용자의 솔직한 속마음("이걸 어떻게 해")을 "비즈니스 정중체"로 변환.
*   **Self-Reflection:** Evaluator 노드가 초안의 논리성과 정중함을 점수화(> 0.7 기준)하고, 미달 시 재생성 요청.

## 5. 비기능 요구사항 (Non-Functional Requirements)
*   **성능:** 30초 이내 응답 생성.
*   **비용:** "Zero Cost" 운영 목표 (무료 티어 및 효율적 리소스 사용).
*   **아키텍처:** 서버리스 및 모듈화된 설계.
*   **UI:** Streamlit을 활용한 직관적이고 반응형 웹 인터페이스.

## 6. 기술 스택 (Technical Stack)
| 구분 | 기술 (Technology) | 선정 이유 |
| :--- | :--- | :--- |
| **언어** | Python 3.10+ | AI 엔지니어링 표준. |
| **오케스트레이션** | LangGraph | 순환적이고 상태 관리가 가능한 에이전트 워크플로우 구현 필수. |
| **프레임워크** | LangChain | 표준 LLM 프레임워크. |
| **LLM** | Upstage Solar API | 한국어 처리 성능 우수; 지원 기업 핵심 제품. |
| **문서 분석** | Upstage Document Parse | 강력한 표/서식 파싱 능력. |
| **Embedding** | **Upstage Embedding** | Pinecone과 호환되는 고성능 한국어 임베딩 모델 사용. |
| **Vector DB** | **Pinecone** | 관리형 벡터 데이터베이스로 확장성 및 성능 확보. |
| **검색** | Tavily API | LLM 에이전트에 최적화된 검색. |
| **프론트엔드** | Streamlit | 빠른 프로토타이핑 및 배포 용이. |

## 7. 시스템 아키텍처 (System Architecture)
```mermaid
graph TD
    User[사용자] -->|기획서 업로드| UI[Streamlit UI]
    UI --> InputProc[문서 처리기]
    InputProc -->|Upstage Doc Parse| CleanData[구조화된 데이터]
    
    subgraph Agent Core [LangGraph]
        Planner((기획자)) -->|전략 선택| Tools
        Tools -->|캘린더/RAG/검색| Context
        Context --> Generator((생성기))
        Generator -->|초안| Evaluator((평가자))
        Evaluator -- "재시도" --> Planner
        Evaluator -- "승인" --> FinalDraft
    end
    
    CleanData --> Agent Core
    FinalDraft --> UI
```
