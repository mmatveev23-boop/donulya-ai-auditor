# 🤖 ДоНуля · AI Аудитор v4

Single-file HTML дашборд AI-аудита звонков для отдела продаж компании ДОНУЛЯ.

> **Триггер для AI-агентов:** `@донуля` или «продолжаем донуля».
> **Полный контекст для AI:** см. [CLAUDE.md](./CLAUDE.md)

## TL;DR

- **Один HTML-файл**, ~12 800 строк, ~770 KB
- **Vanilla** HTML + CSS + JS, без билда, без npm
- **3 роли**: 🎯 Охотники (МПП1) · 💼 Клоузеры (МПП2) · 👑 Директор
- **8 страниц**: Сводка · Звонки · Аналитика · Планы · Промт охотников · Промт клоузеров · Личный кабинет · Карточка звонка (drill-down)

## Запустить

```bash
python3 -m http.server 5717
open http://localhost:5717/index-v4.html
```

## Структура файлов

```
.
├── index-v4.html              # ← актуальный, ~770 KB
├── index-v3.html              # v3 (архив)
├── index-v2.html              # v2 (архив)
├── index.html                 # v1 (исходный)
├── CLAUDE.md                  # инструкции для AI-агентов
└── .claude/launch.json        # конфиг Claude Preview сервера
```

## Что внутри

### 3 роли + переключение

Слева сайдбар «Роль»: Охотники / Клоузеры / Директор. Контент адаптируется (заголовки, метрики, видимость страниц).

### 7 базовых + 1 drill-down страница

| Страница | Что |
|---|---|
| **Сводка** | 4 KPI + воронка + AI-оценка + блок «Требуют разбора» |
| **Звонки/Встречи** | Список с фильтрами, AI-вердиктом, балл, причины |
| **Аналитика** | Все метрики со sparkline. Period switch (7/14/30/90/180д) + Granularity (Дни/Недели/Месяцы) с авто-дефолтом |
| **Планы** | KPI цели на период |
| **🤖 Промт охотников** *(только директор)* | qa_hunter v5.5 (Gate + QA), 12 disputes, бенчмарк, выбор LLM |
| **🤖 Промт клоузеров** *(только директор)* | qa_closer v1.4.2, 8 disputes, бенчмарк, выбор LLM |
| **Личный кабинет** | 3-целевой ЛК сотрудника: ЗП / Звонки / Обучение |
| **Карточка звонка** *(drill-down)* | Плеер + транскрипт + AI-Скоркард с цитатами и точечный HITL |

### Ключевые UI-паттерны

- **Apple Health style sparkline** — кружки с tier-цветом (low/mid/high), badge-подписи значений
- **Annotation queue** для disputes (prev/next, hotkeys J/K, auto-next)
- **Old vs New diff** после re-run одного звонка
- **Regression view** бенчмарка (worst regressions сверху)
- **Evidence quotes с timestamps** — кликабельные цитаты прыгают в плеере к моменту
- **Live-preview re-run** debounced (1.5 сек после правки промта)

Источники: LangSmith, Braintrust, Humanloop, Vellum, Helicone, Anthropic Console + HCI research (PromptHive arXiv 2410.16547).

## Скриншоты

Скриншоты в работе. Можно открыть локально через `python3 -m http.server` для интерактивного просмотра.

## Версии

| Версия | Дата | Что |
|---|---|---|
| v4 | 2026-04-27..28 | AI Coach Prescriptive · промт-страницы closer/hunter · GPT-5 nano как модель · Аналитика с адаптивной гранулярностью · Apple Health sparkline · live-preview re-run · мини-плеер с транскриптом |
| v3 | 2026-04-25 | funnel-first для 3 ролей |
| v2 | 2026-04 | первый редизайн |
| v1 | 2026-04 | исходник |

## Лицензия

Internal — ДОНУЛЯ.

## Связанные проекты

- [`donula-partnerka`](https://github.com/mmatveev23-boop/donula-partnerka) — backend amo-partner-bridge (Python FastAPI, боты)
- [`donulya-partner`](https://github.com/mmatveev23-boop/donulya-partner) — партнёрский лендинг
