# Repository governance

케미체크119 저장소의 변경은 재현 가능성, 근거 추적성, 안전한 배포를 우선합니다.

## 저장소 역할

| 저장소 | 책임 | 기본 브랜치 |
| --- | --- | --- |
| `front` | 현장 대응 UI와 사용자 흐름 | `develop` |
| `back` | BFF, 인증, 영속화, 배포 경계 | `develop` |
| `speech-service` | 음성 전처리·STT·음성 평가 | `main` |
| `analysis-engine` | Parser·Resolver·Retriever·Rule Engine 및 평가 | `main` |
| `data-pipeline` | 데이터 수집·manifest·분할·품질검증 | `main` |
| `infra` | 클라우드 인프라와 운영 자동화(현재 저장소만 생성) | `main` |
| `.github` | 조직 공통 정책과 템플릿 | `main` |

## 변경 흐름

1. 이슈에 문제, 범위, 완료 기준을 기록합니다.
2. `feature/`, `fix/`, `experiment/`, `modeling/`, `docs/`, `chore/` 브랜치에서 작업합니다.
3. 기본 브랜치로 Pull Request를 만들고 템플릿의 검증·근거 항목을 채웁니다.
4. 필수 CI가 통과하고 대화가 해결된 뒤 squash merge합니다.
5. 배포 변경은 staging 검증과 복구 방법을 기록한 뒤 진행합니다.

브랜치 suffix는 영어 kebab-case를 사용하고 `codex`를 붙이지 않습니다. 이슈·커밋·PR
제목은 Conventional Commit 형식으로 통일합니다.

```text
feat(evaluation): 모의 통신 왜곡 LoRA 진입 Gate 강화
fix(speech): 긴 음성 timeout 처리
docs(infra): Cloud Run 복구 절차 보완
test(resolver): 미관측 표현 회귀 평가 추가
```

`type(scope)`는 영어로 쓰고, 뒤 설명은 한국어를 기본으로 하되 `Resolver`, `Recall`,
`Cloud Run`처럼 정확성이 필요한 기술 용어는 영어로 유지합니다. 제목 전체를 한국어로만
작성하거나 `[Feature]` 같은 별도 접두사를 사용하지 않습니다.

## 이슈와 라벨

- 모든 구현·실험은 관련 이슈에 문제, 근거 상태, 완료 조건을 먼저 기록합니다.
- 코드 병합과 실제 데이터 평가 완료를 별도 체크 항목으로 관리합니다.
- 실제 음성·전사문·개인정보·Secret은 이슈와 PR에 첨부하지 않습니다.
- 진행 중 이슈에는 `status:in-progress`, 실제 외부 의존성으로 중단되면
  `status:blocked`를 사용합니다. 두 상태 라벨을 동시에 붙이지 않습니다.
- 측정 근거가 남지 않은 주장은 `evidence-needed`, 2-CAS Gate 등 안전 불변식에 영향을
  주는 변경은 `safety:critical`로 표시합니다.
- 닫기 전에 체크리스트, 재현 명령, artifact hash, 채택·기각 결정을 갱신합니다.

라벨 분류와 적용 기준은 [Label policy](LABELS.md)를 따릅니다.

## 사실 상태

기능과 성과는 다음 중 하나로 표시합니다.

- 구현 완료
- 부분 구현 또는 개발용 데모
- 설계 완료·구현 전
- 검증되지 않은 가설

목표 지표와 실제 측정값을 섞지 않습니다. Resolver 419건 평가와 Parser 442건 평가를 구분하고, 시설 후보 데이터는 현재 재고로 표현하지 않습니다. CAMEO 결과는 확률이 아닌 공개 규칙의 제한적인 서수 결과이며, 두 CAS가 각각 확인되기 전에는 Rule Engine을 실행하지 않습니다.

## 보안과 운영

- Secret, 토큰, 개인정보, 실제 신고 음성은 저장소에 커밋하지 않습니다.
- 로그와 재현 자료는 민감정보를 제거한 뒤 첨부합니다.
- Actions 권한은 읽기 전용을 기본으로 하고 필요한 작업에만 권한을 추가합니다.
- 배포 PR에는 영향, 관측 방법, 복구 절차를 포함합니다.
- 개발용 Cloud Run preview와 로컬 Kubernetes 실습을 상용 운영 경험으로 표현하지 않습니다.

## 예외

긴급 복구로 기본 절차를 따르기 어려우면 최소 변경으로 복구한 뒤 원인, 조치, 검증, 재발 방지를 후속 이슈에 남깁니다.
