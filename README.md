# NestJS Template

Стартовый шаблон для NestJS-сервисов.

## Стек

- **NestJS 12** + **TypeScript 6** (strict)
- **ESLint 10** (flat config, type-checked) + **Prettier**
- **Husky** + **lint-staged** + **commitlint** (Conventional Commits)
- **@nestjs/config** + **zod** — валидация env при старте
- **nestjs-pino** — структурные JSON-логи (pretty в dev)
- **@nestjs/terminus** — `/health/live`, `/health/ready`
- **Swagger** — `/docs` (включается `SWAGGER_ENABLED=true`)
- **helmet**, CORS, глобальный `ValidationPipe`, URI-версионирование (`/api/v1/...`)
- Глобальный exception filter с единым форматом ошибок
- **Jest** (unit + e2e), Docker (multi-stage, non-root, healthcheck), GitHub Actions CI, Dependabot

## Требования

Node.js **24.9+** (см. `.nvmrc`). Nest 12 поставляется как ESM, а Jest грузит его через нативный `require(esm)`, который есть только в Node 24.9+.

## Быстрый старт

```bash
nvm use
npm install
cp .env.example .env
npm run start:dev
```

## Скрипты

| Скрипт                               | Описание                       |
| ------------------------------------ | ------------------------------ |
| `npm run start:dev`                  | запуск в watch-режиме          |
| `npm run build`                      | сборка в `dist/`               |
| `npm run start:prod`                 | запуск сборки                  |
| `npm run lint` / `lint:check`        | ESLint с автофиксом / проверка |
| `npm run format` / `format:check`    | Prettier                       |
| `npm run typecheck`                  | `tsc --noEmit`                 |
| `npm test` / `test:e2e` / `test:cov` | тесты                          |

## Git-хуки

- `pre-commit` → `lint-staged` (eslint + prettier на изменённых файлах, typecheck при изменении `.ts`)
- `commit-msg` → `commitlint` (`feat: ...`, `fix(scope): ...`, `chore: ...`)

## Структура

```
src/
├── common/filters/     # глобальные фильтры, guards, interceptors
├── config/             # схема env (zod) и типизированный AppConfigService
├── modules/health/     # health-check
├── app.module.ts
├── setup-app.ts        # общая настройка приложения (main + e2e)
└── main.ts
test/                   # e2e-тесты
```

## Использование как шаблона

1. Скопируйте репозиторий (или отметьте его как _Template repository_ на GitHub).
2. Поменяйте `name` / `description` в `package.json`.
3. Новые переменные окружения добавляйте в `src/config/env.validation.ts` и `.env.example`.
4. Проверки БД / Redis / внешних сервисов добавляйте в `HealthController.ready()`.
