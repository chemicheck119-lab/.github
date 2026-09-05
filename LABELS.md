# Label policy

이슈는 서로 다른 의미의 라벨 축을 섞지 않고 관리합니다. 기존 GitHub 기본 라벨은
호환성을 위해 유지하지만 신규 작업은 아래 조직 공통 라벨을 우선 사용합니다.

## 공통 라벨

| 축 | 라벨 | 적용 기준 |
| --- | --- | --- |
| type | `type:feature` | 새 기능 또는 기능 확장 |
| type | `type:bug` | 의도와 다른 동작 또는 회귀 |
| type | `type:docs` | 문서와 사용법 변경 |
| type | `type:test` | 평가·실험·회귀 검증 |
| type | `type:chore` | 유지보수와 개발 환경 작업 |
| priority | `priority:p0` | 안전·보안·배포 중단으로 즉시 대응 필요 |
| priority | `priority:p1` | 현재 단계에서 우선 처리 |
| priority | `priority:p2` | 계획된 일반 작업 |
| priority | `priority:p3` | 후속 개선 또는 낮은 우선순위 |
| status | `status:in-progress` | 담당 작업이 실제 진행 중 |
| status | `status:blocked` | 권한·승인·외부 데이터 등으로 진행 불가 |
| evidence | `evidence-needed` | 측정값·원본·artifact hash 등 근거 필요 |
| safety | `safety:critical` | 안전 불변식 또는 위험 정보 노출에 영향 |

이슈마다 `type`과 `priority`는 원칙적으로 하나만 사용합니다. `area`는 영향 범위가 여러
저장소나 구성요소에 걸치면 복수로 사용할 수 있습니다. 작업이 끝나면 status 라벨을
제거하고, 필요한 근거가 연결되면 `evidence-needed`도 제거한 뒤 이슈를 닫습니다.

## 저장소별 area

| 저장소 | 대표 라벨 |
| --- | --- |
| `.github` | `area:governance` |
| `front` | `area:front` |
| `back` | `area:back` |
| `speech-service` | `area:speech`, `area:evaluation`, `area:data`, `area:infra` |
| `analysis-engine` | `area:llm`, `area:evaluation`, `area:data` |
| `data-pipeline` | `area:data`, `area:evaluation` |
| `infra` | `area:infra`, `area:ci`, `area:container` |

## 운영 규칙

1. 작업을 시작하면 `status:in-progress`를 붙입니다.
2. 코드 PR이 병합돼도 실제 평가가 남아 있으면 이슈를 닫지 않습니다.
3. 진행이 멈추면 원인과 다음 해제 조건을 댓글로 남기고 `status:blocked`로 바꿉니다.
4. 측정 결과는 데이터 범위, 실제값, artifact hash와 함께 기록합니다.
5. 민감한 원본 대신 비식별 집계와 private artifact 경로만 기록합니다.
