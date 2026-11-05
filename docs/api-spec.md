# Home IoT 시스템 API 명세서

버전 v0.1 (초안) \| 작성일 2026-11-05 \| 프로젝트: team01-VisionUs

## 1. 개요

가정 전자기기 IoT 연동 시스템의 REST API 명세입니다. 클라이언트(Next.js 대시보드)는 REST API로 기기를 조회·제어하고, 백엔드는 MQTT 브로커를 통해 기기 시뮬레이터와 통신합니다.

| 항목 | 내용 |
|---------------|---------------------------------------------------------|
| Base URL      | /api/v1                                                 |
| 프로토콜      | HTTPS, JSON (Content-Type: application/json)            |
| 인증          | JWT Bearer 토큰 (Authorization: Bearer \<accessToken\>) |
| 백엔드        | Node.js (Express) 마이크로서비스                        |
| 데이터 저장소 | PostgreSQL                                              |
| 기기 통신     | MQTT (백엔드 ↔ 가전 기기 시뮬레이터)                    |
| 시간 형식     | ISO 8601 (UTC), 예: 2026-11-05T09:30:00Z                |

## 2. 공통 규약

### 2.1 요청 헤더

```http
Authorization: Bearer <accessToken>
Content-Type: application/json
```

### 2.2 공통 응답 형식

성공 응답:

```json
{
  "success": true,
  "data": { ... }
}
```

실패 응답:

```json
{
  "success": false,
  "error": {
    "code": "DEVICE_NOT_FOUND",
    "message": "기기를 찾을 수 없습니다."
  }
}
```

### 2.3 HTTP 상태 코드 및 에러 코드

| 상태 | 에러 코드 | 설명 |
|----------|-----------------------------------|---------------------------------------|
| 400      | INVALID_REQUEST                   | 요청 본문 또는 파라미터 형식 오류     |
| 401      | UNAUTHORIZED / TOKEN_EXPIRED      | 토큰 없음, 만료 또는 유효하지 않음    |
| 403      | FORBIDDEN                         | 해당 리소스에 대한 권한 없음          |
| 404      | DEVICE_NOT_FOUND                  | 존재하지 않는 기기 또는 리소스        |
| 409      | DEVICE_OFFLINE / EMAIL_DUPLICATED | 기기 오프라인 상태 또는 중복 데이터   |
| 422      | UNSUPPORTED_COMMAND               | 해당 기기에서 지원하지 않는 제어 명령 |
| 500      | INTERNAL_ERROR                    | 서버 내부 오류                        |
| 504      | DEVICE_TIMEOUT                    | 기기로부터 MQTT 응답 없음             |

## 3. API 목록

### 3.1 인증 (Auth)

| 요구사항 번호 | Method | Path | 인증 | 설명 |
|-------------------|------------|---------------|--------------|-----------------------------------|
| US-01             | POST       | /auth/signup  | 불필요       | 회원가입                          |
| US-02             | POST       | /auth/login   | 불필요       | 로그인 (Access/Refresh 토큰 발급) |
| \-                | POST       | /auth/refresh | Refresh 토큰 | Access 토큰 재발급                |
| \-                | POST       | /auth/logout  | 필요         | 로그아웃 (Refresh 토큰 폐기)      |

### 3.2 사용자 (Users)

| 요구사항 번호 | Method | Path | 인증 | 설명 |
|-------------------|------------|-----------|----------|--------------|
| \-                | GET        | /users/me | 필요     | 내 정보 조회 |

### 3.3 기기 (Devices)

| 요구사항 번호 | Method | Path | 인증 | 설명 |
|-------------------|------------|----------------------------|----------|------------------------------------|
| US-04             | GET        | /devices                   | 필요     | 기기 목록 조회 (type, status 필터) |
| US-05             | GET        | /devices/{deviceId}        | 필요     | 기기 상세 조회                     |
| US-05             | GET        | /devices/{deviceId}/state  | 필요     | 기기 현재 상태 조회                |
| US-14             | GET        | /devices/{deviceId}/events | 필요     | 기기 이벤트·제어 이력 조회         |

### 3.4 기기 제어 (Commands)

| 요구사항 번호 | Method | Path | 인증 | 설명 |
|-------------------|------------|------------------------------|----------|---------------------|
| US-07 ~ US-11     | POST       | /devices/{deviceId}/commands | 필요     | 기기 제어 명령 전송 |

### 3.5 엘리베이터 호출 (Elevator)

| 요구사항 번호 | Method | Path | 인증 | 설명 |
|-------------------|------------|--------------------------|----------|-----------------|
| US-12             | POST       | /elevator/calls          | 필요     | 엘리베이터 호출 |
| US-12             | GET        | /elevator/calls/{callId} | 필요     | 호출 상태 조회  |

## 4. API 상세

### 4.1 POST /auth/signup

Request Body

| 필드 | 타입 | 필수 | 설명 |
|----------|----------|----------|---------------------------|
| email    | string   | Y        | 로그인 이메일 (중복 불가) |
| password | string   | Y        | 8자 이상                  |
| name     | string   | Y        | 사용자 이름               |

Response 201

```json
{
  "success": true,
  "data": {
    "userId": 1,
    "email": "user@example.com",
    "name": "홍길동"
  }
}
```

### 4.2 POST /auth/login

Request Body

```json
{
  "email": "user@example.com",
  "password": "********"
}
```

Response 200

```json
{
  "success": true,
  "data": {
    "accessToken": "<JWT>",
    "refreshToken": "<JWT>",
    "expiresIn": 3600
  }
}
```

### 4.3 GET /devices

Query Parameters

| 파라미터 | 타입 | 필수 | 설명 |
|--------------|----------|----------|--------------------------------------------------|
| type         | string   | N        | light \| aircon \| heater \| purifier \| curtain |
| status       | string   | N        | online \| offline                                |

Response 200

```json
{
  "success": true,
  "data": [
    {
      "deviceId": "light-living-01",
      "name": "거실 조명",
      "type": "light",
      "status": "online"
    }
  ]
}
```

### 4.4 GET /devices/{deviceId}/state

Response 200 (조명 예시)

```json
{
  "success": true,
  "data": {
    "deviceId": "light-living-01",
    "type": "light",
    "power": "on",
    "state": {
      "brightness": 70
    },
    "updatedAt": "2026-11-05T09:30:00Z"
  }
}
```

### 4.5 POST /devices/{deviceId}/commands

기기 유형에 따라 command와 params가 달라집니다. 백엔드는 명령을 MQTT 토픽으로 발행하고, 기기의 상태 응답을 받아 결과를 반환합니다.

Request Body

```json
{
  "command": "SET_POWER",
  "params": {
    "power": "on"
  }
}
```

기기 유형별 지원 명령

| 기기 유형 | command | params |
|-----------------------|----------------------------------------|-------------------------------------------------|
| light (조명)          | SET_POWER / SET_BRIGHTNESS             | power: on\|off, brightness: 0~100               |
| aircon (에어컨)       | SET_POWER / SET_TEMPERATURE / SET_MODE | power, temperature: 16~30, mode: cool\|dry\|fan |
| heater (난방기기)     | SET_POWER / SET_TEMPERATURE            | power, temperature: 15~30                       |
| purifier (공기청정기) | SET_POWER / SET_FAN_SPEED              | power, fanSpeed: low\|mid\|high\|auto           |
| curtain (커튼)        | OPEN / CLOSE / SET_POSITION            | position: 0~100 (0=닫힘, 100=열림)              |

Response 200

```json
{
  "success": true,
  "data": {
    "commandId": "cmd-20261105-0001",
    "deviceId": "light-living-01",
    "status": "completed",
    "state": {
      "power": "on",
      "brightness": 70
    }
  }
}
```

### 4.6 POST /elevator/calls

Request Body

| 필드 | 타입 | 필수 | 설명 |
|-----------|----------|----------|------------|
| floor     | integer  | Y        | 호출 층    |
| direction | string   | N        | up \| down |

Response 202

```json
{
  "success": true,
  "data": {
    "callId": "call-0001",
    "status": "requested",
    "requestedAt": "2026-11-05T09:30:00Z"
  }
}
```

status 값: requested → arriving → arrived \| failed

## 5. MQTT 토픽 규약

백엔드와 가전 기기 시뮬레이터 간 통신에 사용합니다.

| 토픽 | 방향 | 설명 |
|------------------------------------------|---------------|---------------------------------|
| home/{homeId}/devices/{deviceId}/command | 백엔드 → 기기 | 제어 명령 발행                  |
| home/{homeId}/devices/{deviceId}/state   | 기기 → 백엔드 | 상태 변경 및 명령 결과 보고     |
| home/{homeId}/devices/{deviceId}/status  | 기기 → 백엔드 | 온라인/오프라인 상태 (LWT 활용) |
| home/{homeId}/elevator/call              | 백엔드 → 기기 | 엘리베이터 호출 요청            |
| home/{homeId}/elevator/status            | 기기 → 백엔드 | 엘리베이터 호출 진행 상태       |

명령 페이로드 예시

```json
{
  "commandId": "cmd-20261105-0001",
  "command": "SET_BRIGHTNESS",
  "params": {
    "brightness": 70
  }
}
```

## 6. 참고 사항

- Access 토큰 만료 시간, 비밀번호 정책, 호출 타임아웃 값 등은 팀 합의 후 확정합니다.
