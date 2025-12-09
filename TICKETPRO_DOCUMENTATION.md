# TicketPro 프로젝트 문서

## 3줄 요약

1. **TicketPro는 실제 티켓팅 환경을 시뮬레이션하는 웹 기반 연습 플랫폼**으로, 콘서트/굿즈/식당 예약을 난이도별로 연습할 수 있습니다.
2. **Supabase(실시간 DB) + Gemini AI(피드백 분석)** 기술을 활용하여 사용자 기록 저장, 랭킹 시스템, AI 코칭을 제공합니다.
3. **정밀한 타이머(ms 단위) + 대기열 시뮬레이션 + 보안문자**로 실제 티켓팅과 동일한 긴장감을 재현합니다.

---

## 목차

1. [프로젝트 개요](#1-프로젝트-개요)
2. [기술 스택](#2-기술-스택)
3. [프로젝트 구조](#3-프로젝트-구조)
4. [핵심 기능 상세](#4-핵심-기능-상세)
5. [데이터베이스 구조](#5-데이터베이스-구조)
6. [API 연동 방식](#6-api-연동-방식)
7. [페이지별 기능 설명](#7-페이지별-기능-설명)
8. [추가된 기능 목록](#8-추가된-기능-목록)

---

## 1. 프로젝트 개요

### 1.1 프로젝트 소개

**TicketPro**는 실제 티켓 예매 환경을 시뮬레이션하여 사용자가 티켓팅 스킬을 연습할 수 있는 웹 애플리케이션입니다.

### 1.2 주요 목표

- 실제 티켓팅과 동일한 타이밍 연습
- 난이도별 단계적 학습
- AI 기반 개인 맞춤 피드백
- 경쟁을 통한 동기 부여 (랭킹 시스템)

### 1.3 모듈 구성

| 모듈 | 경로 | 설명 |
|------|------|------|
| **Folder A** | `/a/` | AI 채팅 시스템 + 메인 랜딩 페이지 |
| **Folder B** | `/b/` | 멜론티켓 클론 (결제 UI) |
| **Folder C** | `/c/test_ticket/` | TicketPro 메인 (티케팅 시뮬레이터) |

---

## 2. 기술 스택

### 2.1 프론트엔드

| 기술 | 용도 | 버전 |
|------|------|------|
| **HTML5** | 마크업 | - |
| **CSS3** | 스타일링 (Grid, Flexbox, 애니메이션) | - |
| **Vanilla JavaScript** | 게임 로직, API 연동 | ES6+ |
| **Google Fonts** | 웹폰트 (Inter, Noto Sans KR) | - |

### 2.2 백엔드 서비스

| 서비스 | 용도 | 특징 |
|------|------|------|
| **Supabase** | 실시간 데이터베이스 | PostgreSQL 기반, 실시간 구독 지원 |
| **Gemini API** | AI 피드백 분석 | gemini-2.5-flash 모델 사용 |

### 2.3 상태 관리

| 저장소 | 용도 | 지속성 |
|--------|------|--------|
| **sessionStorage** | 로그인 정보 (userId, nickname) | 탭 닫으면 삭제 |
| **localStorage** | 게임 데이터, 설정 | 영구 저장 |
| **URL Parameters** | 페이지 간 데이터 전달 | 일회성 |

---

## 3. 프로젝트 구조

```
ticketpro-main/
├── a/                              # AI 채팅 + 메인 페이지
│   ├── index.html                 # 메인 랜딩 페이지 (3D 카드 UI)
│   └── chat.html                  # AI 채팅 (Gemini 2.5 Flash)
│
├── b/                              # 멜론티켓 클론
│   └── payment.html               # 결제 페이지 UI
│
├── c/test_ticket/                  # TicketPro 메인
│   ├── main-pages/                # 메인 페이지들
│   │   ├── login.html            # 로그인 (닉네임 입력)
│   │   ├── choice.html           # 카테고리 선택
│   │   ├── ranking.html          # 순위표
│   │   ├── chat.html             # 실시간 채팅
│   │   └── ai-feedback.html      # AI 피드백
│   │
│   ├── concert-pages/             # 콘서트 티켓팅
│   │   ├── hall-choice.html      # 공연장 선택
│   │   ├── concert-level.html    # 난이도 선택
│   │   ├── concert-timer.html    # 타이머 게임
│   │   ├── ticketingMain.html    # 공연 정보 + 날짜 선택
│   │   └── yes24hall.html        # 좌석 선택 + 보안문자
│   │
│   ├── goods-pages/               # 굿즈 구매
│   │   ├── goods-choice.html     # 굿즈 종류 선택
│   │   ├── goods-level.html      # 난이도 선택
│   │   ├── goods-timer.html      # 타이머 + 미션 생성
│   │   ├── goods-album.html      # 앨범 구매
│   │   ├── goods-photo.html      # 포토카드 구매
│   │   └── goods-clothes.html    # 의상 구매
│   │
│   ├── restaurant-pages/          # 식당 예약
│   │   ├── restaurant-level.html # 난이도 선택
│   │   └── restaurant-timer.html # 타이머
│   │
│   ├── js/
│   │   └── supabase-config.js    # Supabase 설정 + API 함수
│   │
│   ├── common/
│   │   └── theme.css             # 공통 스타일
│   │
│   └── image/                     # 이미지 리소스
│       ├── concert/              # 아티스트 이미지
│       ├── goods/                # 굿즈 이미지
│       └── restaurant/           # 식당 이미지
│
└── TICKETPRO_DOCUMENTATION.md     # 이 문서
```

---

## 4. 핵심 기능 상세

### 4.1 사용자 인증 시스템

**작동 방식:**
1. 사용자가 닉네임 입력
2. Supabase RPC `check_nickname_exists` 호출하여 중복 체크
3. `login_or_create` 호출하여 로그인/회원가입 처리
4. `sessionStorage`에 userId, nickname 저장
5. 로그인 상태 유지 (탭 닫기 전까지)

```javascript
// 로그인 함수
async function loginOrCreate(nickname) {
    const { data, error } = await supabase
        .rpc('login_or_create', { input_nickname: nickname });

    sessionStorage.setItem('userId', data);
    sessionStorage.setItem('nickname', nickname);
    return data;
}
```

### 4.2 타이머 게임 엔진

**정밀 타이머 구현:**
- `performance.now()` 사용 (ms 단위 정밀도)
- `requestAnimationFrame` 으로 60fps 업데이트
- 드리프트 없는 정확한 시간 측정

**난이도별 설정:**

| 난이도 | 타이머 | 성공 윈도우 | 대기열 기본 | 대기열 증가율 |
|--------|--------|-----------|------------|--------------|
| 쉬움 | 10초 | 150ms | 50명 | 50명/초 |
| 보통 | 5초 | 80ms | 83명 | 150명/초 |
| 어려움 | 5초 | 40ms | 120명 | 300명/초 |

```javascript
function startTimer() {
    state.startTime = performance.now();

    function tick() {
        const elapsed = performance.now() - state.startTime;
        state.remaining = Math.max(0, config.duration - elapsed);

        updateDisplay((state.remaining / 1000).toFixed(2));

        if (state.remaining > 0) {
            requestAnimationFrame(tick);
        }
    }
    requestAnimationFrame(tick);
}
```

### 4.3 대기열 시뮬레이션

**계산 공식:**
```
초기 대기열 = 기본값 + (증가율 × 초과 시간)
```

**애니메이션:**
- 6.5초에 걸쳐 0으로 감소
- 프로그레스 바와 함께 표시
- 완료 후 다음 페이지로 이동

### 4.4 좌석 선택 시스템

**공연장 좌석 배치:**
- A~H열, 총 18~30석
- 좌측/중앙/우측 구역 분리
- 통로 표시

**좌석 상태:**
- 🟢 녹색: 선택 가능
- 🔴 빨간색: 선택됨
- ⚫ 회색: 매진

### 4.5 보안문자 (CAPTCHA)

**구현:**
- 4자리 영숫자 랜덤 생성
- 사용자 입력 검증
- 틀리면 새로운 문자 생성

### 4.6 AI 피드백 시스템

**Gemini API 연동:**
```javascript
const API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent';

const response = await fetch(API_URL, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-goog-api-key': API_KEY
    },
    body: JSON.stringify({
        contents: [{ parts: [{ text: prompt }] }]
    })
});
```

**분석 항목:**
- 종합 점수 (0-100점)
- 종합 평가
- 잘한 점 (3가지)
- 개선할 점 (2가지)
- 실전 팁
- 다음 목표

### 4.7 순위표 시스템

**필터링 옵션:**
- 카테고리: 전체, 콘서트, 굿즈, 식당
- 난이도: 전체, 쉬움, 보통, 어려움

**표시 정보:**
- 순위 (1~3위: 🥇🥈🥉 메달)
- 닉네임
- 난이도 배지 (색상 구분)
- 기록 시간
- 기록 날짜

### 4.8 실시간 채팅

**Supabase Realtime 활용:**
```javascript
const channel = supabase
    .channel(`chat-room-${roomId}`)
    .on('postgres_changes', {
        event: 'INSERT',
        schema: 'public',
        table: 'chat_messages',
        filter: `room_id=eq.${roomId}`
    }, (payload) => {
        onMessage(payload.new);
    })
    .subscribe();
```

### 4.9 굿즈 미션 시스템

**난이도별 미션:**

| 난이도 | 미션 내용 |
|--------|---------|
| 쉬움 | 아무 상품 1개 구매 |
| 보통 | 특정 버전 1-2개 구매 |
| 어려움 | 서로 다른 버전 2개 각 1개씩 |

---

## 5. 데이터베이스 구조

### 5.1 Supabase 테이블

**users (사용자)**
| 컬럼 | 타입 | 설명 |
|------|------|------|
| user_id | UUID | 기본키 |
| nickname | VARCHAR | 닉네임 (고유) |
| created_at | TIMESTAMP | 가입일 |

**booking_records (예매 기록)**
| 컬럼 | 타입 | 설명 |
|------|------|------|
| id | UUID | 기본키 |
| user_id | UUID | 사용자 ID |
| category | VARCHAR | 카테고리 (콘서트/굿즈/식당) |
| difficulty | VARCHAR | 난이도 (쉬움/보통/어려움) |
| elapsed_time | INTEGER | 소요 시간 (ms) |
| selection_data | JSON | 추가 데이터 |
| created_at | TIMESTAMP | 기록 시간 |

**chat_rooms (채팅방)**
| 컬럼 | 타입 | 설명 |
|------|------|------|
| id | UUID | 기본키 |
| name | VARCHAR | 채팅방 이름 |
| is_active | BOOLEAN | 활성화 상태 |

**chat_messages (채팅 메시지)**
| 컬럼 | 타입 | 설명 |
|------|------|------|
| id | UUID | 기본키 |
| room_id | UUID | 채팅방 ID |
| user_id | UUID | 사용자 ID |
| nickname | VARCHAR | 닉네임 |
| message | TEXT | 메시지 내용 |
| created_at | TIMESTAMP | 전송 시간 |

### 5.2 RPC 함수

| 함수명 | 파라미터 | 반환값 | 설명 |
|--------|---------|--------|------|
| `check_nickname_exists` | nickname | boolean | 닉네임 중복 체크 |
| `login_or_create` | nickname | user_id | 로그인/회원가입 |
| `get_ranking` | category, difficulty, limit | array | 랭킹 조회 |
| `get_overall_ranking` | limit | array | 전체 랭킹 |

---

## 6. API 연동 방식

### 6.1 Supabase 연동

**초기화:**
```javascript
const SUPABASE_URL = 'https://klcceivyqgqbpjdwlnvp.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGci...';
const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
```

**CRUD 패턴:**
```javascript
// SELECT
const { data, error } = await supabase
    .from('booking_records')
    .select('*')
    .eq('user_id', userId);

// INSERT
await supabase
    .from('booking_records')
    .insert({ user_id, category, difficulty, elapsed_time });

// UPDATE (Upsert 로직)
const { data: existing } = await supabase
    .from('booking_records')
    .select('id')
    .eq('user_id', userId)
    .eq('category', category)
    .eq('difficulty', difficulty)
    .single();

if (existing) {
    await supabase.from('booking_records')
        .update({ elapsed_time, created_at: new Date() })
        .eq('id', existing.id);
} else {
    await supabase.from('booking_records')
        .insert({ user_id, category, difficulty, elapsed_time });
}
```

### 6.2 Gemini AI 연동

**API 설정:**
```javascript
const API_KEY = 'AIzaSyC26pMkzOZD1AtfZOQf4-5jkezrgW6yW98';
const API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent';
```

**요청 형식:**
```javascript
const response = await fetch(API_URL, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-goog-api-key': API_KEY
    },
    body: JSON.stringify({
        contents: [{
            parts: [{ text: prompt }]
        }],
        generationConfig: {
            temperature: 0.7,
            maxOutputTokens: 2048
        }
    })
});
```

**할당량 (무료 티어):**
| 항목 | 한도 |
|------|------|
| 분당 요청 | 10개 |
| 분당 토큰 | 250,000 |
| 일일 요청 | 250개 |

---

## 7. 페이지별 기능 설명

### 7.1 메인 페이지 (a/index.html)

- 3D 카드 레이아웃 UI
- Spline 3D 배경 애니메이션
- TODAY 플레이 통계 (실시간 DB 연동)
- 카테고리 소개 카드

### 7.2 AI 채팅 (a/chat.html)

- Gemini 2.5 Flash 기반 대화
- 다양한 페르소나 선택 가능
- 티켓팅 관련 질문/답변

### 7.3 로그인 (login.html)

- 닉네임 입력
- 중복 체크
- Supabase 연동 회원가입/로그인

### 7.4 카테고리 선택 (choice.html)

- 콘서트/굿즈/식당 선택
- 순위표/채팅 바로가기
- 로그인 상태 표시

### 7.5 콘서트 플로우

1. **hall-choice.html**: 공연장 선택 (YES24 라이브홀)
2. **concert-level.html**: 난이도 선택 (쉬움/보통/어려움)
3. **concert-timer.html**: 타이머 게임 (정확한 타이밍에 클릭)
4. **ticketingMain.html**: 공연 정보 확인 + 날짜 선택
5. **yes24hall.html**: 좌석 선택 + 보안문자 → 성공/실패

### 7.6 굿즈 플로우

1. **goods-choice.html**: 굿즈 종류 선택 (앨범/포토카드/의상)
2. **goods-level.html**: 난이도 선택
3. **goods-timer.html**: 타이머 + 미션 생성
4. **goods-album/photo/clothes.html**: 구매 시뮬레이션

### 7.7 순위표 (ranking.html)

- 카테고리/난이도 필터
- 난이도 배지 (색상 구분)
- 메달 표시 (1~3위)
- 내 기록 강조

### 7.8 AI 피드백 (ai-feedback.html)

- 게임 결과 분석
- Gemini AI 코칭
- 점수/평가/개선점 표시

### 7.9 실시간 채팅 (chat.html)

- Supabase Realtime 채팅
- 채팅방 선택
- 실시간 메시지 수신

---

## 8. 추가된 기능 목록

### 8.1 이번 세션에서 추가/수정된 기능

| 기능 | 파일 | 설명 |
|------|------|------|
| **난이도 배지** | ranking.html | 순위표에 쉬움(초록)/보통(노랑)/어려움(빨강) 배지 표시 |
| **기록 저장 버튼 강조** | goods-album.html, goods-clothes.html, yes24hall.html | 노란색 그라데이션 + 펄스 애니메이션 |
| **저장 경고 문구** | 성공 모달 | "지금 저장하지 않으면 기록이 저장되지 않습니다!" |
| **순위표 바로가기** | 성공 모달 | 보라색 버튼 추가 |
| **AI 피드백 저장 버튼 제거** | ai-feedback.html | 중복 제거 (성공 모달에만 존재) |
| **Gemini 모델 업그레이드** | ai-feedback.html, chat.html | 2.0-flash → 2.5-flash |
| **랭킹 필터 수정** | ranking.html | 영어 → 한글 (콘서트/굿즈/식당) |
| **기록 업데이트 로직** | supabase-config.js | 같은 사용자+카테고리+난이도면 덮어쓰기 |
| **TODAY 플레이 통계** | a/index.html | 실시간 DB에서 오늘 플레이 수 표시 |

### 8.2 UI/UX 개선사항

- 기록 저장 버튼: 밝은 노란색 그라데이션으로 시각적 강조
- 펄스 애니메이션으로 주목도 향상
- 빨간색 경고 문구로 저장 필요성 안내
- 순위표 바로가기 버튼으로 편의성 향상

### 8.3 기술적 개선사항

- Gemini 2.5 Flash로 업그레이드 (더 나은 응답 품질)
- Upsert 로직으로 중복 기록 방지
- 한글 카테고리/난이도 통일로 필터 오류 해결

---

## 부록: 주요 코드 스니펫

### A. 타이머 구현
```javascript
function tick() {
    const elapsed = performance.now() - state.startTime;
    state.remaining = Math.max(0, config.duration - elapsed);
    display.textContent = (state.remaining / 1000).toFixed(2);
    if (state.remaining > 0) requestAnimationFrame(tick);
}
```

### B. 기록 저장 (Upsert)
```javascript
async function saveBookingRecord(category, difficulty, elapsedTime) {
    const { data: existing } = await supabase
        .from('booking_records')
        .select('id')
        .eq('user_id', userId)
        .eq('category', category)
        .eq('difficulty', difficulty)
        .single();

    if (existing) {
        await supabase.from('booking_records')
            .update({ elapsed_time: elapsedTime, created_at: new Date() })
            .eq('id', existing.id);
    } else {
        await supabase.from('booking_records')
            .insert({ user_id: userId, category, difficulty, elapsed_time: elapsedTime });
    }
}
```

### C. 실시간 채팅 구독
```javascript
const channel = supabase
    .channel(`chat-room-${roomId}`)
    .on('postgres_changes', {
        event: 'INSERT',
        schema: 'public',
        table: 'chat_messages',
        filter: `room_id=eq.${roomId}`
    }, (payload) => onMessage(payload.new))
    .subscribe();
```

---

## 작성 정보

- **작성일**: 2025년 12월 9일
- **프로젝트**: TicketPro 티켓팅 시뮬레이터
- **GitHub**: https://github.com/tjandud/smy0211
- **배포 URL**: https://tjandud.github.io/smy0211/
