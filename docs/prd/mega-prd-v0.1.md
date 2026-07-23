# Mega PRD v0.1 — AI Native Company 통합 업무 시스템

| 항목 | 내용 |
| --- | --- |
| 문서 버전 | v0.1 |
| 작성일 | 2026-07-23 |
| 제품 책임자 | 장동인 교수 (AIBB LAB 대표, KAIST 김재철AI대학원 책임교수) |
| 목적 | 장동인 교수의 전체 업무를 AI Native 방식으로 통합 운영하는 시스템의 최상위 설계 문서 |
| 상태 | 초안 — 콕핏 v0.5 목업과 함께 검증 중 |

## 1. 비전

사람은 결정하고, AI는 실행한다. 강의·집필·콘텐츠·과정 운영·정산에 이르는 전체 업무를
하나의 콕핏에서 보고, Claude 엔진이 데이터 수집·상태 판단·실행을 담당하는
**개인 단위 AI Native Company**를 구축한다.

이 시스템 자체가 저서 《CEO와 경영진을 위한 AI Native — 기업의 AX를 위한 바이브 코딩》과
AI4CEO 스쿨이 가르치는 방법론의 살아있는 증거물(living proof)이 된다.

## 2. 범위 — 10개 프로세스

| # | 프로세스 | 설명 | 데이터 원천 |
| --- | --- | --- | --- |
| 1 | ai4ceo | AI4CEO Portal 제품 개발 (하위 PRD 별도) | github.com/donchang07/ai4ceo-portal |
| 2 | 스케줄 관리 | 일정·강의 일정 통합 | Google Calendar |
| 3 | AI 뉴스·공부·정리·SNS 포스팅 | 수집→정리→초안→발행 파이프라인 | 뉴스 소스, LinkedIn 등 |
| 4 | 책쓰기 | 원고 집필·버전 관리·책→강의 변환 | Google Drive (글쓰기 폴더) |
| 5 | 강의 준비 | 문의→확정→자료 준비 | Calendar, Gmail, data/lectures.json |
| 6 | 강의 후 팔로업 | 만난 사람 관리, 48시간 내 첫 메일 | Gmail, data/followups.json |
| 7 | 강사료 정산 서류 전달 | 서류 발송 추적 | Gmail, data/lectures.json |
| 8 | 강사료 입금 확인 | 입금 대기·지연 알림 (발송 후 14일 규칙) | Gmail, data/lectures.json |
| 9 | CAIO-LMS | 기수 운영 (수강생·과제·출석) | 방식 확정 필요 |
| 10 | CAIO-Alumni | 졸업생 네트워크·뉴스레터·재강의 유입 | 방식 확정 필요 |

프로세스 간 연결:
- AI 뉴스 → 공부·정리 → SNS 포스팅 → 강의 콘텐츠에 반영
- 강의 문의 → 일정 확정 → 준비 → 강의 → 팔로업 → 정산 서류 → 입금 확인
- 책쓰기 → 책 챕터 → 강의 PPT 변환 → 강의·CAIO 교재로 활용
- CAIO-LMS → 수료 → CAIO-Alumni → 재강의·추천으로 순환
- 모든 운영 경험 → 이 Mega PRD에 축적 → ai4ceo-portal 제품화

## 3. 아키텍처 — 3층 구조

1. **데이터층 (진실의 원천)**
   - GitHub: 이 레포(mega-prd)의 docs/prd + data/*.json, ai4ceo-portal
   - Google Calendar: 일정
   - Gmail: 팔로업·정산·입금 알림
   - Google Drive: 책 원고 (글쓰기 › CEO를 위한 AI Navtive Enterprise)
2. **엔진층 (Claude + 예약 작업)**
   - 매일 아침 예약 작업이 각 소스를 스캔 → 상태 판단(지연·임박·완료) → 콕핏 갱신
   - 실행(메일 발송, 일정 등록, 커밋)은 사용자의 지시 후 Claude가 수행
3. **뷰층 (라이브 아티팩트 콕핏)**
   - 탭: ai4ceo / 홈·오늘 / Mega PRD / 스케줄 / AI 뉴스·SNS / 책쓰기 / 강의 파이프라인 / CAIO-LMS / CAIO-Alumni / 시스템
   - 원칙: 콕핏은 뷰(view)다. 버튼이 실행하는 것이 아니라 Claude가 실행하고 화면에 반영한다.

## 4. 데이터 설계 원칙

- 상태 데이터(강의 파이프라인, 팔로업, 정산)는 이 레포의 `data/*.json`을 단일 원천으로 한다.
  버전 관리가 되고, Claude가 읽고 쓸 수 있으며, 콕핏이 직접 읽을 수 있다.
- 원본 문서(책 원고, PRD)는 원래 위치(Drive, GitHub)에 두고 참조만 한다. 복사본을 만들지 않는다.
- 개인정보(팔로업 연락처 등)는 public 레포에 두지 않는다. 이름 이니셜·회사만 기록하고
  상세는 비공개 저장소 또는 로컬에 둔다. (레포 공개 여부 결정에 따라 조정)

## 5. 현재 상태 (2026-07-23 기준 — 사실)

- 콕핏 v0.5: 10개 탭 목업 완성, 애플 디자인 시스템 적용. 책쓰기 탭은 Drive 실데이터 연결 완료.
- 책: v14 사실검증 수정본 (2026-07-05 수정, 약 257쪽, 9부 구성). 18일간 수정 없음.
- ai4ceo-portal: PRD v3.2 확정 (2026-07-21), 모노레포 구조 (docs/01-plan~04-report 체계).
- 미연결: Google Calendar, Gmail, CAIO-LMS/Alumni 데이터.

## 6. 로드맵

| Phase | 내용 | 상태 |
| --- | --- | --- |
| 1 | 통합 콕핏 골격 (목업) | 완료 (v0.5) |
| 2 | 실데이터 연결 — Drive(완료), Calendar, Gmail, GitHub + 아침 자동 갱신 예약 작업 | 진행 중 |
| 3 | 강의 파이프라인·CAIO 데이터 스키마 확정, data/*.json 운영 개시 | 대기 |
| 4 | 운영 경험의 제품화 — ai4ceo-portal에 반영 | 대기 |

## 7. 성공 지표 (안)

- 팔로업 착수: 강의 후 48시간 이내 100%
- 정산 누락: 0건 (발송 후 14일 초과 시 자동 경고)
- 주간 SNS 발행: 2건 이상
- 콕핏 갱신: 매일 아침 자동, 수동 개입 없이
- 책·강의 변환: 챕터 → PPT 변환 소요 시간 1일 이내

## 8. 하위 PRD

- AI4CEO Portal PRD v3.2 (최종본): `ai4ceo-portal/docs/prd/prd-v3.2.md`
- AI4CEO Portal PRD v3.0: `ai4ceo-portal/docs/prd/prd-v3.0.md`

## 9. 미결 사항

- [ ] 이 레포의 공개 여부 (public이면 콕핏이 GitHub API로 직접 읽기 가능, 대신 개인정보 규칙 4장 적용)
- [ ] CAIO-LMS 데이터 접근 방식
- [ ] 강사료 데이터의 민감도 처리 (금액을 레포에 둘지, 로컬에 둘지)
- [ ] 책 v14 이후 퇴고 재개 일정
