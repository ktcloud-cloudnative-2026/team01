# 공통 규칙

[← README](../README.md)

## 1. Git 전략 — GitHub Flow
- `main`은 항상 배포할 수 있는 상태. 직접 push 금지
- 보호 규칙: PR 필수 · 승인 1명 · CI 통과

| 항목 | 규칙 | 예 |
|---|---|---|
| 브랜치 이름 | 종류/이슈번호-내용 | `feat/12-seat-list`, `fix/15-booking-409` |
| 커밋 메시지 | [Conventional Commits](https://www.conventionalcommits.org) | `feat: 좌석 조회 API (#12)` |
| PR 크기 | 이슈 하나에 PR 하나 | |
| 리뷰 | 1명 승인. 남의 서비스를 고치면 그 담당자가 승인 | |
| merge 방식 | Squash merge | |
| 충돌 | main을 내 브랜치에 먼저 합치고 해결 | `git pull origin main` |
| prod 배포 | 태그를 붙인 버전만 | `v1.0.0` |

### 작업 순서
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
- 비밀번호 · 토큰 · 키는 Git에 넣지 않음. 형식만 [.env.example](../.env.example)에
- 실제 값은 Kubernetes Secret으로. 만드는 사람: <이름>
- 자원(requests/limits), 클러스터 관리자: <정해서 적기>
