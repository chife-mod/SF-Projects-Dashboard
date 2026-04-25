# SF Projects Dashboard — Project Log

## Сессия 1 · Апрель 2026

### Задача
Создать дашборд-индекс всех веб-проектов Vsevolod для SF Group.
Требования: тёмная тема, плитки с обложками, статусы, ссылки на GitHub Pages.

---

### Что сделано

#### Дашборд (index.html)
- Создан единый static HTML-файл без сборщика
- Дизайн взят из Figma SF Group Site — токены, типографика, компоненты
- Хедер pixel-perfect из Figma (node `14:690`): два glass-pill блока
- CSS Grid карточек с обложками, badge, тегами, ссылками
- Две секции: **Live on GitHub Pages** (5 проектов) и **Local / Not yet deployed** (4 проекта)
- Статус-лейбл ("Published" / "In Progress") размещён над заголовком секции
- Badge всегда слева в карточке

#### Обложки (assets/covers/)
Скриншоты живых страниц — реальные, не заглушки:
- `wwg-novelties.png` — headless Chrome screenshot
- `watch360-linkedin-pdf.png` — headless Chrome screenshot
- `watch360-pdf-reports.png` — headless Chrome screenshot
- `market360-signal-selector.jpg` — скрин от пользователя (из чата)
- `support-icon.png` — headless Chrome screenshot

#### Деплой
- Создан публичный репозиторий `chife-mod/SF-Projects-Dashboard`
- GitHub Pages настроен из ветки `main`, корень
- Live: https://chife-mod.github.io/SF-Projects-Dashboard/

---

### Ключевые правки по ходу работы

| Что | Почему |
|---|---|
| Логотип → `<div>` вместо `<a>` | Ссылка вела на sf-group.com → французская компания Soletanche Freyssinet |
| Убран `.hero-divider` | Пользователь попросил убрать градиентную линию под подзаголовком |
| Badge перенесён влево | Изначально был справа от названия |
| Статус-лейбл над заголовком секции | Изначально был под заголовком |
| Hero title → одна строка | "Projects Dashboard" без переноса |
| `img src` `.webp` → `.png` | headless Chrome сохраняет в PNG |

---

### Исправленные баги в смежных проектах

#### Market360 Signal Selector
Репо переименовано `M360_C` → `Market360_Signal_Selector`, страница перестала загружаться.

**Диагностика:**
1. JS-чанки грузились из `/M360_C/_next/...` — неверный basePath
2. `.nojekyll` отсутствовал — Jekyll блокировал папку `_next/`
3. `NEXT_PUBLIC_BASE_PATH` не обновлён — иконки грузились по неверным путям

**Фикс:** `next.config.ts`:
```ts
basePath: "/Market360_Signal_Selector",
env: { NEXT_PUBLIC_BASE_PATH: "/Market360_Signal_Selector" }
```
+ `.nojekyll` в gh-pages ветку + пересборка + деплой через `gh-pages`.

---

### Незакрытые задачи

- [ ] Обложки для карточек "Local / Not yet deployed" — пока взяты из `.preview/cover.png`
- [ ] Обложки Watch360 LinkedIn PDF и WWG — пользователь присылал скрины в чате, но они ещё не заменены (используются headless-версии)
- [ ] Добавить новые проекты по мере деплоя

---

### Технические заметки

**Как извлечь скрин из чата Claude:**
Пользователь присылает скрин прямо в чат → он хранится в JSONL-файле сессии как base64.
Скрипт извлечения — в `CLAUDE.md` раздел "Обложки".

**Проблема headless Chrome + Next.js CSR:**
`--virtual-time-budget` не ждёт hydration React-приложений.
Для CSR-страниц нужен реальный браузер (Chrome MCP) или Playwright.

**Jekyll и `_next/`:**
GitHub Pages по умолчанию запускает Jekyll, который игнорирует папки с `_`.
Всегда добавлять `.nojekyll` в корень деплоя для Next.js/Vite/CRA проектов.
