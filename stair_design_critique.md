# STAiR 스테어 커피 웹앱 · 디자인 크리틱

> 파일: `index.html` · 검토 범위: UI/UX 전반 · 맥락: 매장 아이패드 키오스크 + 관리자 운영

## Overall Impression
에디토리얼 매거진 톤(크림 + 골드 + 잉크, Pretendard, 영문 트래킹 라벨)이 "사이폰 전문점"의 정체성을 잘 드러냅니다. 다만 **손님 아이패드**라는 사용 맥락에 비해 진입 장벽(로그인)과 주문 피드백(네이티브 alert), 모바일 카트 UX가 목적과 어긋나는 부분이 가장 큰 개선 포인트입니다.

## Usability

| Finding | Severity | Recommendation |
|---|---|---|
| 손님이 메뉴를 보려면 먼저 로그인해야 함 (`!session` → `LoginView`) | Critical | 공개 메뉴를 기본 화면으로, 관리자/직원은 `?role=admin` 또는 히든 버튼 진입 |
| 주문 완료가 네이티브 `alert()` 한 줄 | Critical | 전체화면 성공 뷰 — "주문 #23 · 약 4분 후 호출" + 진동/사운드 |
| 로그인 화면에 "admin / admin" 평문 노출 | Critical | 운영 환경에서는 제거. 초기 1회 마법사로 교체 |
| 모달이 바깥 탭으로 닫힘 — 옵션 고르다 손 빗나가면 초기화 | Moderate | 바깥 탭은 확인 프롬프트 또는 닫기 버튼만으로 제한 |
| 카트가 우측 고정 360px, `lg` 미만엔 아래로 내려감 | Moderate | 모바일/태블릿 세로는 하단 스티키 바(총액 + 주문 보기) → 바텀시트 |
| `ItemRow`의 옵션 chip이 전부 동일 외형 — 아이스/로스팅/종류 구분 없음 | Moderate | 섹션 라벨 + 단독 선택은 segmented control로 통일 |
| 필터 칩이 전역처럼 보이지만 브루잉에만 적용 | Moderate | "브루잉 필터:" 라벨 추가 또는 섹션 안쪽 들여쓰기 |
| 카드에 "장바구니에 있음" 상태 없음 | Moderate | 카드에 수량 뱃지, 두 번째 탭 시 +1 |
| Modal vs ItemRow 담기 UX가 완전히 다름 (모달 vs 인라인) | Moderate | 규칙화 — 싱글 오리진은 모달, 기타는 인라인. 카드에 `ⓘ 상세` 힌트 |
| 수량 버튼 28×28 — WCAG 44×44 AA 미달 | Moderate | 최소 36px, 탭 여백 포함 44px |

## Visual Hierarchy

- **시선 1순위**: 헤더 로고/탭. 올바름. 단, `사장님의 한마디` 카드가 너무 커서(3xl/4xl) 첫 스크롤에서 메뉴 도달이 늦음. 인사말은 중요도 낮음 — 높이 반으로 줄이고 "오늘의 원두 핀" 카드를 승격.
- **읽는 흐름**: 카테고리 세로 스택. 가장 비싼 게이샤(19,000원)가 브루잉 중단에 묻힘. "시그니처 / 오늘의 추천"을 최상단 캐러셀로.
- **강조**: 원두색 원(40px) < 이름(22px) < 태그라인(13px italic). 좋음. 문제: "사장님 한마디" 박스(`#fff6e5`)가 카드 내부에 들어가 paper · paper-2 · 아이보리 3색 중첩 — 위계 흐려짐.

## Consistency

| Element | Issue | Recommendation |
|---|---|---|
| chip 5종(strong/gold/rose/blue/green) | 파스텔 난립 | `tone: strong/warm/cool` 2차원으로 축소. difficulty만 색 유지 |
| `font-display`가 `inherit` | 실제로 디스플레이 폰트가 없음 | Fraunces나 Cormorant 한 종 추가로 매거진 톤 완성 |
| 버튼 사이즈 세 종류 나란히 | 토큰 없음 | `btn-sm / btn-md` 도입 |
| Guide의 "localStorage 저장" 설명 | 실제는 Supabase — 문서 틀림 | 즉시 교체 |
| chip 내 영문/한글 혼재 | "Washed", "Geisha" | 정책 결정 후 통일 |

## Accessibility

- `maximum-scale=1.0, user-scalable=no` — **WCAG 1.4.4 위반**. 즉시 제거.
- `--ink-soft (#71717a)` on `--paper (#fafaf9)` → **4.03:1** — 작은 11~13px 칩 텍스트에서 사실상 실패. `#52525b` 이상으로.
- `:focus-visible` 스타일 전무. 키보드 탐색 불가. 글로벌 1줄 추가.
- 아이콘만 버튼(`✕`, `−/+`)에 `aria-label` 없음.
- 로그인 input은 placeholder만 — `<label className="sr-only">` 추가.
- `<html lang="ko">` OK.

## What Works Well

- 색·타이포 시스템이 "사이폰/스페셜티" 정체성을 또렷하게 전달. 체인점 감성을 피하고 인디 카페의 고유성을 살림.
- 원두 카드 **태그라인**과 **Rating(바디/산미/단맛/밸런스)** — 스페셜티 언어 몰라도 선택 가능하게 돕는 탁월한 도구.
- **추천 타이밍**(비 오는 날, 업무 집중 등) 칩 — 맥락 기반 추천을 시각화한 영리한 패턴.
- 난이도 색 매핑(green/gold/rose) — 진입 공포 완화.
- 관리자 사장님 한마디 편집 — 페이지가 살아있는 느낌.

## Priority Recommendations — 더 좋은 대안

### 1. 첫 화면을 "주문 키오스크"에서 "매장 잡지"로 전환

현재 첫 스크린이 로그인/메뉴 나열이지만, 이 카페의 강점은 *원두의 이야기*입니다.

```
[1] 오늘의 원두 히어로 (큰 사진/컬러/태그라인/사장님 한줄)
[2] "기분으로 고르기" — whenMoods 칩
[3] "난이도로 고르기" — 입문/중급/고급
[4] 전체 메뉴 그리드
```

로그인은 하단 히든 또는 `?role=admin` 쿼리스트링. 익명 `guest` 세션 자동 생성.

### 2. 주문 확정을 "알림"에서 "순간"으로

- `alert()` 대신 전체화면 성공 뷰
- 큰 체크 애니메이션 + "주문 #23" + 예상 대기 시간 + 진동(모바일)
- 3~4초 후 자동 복귀 또는 "다음 손님을 위해 처음으로" 버튼

### 3. 모바일/태블릿 세로 모드 카트 패턴

```jsx
<button className="fixed bottom-4 left-4 right-4 btn-primary z-30 justify-between">
  <span>{cart.length}개 · {KRW(total)}</span>
  <span>주문 보기 →</span>
</button>
```

탭하면 바텀시트 Drawer로 카트 펼침. sticky right panel은 `xl` 이상.

### 4. 색/칩 체계 단순화

- 축 1(톤): `neutral` / `strong`
- 축 2(의미): difficulty만 색 매핑, 나머지는 neutral
- notes는 배경색 제거, 점 모양 컬러 prefix만: `● 자몽` — 시각 소음 50% 감소

### 5. 접근성 한 번에 정리

```html
<!-- 뷰포트 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

```css
*:focus-visible {
  outline: 2px solid var(--gold);
  outline-offset: 2px;
  border-radius: 6px;
}
```

모든 icon-only 버튼에 `aria-label`, `sr-only` 라벨 보강.

### 6. 가이드 정확성 + "오늘의 추천 핀"

- 가이드 "localStorage 저장" → "Supabase 기반, 기기 독립 저장"으로 교체
- `owner_notes.is_today_pick` 컬럼으로 당일 추천 핀 기능. 재방문 동기 생성.

### 7. 원두 선택 대안 UI

- **(A) 기분 매칭 Quiz-lite**: "오늘 기분은?" 3~4탭 → whenMoods·difficulty·flavor 교차 → 상위 3잔. 20초 시나리오.
- **(B) 맛 맵 2D 플롯**: X=산미, Y=단맛. `body/acid/sweet` 데이터로 `recharts` 바로 구현. 애호가용.

둘 중 하나를 상단 CTA로 올리면 전환율 상승.

---

## 결론

디자인 언어와 콘텐츠 설계는 이미 단순 키오스크의 수준을 넘어섭니다. 개선의 요체는 **"잡지처럼 읽히는 주문 경험"으로 재프레이밍**하는 일이며, 기술적으로는 (1) 로그인 게이트 해제, (2) 주문 성공 순간의 연출, (3) 모바일 카트 바텀시트, (4) 접근성 기본 3종 — 이 네 가지를 먼저 처리하면 체감 품질이 급격히 올라갑니다.
