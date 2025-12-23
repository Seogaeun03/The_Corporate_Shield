# The Corporate Shield 🛡️

> "무리한 업무 요청을 논리적으로 거절하는 AI 에이전트"

**AI 직장인 생존 전략가** - Upstage AI Agent Engineer 포지션 지원을 위한 Agentic AI 프로젝트

## 📋 프로젝트 개요

The Corporate Shield는 상사의 불가능한 업무 지시나 모호한 수정 요청에 대해, 복합 문서와 일정을 분석하여 **가장 논리적이고 정중한 거절 사유**와 **일정 연기 핑계**를 설계해주는 AI 에이전트입니다.

B급 유머를 표방하지만, 기술적으로는 **Upstage가 요구하는 Agent Engineering 역량(LangGraph, RAG, Document Parse)**을 고도화하여 구현합니다.

## 🎯 핵심 기능

- **📄 The Spec Decoder**: Upstage Document Parse를 활용한 복잡한 문서 분석
- **🧠 The Excuse Planner**: LangGraph 기반 상태 관리형 에이전트 워크플로우
- **🔍 The Evidence Collector**: Pinecone RAG + Tavily Search를 통한 증거 수집
- **✉️ Polite Refusal Generator**: 정중하고 논리적인 거절 메시지 생성

## 🛠️ 기술 스택

| 구분 | 기술 |
|------|------|
| **언어** | Python 3.10+ |
| **오케스트레이션** | LangGraph |
| **프레임워크** | LangChain |
| **LLM** | Upstage Solar API |
| **문서 분석** | Upstage Document Parse |
| **Embedding** | Upstage Embedding |
| **Vector DB** | Pinecone |
| **검색** | Tavily API |
| **프론트엔드** | Streamlit |

## 📚 문서

- [📝 PRD (Product Requirements Document)](docs/PRD.md)
- [✅ Tasks (작업 목록)](docs/tasks.md)
- [💡 Ideation (아이디어 노트)](ideation.md)

## 🚀 개발 일정

- **1주차**: 환경 설정 및 기본 구조 (LangGraph, Pinecone Setup)
- **2주차**: 입력 처리 및 문서 분석기 구현
- **3주차**: 에이전트 핵심 로직 및 도구 연동
- **4주차**: UI 구현 및 최종 배포

## 📄 라이선스

MIT License

---

**Made with ☕ for Upstage AI Agent Engineer Position**
