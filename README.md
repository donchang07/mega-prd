# mega-prd

장동인 교수(AIBB LAB · KAIST 김재철AI대학원)의 **AI Native Company 통합 업무 시스템** 최상위 설계 문서와 운영 데이터 저장소입니다.

## 구조

```
docs/prd/          Mega PRD 버전별 문서 (mega-prd-v0.1.md, v0.2.md, ...)
data/              운영 데이터 (콕핏과 Claude 엔진이 읽고 쓰는 단일 원천)
  book.json        책쓰기 현황 (Google Drive 스캔 결과)
  processes.json   10개 프로세스 레지스트리
  lectures.json    강의 파이프라인 (준비→강의→팔로업→정산→입금)
```

## 운영 방식

- **뷰**: 라이브 아티팩트 콕핏(AI Native Cockpit)이 이 레포의 `docs/prd`와 `data/`를 읽어 표시합니다.
- **엔진**: Claude가 매일 아침 예약 작업으로 소스를 스캔하고, 이 레포의 데이터를 갱신합니다.
- **결정**: 사람(장동인 교수)이 합니다. Claude는 제안하고 실행합니다.

새 PRD 버전은 `docs/prd/mega-prd-vX.Y.md`로 추가합니다. 기존 버전은 수정하지 않습니다.

관련 저장소: [ai4ceo-portal](https://github.com/donchang07/ai4ceo-portal) — 하위 제품 PRD(v3.2)와 구현.
