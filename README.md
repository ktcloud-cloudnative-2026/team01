# <비전어스> — <Home IoT 플랫폼>

> 이 저장소가 프로젝트의 원본입니다. 기획 · 설계 문서, 코드, 배포 파일, 결정 기록을 모두 여기에 둡니다.
>
> `docs/`의 문서와 규칙은 시작용 예시입니다. 팀에 맞게 고치거나 지우고, 필요한 문서는 새로 만들어도 됩니다.

## 한 줄 소개
<무엇을 하는 서비스인가 — 예: 같은 좌석이 두 번 팔리지 않는 공연 예매 서비스>

## 문서

| 구분 | 문서 | 내용 |
|---|---|---|
| 기획 | [기획서](docs/plan.md) | 목표 · 범위 · 성공 기준 · 일정 |
| | [요구사항](docs/requirements.md) | 기능(US-번호) · 비기능 |
| 설계 | [아키텍처](docs/architecture.md) | 컨텍스트 · 서비스 구성 · 핵심 시퀀스 · 배포 구성 |
| | [기술 스택](docs/tech-stack.md) | 무엇을 왜 어디에 |
| | [API 명세](docs/api/) | 서비스별 OpenAPI |
| | [ERD](docs/erd/) | 서비스별 DB |
| | [관측 설계](docs/observability.md) | 지표 · 로그 · 알림 |
| 협업 | [역할](docs/roles.md) | 서비스 담당 · 공통 영역 담당 |
| | [공통 규칙](docs/conventions.md) | Git 전략 · 코드 규칙 · 비밀값 |
| | [팀 운영 약속](docs/team.md) | 완료 기준 · 회의 · 의사결정 |
| | [GitHub 사용 가이드](docs/github-guide.md) | clone · 브랜치 · PR · 리뷰 |
| 운영 | [런북](docs/runbook.md) | 장애별 확인 명령 · 조치 |
| | [결정 기록](docs/adr/) | 무엇을 왜 골랐나 |

## 외부 도구로 작성하기

문서는 이 저장소에 직접 써도 되고, 더 편한 도구로 써도 됩니다. 외부 도구로 쓰면 해당 `docs/` 파일에 링크와 한 줄 요약을 남겨, 이 README에서 모든 문서를 찾을 수 있게 합니다.

| 문서 | 잘 맞는 도구 |
|---|---|
| 기획서 · 기술 스택 · 팀 운영 약속 · 회의록 | [Notion](https://www.notion.so), [Google Docs](https://docs.google.com) |
| 요구사항 · 할 일 | GitHub Projects · Issues, [Notion](https://www.notion.so) 데이터베이스 |
| 아키텍처 그림 | [draw.io](https://app.diagrams.net), [Excalidraw](https://excalidraw.com), [FigJam](https://www.figma.com/figjam/), [Miro](https://miro.com), [Mermaid](https://mermaid.js.org)(Markdown 안에 바로) |
| 화면 설계 · 와이어프레임 | [Figma](https://www.figma.com) |
| API 명세 | [Swagger Editor](https://editor.swagger.io)(OpenAPI 파일), [Postman](https://www.postman.com) |
| ERD | [dbdiagram.io](https://dbdiagram.io), [ERDCloud](https://www.erdcloud.com) |

- 저장소가 공개이므로 외부 문서는 "링크가 있는 사람 보기"로 공유합니다. 비밀값은 외부 문서에도 쓰지 않습니다
- 그림은 링크와 함께 PNG 한 장을 `docs/`에 같이 올려 두면 도구가 없어도 볼 수 있습니다
- API 명세는 코드와 함께 바뀌므로 OpenAPI 파일을 저장소에 두는 것을 권합니다

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
