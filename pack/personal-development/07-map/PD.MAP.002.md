---
id: PD.MAP.002
name: Personal Development Pattern Map
scope: cross-cutting (03-methods, 05-failure-modes, 02-domain-entities/principles, 06-sota)
created: 2026-09-08
last_updated: 2026-09-08
generated: false
source_session: "MC-sessions:2026-09/08/2026-09-08-21-wp385-lr-patterns-fpf"
related_wp: "WP-565"
---

# [PD.MAP.002] Personal Development Pattern Map

Восемь стержневых паттернов личного развития: устойчивых конфигураций «контекст + противоборствующие силы → принцип разрешения», каждая опирается на несколько уже существующих методов и анти-паттернов пака вместо того, чтобы вводить новую сущность на каждую комбинацию. Собрана по итогам пир-сессии (Claude+Kimi+Codex, 4 раунда, консенсус без эскалаций) — источник во frontmatter.

## Онтология и связь с FPF (принятая модель сочетания)

Паттерны здесь — доменные специализации родовых функциональных паттернов Анатолия Левенчука (FPF), а не параллельная онтология. Границы:

- **Первоисточник FPF не копируется.** Каждый паттерн ссылается на конкретный файл и раздел в `~/IWE/FPF/`, с зафиксированным коммитом на момент цитирования — FPF-репозиторий обновляется почти ежедневно, живая ссылка без снимка версии через месяц может означать другое.
- **Формат ссылки:** `<путь-в-FPF>#<якорь-или-код> (FPF @<короткий-хэш>, <дата>)`.
- **При конфликте с FPF выигрывает FPF**, если у пака нет задокументированной эмпирической причины разойтись (см. паттерн 1 — здесь причина есть и уже совпадает с существующими анти-паттернами пака, не расхождение, а подтверждение).
- **Два паттерна без прямого аналога** в FPF (2 и 5) помечены «доменный оригинал» — не потому что хуже обоснованы, а потому что развитие способностей в FPF (свод HCD.1-19) пока не выделяет их отдельно. Пересматривать на следующей содержательной сверке с апстримом, не по расписанию.
- **Ступень мастерства (контракт РП380, ступени 1-5) — не агрегатный балл.** FPF (HCD.4) явно предупреждает: «overall mastery: 72%» скрывает, какие именно вклады подтверждены. Здесь ступень — метка диапазона применения паттерна, program-binding (какая ступень → какой паттерн → какой сигнал) — отдельная секция в `DS-my-strategy/docs/personal-dev-program-architecture.md`, не агрегат внутри этой карты.
- **Сигнал называется, не реализуется здесь.** Пак фиксирует, какой наблюдаемый признак относится к паттерну; сама метрика (WakaTime, событие в цифровом двойнике, ступень из диагностики) — часть продукта, не пака.

---

## 1. Налёт часов практики

**Проблема/силы:** развитие требует накопления часов в целевой деятельности, но «срочное» вытесняет «важное» (см. `PD.PRINC.041`), а субъективная оценка потраченного времени искажена без учёта.

**Методы пака:** [PD.METHOD.001](../03-methods/PD.METHOD.001-time-accounting.md) (учёт времени), [PD.METHOD.012](../03-methods/PD.METHOD.012-day-rhythm.md) (ритм дня), [PD.METHOD.025](../03-methods/PD.METHOD.025-planning-cascade.md) (каскад планирования).

**Анти-паттерны пака:** [PD.FAIL.001](../05-failure-modes/PD.FAIL.001-time-accounting-is-pomodoro.md)-[004](../05-failure-modes/PD.FAIL.004-time-accounting-is-productivity-hack.md) (учёт времени — не помодоро/не дисциплина/не контроль/не продуктивити-хак), [PD.FAIL.079](../05-failure-modes/PD.FAIL.079-practice-without-deliberate-work.md) (практика без осознанной работы), [PD.FAIL.036](../05-failure-modes/PD.FAIL.036-overtraining-without-recovery.md) (перетренированность без восстановления), [PD.FAIL.010](../05-failure-modes/PD.FAIL.010-high-accounting-without-leisure.md) (высокий учёт без досуга).

**Уже зафиксированная SoTA-оговорка:** [PD.SOTA.011](../06-sota/PD.SOTA.011-deliberate-practice-replication-failure-2019.md) — исходный эксперимент о целенаправленной практике не подтверждён прямой репликацией (Macnamara & Maitra, 2019); часы сами по себе не гарантия способности.

**FPF-первоисточник:** `FPF/Engineering DPF Suite/HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md#hcd-9` — Perform Deliberate Practice with Feedback (FPF @f6ee315, 2026-09-08). Явная формулировка апстрима: практика «not for accumulating nominal hours» — часы, накопленные без информативной обратной связи, скрывают, была ли реальная попытка.

**Коррекция от первоисточника (принята, не расхождение):** паттерн переформулирован по FPF — ядро не «сколько часов», а петля «попытка → информативная обратная связь → коррекция → повтор»; часы — прокси доступа к практике, не мера способности. Совпадает с `PD.FAIL.001-004` и `PD.SOTA.011` — FPF здесь усиливает уже имеющуюся позицию пака, не спорит с ней.

**Ступень применения:** преимущественно 1-3 (контракт `PD.NAMING` ступеней, program-binding — `docs/personal-dev-program-architecture.md`).

**Статус:** специализация FPF.

---

## 2. Фокусировка

**Проблема/силы:** прогресс требует глубокой концентрации на одной работе, но контекстные переключения и реактивная среда дробят внимание.

**Методы пака:** [PD.METHOD.023](../03-methods/PD.METHOD.023-flow-management.md) (управление потоком), [PD.METHOD.022](../03-methods/PD.METHOD.022-state-management.md) (управление состоянием), [PD.METHOD.015](../03-methods/PD.METHOD.015-first-development-slot.md) (первый слот развития).

**Анти-паттерны пака:** [PD.FAIL.018](../05-failure-modes/PD.FAIL.018-urgent-displaces-important.md) (срочное вытесняет важное), [PD.FAIL.042](../05-failure-modes/PD.FAIL.042-unclosed-loops-accumulation.md) (накопление незакрытых петель), [PD.FAIL.041](../05-failure-modes/PD.FAIL.041-quick-pleasure-displacement-cycle.md) (цикл вытеснения быстрым удовольствием), [PD.FAIL.058](../05-failure-modes/PD.FAIL.058-no-time-thinking-bug.md) (баг «нет времени думать»), [PD.FAIL.047](../05-failure-modes/PD.FAIL.047-pomodoro-without-conceptual-guidance.md) (помодоро без концептуального ориентира).

**FPF-первоисточник:** дедицированного паттерна о личной концентрации в FPF не найдено (проверено `HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md` целиком + `OPERATIONS-MANAGEMENT-PRINCIPLES-FRAMEWORK.md#ops-5`, FPF @f6ee315, 2026-09-08). Ближайшие структурные аналоги: `OPS.5 — Admit Work and Limit Starts` (лимит одновременных стартов работы — про допуск работ, не про внимание носителя) и `HCD.4 — Architect a Balanced Human Capability Profile Across Simultaneous Work`.

**Статус:** доменный оригинал. Пересмотреть при следующей сверке — не исключено, что апстрим позже выделит паттерн внимания отдельно.

**Ступень применения:** преимущественно 2-4.

---

## 3. Первый слот развития

**Проблема/силы:** если развитие не закреплено в первом стабильном слоте дня или цикла, операционная работа съедает его без остатка.

**Методы пака:** [PD.METHOD.015](../03-methods/PD.METHOD.015-first-development-slot.md), [PD.METHOD.012](../03-methods/PD.METHOD.012-day-rhythm.md), [PD.METHOD.001](../03-methods/PD.METHOD.001-time-accounting.md).

**Анти-паттерны пака:** [PD.FAIL.018](../05-failure-modes/PD.FAIL.018-urgent-displaces-important.md), [PD.FAIL.058](../05-failure-modes/PD.FAIL.058-no-time-thinking-bug.md).

**Принципы пака:** [PD.PRINC.044](../02-domain-entities/principles/PD.PRINC.044-self-development-as-hygiene.md) (саморазвитие как гигиеническая норма — слот неизымаем), [PD.PRINC.034](../02-domain-entities/principles/PD.PRINC.034-evening-alarm.md) (вечерний будильник защищает завтрашний слот через сон).

**FPF-первоисточник:** `FPF-Spec.md#a-15-3-slotfillingsplanitem` (A.15.3 — SlotFillingsPlanItem: фиксация конкретного заполнения объявленного слота плана до начала работы) и `FPF-Spec.md#a-15-5` (A.15.5 — Work-Entry Readiness and Full-Kit Preparation) (FPF @f6ee315, 2026-09-08).

**Ступень применения:** преимущественно 1-2.

**Статус:** специализация FPF.

---

## 4. Каскад планирования развития

**Проблема/силы:** без декомпозиции долгой цели на короткие горизонты развитие распадается на несвязанные усилия.

**Методы пака:** [PD.METHOD.025](../03-methods/PD.METHOD.025-planning-cascade.md), [PD.METHOD.015](../03-methods/PD.METHOD.015-first-development-slot.md), [PD.METHOD.023](../03-methods/PD.METHOD.023-flow-management.md).

**Анти-паттерны пака:** [PD.FAIL.017](../05-failure-modes/PD.FAIL.017-blaming-everything-on-planning.md) (списывание всего на планирование), [PD.FAIL.048](../05-failure-modes/PD.FAIL.048-knowledge-base-without-priorities.md) (база знаний без приоритетов), [PD.FAIL.042](../05-failure-modes/PD.FAIL.042-unclosed-loops-accumulation.md).

**Принципы пака:** [PD.PRINC.036](../02-domain-entities/principles/PD.PRINC.036-no-wp-no-completion.md) (нет рабочего продукта — задача не выполнена: каскад завершается артефактом на каждом горизонте).

**FPF-первоисточник:** `FPF-Spec.md#a-15-2-u-workplan` (A.15.2 — U.WorkPlan) + `FPF-Spec.md#c-27-ta` (C.27.TA — Temporal Aspect: Time Windows, Rhythm, Cadence) + `Engineering DPF Suite/HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md#hcd-2` (HCD.2 — Compose and Compare Capability-Development Programmes) (FPF @f6ee315, 2026-09-08). Целостного паттерна многогоризонтного каскада как единицы в FPF нет — здесь оригинал-композиция из трёх частей апстрима.

**Ступень применения:** преимущественно 2-4.

**Статус:** специализация FPF (композиция нескольких первоисточников).

---

## 5. Саморегуляция состояния

**Проблема/силы:** результативность зависит от когнитивного и энергетического состояния, но носитель систематически переоценивает «силу воли» как замену регуляции.

**Методы пака:** [PD.METHOD.022](../03-methods/PD.METHOD.022-state-management.md), [PD.METHOD.012](../03-methods/PD.METHOD.012-day-rhythm.md), [PD.METHOD.023](../03-methods/PD.METHOD.023-flow-management.md).

**Анти-паттерны пака:** [PD.FAIL.035](../05-failure-modes/PD.FAIL.035-force-through-instead-of-regulation.md) (продавливание силой вместо регуляции), [PD.FAIL.021](../05-failure-modes/PD.FAIL.021-ignoring-sleep-and-routine.md) (игнорирование сна и режима), [PD.FAIL.029](../05-failure-modes/PD.FAIL.029-discomposure-cycle.md) / [030](../05-failure-modes/PD.FAIL.030-excessive-composure.md) (цикл разбалансировки / избыточная собранность), [PD.FAIL.066](../05-failure-modes/PD.FAIL.066-evening-trap.md) (вечерняя ловушка — автоматизмы берут верх над намерением при истощённом волевом ресурсе; точнее сюда, чем к слоту развития, см. `applies_to` в самой карточке).

**Принципы пака:** [PD.PRINC.031](../02-domain-entities/principles/PD.PRINC.031-productive-state-primacy.md) (примат продуктивного состояния над расписанием), [PD.PRINC.043](../02-domain-entities/principles/PD.PRINC.043-anti-entropy-through-architecture.md) (энтропия побеждается архитектурой, не волей).

**FPF-первоисточник:** дедицированного паттерна о саморегуляции состояния в FPF не найдено (проверено `HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md` целиком, FPF @f6ee315, 2026-09-08). Ближайшие упоминания: `HCD.3 — Diagnose the Limiting Capability, Misconception, or Behaviour` (self-regulation — один из перечисляемых таргетов, не отдельный паттерн) и `HCD.5 — Select Capability-Development Methods` (self-regulation aid — одно из семейств интервенций, с явным предупреждением против «motivation rhetoric»). Ближайший структурный аналог другого рода — `FPF-Spec.md#e-23-cae` (E.23.CAE — Capability Access and Expression Differential Probe: различить отсутствие способности от её недоступности из-за контекста/состояния).

**Статус:** доменный оригинал. У пака здесь более развитая, эмпирически накопленная фактура (4 метода + принципы), чем у апстрима на сегодня.

**Ступень применения:** преимущественно 3-5.

---

## 6. Петля гипотез обучения

**Проблема/силы:** без явных гипотез и регулярной ретроспективы опыт не кумулируется, а повторяющиеся ошибки маскируются под «занятость».

**Методы пака:** [PD.METHOD.060](../03-methods/PD.METHOD.060-pre-registration-hypotheses-metrics-before-data-query.md) (пре-регистрация гипотез и метрик), [PD.METHOD.010](../03-methods/PD.METHOD.010-daily-reflective-review.md) (ежедневный рефлексивный обзор), [PD.METHOD.028](../03-methods/PD.METHOD.028-reflection-with-readback-agent.md) (рефлексия с зачитыванием агентом), [PD.METHOD.039](../03-methods/PD.METHOD.039-epistemic-rhythm.md) (эпистемический ритм).

**Анти-паттерны пака:** [PD.FAIL.079](../05-failure-modes/PD.FAIL.079-practice-without-deliberate-work.md), [PD.FAIL.061](../05-failure-modes/PD.FAIL.061-insight-without-rhythm.md) (инсайт без ритма).

**Принципы пака:** [PD.PRINC.045](../02-domain-entities/principles/PD.PRINC.045-error-as-signal.md) (ошибка как сигнал — не сбой, а признак живой экспериментирующей системы), [PD.PRINC.032](../02-domain-entities/principles/PD.PRINC.032-close-the-loop.md) (закрывай цикл или пиши рефлексию).

**FPF-первоисточник:** `Engineering DPF Suite/HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md#hcd-14` — Revise the Development Arrangement from Evidence (исполняемая гипотеза пересмотра + свежий тест + правила retain/adapt/abandon — буквально петля гипотез) + `FPF-Spec.md#b-5` (Canonical Reasoning Cycle) + `FPF-Spec.md#b-4` (Canonical Evolution Loop) (FPF @f6ee315, 2026-09-08). Сильнейшее совпадение среди всех восьми паттернов.

**Ступень применения:** преимущественно 2-5.

**Статус:** специализация FPF.

---

## 7. Ступенчатое продвижение

**Проблема/силы:** одна и та же интервенция не работает на разных уровнях зрелости — нужен подбор практики под текущую ступень, а не универсальный рецепт.

**Методы пака:** [PD.METHOD.025](../03-methods/PD.METHOD.025-planning-cascade.md), [PD.METHOD.022](../03-methods/PD.METHOD.022-state-management.md), [PD.METHOD.016](../03-methods/PD.METHOD.016-self-diagnostics.md) (самодиагностика; в продукте — роль Диагност R28, FORM.089).

**Анти-паттерны пака:** [PD.FAIL.023](../05-failure-modes/PD.FAIL.023-high-agency-without-mastery.md) (высокая агентность без мастерства), [PD.FAIL.011](../05-failure-modes/PD.FAIL.011-master-stagnation.md) (стагнация мастера), [PD.FAIL.064](../05-failure-modes/PD.FAIL.064-five-breakpoints-of-adult-learning.md) (пять точек разрыва взрослого обучения).

**Принципы пака:** [PD.PRINC.030](../02-domain-entities/principles/PD.PRINC.030-continuous-reflash.md) (непрерывная перепрошивка — развитие не курс, а слот+инкремент+обзор), [PD.PRINC.035](../02-domain-entities/principles/PD.PRINC.035-readiness-precedes-resources.md) (готовность предшествует ресурсам).

**FPF-первоисточник:** `Engineering DPF Suite/HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md#hcd-10` — Vary, Space, Interleave, and Progress Practice (прогрессия = условное изменение сложности/самостоятельности/поддержки по итогам предыдущих попыток) + `FPF-Spec.md#a-2-2` (A.2.2 — U.Capability: System Ability Envelope and Measures) (FPF @f6ee315, 2026-09-08).

**Явное предупреждение первоисточника (учтено в контракте ступеней):** `HCD.4` прямо предостерегает против агрегатного балла мастерства («overall mastery: 72%» скрывает, какие вклады подтверждены). Ступень 1-5 (контракт `PD.NAMING`/WP-380) — метка диапазона, program-binding по подтверждённым вкладам живёт в продукте, не как единый агрегат внутри пака.

**Ступень применения:** 1-5 как тип; конкретная привязка к переходам — program-binding.

**Статус:** специализация FPF.

---

## 8. Среда развития

**Проблема/силы:** личное развитие сильно зависит от социального и физического контекста — доступа к провайдерам, инструментам, поддерживающему окружению; без устройства среды даже верный метод не удерживается.

**Методы пака:** [PD.METHOD.007](../03-methods/PD.METHOD.007-environment-formation.md) (формирование среды), [PD.METHOD.021](../03-methods/PD.METHOD.021-learner-culture.md) (культура ученика).

**Анти-паттерны пака:** [PD.FAIL.028](../05-failure-modes/PD.FAIL.028-social-environment-dependency.md) (зависимость от социальной среды), [PD.FAIL.025](../05-failure-modes/PD.FAIL.025-indoctrination-default.md) (индоктринация по умолчанию), [PD.FAIL.012](../05-failure-modes/PD.FAIL.012-environment-driven-reading.md) (чтение, навязанное средой).

**FPF-первоисточник:** `Engineering DPF Suite/HUMAN-CAPABILITY-DEVELOPMENT-PRINCIPLES-FRAMEWORK.md#hcd-7` — Arrange Providers, Access, Tools, and AI Support + `#hcd-8` — Build the Recursive Capability-Development Arrangement + `Engineering DPF Suite/MUSIC-AND-DANCE-PRACTICE-...-FRAMEWORK.md#mdpe-22` — Test a Support-Environment Change for a Practice + `#hcd-17` — Deliberately Continue and Change HCD Culture (FPF @f6ee315, 2026-09-08).

**Сигнал:** прямого уже измеряемого прод-сигнала нет — помечен «в разработке» до запроса наставника (program-binding, не блокер карты).

**Ступень применения:** 1-5 (среда актуальна на любой ступени).

**Статус:** специализация FPF.

---

## Открытые вопросы

- Паттерны 2 (фокусировка) и 5 (саморегуляция состояния) — доменные оригиналы без прямого аналога в FPF на дату `2026-09-08`. Пересмотреть при следующей содержательной сверке с апстримом (не по расписанию — на следующем пересмотре программы личного развития).
- Program-binding (ступень → паттерн → сигнал → ответственный) — не в этой карте, секция в `DS-my-strategy/docs/personal-dev-program-architecture.md`.
