# Design — Mega PRD v1.2 콕핏 구현

원천: docs/01-plan/plan-v1.2.md · docs/prd/mega-prd-v1.2.md

## 1. 아키텍처 결정

- 기존 정적 단일 파일(`index.html`) 패턴 유지. 신규 기능은 **독립 `<script>` 모듈**로 추가해 기존 탭 코드에 영향 없음.
- 샘플 데이터는 스크립트 내 JS 상수(`PEOPLE`, `INTROS`, `IMPORTS`, `WORKOUTS`)로 보관 — `file://`·CORS 제약 없이 검색·필터·상태변경 동작 보장.
- 상태 변경(팔로업 토글, 연결 추천 처리, 선택 삭제, 인라인 메모)은 in-memory + 필요한 곳만 localStorage. 정적 프로토타입 원칙에 부합.

## 2. 화면 → 구현 매핑

### 인맥 탭 (`#tab-people`) — 하위 4화면을 서브내비로 전환
- **S-001 인물 목록**: 검색 input(이름·회사·needs/offers 부분일치) + 필터 select(기수/청강자/재접촉필요). 카드 클릭 → S-002.
- **S-002 인물 상세**: 리마인드 블록(마지막 만남·나눈 얘기·Needs·Offers) + 타임라인(시간순) + `[+ 한 줄 메모]` 인라인 입력(날짜·장소·메모 → 타임라인 prepend) + 팔로업 상태 토글(대기/완료).
- **S-005 연결 추천**: needs/offers 매칭 쌍 목록. `[소개했음]`→양쪽 타임라인에 "소개됨" + 목록 제외, `[보류]`→하단 이동, `[관심없음]`→재추천 안 함(dismiss set).
- **S-006 일괄 업로드 결과**: `IMPORTS`(통화·명함 원본) 전량 렌더 — (Unknown)·배달콜 추정 포함. 검색 + 체크박스 선택 → `[선택 삭제]`. 자동 필터/숨김 없음. 상단 배너: "AI는 판단하지 않습니다 — 전량 업로드, 정리는 직접."

### 운동 탭 (`#tab-workout`)
- **S-EX-001 운동 홈**: 이번 주/이번 달/연속일수 stat 3개 + 최근 활동 리스트(최신순).
- **S-EX-002 전체 이력**: 기간 필터 select(전체/이번달/최근7일) + 표(날짜·종류·거리·시간·페이스).

### 홈·오늘 재설계 (11.3)
기존 상단 grid-4 유지 + 하단에 **신호 6종** 중 신규 2카드 추가:
- 재접촉 필요 인원: `PEOPLE`에서 팔로업 pending 또는 last_contact 오래된 인원 수.
- 이번 주 운동: `WORKOUTS` 이번 주 건수.
두 카드는 인맥/운동 탭으로 바로 이동.

### 뉴스 "내 생각" (11.8)
수집 칸 각 `pipe-card`에 한 줄 input 추가. `localStorage['thought:<newsId>']`로 저장·복원.

### 시스템 탭 (11.7)
소스 상태표에 Supabase(10테이블)·Drive/SMS·리멤버 CSV·Strava·Vercel 행 반영.

## 3. 데이터 모델 (샘플 상수, PRD CRM-08 스키마 준수)

```
PEOPLE[]  : {id,name,company,title,cohort,contact,needs,offers,source,firstMet,lastContact,followup,timeline[]}
INTROS[]  : {id,aId,bId,reason,status}          // suggested|done|dismissed
IMPORTS[] : {id,kind,name,phone,note,count}     // kind: call|card ; (Unknown)·스팸 포함
WORKOUTS[]: {id,date,type,distanceKm,durationSec}
```

## 4. 스타일

기존 CSS 토큰·클래스(`stat-tile`,`card`,`badge`,`simple`,`pipe-card`,`plain`) 재사용. 신규 클래스 최소화(서브내비·리마인드 블록만).

## 5. 무결성 수정

`index.html` LEC_DETAILS 객체 `lec-15` 항목 뒤 콤마 누락 → 전체 `<script>` 파싱 실패. 콤마 추가로 복구(회귀: 탭 전환·PRD 뷰어·강의 상세 정상화).
