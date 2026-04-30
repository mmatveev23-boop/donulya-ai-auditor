# ДоНуля · AI Аудитор v4

## Что это за проект

Single-file HTML мокап операционного дашборда для отдела продаж компании **ДОНУЛЯ** (списание долгов по 127-ФЗ — БФЛ + РДГ). Используется как **референс для прогера** при разработке боевого дашборда и как playground для итераций UI/UX.

«Operational system», не «витрина метрик» — каждый блок служит конкретному действию (RUS): директор калибрует AI-промт, РОПы разбирают disputes, охотники/клоузеры видят свою ЗП и зоны роста.

**Триггер для AI** при работе с проектом: `@донуля` или фраза «продолжаем донуля» в чате.

## 3 роли

| Роль | Кто | Воронка | Основные метрики |
|---|---|---|---|
| 🎯 Охотники (МПП1) | 18-20 чел., продают 99₽ трипваер | 4-этапная (Лид → Дозвон → Квалификация → Оплата 99₽) | Дозвон, квалификация, SLA |
| 💼 Клоузеры (МПП2) | 9 чел., продают основную услугу 110-170к ₽ | Встреча 30-60 мин → Договор → Оплата | Закрытия, калькулятор РДГ, БРАК |
| 👑 Директор | 1, общий вид | Обе воронки | LTV/CAC, расторги, точность AI |

## 7 страниц + Карточка звонка

1. **Сводка** — 4 KPI + воронка + AI-оценка + блок «Требуют разбора»
2. **Звонки/Встречи** — список с фильтрами, AI-вердиктом, балл, причины
3. **Аналитика** — все метрики с sparkline (адаптивная гранулярность дни/недели/месяцы по выбранному периоду 7/14/30/90/180д)
4. **Планы** — KPI цели на период
5. **🤖 Промт охотников** (только директор) — `qa_hunter v5.5` Gate + QA, 12 disputes, бенчмарк на 19 эталонах, выбор модели YandexGPT 5.1 Pro / GPT-5 nano
6. **🤖 Промт клоузеров** (только директор) — `qa_closer v1.4.2`, 8 disputes, бенчмарк, переключение моделей
7. **Личный кабинет** (3-целевой принцип ЗП / Звонки / Обучение) — для сотрудника
8. **Карточка звонка** (drill-down) — плеер, транскрипт, AI-Скоркард с цитатами и тайм-кодами, точечный HITL по 7 критериям

## Стек

- **Pure HTML + CSS + Vanilla JS** — никаких фреймворков, no build, no npm
- **~12 800 строк / ~770 KB** в одном файле `index-v4.html`
- Тёмная тема с дизайн-токенами (CSS-переменные)
- Sparkline в стиле Apple Health: цветные кружки с tier-цветом (low/mid/high), подписи значений как badge
- Inline mini-player + транскрипт в правой колонке промт-страниц (P1-4 паттерн «evidence quotes with timestamps» — наш unique moat над off-the-shelf prompt-eval тулзами)

## Как запустить

```bash
# Любой статичный HTTP-сервер (Python — самый простой):
cd /путь/к/donulya-dashboard
python3 -m http.server 5717

# Открыть:
open http://localhost:5717/index-v4.html
```

Или использовать Claude Preview через `.claude/launch.json` (уже настроен на порт 5717).

## Версии

| Файл | Что |
|---|---|
| `index-v4.html` | **Актуальный.** v4 → operational system. AI Coach, Стена возражений, Антисаботаж, AI-Скоркард в карточке звонка, Конструктор qa-промпта (заменён на 2 страницы Промт охотников/клоузеров), HITL Feedback Loop, Аналитика с адаптивной гранулярностью. |
| `index-v3.html` | v3, funnel-first для 3 ролей. Заменён на v4. |
| `index-v2.html` | v2, после первого редизайна. Архив. |
| `index.html` | v1, исходник. Архив. |

## Стандарт калибровки

Калибровка AI-промта (qa_hunter / qa_closer) проводилась на 19 эталонных встречах клоузеров: Камран, Куделькин, Лим (Тамара / Олег СВО), Ляшенко, Марченко (продажа / отказ), Петрова. Транскрипты + 38 жёлтых штрафов ОКК + 18 красных расторгов — основа для калибровки.

Реальные тексты промтов и транскрипты живут в **iCloud копии call-quality** (не в этом репо):
- `~/Library/Mobile Documents/com~apple~CloudDocs/CLAUD/call-quality/docs/qa_closer_v1_production.md`
- `~/Library/Mobile Documents/com~apple~CloudDocs/CLAUD/call-quality/docs/qa_prompts_v5_5_production.md`
- `~/Library/Mobile Documents/com~apple~CloudDocs/CLAUD/call-quality/docs/scripts/mpp_*.md`
- `~/Library/Mobile Documents/com~apple~CloudDocs/CLAUD/call-quality/docs/transcripts_closer/*.srt`

## Дизайн-паттерны

Применённые UI-паттерны индустрии prompt-eval тулзов (LangSmith / Braintrust / Humanloop / Vellum / Helicone / Anthropic Console):

1. **Annotation queue** для disputes (prev/next, hotkeys J/K, auto-next после resolve)
2. **Side-by-side old vs new diff** после re-run одного звонка
3. **Regression view** бенчмарка с сортировкой по delta (worst regressions сверху)
4. **Evidence quotes with timestamps** — кликабельные цитаты прыгают в плеере к моменту (наш unique moat — call-domain)
5. **Live-preview re-run** debounced (Pattern Anthropic Workbench / Braintrust)
6. **Two-level prompt abstraction** — стабильные критерии vs калибровочные правки (PromptHive HCI)
7. **SME framing** — «Калибровка» вместо «Черновик», «Применить» вместо «Опубликовать»

Полный research: см. session note `2026-04-27 UI-аудит промт-страниц` в knowledge-vault.

## Стиль кода

- **Vanilla**: никаких React / Vue / jQuery. Всё через `document.querySelector` + `addEventListener`.
- **CSS**: тёмная тема, дизайн-токены (`--bg-1`, `--text-1`, `--green`, `--red`, `--yellow`, `--blue` etc), `font-feature-settings: "tnum"` для табличных цифр.
- **Никаких inline-стилей за исключением динамических координат** (например absolute positioning sparkline-точек).
- **Тексты промтов** хранятся в `<script type="text/plain" id="prompt-...">` блоках в конце файла, JS подгружает в textarea.

## Связанные репо

| Репо | Что |
|---|---|
| `mmatveev23-boop/donula-partnerka` (private) | Backend amo-partner-bridge — Python FastAPI, боты TG/MAX/VK, chat-relay amoCRM ↔ мессенджеры |
| `mmatveev23-boop/donulya-partner` (public) | Партнёрский лендинг ДОНУЛЯ |
| **этот репо** | UI-мокап AI-аудитора звонков (v4) |

## Не для этого репо

- Транскрипты звонков (живут в iCloud)
- Реальные тексты qa-промтов в `.md` (живут в iCloud)
- Логи / БД / .env — у нас нет бэкенда в этом репо
- Production credentials — никогда

## Правила работы AI с этим проектом

1. **Прежде чем править index-v4.html** — открой в Preview через `python3 -m http.server 5717` и сделай скриншот. Файл большой (770 KB) — лучше grep'ом находить нужное место чем читать целиком.
2. **Sparkline-логика** в `redrawAnalyticsSparklines()` (около строки 9900) — детерминированный mock-генератор по seed = ID плитки + period + granularity.
3. **Промт-страницы** (около строк 8000-8800) — 3-колоночный layout disputes / textarea / контекст. Тексты промтов в `<script type="text/plain">` блоках в конце файла.
4. **Карточка звонка** — drill-down с плеером и транскриптом, AI-Скоркард со встроенным HITL по каждому критерию.
5. **Бэкап перед крупной правкой:** `cp index-v4.html index-v4.html.bak-before-<тема>` (бэкапы в .gitignore).
6. **После правок** — обновляй [knowledge-vault/Projects/call-quality/index.md](https://github.com/mmatveev23-boop/) (живёт в iCloud, не в репо).
