---
name: travel-confirmation
description: >
  여행 확정서(HTML)를 만들 때 반드시 이 스킬을 사용하라.
  "확정서 만들어줘", "여행확정서", "가이드 확정서", "손님 확정서" 등
  여행 확정서 관련 요청이 오면 항상 이 스킬을 먼저 읽고 작업하라.
---

# 여행 확정서 제작 스킬

## 기본 원칙
- 항상 HTML 단일 파일로 제작
- 출력 경로: `/mnt/user-data/outputs/[파일명].html`
- 파일 완성 후 반드시 `present_files` 호출
- 파일과 함께 손님 전달 링크도 항상 같이 제공
  - 링크 형식: `https://myhanil82-alt.github.io/travel-booking/[파일명].html`
- 작업 전 필요한 정보가 부족하면 **먼저 질문을 다 하고** 답변을 모두 들은 후 작업 시작
- 호텔은 별도 언급 없으면 **전 일정 포함**으로 간주

---

## 디자인 시스템

### CSS 변수 (반드시 동일하게 유지)
```css
:root {
  --coral: #E8593A;
  --coral-bg: #FFF4F1;
  --navy: #1C3557;
  --navy-light: #2E4E72;
  --gold: #C8973A;
  --gold-light: #F0C870;
  --mint: #3AA8A0;
  --bg: #FAFAF8;
  --card-bg: #FFFFFF;
  --text-main: #1A1A1A;
  --text-sub: #5A5A5A;
  --text-muted: #8A8A8A;
  --border: #E8E4DC;
  --shadow: 0 2px 16px rgba(28,53,87,0.08);
  --max-w: 680px;
}
```

### 폰트
```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;600;700&family=Noto+Serif+KR:wght@400;600&display=swap" rel="stylesheet">
```
- body 기본 font-size: **16px** (Noto Sans KR)
- 헤더 타이틀: Noto Serif KR

### 전체 레이아웃 원칙
- 헤더, 트립밴드, 컨테이너, 푸터 모두 `max-width: 680px` 동일 적용
- 헤더·푸터는 `width: 100%`로 full-width, 내부 콘텐츠는 max-width로 정렬
- 모바일 대응: `@media (max-width: 480px)` 그리드 1열 처리

---

## 구조 (섹션 순서)

### 1. HEADER
```
배경: --navy (그라디언트 포함)
내용: header-tag(Travel Confirmation) / header-title(여행지+확정서) / header-subtitle(영문+연도) / header-names(손님 이름)
```

### 2. TRIP BAND
```
배경: --coral
항목 4개: 출발일 / 귀국일 / 일정(N박M일) / 투어타입(자유여행 or 패키지)
```

### 3. CONTAINER (섹션들)
섹션마다 section-num(원형 네이비 번호) + section-title 조합

---

## 섹션별 컴포넌트

### 상품 안내
- 모두투어 링크를 product-link-box 카드로 표시 (클릭 시 새 탭)
- 자유여행이면: 가이드 없는 자유여행 / 공항 미팅 없음 alert-navy

### 공항 미팅 안내
**패키지 or 미팅 있는 경우:**
- meeting-box (coral 테두리)
- 미팅 시간 크게 표시
- 장소 / 도착 권장시간 row

**자유여행 + 미팅 없는 경우:**
- 미팅 섹션 생략, 상품 안내에 개별수속 안내만 표시

**자유여행이지만 미팅 장소 있는 경우 (일행 많을 때 등):**
- meeting-box 사용하되 하단에 "가이드 없는 자유여행" 명시

### 항공 일정
- 출발편 / 귀국편 각각 flight-card로 분리
- flight-header: 네이비 그라디언트, 편명(gold-light)
- flight-route: 출발공항코드(30px bold) → 화살표+소요시간 → 도착공항코드
- 하단 flight-badges: 항공사 / 공항 / 수하물 / 기내식 여부
- 기내식 미포함이면 반드시 별도 alert-coral로 안내

### 수하물 안내
- baggage-grid (2열)
- 위탁수하물 / 기내수하물 분리
- 유아 수하물 별도 있으면 위탁수하물 카드에 같이 기재

### 체크인 카운터 안내
- counter-box (gold 테두리)
- 스텝 번호(금색 원) + 단계별 안내
- 공항명 / 터미널 / 카운터 위치 / 주의사항 포함
- **제주항공 웹체크인: 출발 24시간 전부터 가능** (48시간 아님)

### 현지 가이드 & 행사 정보 (패키지 전용)
- guide-box (navy 테두리)
- 가이드명 / 연락처 / 비상연락처 / 행사인원 / 차량

### 현지 비상연락처 (자유여행)
- emer-box (navy 테두리)
- 이름 / 전화번호(링크) / 카카오톡 ID row

### 투숙 호텔
- hotel-card (mint 테두리 2px)
- 헤더: mint 그라디언트, 포함 박수 badge
- 호텔명 크게
- 투숙기간 / 룸타입 / 체크아웃 시간 등 row
- **1박만 포함인 경우**: remark-box(빨간 점선)로 "N일차~N일차 N박 불포함" 명시

### 포함/불포함
- incl-grid (2열)
- 포함 없애도 된다고 하면 불포함만 단독 표시
- 불포함 항목은 담당자가 지정한 것만 기재

### 입국 필수 사항 (국가별)
**인도네시아(바탐):**
- visa-box (coral 테두리 2px, 강조 박스)
- 도착비자 비용 / 전자비자 안내 / 통합신고(All Indonesia) 전체 상세 기재
- 매뉴얼 링크: https://m.blog.naver.com/melbourne_wanderlust/224022704761

**베트남:**
- 출입국신고서 불필요 (여권만) alert-navy
- 이티켓 보관 필수 alert-gold
- 아동 동반 주의사항(영문 가족관계증명서) alert-red
- 해변 안전 주의사항 alert-coral

### 환전 안내
- exchange-box (gold 테두리)
- 국가별 환전 방법 명시
- **호주(AUD)**: 가이드/기사 경비는 반드시 USD / 선택관광은 AUD 현금 / 마트는 카드
- **베트남(VND)**: USD로 환전 후 현지 재환전 권장 + 동 계산법 예시 포함
- **일본(JPY)**: 개인 경비 엔화 환전 또는 트래블월렛
- **인도네시아(IDR)**: 루피아 또는 트래블월렛

### 날씨 & 준비물
- prep-grid (2열)
- 날씨 / 옷차림 / 환전 or 준비물 / 현지이동 or 기타
- 날씨는 실제 해당 월 현지 날씨 기준으로 정확하게 기재

### 여행자보험 (요청 시)
- insurance-box (파란색 #4A7EC8 테두리)
- 한화손해보험 Tel: 02-728-8008 / Fax: 02-936-1771
- "건강 문제 병원 방문 또는 보험 관련 문제 발생 시 연락" 안내

### Alert 스타일 용도
- alert-navy: 일반 정보 안내
- alert-coral: 주의 / 기내식 미포함 / 자유여행 안내
- alert-gold: 숙박세 / 이티켓 / 팁 안내
- alert-mint: 완료 사항 (사전신청 완료 등)
- alert-red: 필수/위험 사항 (아동 입국, 중요 리마크)

---

## FOOTER (고정)
```html
전화: 1544-5714 (tel 버튼, coral 색)
카카오톡: http://pf.kakao.com/_xhxfxlKd/chat (kakao 버튼, #FEE500)
```

---

## 국가별 공항 코드
- 인천: ICN / 김해(부산): PUS
- 후쿠오카: FUK / 오사카(간사이): KIX / 도쿄(나리타): NRT
- 시드니: SYD / 나트랑: CXR / 바탐: BTH
- 방콕(수완나품): BKK / 다낭: DAD / 세부: CEB / 발리: DPS

---

## 작업 전 반드시 확인할 사항
1. 여행 기간 (출발일·귀국일·박수)
2. 항공사·편명·시간
3. 호텔 포함 여부 (따로 언급 없으면 전박 포함)
4. 가이드 여부 (자유여행 vs 패키지)
5. 공항 미팅 여부 및 장소·시간
6. 인원 (성인/아동 구분)
7. 연락처 (푸터는 고정, 가이드·비상연락처는 별도)

