# 디자인 가이드

비주얼/UI 작업이나 스타일링 결정을 수행할 때 Frontend Master가 로드합니다.

## 디자인 씽킹 프레임워크

비주얼 작업을 구현 전에 다음을 정의:
1. **목적**: 이 UI가 전달하거나 달성해야 하는 것은?
2. **톤**: 어떤 감정적 레지스터? (권위 있는, 장난스러운, 차분한, 긴급한 등)
3. **제약**: 기술적 한계, 브랜드 요구사항, 접근성 요구
4. **차별화**: 이것이 제네릭 AI 출력과 다른 점은?

## Anti-AI 미학 규칙

절대로 "AI스러운" 제네릭 UI를 만들지 않는다:

| 패턴 | 대신 |
|------|------|
| 보라색/인디고 그라디언트 | 의도적이고 독특한 색상 체계 사용 |
| 과도한 둥근 모서리 (rounded-2xl 남발) | border-radius를 의도적으로 변화 |
| 제네릭 히어로 섹션 ("미래를 환영합니다") | 가치 중심, 구체적인 메시지로 시작 |
| Lorem ipsum 또는 플레이스홀더 텍스트 | 실제 의미 있는 콘텐츠 작성 |
| 과도한 패딩/히어로 섹션 | 밀도를 존중 — 모든 것이 여백을 필요로 하지 않음 |
| 스톡 카드 그리드 (3열 아이콘+제목+설명) | 그리드를 깨고 에디토리얼 레이아웃 사용 |
| 그림자 과다 카드 디자인 | 보더, 대비, 또는 배경색 변화 사용 |
| 예측 가능한 대칭 레이아웃 | 비대칭, 겹침, 시각적 긴장 도입 |

## 타이포그래피

**금지 폰트**: Inter, Roboto, Arial, system-ui를 주 아이덴티티 폰트로 사용하지 않는다.

규칙:
- **독특한 디스플레이 폰트**(제목, 아이덴티티)와 **세련된 본문 폰트**(가독성) 페어링
- 엄격한 계층 확립: h1 → h2 → h3 → body → small
- 0.25rem(4px) 단위의 일관된 간격 스케일 사용

### 미학 방향별 폰트 페어링

| 방향성 | 디스플레이 폰트 (제목) | 본문 폰트 (가독성) | 분위기 |
|-----------|------------------------|---------------------|------|
| **Brutally minimal** | Space Grotesk, Syncopate | Inter, Space Grotesk (light) | 날카로움, 기하학적, 반성 없음 |
| **Maximalist chaos** | Abril Fatface, Rage Italic | Space Mono, IBM Plex Mono | 밀도 높은, 시끄러운, 압도적 |
| **Retro-futuristic** | Orbitron, Press Start 2P | Rajdhani, VT323 | 향수 어린 기술, 아케이드 |
| **Organic/natural** | Comfortaa, Alegreya | Quicksand, Lora | 유동적, 따뜻함, 인간적 |
| **Luxury/refined** | Cormorant Garamond, Playfair Display | Lato, EB Garamond | 프리미엄, 우아함, 시대를 초월 |
| **Playful/toy-like** | Fredoka, Poppins | Nunito, Quicksand | 재미, 친근함, 입체감 |
| **Editorial/magazine** | Merriweather, Source Serif Pro | Libre Baskerville, Source Sans Pro | 콘텐츠 우선, 저널리스트 |
| **Brutalist/raw** | Archivo Black, Bebas Neue | Archivo, Roboto Mono | 노출, 산업적, 원시적 |
| **Art deco/geometric** | Monoton, Righteous | Montserrat, Work Sans | 날카로운 각도, 대칭 |
| **Soft/pastel** | Nunito, Muli | Quicksand, Poppins | 부드러운, 둥근, 가벼움 |
| **Industrial/utilitarian** | JetBrains Mono, Space Mono | IBM Plex Sans, Roboto | 기능적, 밀도 높은, 기술적 |

### 안전한 기본값 (불확실할 때)

미학 방향성이 불확실할 때, 대부분의 프로젝트에 잘 맞는 선택:
- **테크/스타트업**: Plus Jakarta Sans (디스플레이) + Inter (본문)
- **콘텐츠/에디토리얼**: Source Serif Pro (디스플레이) + Source Sans Pro (본문)
- **이커머스**: Poppins (디스플레이) + Open Sans (본문)
- **SaaS**: Geist (디스플레이) + Geist Sans (본문)

위 모든 폰트는 Google Fonts에서 무료로 제공되며 `next/font/google`로 쉽게 로드 가능하다.

## 색상

- **시맨틱 토큰** 사용 (`text-primary`, `bg-surface`, `border-muted`), 원시 hex 값이 아님
- 테마 종속 값에 CSS 변수 정의
- 1-2개의 날카로운 악센트 색상이 있는 지배적 팔레트 선택
- 명시적으로 요청하지 않는 한 제네릭 블루 (#3B82F6)를 기본으로 사용하지 않음
- WCAG 2.2 AA 명암비 준수 (일반 텍스트 4.5:1, 큰 텍스트 3:1)

## 모션 & 애니메이션

- 정적 HTML: CSS 전용 애니메이션 (키프레임, 트랜지션)
- React 컴포넌트: CSS 트랜지션 우선; 복잡한 시퀀스에는 Motion 라이브러리 사용
- 리스트와 그리드에 `animation-delay`로 스태거드 리빌 사용
- `prefers-reduced-motion` 존중 — 항상 감소된 모션 대체 제공
- 애니메이션은 장식이 아닌 목적을 가져야 함 (주의 유도, 피드백 제공)

## 공간 구성

- 예측 가능한 대칭을 깨기 — 비대칭, 겹침, 대각선 흐름 도입
- 여백을 기본 패딩이 아닌 의도적인 디자인 요소로 사용
- 색상뿐만 아니라 크기 대비로 시각적 계층 생성
- 모든 것을 중앙 정렬하지 않기 — 정렬로 시각적 긴장 생성

## 미학 방향성

새로 시작할 때, 프로젝트 목적에 맞는 방향성을 선택:

- **Brutally minimal**: 강렬한 대비, 여백, 절제
- **Maximalist chaos**: 밀도 높은, 레이어된, 압도적인 디테일
- **Retro-futuristic**: 향수 + 현대 기술 미학
- **Organic/natural**: 유동적인 형태, 어스톤, 부드러운 엣지
- **Luxury/refined**: 프리미엄, 우아함, 절제
- **Playful/toy-like**: 재미, 친근함, 입체감
- **Editorial/magazine**: 강렬한 타이포그래피, 그리드 기반, 콘텐츠 우선
- **Brutalist/raw**: 노출된 구조, 원시 재료, 의도적인 거칠음
- **Art deco/geometric**: 날카로운 각도, 대칭, 메탈릭 악센트
- **Soft/pastel**: 부드러운 그라디언트, 둥근 형태, 밝은 팔레트
- **Industrial/utilitarian**: 기능적, 밀도 높은, 데이터 중심
