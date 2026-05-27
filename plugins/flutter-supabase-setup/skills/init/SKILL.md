---
name: init
description: "(flutter-supabase-setup) 기획서(마크다운)를 프로젝트 폴더에서 자동 탐색하여 Flutter + Supabase 개발환경을 셋업하는 스킬. 프로젝트 생성, 폴더 구조, 의존성 설치, 환경설정, 빌드 검증까지만 수행. 기능 코드(페이지, 위젯, 비즈니스 로직)는 일절 생성하지 않는다. Use when: user asks to set up a Flutter + Supabase project. Do NOT use when: debugging, implementing features, or non-setup tasks."
---

# Flutter + Supabase Setup — init

Sets up a Flutter + Supabase **development environment** from a planning document. Creates project structure, installs dependencies, configures environment, and verifies the build.

This skill generates **zero feature code** — no pages, no widgets, no business logic. Only infrastructure needed to start `flutter run`.

> **Language**: Always respond in **Korean (한국어)**.

---

## Role

One-shot environment setup. Creates a buildable Flutter + Supabase project shell.

## Trigger Condition

Activate when the user requests Flutter + Supabase project setup. 기획서는 프로젝트 폴더에서 자동으로 탐색한다 — 사용자에게 먼저 달라고 하지 않는다. Do NOT activate for feature development, debugging, or code review.

---

## Fixed Stack

| Component | Technology |
|---|---|
| Framework | Flutter (latest stable) |
| Backend | Supabase (`supabase_flutter`) |

## Baseline Stack

| Category | Default | Packages |
|---|---|---|
| State management | Riverpod (codegen) | `flutter_riverpod`, `riverpod_annotation`, `riverpod_generator` |
| Routing | go_router | `go_router` |
| Models | freezed + json_serializable | `freezed_annotation`, `json_annotation`, `freezed`, `json_serializable` |
| Environment | `--dart-define-from-file` | No extra package |
| Logging | logger | `logger` |
| Architecture | Feature-first + lightweight Clean | N/A (folder convention) |
| Testing | mocktail | `mocktail` |

## Stack Decision Guide

| Signal in plan | Adjustment |
|---|---|
| "prototype", "MVP", <= 2 features | Simplify: setState over Riverpod, plain classes over freezed, flat structure |
| "BLoC", "Bloc" | Switch to `flutter_bloc` |
| "offline", "cache", "오프라인" | Add `hive` or `drift` |
| "i18n", "다국어" | Add `slang` or `easy_localization` |
| "social login" | Add `google_sign_in`, `sign_in_with_apple` |
| "push notification", "FCM" | Add `firebase_messaging` + `firebase_core` |
| "image", "사진" | Add `cached_network_image`, `image_picker` |
| "real-time", "채팅" | Ensure Supabase Realtime; consider `timeago` |
| 4+ features, complex data | Full Clean Architecture (`data/domain/presentation`) |
| <= 3 features, simple | Flat feature structure |

---

## Phase 1 — Planning Document Discovery & Analysis

1. **자동 탐색**: 현재 프로젝트 디렉토리에서 기획서를 자동으로 찾는다. 탐색 순서:
   - `docs/` 폴더 내 마크다운 파일 (`*.md`)
   - 프로젝트 루트의 마크다운 파일 (README.md, CLAUDE.md 등 표준 파일 제외)
   - 파일명에 `기획`, `plan`, `spec`, `요구사항`, `requirement`, `PRD` 등이 포함된 파일 우선
   - 후보가 여러 개면 목록을 보여주고 사용자에게 선택을 요청
   - 후보가 0개면 그때만 사용자에게 기획서 경로를 요청
2. 기획서를 읽고 분석: core features, data model hints, technical requirements, auth needs
3. Present analysis in Korean and wait for validation

```
## 기획서 분석 결과

### 핵심 기능
1. {feature}: {description}
...

### 데이터 모델 (추정)
- {Entity}: {fields}
...

### 기술 요구사항
- {requirement}
...

분석이 정확한지 확인해주세요.
```

---

## Phase 2 — Project Info Proposal

사용자에게 묻지 말고, 기획서 분석 결과를 바탕으로 **직접 추론하여 제안**한다. 사용자는 확인/수정만 하면 된다.

- **프로젝트명**: 기획서의 앱 이름·성격에서 snake_case로 추론 (예: 커플 앱 → `couple_diary`, 할일 앱 → `task_flow`)
- **조직명**: 기획서에 회사/팀 정보가 있으면 역도메인으로 추론, 없으면 `com.example` 사용
- **플랫폼**: `android,ios` 고정 (Flutter 기본). 기획서에 "웹", "web", "데스크톱" 등이 명시된 경우에만 자동 추가

Supabase credentials are NOT needed here. They are configured later via `/flutter-supabase-setup:env`.

Phase 1 분석 결과와 함께 **한 메시지로** 아래 형식으로 제안:

```
## 프로젝트 설정

| 항목 | 값 | 근거 |
|---|---|---|
| 프로젝트명 | `{inferred_name}` | {기획서에서 추론한 이유} |
| 조직명 | `{inferred_org}` | {근거 또는 "기획서에 명시 없어 기본값 사용"} |
| 플랫폼 | `android, ios` | Flutter 기본 |

이대로 진행할까요? 변경하고 싶은 항목이 있으면 말씀해주세요.
```

---

## Phase 3 — Stack Proposal

Present the proposed stack with 2-3 line justification per choice. Korean output.

```
기획서를 분석한 결과, 다음 스택을 제안드립니다:

- **상태관리**: Riverpod (코드 생성)
  {reason}

- **라우팅**: go_router
  {reason}

- **모델**: freezed + json_serializable
  {reason}

- **폴더 구조**: {choice}
  {reason}

- **추가 라이브러리**: {libraries or "없음"}
  {reason}

이대로 진행할까요?
```

---

## Phase 4 — User Confirmation

1. Apply user's changes if any
2. Display final summary table
3. Proceed only after explicit confirmation

---

## Phase 5 — SPEC.md Generation

Create `docs/SPEC.md` with: project info, tech stack rationale, folder structure, env variables, feature list from planning doc, run instructions.

---

## Phase 6 — Project Scaffolding

### 6-0. Pre-checks

```bash
flutter --version
```

- If Flutter < 3.7 → warn: `--dart-define-from-file` not supported
- If target directory already exists → ask user before overwriting

### 6-1. Create Flutter project

```bash
flutter create --org {ORG} --project-name {PROJECT_NAME} --platforms {PLATFORMS} {PROJECT_NAME}
cd {PROJECT_NAME}
```

### 6-2. Folder structure

**Full Clean Architecture:**
```
lib/
├── main.dart
├── app.dart
├── core/
│   ├── router/
│   │   └── app_router.dart
│   ├── theme/
│   │   └── app_theme.dart
│   ├── providers/
│   │   └── supabase_provider.dart
│   ├── constants/
│   │   └── app_constants.dart
│   └── extensions/
│       └── context_extensions.dart
├── features/
│   └── .gitkeep
└── shared/
    └── widgets/
        └── .gitkeep
```

**Flat structure:**
```
lib/
├── main.dart
├── app.dart
├── core/
│   ├── router.dart
│   ├── theme.dart
│   ├── supabase.dart
│   ├── constants.dart
│   └── extensions.dart
├── features/
│   └── .gitkeep
└── shared/
    └── widgets/
        └── .gitkeep
```

`features/` is intentionally empty. The developer creates feature modules during development.

### 6-3. Dependencies

```bash
# Always
flutter pub add supabase_flutter logger

# Baseline (Riverpod)
flutter pub add flutter_riverpod riverpod_annotation
flutter pub add --dev riverpod_generator riverpod_lint

# Baseline (go_router)
flutter pub add go_router

# Baseline (freezed)
flutter pub add freezed_annotation json_annotation
flutter pub add --dev freezed json_serializable build_runner

# Testing
flutter pub add --dev mocktail
```

Adjust based on confirmed stack. Use `flutter pub add` only (no hardcoded versions).

### 6-4. Environment configuration

Create `.env.example`:
```
SUPABASE_URL=
SUPABASE_ANON_KEY=
```

Append to `.gitignore`:
```
# Environment
.env
*.env.local
```

Do NOT create `.env` — that is handled by `/flutter-supabase-setup:env`.

### 6-5. Core boilerplate

Generate using templates from **Appendix A**. Replace `{PLACEHOLDER}` values.

| File | Template |
|---|---|
| `lib/main.dart` | A-1 |
| `lib/app.dart` | A-2 |
| `lib/core/router/app_router.dart` | A-3 |
| `lib/core/theme/app_theme.dart` | A-4 |
| `lib/core/providers/supabase_provider.dart` | A-5 |
| `lib/core/constants/app_constants.dart` | A-6 |
| `lib/core/extensions/context_extensions.dart` | A-7 |

For Flat structure, collapse paths per Appendix B-2.

### 6-6. Configuration files

**`analysis_options.yaml`:**
```yaml
include: package:flutter_lints/flutter.yaml

linter:
  rules:
    prefer_const_constructors: true
    prefer_const_declarations: true
    avoid_print: true
    prefer_single_quotes: true
    sort_child_properties_last: true
    unawaited_futures: true

analyzer:
  exclude:
    - "**/*.g.dart"
    - "**/*.freezed.dart"
  errors:
    invalid_annotation_target: ignore
```

**`README.md`:**
```markdown
# {Project Name}

{One-line description from planning document}

## Prerequisites
- Flutter SDK (stable, >= 3.7)
- Supabase project

## Setup
1. Clone the repository
2. Run `flutter pub get`
3. Run `dart run build_runner build --delete-conflicting-outputs`
4. Copy `.env.example` to `.env` and fill in Supabase credentials
5. Run `flutter run --dart-define-from-file=.env`

## Project Structure
See `docs/SPEC.md` for the full technical specification.
```

**`docs/SPEC.md`:** Write using Phase 5 template.

---

## Phase 7 — Verification

```bash
flutter pub get
dart run build_runner build --delete-conflicting-outputs  # if codegen
flutter analyze
```

Report results. Fix issues until all pass. Do NOT proceed with failures.

---

## Phase 8 — Next Steps Guide

````
## 셋업 완료!

프로젝트 환경이 준비되었습니다.

### 바로 다음에 할 일
1. `/flutter-supabase-setup:env`를 실행하여 Supabase 크레덴셜을 설정하세요
2. `lib/features/` 폴더에 첫 번째 기능 모듈을 만드세요

### 실행 방법
```bash
cd {project_name}
flutter run --dart-define-from-file=.env
```

### 프로젝트 구조
- `lib/core/` — 라우터, 테마, 프로바이더, 상수, 유틸리티
- `lib/features/` — 기능 모듈 (비어있음 — 여기에 추가)
- `lib/shared/` — 공용 위젯
- `docs/SPEC.md` — 기술 스택 결정 사항

### Supabase 대시보드에서 할 일
- [ ] 기획서 기반으로 테이블 스키마 생성
- [ ] RLS(Row Level Security) 정책 설정
- [ ] 필요한 Auth Provider 활성화
````

---

## Appendix A — Core Code Templates

Replace `{PLACEHOLDER}` values with actual project values.

### A-1. main.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:logger/logger.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

import 'app.dart';

final logger = Logger();

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();

  const supabaseUrl = String.fromEnvironment('SUPABASE_URL');
  const supabaseAnonKey = String.fromEnvironment('SUPABASE_ANON_KEY');

  if (supabaseUrl.isNotEmpty && supabaseAnonKey.isNotEmpty) {
    await Supabase.initialize(url: supabaseUrl, anonKey: supabaseAnonKey);
    logger.i('Supabase initialized');
  } else {
    logger.w('Supabase credentials not provided. Run /flutter-supabase-setup:env to configure.');
  }

  runApp(const ProviderScope(child: App()));
}
```

### A-2. app.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import 'core/router/app_router.dart';
import 'core/theme/app_theme.dart';

class App extends ConsumerWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final router = ref.watch(routerProvider);

    return MaterialApp.router(
      title: '{APP_DISPLAY_NAME}',
      theme: AppTheme.light,
      darkTheme: AppTheme.dark,
      routerConfig: router,
      debugShowCheckedModeBanner: false,
    );
  }
}
```

### A-3. app_router.dart

```dart
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';
import 'package:riverpod_annotation/riverpod_annotation.dart';

part 'app_router.g.dart';

@riverpod
GoRouter router(Ref ref) {
  return GoRouter(
    initialLocation: '/',
    routes: [
      GoRoute(
        path: '/',
        builder: (context, state) => const Scaffold(
          body: Center(child: Text('{APP_DISPLAY_NAME} is running')),
        ),
      ),
    ],
  );
}
```

### A-4. app_theme.dart

```dart
import 'package:flutter/material.dart';

class AppTheme {
  AppTheme._();

  static ThemeData get light => ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.blue,
        brightness: Brightness.light,
      );

  static ThemeData get dark => ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.blue,
        brightness: Brightness.dark,
      );
}
```

### A-5. supabase_provider.dart

```dart
import 'package:riverpod_annotation/riverpod_annotation.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

part 'supabase_provider.g.dart';

@riverpod
SupabaseClient supabaseClient(Ref ref) {
  return Supabase.instance.client;
}

@riverpod
GoTrueClient supabaseAuth(Ref ref) {
  return ref.watch(supabaseClientProvider).auth;
}
```

### A-6. app_constants.dart

```dart
class AppConstants {
  AppConstants._();

  static const String appName = '{APP_DISPLAY_NAME}';
}
```

### A-7. context_extensions.dart

```dart
import 'package:flutter/material.dart';

extension BuildContextX on BuildContext {
  ThemeData get theme => Theme.of(this);
  TextTheme get textTheme => theme.textTheme;
  ColorScheme get colorScheme => theme.colorScheme;
  Size get screenSize => MediaQuery.sizeOf(this);
  bool get isDarkMode => theme.brightness == Brightness.dark;
}
```

---

## Appendix B — Conditional Adaptation Guide

### B-1. If Bloc is chosen

- `main.dart`: `ProviderScope` → `MultiBlocProvider`
- `app.dart`: `ConsumerWidget` → `StatelessWidget`
- `app_router.dart`: Remove `@riverpod`, use plain GoRouter instance
- `supabase_provider.dart`: Remove (use static access or GetIt)

### B-2. If Flat structure is chosen

- `lib/core/router/app_router.dart` → `lib/core/router.dart`
- `lib/core/theme/app_theme.dart` → `lib/core/theme.dart`
- `lib/core/providers/supabase_provider.dart` → `lib/core/supabase.dart`
- `lib/core/constants/app_constants.dart` → `lib/core/constants.dart`
- `lib/core/extensions/context_extensions.dart` → `lib/core/extensions.dart`
- Update all import paths accordingly.

### B-3. If freezed is NOT chosen

- Use plain Dart classes with manual `fromJson`
- Remove `freezed`, `freezed_annotation` from dependencies
- Remove `*.freezed.dart` excludes from `analysis_options.yaml`

### B-4. If dark mode is disabled

- Remove `dark` getter from `app_theme.dart`
- Remove `darkTheme` from `MaterialApp.router`

---

## Constraints

- **NEVER generate feature code** — no pages, widgets, repositories, or business logic
- **NEVER ask for Supabase credentials** — that is `/flutter-supabase-setup:env`
- **NEVER skip user confirmation** — Phase 4 must complete before Phase 5
- **NEVER ask unnecessary questions** — 기획서에서 추론 가능한 정보(프로젝트명, 조직명, 플랫폼 등)는 직접 제안하고 확인만 받는다
- **NEVER ask for the planning document** — 프로젝트 폴더에서 자동 탐색한다. 못 찾을 때만 경로를 요청
- **ALWAYS respond in Korean**
- **ALWAYS run verification** (Phase 7) before declaring complete
- **ALWAYS use `flutter pub add`** for dependencies
