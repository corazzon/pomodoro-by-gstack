# 뽀모도로 타이머 — 프로젝트 컨텍스트

## 개요

인터랙티브 SVG 애니메이션이 있는 뽀모도로 타이머 웹앱. 단일 HTML 파일로 구성되어 빌드 도구 없이 동작합니다.

## 프로젝트 구조

```
cc_timer/
├── index.html          # 앱 전체 (HTML + CSS + JS, ~1000줄)
├── docs/
│   └── designs/        # 디자인 리소스
├── README.md           # 프로젝트 설명
├── CHANGELOG.md        # 변경 이력
└── CLAUDE.md           # 이 파일
```

## 기술 스택

- 순수 HTML/CSS/JS (프레임워크 없음, 빌드 도구 없음)
- CSS Custom Properties (다크 모드)
- 인라인 SVG + requestAnimationFrame (애니메이션)
- Web Audio API (알림음)
- LocalStorage (설정, 기록, 타이머 상태 저장)
- Web Notifications API (브라우저 알림)

## 아키텍처

### index.html 내부 구조 (섹션별)

| 섹션 | 라인 범위 (대략) | 설명 |
|------|-----------------|------|
| CSS 스타일 | 7~193 | 전체 스타일, 다크 모드, 반응형 |
| HTML 마크업 | 195~286 | 헤더, 타이머/설정/기록 페이지, 트랜지션 오버레이 |
| Audio Engine | 289~312 | Web Audio API 기반 알림음 6종 |
| Constants | 316~329 | SVG 경로, 프리셋 데이터 |
| State | 333~341 | 앱 상태 관리 (settings, history, cycle) |
| SVG Rendering | 346~457 | 토마토/커피잔 SVG 렌더링 + 물결 애니메이션 |
| Animation Loop | 462~487 | requestAnimationFrame 루프 |
| Timer Logic | 492~550 | performance.now() 기반 타이머 |
| Cycle Management | 555~622 | 작업/휴식 페이즈 전환 |
| Transition Overlay | 627~679 | 페이즈 전환 시 오버레이 UI |
| UI Updates | 684~770 | DOM 업데이트 함수들 |
| Harvest Counter | 775~796 | 헤더의 미니 토마토 카운터 |
| History & Stats | 800~858 | 기록 저장 및 통계 페이지 |
| Settings | 863~920 | 설정 관리 및 프리셋 |
| Navigation | 925~938 | 탭 네비게이션 |
| Persistence | 942~978 | beforeunload 시 상태 저장/복원 |
| Init | 988~994 | 초기화 |

### 주요 상태

- `settings` — 작업/휴식 시간, 사이클 수, 알림음 (LocalStorage `mt_settings`)
- `historyData` — 완료 기록 배열, 최대 200건 (LocalStorage `mt_history`)
- `cycle` — 현재 타이머 상태: phase, round, status, remainSec 등

### SVG 렌더링 방식

- **토마토 (작업)**: 클리핑 패스로 토마토 모양 안에 물결 애니메이션으로 채움
- **커피잔 (휴식)**: 같은 방식으로 커피잔 내부를 채움
- SVG DOM은 한 번 생성 후 속성만 업데이트 (성능 최적화)

## 개발 가이드

### 실행

```bash
open index.html
# 또는
python -m http.server 8000
```

### 코드 수정 시 참고

- 단일 파일이므로 CSS/HTML/JS 모두 index.html에 있음
- CSS 변수를 수정하면 다크 모드에도 자동 반영됨
- 새 알림음 추가: `SOUNDS` 객체에 키/라벨/fn 추가
- 새 프리셋 추가: `PRESETS` 배열에 객체 추가
- SVG 경로 수정 시 `ensureTomatoDOM` / `ensureCoffeeDOM` 함수 확인

## 관련 문서

- [README.md](./README.md) — 사용자 안내
- [CHANGELOG.md](./CHANGELOG.md) — 변경 이력
