# 삼성전자 AI 투자 비서

삼성전자의 분기보고서·사업보고서(DART 공시 PDF)를 근거로 질문에 답하는 RAG(검색 증강 생성) 투자 어시스턴트입니다. Streamlit 대시보드에서 재무 건전성 지표, 시장 심리, 동향 스코어를 함께 확인하면서 챗봇에 질문할 수 있습니다.

## 동작 방식

1. `[삼성전자]분기보고서(2026.05.15).pdf`, `[삼성전자]사업보고서(2026.03.10).pdf`를 로드해 문서를 청크 단위로 분할
2. OpenAI 임베딩(`text-embedding-3-small`)으로 벡터화한 뒤 FAISS 벡터스토어에 저장(`faiss_index/`, 최초 실행 시 생성 후 재사용)
3. 질문이 들어오면 관련 청크를 검색해 컨텍스트로 삼아 `gpt-4o-mini`가 답변 생성
4. 답변과 함께 근거로 사용한 문서/페이지를 출처로 함께 표시

## 기술 스택

Streamlit · LangChain · FAISS · OpenAI(gpt-4o-mini, text-embedding-3-small) · Plotly

## 실행

```bash
pip install -r requirements.txt
```

프로젝트 루트에 `.env` 파일을 만들고 OpenAI API 키를 설정합니다.

```
OPENAI_API_KEY=sk-...
```

```bash
streamlit run app.py
```

## 참고

- 사이드바의 재무 건전성/시장 심리/동향 스코어는 데모용 예시 데이터입니다.
- 분석 대상 PDF를 다른 기업의 공시 문서로 교체하면 해당 기업 기준으로 질의응답하도록 재사용할 수 있습니다.
