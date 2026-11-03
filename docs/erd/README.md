# ERD

[← README](../../README.md)

- 서비스마다 자기 DB를 가집니다. 다른 서비스의 DB는 직접 읽지 않고 API를 호출합니다
- [dbdiagram.io](https://dbdiagram.io)의 DBML 또는 아래처럼 Mermaid로 씁니다

## <서비스 A>
```mermaid
erDiagram
  ITEM {
    int id PK
    string status
    datetime created_at
  }
```

## DB 테이블 변경 · 시드 데이터
| 항목 | 방법 |
|---|---|
| 테이블 변경 | <서비스 시작 때 생성 / migrations 스크립트> |
| 시드 데이터 | <scripts/seed 실행 방법> |
