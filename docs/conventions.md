# 공통 규칙

[← README](../README.md)

> 시작용 예시입니다. 팀에서 정한 내용으로 고치거나, 필요 없으면 지워도 됩니다.
> 외부 도구로 써도 됩니다(추천: [Notion](https://www.notion.so)). 그 경우 이 파일에는 링크와 한 줄 요약만 남깁니다.

## 1. Git 전략
- 저장소에는 main 보호(PR + 승인 1명)가 기본으로 걸려 있습니다. 팀에 맞게 바꿔도 됩니다
- 아래는 GitHub Flow를 쓸 때의 예입니다

| 항목 | 정할 것 | 예 |
|---|---|---|
| 브랜치 이름 | 이름 짓는 방식 | `feat/12-seat-list`, `fix/15-booking-409` |
| 커밋 메시지 | [Conventional Commits](https://www.conventionalcommits.org) | `feat: 좌석 조회 API (#12)` |
| PR 크기 | PR 하나에 담을 범위 | 이슈 하나에 PR 하나 |
| 리뷰 | 누가 몇 명 승인하나 | 1명 승인, 남의 서비스를 고치면 그 담당자 |
| merge 방식 | Squash / Merge / Rebase 중 무엇 | Squash merge |
| 충돌 | 해결하는 방법 | main을 내 브랜치에 먼저 합침, `git pull origin main` |
| prod 배포 | 어떤 버전을 올리나 | 태그 붙인 버전만, `v1.0.0` |

### 작업 순서 (예)
1. 이슈를 만든다 — [일감 양식](../.github/ISSUE_TEMPLATE/task.yml), 근거 요구사항과 완료 조건을 적음
2. Projects 보드에서 진행 중으로 옮긴다
3. 브랜치를 만든다 — `feat/<이슈번호>-<내용>`
4. PR을 연다 — 본문에 `Closes #<이슈번호>`
5. 리뷰 승인 1명 · CI 통과 후 Squash merge → 이슈가 자동으로 닫히고 보드에서 완료로 이동

## 2. 코드 · 서비스 공통
| 항목 | 규칙 |
|---|---|
| 기술 스택 | [기술 스택](tech-stack.md) |
| 폴더 구조 | `services/<서비스>/` 아래 `src` · `test` · `Dockerfile` |
| 공통 API | `/health`, 포트 <3000> |
| 환경변수 이름 | <`DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`> |
| 에러 · 로그 형식 | [API 명세 공통 규칙](api/README.md#공통-규칙), [관측 설계](observability.md#3-로그-형식-모든-서비스-같게) |
| 작업 범위 | 내 서비스 폴더만 고침. 새 패키지는 PR에 이유 |

## 3. 비밀값 · 환경
- 비밀번호 · 토큰 · 키는 Git에 넣지 않습니다. 형식만 [.env.example](../.env.example)에
- 실제 값은 Kubernetes Secret으로. 만드는 사람: <이름>
- 자원(requests/limits), 클러스터 관리자: <정해서 적기>
