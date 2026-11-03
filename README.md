# <팀 이름> — <서비스 이름>

> 이 저장소가 프로젝트의 원본입니다. 기획 · 설계 문서, 코드, 배포 파일, 결정 기록을 모두 여기에 둡니다.
> 템플릿으로 만든 뒤 `<...>` 부분을 팀 내용으로 바꿉니다.

## 한 줄 소개
<무엇을 하는 서비스인가 — 예: 같은 좌석이 두 번 팔리지 않는 공연 예매 서비스>

## 문서

| 구분 | 문서 | 내용 |
|---|---|---|
| 기획 | [기획서](docs/plan.md) | 목표 · 범위 · 성공 기준 · 측정 계획 · 일정 |
| | [요구사항](docs/requirements.md) | 기능(US-번호) · 비기능 |
| 설계 | [아키텍처](docs/architecture.md) | 컨텍스트 · 서비스 구성 · 핵심 시퀀스 · 배포 구성 |
| | [기술 스택](docs/tech-stack.md) | 무엇을 왜 어디에 |
| | [API 명세](docs/api/) | 서비스별 OpenAPI |
| | [ERD](docs/erd/) | 서비스별 DB |
| | [관측 설계](docs/observability.md) | 지표 · 로그 · 알림 |
| 협업 | [역할](docs/roles.md) | 서비스 담당 · 공통 영역 담당 |
| | [공통 규칙](docs/conventions.md) | Git 전략 · 코드 규칙 · 비밀값 |
| | [팀 운영 약속](docs/team.md) | 완료 기준 · 회의 · 의사결정 |
| 운영 | [런북](docs/runbook.md) | 장애별 확인 명령 · 조치 |
| | [결정 기록](docs/adr/) | 무엇을 왜 골랐나 |

## 링크

| 항목 | 링크 |
|---|---|
| 할 일 보드 (GitHub Projects) | <Projects 주소> |
| dev 주소 | <dev 주소> |
| prod 주소 | <prod 주소> |
| 대시보드 | <Grafana 주소> |

## 폴더

```
docs/       설계 문서 (이 README의 표)
services/   서비스 코드 — 서비스마다 폴더 하나
deploy/     Kubernetes 매니페스트 · Argo CD
.github/    PR · 이슈 템플릿, CI 워크플로
```

## 시작하기
1. [역할](docs/roles.md)과 [팀 운영 약속](docs/team.md)을 채웁니다
2. [기획서](docs/plan.md) → [요구사항](docs/requirements.md) → [아키텍처](docs/architecture.md) → [기술 스택](docs/tech-stack.md) → [API 명세](docs/api/) 순서로 채웁니다
3. 문서를 바꿀 때도 브랜치 → PR → 리뷰를 거칩니다

## 첫날 설정 (팀이 직접)
강사가 저장소와 main 보호 규칙(PR 필수 · 승인 1명)을 만들어 둡니다. 나머지는 팀이 합니다.
- [ ] 맨 위 제목 · 한 줄 소개를 팀 서비스로 바꿈
- [ ] [역할](docs/roles.md)에 팀원 · GitHub 아이디 · 맡은 서비스 · 공통 영역 적기
- [ ] [CODEOWNERS](.github/CODEOWNERS)에 서비스 폴더별 담당자 적기
- [ ] Projects 보드 만들기 — 조직 Projects → New project → Board, Status에 In Review 추가(Todo · In Progress · In Review · Done), 필드: 서비스 → 이 저장소 Projects 탭에서 Link a project → 위 "링크" 표에 주소 적기
- [ ] [팀 운영 약속](docs/team.md) 채우기
- [ ] 첫 이슈 만들기 — "New issue → 일감"으로 [기획서](docs/plan.md) 작성 이슈
