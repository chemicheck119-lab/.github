<div align="center">

# 🧪 ChemiCheck119

### 소방안전 빅데이터 기반 화학사고 현장대응 지원 서비스

불완전한 신고를 화학물질 후보와 공식 대응 근거로 연결하고,  
**현장에서 확인된 물질 조합만 규칙 기반으로 검토**하여 출동대원의 판단을 지원합니다.

[![Live Demo](https://img.shields.io/badge/Live_Demo-chemicheck119.site-E65100?style=for-the-badge)](https://chemicheck119.site)
[![Competition](https://img.shields.io/badge/소방안전_빅데이터-서비스_개발_부문-C62828?style=for-the-badge)](https://www.bigdata-119.kr/)
[![Safety](https://img.shields.io/badge/Safety-Human_in_the_Loop-1565C0?style=for-the-badge)](https://github.com/chemicheck119/llm/blob/main/docs/SAFETY_AND_LIMITATIONS.md)

</div>

> **공개 데모 안내**  
> 현재 서비스는 공모전용 staging입니다. 신고·사고 위치·현장 확인은 개인정보가 없는 합성 데이터이며, 실제 119 지령망이나 소방기관 운영 시스템과 연결되어 있지 않습니다.

## 서비스 흐름

```text
화학사고 신고
  → 신고문 구조화
  → 사고물질·시설 과거 취급물질 후보 검색
  → 출동대원의 CAS 확인
  → CAMEO 규칙 기반 물질 간 충돌 검토
  → KOSHA MSDS·CAMEO 근거가 연결된 현장 브리프
  → 대응 결과 기록
```

| 핵심 기능 | 구현 방식 | 안전 장치 |
|---|---|---|
| **물질 후보 식별** | 물질명·CAS·별칭·오탈자를 결합한 Resolver | 후보를 확정값이나 확률로 표현하지 않음 |
| **공식 근거 검색** | KOSHA MSDS·CAMEO 문서 기반 검색 | 검색되지 않은 대응정보의 임의 생성 제한 |
| **물질 간 충돌 검토** | CAMEO 반응성 그룹·호환성 규칙 조회 | 현장에서 CAS 2개를 확인하기 전 판정 차단 |
| **현장 브리프 제공** | 위험 조합과 확인사항을 짧은 표로 압축 | 대응 명령이 아닌 판단 지원 정보로 제공 |

## 모델 성능

| 평가 항목 | 결과 | 실제 의미 |
|---|---:|---|
| Resolver Top-1 Accuracy | **89.74%** | 소방 사고 표현 419건 중 정답 물질을 1순위로 제시한 비율 |
| Resolver Top-3 Recall | **90.21%** | 정답 물질을 상위 3개 후보 안에 포함한 비율 |
| 사고유형 Recall | **83.76%** | 2021~2025년 전국 화학사고 442건에서 사고유형을 찾은 비율 |
| 물질명 언급 Recall | **81.50%** | 같은 평가에서 사고문에 언급된 물질명을 찾은 비율 |

> 수치는 저장소에 고정된 평가셋의 재현 결과이며 전국 현장 정확도를 의미하지 않습니다.  
> 평가 기준과 실패 사례는 [AI 모델 평가 문서](https://github.com/chemicheck119/llm/blob/main/docs/EVALUATION.md)에서 확인할 수 있습니다.

## 저장소 안내

| 저장소 | 역할 | 주요 기술 |
|---|---|---|
| [**front**](https://github.com/chemicheck119/front) | 출동 위치·물질 확인·충돌 결과·대응 기록을 연결하는 태블릿 대시보드 | TypeScript · React · Vite |
| [**back**](https://github.com/chemicheck119/back) | 인증·사고 상태·AI 연동·현장 확인·기록 저장을 담당하는 BFF | Java 17 · Spring Boot · PostgreSQL |
| [**llm**](https://github.com/chemicheck119/llm) | 신고문 분석·물질 Resolver·근거 검색·CAMEO Rule Engine | Python 3.11 · FastAPI · TF-IDF/BM25 |
| [**.github**](https://github.com/chemicheck119/.github) | 조직 소개와 공통 GitHub 설정 | Markdown |

## 활용 데이터

- **소방안전 빅데이터:** 실제 사고 표현 학습과 상태·색상·냄새 기반 물질 후보 검색
- **KOSHA MSDS:** 유해성·보호구·응급조치·누출 대응 근거
- **NOAA/EPA CAMEO Chemicals:** 반응성 그룹과 물질 간 호환성 규칙
- **ICIS·PRTR:** 전국 시설의 과거 취급·배출·이동 이력 후보
- **화학물질안전원 화학사고 정보:** 신고문 처리 성능 외부 평가

## Safety by Design

- 모호한 물질은 하나로 단정하지 않고 **Top-K 후보와 추가 확인사항** 제공
- 현장에서 확인된 사고물질 CAS와 시설물질 CAS가 모두 있을 때만 **Rule Engine 실행**
- 위험등급과 반응성은 LLM이 아닌 **검증된 CAMEO 규칙으로 판정**
- 생성 문장은 공식 근거와 연결하고, 인용 검증 실패 시 **추출형 응답으로 전환**
- 전국 시설 정보는 **과거 공개 취급 후보**이며 현재 재고로 표현하지 않음
- 최종 대응 판단 권한은 **현장 지휘관과 출동대원에게 유지**

---

<div align="center">

**AI가 물질을 단정하거나 대응을 지시하지 않도록 설계한, 근거 중심 화학사고 현장대응 지원 시스템**

[서비스 데모](https://chemicheck119.site) ·
[AI 아키텍처](https://github.com/chemicheck119/llm/blob/main/docs/ARCHITECTURE.md) ·
[API 문서](https://github.com/chemicheck119/llm/blob/main/docs/API.md)

</div>
