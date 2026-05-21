---
name: env
description: "Flutter + Supabase 프로젝트의 Supabase 크레덴셜(URL, Anon Key)을 설정한다. .env 파일을 생성/업데이트하고 연결 상태를 확인한다. Use when: user wants to configure Supabase credentials, says 'Supabase 연결', 'env 설정', 'credentials'. Do NOT use when: project doesn't exist yet (run init first)."
---

# Flutter + Supabase Setup — env

Configures Supabase credentials for a Flutter project created by `/flutter-supabase-setup:init`.

> **Language**: Always respond in **Korean (한국어)**.

---

## Pre-check

1. Verify `pubspec.yaml` exists in the current directory (or ask the user to `cd` into the project)
2. Verify `supabase_flutter` is in dependencies
3. If either check fails → guide the user to run `/flutter-supabase-setup:init` first

## Step 1 — Collect Credentials

Ask in a single message:

```
Supabase 크레덴셜을 입력해주세요:

1. **Supabase URL** (대시보드 → Settings → API → Project URL):
2. **Supabase Anon Key** (대시보드 → Settings → API → anon/public key):
```

## Step 2 — Write .env

Create or overwrite `.env`:
```
SUPABASE_URL={user_provided_url}
SUPABASE_ANON_KEY={user_provided_key}
```

Update `.env.example` if values are still empty:
```
SUPABASE_URL=your_supabase_url_here
SUPABASE_ANON_KEY=your_supabase_anon_key_here
```

Verify `.env` is in `.gitignore`. If not, append it.

## Step 3 — Verify

1. Verify `.env` file exists and both values are non-empty
2. Run `flutter analyze` to confirm no build issues
3. Report:

```
## Supabase 설정 완료

| 항목 | 결과 |
|---|---|
| .env 파일 생성 | ✅ |
| .gitignore 확인 | ✅ |
| flutter analyze | ✅ / ❌ {details} |

이제 `flutter run --dart-define-from-file=.env`로 실행할 수 있습니다.
```

## Constraints

- **NEVER commit .env** — always verify .gitignore
- **NEVER modify project structure** — only .env and .gitignore
- **ALWAYS respond in Korean**
