# API 명세

[← 기술 스택](../tech-stack.md) · [README](../../README.md)

> 채울 항목의 예입니다. 팀 프로젝트에 맞게 항목을 더하거나 빼도 됩니다.
>
> 외부 도구로 써도 됩니다(추천: [Swagger Editor](https://editor.swagger.io), [Postman](https://www.postman.com)). 그 경우 이 파일에는 링크와 한 줄 요약만 남깁니다.

- 서비스마다 OpenAPI 파일 하나: `docs/api/<서비스>.yaml` — 예시는 [example-service.yaml](example-service.yaml)
- 편집 · 미리 보기: [Swagger Editor](https://editor.swagger.io)에 붙여 넣기
- **개발 전에 확정**합니다. 바꿀 때는 PR로 바꾸고, 그 API를 쓰는 서비스 담당자가 승인합니다
- 엔드포인트마다 관련 요구사항 번호(`US-NN`)를 `description`에 적습니다

| 서비스 | 파일 | 담당 |
|---|---|---|
| <서비스 A> | [a.yaml](a.yaml) | <이름> |

## 공통 규칙
- 에러 응답: `{"error": "메시지"}`, 내부 주소 · 경로는 응답에 넣지 않음
- 날짜: ISO 8601(`2026-11-04T09:00:00+09:00`)
- ID: 숫자
