---
id: PD.SPEC.001
type: spec
name: "Контракт контекста Портного (Tailor Context Contract)"
status: draft
created: 2026-03-30
updated: 2026-08-25
wp: WP-149 (Блок А)
related:
  consumed_by: [MIM.SOP.001]
  realized_by: [WP-151, WP-222, WP-245]
  upstream: [SA.CAT.001, PD.CAT.002, PD.CAT.003, PD.FORM.003, PD.FORM.081]
  applies_distinctions: ["HD #27 Персона/Память/Контекст", "HD #49 MCP-сервис ≠ Роль"]
summary: >
  Спецификация формата входных данных Портного (R27 в DP.ROLE.001): какие поля
  читаются из Персоны, Памяти и текущей работы, в каком формате, какое поведение
  при отсутствии данных. Контракт поддерживает выбор развивающего шага внутри
  реального РП, проекта или рабочего продукта; отдельное занятие — частный случай.
  Контракт = объект-формат (это часть Pack — что существует). Способ доставки
  (psycopg2 directly / MCP tool) — деталь реализации носителя роли (WP-222).
---

# PD.SPEC.001 — Контракт контекста Портного

> **Принцип least privilege:** Портной читает минимально необходимый срез Памяти.Derived.
> Полная Память.Derived пользователя — в Neon `indicators` schema (WP-253). Портной получает только этот контракт.
> **Owner-Integrity:** этот файл = требования (что нужно как **объект-формат**). Реализация хранения → WP-151. Текущий носитель Портного → WP-222 (`render-pilot-guides.py` + `planner.py`).

---

## 1. Назначение

Этот документ отвечает на вопрос: **какой минимально необходимый формат данных получает Портной перед встраиванием развивающего шага в текущую работу?** Персональное руководство или отдельное занятие — лишь один из возможных носителей такого шага.

SOP.001 описывает алгоритм («как»). Этот файл описывает вход («что»).
Контракт = объект-формат (поля, типы, fallback). Способ получения (текущий: psycopg2 → Neon `indicators` directly; будущий: MCP tool через Gateway) — деталь реализации носителя, не часть контракта.

**Принцип роль ≠ исполнитель** (FPF, distinctions.md):
- Роль R27 «Портной» определена в `DP.ROLE.001` (PACK-digital-platform) — переносима между носителями.
- Контракт `tailor_context` (этот файл) — объект-формат, неизменен при смене носителя.
- Текущий носитель: Python-скрипт `render-pilot-guides.py` (`DS-autonomous-agents`).
- Будущий носитель: ИИ-агент с tool-use циклом (зависит от WP-150 Ф6 Unified MCP Gateway).

---

## 2. Полный формат `tailor_context`

```yaml
tailor_context:
  schema_version: 2         # int. v2 добавляет развитие внутри текущей работы

  # ── Профиль (L1 declarative, самооценка) ──────────────────────────────────
  student_stage: 1          # int 0–4. Ступень ученика (PD.FORM.003)
  it_level: 1               # int 0–3. ИТ-уровень (SOP.001 §Scaffolding)
  dominant_role: "learner"  # str. Из PD.FORM.087: learner | intellectual |
                            #        professional | researcher | enlightener
  style:
    format: "brief"         # str: brief | detailed | analogies
    duration_min: 20        # int. Целевая длительность занятия в минутах

  # ── Личный контекст (Персона, только с согласия пользователя) ─────────────
  # Передаётся только срез, релевантный текущей работе. Отсутствие поля = omit,
  # а не разрешение Портному вывести личный факт или подставить его самому.
  personal_inputs:          # dict | null. Опционально
    consent_scope: "current_work"  # str. На что дано согласие в этой сессии
    goals:                  # list[dict]. Только выбранные пользователем цели
      - ref: "GOAL-NNN"     # str | null. Ссылка на декларацию Персоны
        statement: "Желаемое изменение"  # str. Минимальный нужный фрагмент
    dissatisfactions:       # list[dict]. Разрывы «сейчас → желаемое»
      - ref: "DISS-NNN"     # str | null
        statement: "Что не устраивает сейчас"  # str

  # ── Квалификация МИМ (отдельно от ступени и состояний) ───────────────────
  qualification_mim:       # dict | null. Снимок по канонической модели МИМ
    model_ref: "MIM.M.004" # str. Модель квалификации; не заменяется student_stage
    dimensions:            # dict. Ключи задаёт upstream-модель, не этот контракт
      "dimension-id":
        level: null         # str | int | null
        source: null        # str | null. memory_derived | persona_self_assessment
        as_of: null         # str YYYY-MM-DD | null

  # ── Шесть независимых состояний пользователя ─────────────────────────────
  # state выше (chaos/stuck/turn/development) — legacy-сигнал занятия и НЕ
  # является заменой этого вектора. Неизвестная ось остаётся null.
  state_axes:
    permission:  {state_id: null, source: null, as_of: null}
    belonging:   {state_id: null, source: null, as_of: null}
    engagement:  {state_id: null, source: null, as_of: null}
    mastery:     {state_id: null, source: null, as_of: null}
    community:   {state_id: null, source: null, as_of: null}
    mentorship:  {state_id: null, source: null, as_of: null}

  # ── Состояние (L1 или L3 derived) ────────────────────────────────────────
  state: "development"      # str: chaos | stuck | turn | development
                            # L3 приоритетнее L1. Fallback: "development"
  energy: 4                 # int 1–5. Самооценка. Fallback: 3
  phase: 1                  # int 1–4. Вычислено из student_stage (SOP.001 Шаг 2)
                            # 0→Ф1, 1→Ф1/Ф2, 2→Ф2/Ф3, 3→Ф3/Ф4, 4→Ф4

  # ── Мастерство по областям (L3 derived или L1 fallback) ──────────────────
  # Текущая максимальная глубина/степень, достигнутая по каждой области.
  # Bloom 0–5 для мемов (worldview). Степень 0–4 для практик (mastery).
  mastery_by_area:
    knowledge: 2            # int 0–5 (Bloom). Область 1
    tools: 1                # int 0–5. Область 2
    constraints: 2          # int 0–5. Область 3
    environment: 1          # int 0–5. Область 4
    organism: 1             # int 0–5. Область 5

  # ── История (L2 collected, последние 10 занятий) ─────────────────────────
  # Маппинг из схемы БД: direction(1–6) → area(1–5), topic_id → element_id,
  # bloom_level → depth. Поле area = int 1–5 (v3: 5 областей, не 6 направлений).
  last_area: 3              # int 1–5. Область вчерашнего занятия (для ротации)
  recent_history:           # list[dict]. Последние 10 записей из 2_10.
    - element_id: "M-042"   # str. ID мема (M-NNN) или практики (A1, Б2, METHOD.NNN)
      element_type: "meme"  # str: meme | practice
      area: 3               # int 1–5 (маппинг: direction 6→5 при миграции на v3)
      depth: 1              # int 1–5 (Bloom) или 1–4 (степень)
      passed: true          # bool. Can-do подтверждены Оценщиком
      errors: []            # list[str]. Коды ошибок из БД (пустой = нет ошибок)
      rating: 1             # int | null. 1=👍, -1=👎, null=нет оценки (WP-151)
      date: "2026-03-28"    # str YYYY-MM-DD

  # ── GAP-профиль (L3 derived) ──────────────────────────────────────────────
  # Элементы с gap > 0: текущая глубина < целевая для текущей фазы.
  # Используется в SOP.001 Шаг 4b (Bottleneck-first).
  worldview_gaps:           # list[dict]. Мемы CAT.001 с gap > 0
    - id: "M-001"
      area: 1
      current_depth: 0      # int 0–3. Текущая глубина (0 = не начат)
      target_depth: 2       # int 1–3. Целевая по фазе (из PD.FORM.080)
      can_do_passed: false  # bool. Mastery-gate: прошёл ли can-do текущей глубины

  mastery_gaps:             # list[dict]. Практики CAT.002/CAT.003 с gap > 0
    - id: "A1"
      area: 5
      catalog: "CAT.002"    # str: CAT.002 | CAT.003
      current_degree: 0     # int 0–4. Текущая степень (0 = не начата)
      target_degree: 2      # int 1–4. Целевая по фазе
      can_do_passed: false  # bool. Mastery-gate

  # ── Профессиональный домен (L1 declarative) ───────────────────────────────
  domain: "backend development"  # str | null. Для адаптации примеров (SOP.001 Шаг 6)

  # ── Реальная работа пользователя (L3 Контекст, опционально) ───────────────
  # Активные work products и фокус недели из governance-репо пилота (если есть).
  # Назначение: Портной показывает связь учебной темы с актуальной задачей пилота.
  # Если пилот не ведёт governance-репо (типично на ранних ступенях) — поле omit
  # или {}; Портной работает без этого сигнала (legacy-режим).
  strategy_inputs:                   # dict | null. Опционально
    week_focus: "Закрыть архитектурную развилку X"  # str | null. Фокус недели
    projects:                        # list[dict] | []. Активные проекты
      - id: "PROJECT-NNN"            # str
        title: "Краткое название"    # str
    active_wp:                       # list[dict] | []. Активные work products
      - id: "WP-NNN"                 # str. Идентификатор work product
        title: "Краткое название"    # str. ≤80 символов
        phase: "Ф2"                  # str | null. Текущая фаза, опционально
    current_work_product:            # dict | null. Что должно измениться сейчас
      id: "WP-NNN:artifact"          # str | null
      kind: "concept"                # str | null
      desired_change: "Развести проблему, требования и решение"  # str
```

---

## 3. Правила fallback

| Поле | Условие | Fallback |
|------|---------|---------|
| `student_stage` | L3 не вычислен | Из L1 самооценки. Если нет → `0` (Случайный) |
| `it_level` | Нет в профиле | `0` |
| `dominant_role` | Нет в профиле | `"learner"` |
| `style.format` | Нет в профиле | `"detailed"` |
| `style.duration_min` | Нет в профиле | `20` |
| `state` | Нет в Памяти.Derived | `"development"` |
| `energy` | Нет в Памяти.Derived | `3` |
| `phase` | — | Всегда вычисляется из `student_stage` по таблице SOP.001 Шаг 2 |
| `mastery_by_area.*` | Нет истории | `0` для всех областей |
| `last_area` | Нет истории | `null` (ротация не применяется) |
| `recent_history` | Нет истории | `[]` (новый пользователь) |
| `worldview_gaps` | L3 не вычислен | `[]` — Портной сам фильтрует CAT.001 по фазе |
| `mastery_gaps` | L3 не вычислен | `[]` — Портной сам фильтрует CAT.002+CAT.003 по фазе |
| `domain` | Нет в профиле | `null` (Портной не адаптирует примеры по домену) |
| `personal_inputs` | Нет согласия или релевантных деклараций | omit. Не выводить цели или неудовлетворённости из поведения |
| `qualification_mim` | Нет снимка с provenance | `null`. Не заменять `student_stage` или общей оценкой |
| `state_axes.*` | Ось неизвестна или не готова к исполнимому выбору | `{state_id: null, source: null, as_of: null}`. Не подставлять legacy `state` |
| `strategy_inputs` | Нет governance-репо | `{}` или omit — Портной работает без связки с рабочими задачами (legacy-режим). См. WP-364 Развилка 1 |

**Правило для нового пользователя (student_stage = 0, пустая история):**
Портной получает `tailor_context` с максимальным fallback-набором: все области = 0,
`worldview_gaps = []`, `mastery_gaps = []`. При пустых gaps Портной (SOP.001 Шаг 4b)
выбирает из каталогов по фазе напрямую — все элементы фазы 1 имеют `current_depth = 0`,
значит все являются gap. Это корректное состояние, не ошибка.

> **Различение `[]` vs предвычисленный список:**
> Если L3 вычислен → endpoint возвращает только элементы с gap > 0 (быстрый путь).
> Если L3 не вычислен → `[]` → Портной работает напрямую с каталогом (медленный путь).
> Оба пути дают одинаковый результат. После WP-151 все пользователи на быстром пути.

---

## 4. Минимально необходимый набор (MVP)

Для запуска первого занятия достаточно:

```yaml
tailor_context:
  student_stage: 0
  it_level: 0
  dominant_role: "learner"
  style: {format: "detailed", duration_min: 20}
  state: "development"
  energy: 3
  phase: 1
  mastery_by_area: {knowledge: 0, tools: 0, constraints: 0, environment: 0, organism: 0}
  last_area: null
  recent_history: []
  worldview_gaps: []    # [] = L3 не вычислен. Портной фильтрует CAT.001 по фазе сам (§3)
  mastery_gaps: []      # [] = L3 не вычислен. Портной фильтрует CAT.002/003 по фазе сам
  domain: null
```

Этот набор Портной получает при отсутствии данных в ЦД (onboarding, первое занятие).

### 4.1. Минимум для развития внутри реальной работы (v2)

Для режима «работа → развитие» одного учебного профиля недостаточно. Контракт должен содержать `schema_version: 2`, явный `personal_inputs.consent_scope` и `strategy_inputs.current_work_product.desired_change`. `qualification_mim` и каждая из `state_axes` могут быть `null`, но обязаны сохранять семантику «неизвестно», а не получать универсальный fallback.

Если текущая работа известна, а личный контекст не разрешён, Портной может помочь с рабочим продуктом без персонального вывода. Если развивающий шаг не улучшает текущую работу, корректный результат выбора — `no_intervention`.

---

## 5. Источник данных для каждого поля (после WP-268 cut-over)

> Терминология обновлена: «ЦД L1/L2/L3» → Персона/Память/Контекст (HD #27, DP.D.052). «digital_twins/dt_calc.py» → `indicators` schema + projection-worker WP-253.

| Поле | Слой | Источник в Neon | Кто пишет |
|------|------|-----------------|-----------|
| `student_stage` | Память.Derived | `indicators.calculated_profile.indicators` (FORM.093 graduate_profile) | projection-worker (WP-253), B-lite через FORM.093 |
| `it_level` | Персона | declarative profile (бот онбординг) | Онбординг (бот) |
| `dominant_role` | Персона | declarative profile | Навигатор / онбординг |
| `style.*` | Персона | declarative profile | Онбординг / настройки |
| `state` | Память.Derived ∨ Персона | `indicators.calculated_profile.state` ∨ declarative | Расчёт ∨ сам пользователь |
| `energy` | Персона | daily check-in | Ежедневный check-in в боте |
| `mastery_by_area` | Память.Derived | `indicators.calculated_profile.mastery_by_area` | projection-worker из `learning.domain_event` |
| `last_area` | Память (Observed) | `learning.domain_event` (last entry where event_type=lesson_completed) | event-gateway / aist_bot (WP-268 cut-over) |
| `recent_history` | Память (Observed) | `learning.domain_event` (последние 10 событий за 7 дней — B2) | event-gateway |
| `worldview_gaps` | Память.Derived | `indicators.calculated_profile.worldview_gaps` | projection-worker (WP-151) |
| `mastery_gaps` | Память.Derived | `indicators.calculated_profile.mastery_gaps` | projection-worker (WP-151) |
| `rcs_profile` | Память.Derived | `indicators.calculated_profile.rcs_current` (7 слотов FORM.089) | projection-worker (WP-151 Ф12) |
| `domain` | Персона | declarative profile | Онбождинг / настройки |
| `personal_inputs.goals` | Персона | Разрешённые декларации целей | Пользователь или агент по поручению с явным принятием |
| `personal_inputs.dissatisfactions` | Персона | Разрешённые декларации неудовлетворённостей | Пользователь или агент по поручению с явным принятием |
| `qualification_mim` | Память.Derived ∨ Персона | Официальный снимок квалификации ∨ самооценка с `source` и `as_of` | Расчёт квалификации ∨ пользователь |
| `state_axes.*` | Память.Derived ∨ Персона | Снимок оси ∨ самооценка с `source` и `as_of` | Проекционный сервис ∨ пользователь |
| `strategy_inputs` | Контекст | governance-репо: активные проекты, РП, недельный фокус и текущий рабочий продукт | Пользователь (стратегирование). Опционально: если репо нет — `{}`. Источник решения: WP-364 Развилка 1 (29 мая 2026) |

---

## 6. Способы получения `tailor_context` (носители роли)

> **Принцип:** контракт = объект-формат (§2). Способ доставки — деталь реализации носителя. Контракт неизменен при смене носителя.

### 6.1. Текущий носитель: Python-скрипт (psycopg2 directly)

**Реализация:** `DS-autonomous-agents/scripts/render-pilot-guides.py` функции `get_rcs_profile()` + `get_recent_events()`.

**Доступ:** `psycopg2.connect(NEON_INDICATORS_URL)` → unpooled connection → SQL по `indicators.calculated_profile`, `indicators.digital_twins`, `learning.domain_event`. RLS-policy: read-only через user_id фильтр в WHERE.

**Triggering:** systemd timer на tsekh-1 NixOS (ПН 05:00 / Вт-Вс 06:00 EEST).

**Fallback:** при пустом RCS — B-lite на FORM.093 stage+path (worldview_gaps=[], mastery_gaps=[]).

### 6.2. Будущий носитель: ИИ-агент через MCP Gateway (планируется)

**Реализация:** ИИ-агент в Claude tool-use loop. Tools: `dt_read_digital_twin` (Память.Derived), `personal_search` (PACK-personal), `knowledge_search` (PACK-MIM, каталоги).

**Доступ:** через `https://mcp.aisystant.com/` (Aisystant MCP / Gateway паттерн). OAuth 2.1 + JWT.

**Triggering:** Оркестратор (WP-203 Ф1.5) по 7 типам триггеров коррекции.

**Blocked by:** WP-150 Ф6 (Unified MCP Gateway в VS Code).

### 6.3. Требования к контракту (любой носитель)

- Возвращает полный объект (никогда None / 404)
- При отсутствии поля — применяет fallback из §3
- Атомарен: контракт выдаётся за одну операцию (для текущего носителя — один SQL-набор; для будущего — один проход tool-calls)
- Версионирован: `schema_version: 2` в payload; носитель, понимающий только v1, игнорирует новые опциональные поля
- Минимизирует личные данные: передаёт только поля в `consent_scope`, релевантные `current_work_product`
- Сохраняет provenance: квалификация и состояния без `source`/`as_of` считаются неизвестными

---

## 6b. Маппинг ключей областей

`mastery_by_area` использует именованные ключи. Соответствие номерам областей (PD.FORM.081):

| Ключ | Номер | Область |
|------|-------|---------|
| `knowledge` | 1 | Знания |
| `tools` | 2 | Инструменты |
| `constraints` | 3 | Ограничения |
| `environment` | 4 | Окружение |
| `organism` | 5 | Организм |

`last_area`, `recent_history[*].area`, `worldview_gaps[*].area`, `mastery_gaps[*].area` — все используют int 1–5 по той же таблице.

---

## 7. Соответствие полям SOP.001

| SOP.001 Шаг | Поля контракта |
|-------------|---------------|
| Шаг 1. Прочитать профиль | `student_stage`, `it_level`, `state`, `energy`, `style`, `domain`, `recent_history` |
| Шаг 2. Определить фазу | `phase` (вычислено из `student_stage`) |
| Шаг 3. Выбрать область | `mastery_by_area`, `state`, `energy`, `dominant_role`, `last_area` |
| Шаг 4. Выбрать элемент | `worldview_gaps`, `mastery_gaps`, `recent_history[*].passed` |
| Шаг 5–6. Собрать и адаптировать | `domain`, `style`, `it_level`, `recent_history[*].errors`, `recent_history[*].rating` |

### 7.1. Режим развития внутри работы (v2)

| Шаг | Поля контракта |
|-----|---------------|
| Уточнить наблюдаемый рабочий результат | `strategy_inputs.current_work_product`, `strategy_inputs.active_wp`, `strategy_inputs.projects` |
| Ограничить разрешённый личный контекст | `personal_inputs.consent_scope`, `personal_inputs.goals`, `personal_inputs.dissatisfactions` |
| Выбрать ближайший полезный разрыв | `qualification_mim`, `state_axes`, `worldview_gaps`, `mastery_gaps` |
| Встроить метод либо не вмешиваться | Все поля выше; допустимый выбор `no_intervention` |

---

## 8. Открытые вопросы (для WP-151)

1. **Сколько записей `recent_history`?** Предложение: последние 10. Достаточно для ротации (R1) и retrieval (R2). При необходимости — параметр в endpoint.
2. **Как вычисляется `target_depth` для gaps?** Ответ: из PD.FORM.080 (нормативная матрица ступень × область). WP-151 читает матрицу при расчёте gaps.
3. **Как обрабатывать конфликт L1 vs L3 для `state`?** Правило: L3 приоритетнее L1. Если L3 вычислен — брать L3. Если нет — L1. Если нет L1 — fallback `"development"`.
4. **`lesson_rating` и `completion_status`** — добавляются в `2_10_learning_history` (задача WP-151). После добавления — `recent_history` включает поле `rating`.

---

*Создан: 2026-03-30 | Верифицирован субагентом: CONDITIONAL PASS → правки внесены | WP-149 Блок А | Обновлён 2026-08-25: schema v2 для развития внутри реальной работы; добавлены разрешённый личный контекст, квалификация МИМ, шесть независимых состояний и текущий рабочий продукт без выдуманных fallback.*
