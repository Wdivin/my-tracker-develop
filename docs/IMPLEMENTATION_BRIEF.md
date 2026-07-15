# My Tracker — IMPLEMENTATION BRIEF v1.0

## 1. Статус и назначение

Версия: **1.0**.

Статус: **утверждённое единое задание на проектирование и реализацию**.

Этот файл является главным источником требований. Исполнитель не должен молча исключать обязательные функции. Неясности разрешаются в пользу целостности данных, безопасности, простоты для пользователя и одинаковых правил Web/API. Существенные предположения и компромиссы перечисляются в документации и итоговом отчёте.

## 2. Цель и принципы продукта

Создать современный универсальный корпоративный трекер задач, использующий сильные предметные идеи Redmine/Jira, но не копирующий их интерфейс и устаревшие технические решения.

Главный принцип:

> **Гибкость корпоративной системы внутри, простота современного приложения снаружи.**

Продукт должен подходить для IT, поддержки, заявок, согласований, маркетинга, административных и других внутренних процессов.

Обязательные принципы:

- сложность раскрывается постепенно;
- mobile — полноценное рабочее место;
- у задачи максимум один основной исполнитель;
- скрытая автоматизация и неочевидное наследование запрещены;
- Web UI и REST API используют одну бизнес-логику;
- история, архивирование и мягкое удаление предпочтительнее потери данных;
- необратимые и массовые действия безопасны по умолчанию;
- интерфейс не выглядит как набор сырых CRUD-форм;
- каждое усложнение должно приносить практическую пользу.

## 3. Объём и этапность

Полный объём документа является целевой версией 1.0.

Реализация разбивается на последовательные этапы. Первый этап — рабочее ядро, пригодное для реального использования. Последующие этапы доводят продукт до полного объёма 1.0. После каждого этапа приложение должно запускаться и быть проверяемым.

До массовой реализации требуется представить архитектуру, этапы, зависимости, риски и критерии готовности согласно `docs/START_TASK.md`.

## 4. Обязательный технологический фундамент и свобода реализации

Обязательные ограничения стека:

- backend реализуется на **Laravel**;
- база данных — **MySQL или MariaDB**;
- выбранные версии Laravel, PHP и СУБД должны быть актуальными стабильными версиями, совместимыми между собой и пригодными для production;
- миграции и запросы должны реально проверяться на выбранной MySQL/MariaDB, а не только на SQLite или другой тестовой СУБД.

Все остальные технологические решения исполнитель выбирает самостоятельно и обосновывает, в том числе:

- внутреннюю архитектуру и модульные границы Laravel-приложения;
- физическую схему БД, таблицы, связи и индексы;
- UUID, ULID или иной стабильный публичный идентификатор;
- способ хранения workflow, пользовательских полей и истории;
- frontend-стек и способ его связи с Laravel, включая Inertia, отдельный SPA или иной разумный вариант;
- UI-библиотеку и управление состоянием;
- способ реализации поиска средствами MySQL/MariaDB и приложения;
- необходимость Redis и конкретную реализацию очередей, кэша, rate limiting и фоновых задач;
- файловое хранилище;
- конкретные REST-контракты в установленных продуктовых границах;
- стратегию тестирования, локального запуска и production deployment.

Независимо от выбранной реализации обязательны:

- вся бизнес-логика, права, workflow, валидация, история, аудит и уведомления находятся в общих application/domain services Laravel и одинаково применяются Web UI и REST API;
- дублирование бизнес-правил между Web- и API-контроллерами запрещено;
- целостность и корректные ограничения данных;
- транзакционность составных действий;
- защита от циклов;
- отсутствие скрытой потери данных;
- безопасное мягкое удаление;
- индексы для списков, фильтров и поиска;
- воспроизводимые миграции и развёртывание;
- возможность backup/restore;
- документирование ключевых решений и компромиссов.

## 5. Организация и пользователи

Одна установка обслуживает одну организацию. Multi-tenancy не входит в 1.0, но архитектура не должна намеренно делать его невозможным.

Организация хранит название, логотип, язык по умолчанию, часовой пояс, форматы дат/времени, настройки уведомлений, файловые лимиты и параметры безопасности.

Пользователи создаются системным администратором, могут приглашаться для установки пароля, используют локальные учётные записи, восстанавливают пароль, выбирают язык и часовой пояс, деактивируются без удаления истории и не регистрируются публично самостоятельно.

Деактивация завершает активные сессии, отзывает API-токены и исключает пользователя из новых назначений. Старые задачи, комментарии, записи времени и история сохраняются. Автоматического переназначения задач нет.

Active Directory/LDAP, SAML и OAuth-login не входят в 1.0. Начальная установка создаёт первого локального системного администратора.

## 6. Системные администраторы

Системный администратор может создавать и деактивировать пользователей, назначать и снимать права системного администратора у других пользователей, управлять глобальными справочниками, типами задач, workflow, полями, ролями, настройками, service users, токенами, корзиной и аудитом.

Администратор не редактирует и не подменяет аудит, не видит пароли и полные секреты, не получает универсальный обход workflow обычных задач и выполняет критические действия с подтверждением и аудитом.

## 7. Проекты и подпроекты

Проект — основной рабочий контейнер. Подпроект — тот же проект с родителем.

Разрешено максимум два уровня подпроектов:

```text
Корневой проект
└── Подпроект первого уровня
    └── Подпроект второго уровня
```

Циклы запрещены. Перенос проекта проверяет глубину и циклические связи.

Основные данные проекта: название; неизменяемый после создания уникальный код; автоматически созданный уникальный slug, доступный для изменения по праву; стабильный UUID/ULID; описание; родитель; владелец и руководитель; даты начала, планового и фактического завершения; статус; видимость; цвет или иконка; порядок; автор и timestamps.

Код используется в ключах задач, например `WEB-142`. UUID/ULID используется для внутренних связей и интеграций. Изменение slug не меняет стабильный ID; полная история старых slug не обязательна.

Статусы проекта:

- `планируется` — работа разрешена по правам;
- `активный` — обычный режим;
- `приостановлен` — виден участникам, рабочие изменения по умолчанию запрещены;
- `завершён` — доступен участникам для просмотра, новые задачи и обычные изменения запрещены;
- `архивный` — скрыт для всех, кроме системных администраторов.

Возврат завершённого/архивного проекта требует отдельного права, подтверждения и аудита. Нельзя архивировать проект при активных подпроектах.

Видимость:

- закрытый — видят участники и системные администраторы;
- внутренний — видят все активные пользователи организации, но без роли имеют только чтение.

Публичных проектов нет. Если пользователь видит проект, он видит все задачи проекта. Приватных задач и confidential fields в 1.0 нет.

## 8. Наследование и конфигурация проекта

Наследование конфигурации и наследование участников — независимые режимы.

Подпроект либо полностью наследует конфигурацию родителя, либо использует собственную. Частичное смешанное наследование запрещено.

Конфигурация включает доступные типы задач, workflow для каждого типа, доступные проектные роли, проектные представления и глобальный или собственный полный набор видов работ.

Пользовательские поля проект напрямую не включает: набор полей определяется типом задачи.

Участники подпроекта могут наследоваться с ролями родителя и дополняться локальными либо использовать полностью собственный состав. Интерфейс явно показывает источник участника/роли. Унаследованную роль нельзя локально отнять, пока наследование включено.

## 9. Роли, права и группы

Контексты доступа: системное администрирование; проектные роли; отношение пользователя к задаче — автор, исполнитель, наблюдатель.

Проектные роли настраиваемые. Стартовые шаблоны: руководитель проекта, участник/исполнитель, читатель.

Пользователь может иметь несколько ролей; разрешения складываются. Явных deny-прав нет.

Права группируются по понятным областям: проект, задачи, поля, ответственность, структура, комментарии/файлы/чек-лист, время, участники, отчёты, массовые операции, удаление и восстановление.

Группы используются только для прав: группу можно добавить в проект и назначить ей роль; группа не может быть автором, исполнителем, наблюдателем, получателем уведомления или владельцем записи времени.

Автор задачи неизменяем. Авторство само по себе не даёт права редактирования. Исполнитель получает только явно предусмотренные операционные права. Наблюдение не расширяет доступ.

Права проверяются server-side для Web и API. Скрытие кнопки не считается защитой.

## 10. Типы задач и конфигурация форм

Тип задачи является глобальной сущностью и определяет название, код, описание, иконку/цвет, стандартные и пользовательские поля, видимость, порядок и секции полей, обязательность и простую валидацию, использование прогресса, оценки, времени, подзадач и чек-листа.

Проект выбирает доступные типы задач. Для каждой пары `проект + тип задачи` проект выбирает глобальный шаблон workflow либо независимую локальную копию.

Начальный статус определяется только workflow. Отдельного начального статуса в типе задачи нет.

## 11. Пользовательские поля

Пользовательские поля глобальные и назначаются типам задач.

Поддерживаются короткий текст, многострочный текст, целое число, decimal, дата, дата и время, boolean, single select, multi select и ссылка на проект.

Не входят user/task/file reference, formula/computed, arbitrary JSON, project-specific fields и сложные conditional forms.

Обязательность, defaults и validation должны быть явными и видимыми. Использованное поле или значение справочника архивируется, а не удаляется с потерей данных.

API отдаёт практические метаданные формы задачи: доступные типы, поля, обязательность, значения, простые ограничения, порядок/секции и возможность редактирования. Универсальный schema engine не требуется.

## 12. Задача и базовые поля

Задача принадлежит одному проекту и имеет как минимум стабильный ID, человекочитаемый ключ, тип, название, описание, статус, автора, максимум одного исполнителя, приоритет, дату начала, срок, дату завершения, прогресс при его включении, оценку времени, теги, watchers, parent task, custom field values, timestamps, версию записи и источник создания.

Минимально обязательны проект, тип и название. Остальные обязательные поля определяются типом и workflow.

Создание доступно из глобальной кнопки, проекта, доски, как подзадача и через API.

Локальное автосохранение незавершённой формы не требуется в первом рабочем релизе.

## 13. Прогресс

Прогресс хранится отдельно от статуса и меняется вручную: диапазон 0–100%, default 0%, тип задачи может отключить поле, основной UX — slider, системный шаг slider 1%, 5% или 10% с default 5%, точный числовой ввод допустим, изменения фиксируются в истории, поле участвует в API, фильтрах и отчётах.

При закрытии система предлагает установить 100%, но пользователь может отказаться. Закрытая задача может иметь прогресс меньше 100%. Переход может явно потребовать определённый прогресс.

## 14. Workflow

Workflow состоит из статусов и явных переходов: ровно один начальный статус; у статуса есть явный признак закрытия; категории статусов не используются; статус меняется только через доступный переход; универсального «перевести в любой статус» нет даже у администратора; между одной парой статусов не более одного перехода; переход может иметь пользовательское название действия.

Переход настраивает роли и отношения к задаче, обязательность комментария, обязательные поля, продуктовые проверки, уведомления и системные действия.

Workflow сохраняется только валидным; отдельных черновиков нет. Проверяются начальный статус, ссылки, достижимость и дубли переходов. Незакрывающий статус без исходящих переходов вызывает предупреждение, но может быть подтверждён администратором.

Закрытие родительской задачи запрещено, пока есть незакрытые подзадачи. Обхода правом нет. Блокирующая связь выдаёт предупреждение и подтверждение, но не является жёстким запретом.

Повторное открытие выполняется обычным переходом; текущая дата завершения очищается, история прошлых закрытий сохраняется, прогресс автоматически не уменьшается.

Обязательный комментарий перехода создаётся как обычный комментарий, связан с переходом, виден в ленте, может редактироваться автором с сохранением редакций. Редактирование не отменяет переход.

## 15. Редактирование и структурные операции

В карточке задачи inline допускается для обычных рабочих полей: название, описание, исполнитель, приоритет, дата начала, срок, прогресс, оценка, теги, простые custom fields, наблюдение и checklist items.

В desktop-списке inline обязательно поддерживаются как минимум доступный workflow transition, исполнитель, приоритет, срок, прогресс и название.

Через отдельное действие или мастер выполняются смена статуса, перенос в другой проект, смена типа, изменение parent task, удаление/восстановление и структурные операции с последствиями.

Перенос проекта и смена типа заранее показывают несовместимые статусы, поля, исполнителя и последствия. Молчаливое обнуление запрещено. Скрытые значения custom fields физически не теряются.

Нельзя мягко удалить задачу при наличии неудалённых подзадач. Сначала их удаляют, переносят или отвязывают.

## 16. Подзадачи, связи и чек-лист

Задачи поддерживают иерархию подзадач с защитой от циклов.

Связи включают как минимум `связана с`, `блокирует / заблокирована`, `дублирует / дублируется`. Связи двусторонне согласованы. Отдельного клонирования задачи нет.

Checklist относится к задаче, поддерживает порядок, выполненность и историю изменений без засорения ленты отдельной записью на каждую галочку.

## 17. Комментарии и лента активности

Комментарии поддерживают Markdown, mentions и attachments. Упомянуть можно только пользователя с доступом. Вложенных тредов нет. Ответ создаётся обычным комментарием с упоминанием, ссылкой и краткой цитатой.

Автор может редактировать свой комментарий в любое время. Все редакции сохраняются; виден признак изменения, история открывается отдельно.

Удаление комментария мягкое. Автор может удалить и восстановить свой комментарий. Отдельное право позволяет удалить/восстановить чужой. На месте удалённого текста остаётся отметка. Физическое удаление отдельно не выполняется.

Единая лента содержит комментарии, workflow, изменения полей, файлы, время и системные события. Фильтры: всё, комментарии, изменения, время. Одно сохранение нескольких полей создаёт сгруппированное событие.

## 18. История задачи

История фиксирует создание, поля, проект/тип, workflow, исполнителя, сроки, приоритет, прогресс, оценку, теги, custom fields, parent/subtasks/relations, watchers, checklist, files, time, delete/restore.

Сохраняются actor, время, источник, old/new и связанный комментарий. Источники: Web, API, service user/integration, system process.

Для коротких полей сохраняются old/new; для длинных текстов — версии или отдельное сравнение; для коллекций — добавленные/удалённые элементы; для файлов — метаданные и факт действия. Секреты не сохраняются.

История хранится весь срок задачи, загружается страницами и фильтруется server-side. После физического удаления задачи её история удаляется.

## 19. Системный аудит

Аудит фиксирует входы, роли/права, участников, workflow/справочники, tokens, delete/restore, settings, bulk operations, exports и admin actions.

Хранятся actor, время, action, object, result, source, IP, user agent и token/service identity без секретов. IP/user agent доступны только администраторам и удаляются по retention.

Записи нельзя редактировать или выборочно удалять через UI. Default retention — 365 дней; автоочистку можно отключить. Очистка создаёт агрегированную запись. WORM/криптографический audit не требуется.

## 20. Наблюдение и уведомления

Пользователь с доступом может подписаться и отписаться. Добавление/удаление других watchers требует права. Наблюдение не даёт доступ.

Получатели: voluntary watchers, author, current assignee, new/previous assignee для assignment events, mentioned users и иные явно определённые пользователи. Дубликаты удаляются; инициатор обычно не уведомляется; проверяются доступ и настройки каналов. Автор/исполнитель не обязаны храниться как watchers. Добровольная подписка прежнего исполнителя сохраняется.

Каналы: in-app, notification center/unread, email и user settings.

Для каждого события задаётся обязательность in-app, возможность отключения, email default/disable и режим отправки. Абстрактные critical/normal/info не нужны.

Обязательные события: assignment/unassignment, mention, comment, status/reopen, due change, due today, overdue, delete/restore, access loss, blocking relation и закрытие blocker.

Напоминания по timezone получателя; default 09:00; одно в день срока и одно следующим утром при просрочке. Ежедневных повторов нет.

Telegram, SMS, Slack/Teams, push, digests, email replies и AI summaries не входят.

## 21. Файлы

Attachments у tasks/comments. Изображения в описании/комментарии остаются защищёнными attachments.

Desktop: picker, multiple upload, drag-and-drop, paste. Mobile: file/photo/camera. DnD не единственный способ.

Defaults: 25 MB/file, 10 files/operation, 500 MB active attachments/task. Лимиты настраиваются. В квоту входят только active; soft-deleted занимают storage, но не квоту.

Права наследуются от task. Публичных ссылок нет. Любая операция проверяет access. Storage вне public executable directory.

Типы: system-dangerous всегда blocked; denylist priority; заполненный allowlist разрешает только перечисленное; пустой allowlist разрешает всё кроме опасного/denylist; проверяются extension/MIME/signature.

HTML/опасный SVG не inline. ZIP только хранится/скачивается, не распаковывается, не исполняется, не preview и не индексируется.

Images: thumbnails/fullscreen/zoom/original. PDF: browser/simple viewer. Office preview не требуется.

Версий файлов нет. Старые files редакций comments не хранятся; остаются метаданные/факт. Незавершённые uploads имеют TTL, не входят в quota и очищаются; повторное completion не создаёт дубль.

Integrity/orphan diagnostic после 1.0.

## 22. Учёт времени

Manual, task-bound. Обязательны date, duration, work type; optional comment/source.

Rules: 1 minute minimum, 24 hours maximum, любое целое число минут, `+/-` шаг 5 минут, future date forbidden, no new time in closed task, progress independent.

Own entries edit/soft-delete while period open. Others/admin override — separate rights. Нужны My Time, basic report и CSV export по праву.

## 23. Корзина

Admin trash содержит deleted tasks, отдельно deleted task attachments и deleted time entries. Comments восстанавливаются внутри task/admin view и не выводятся общим списком.

Хранение до manual physical deletion; auto cleanup нет. Physical delete только system admin с confirmation и audit. Целостность связей обязательна.

## 24. Экраны и навигация

Обязательны My Work, Projects, project task list, board, All Tasks, Recent, Favorites, Notifications и Administration.

My Work: требуют внимания, назначены мне, созданные мной, наблюдаемые. Причина attention видна: overdue, due today, unread assignment, reopened, mention, blocked. Hidden ranking нет.

Recent: tasks/projects, clearable, не audit. Favorites: projects и saved views; favorite tasks нет.

Project navigation: overview, tasks, board, participants, reports, settings. Subprojects: breadcrumbs/compact lists, не обязательное большое mobile tree.

## 25. Списки, фильтры и views

Desktop — table, mobile — cards. Base columns: key, title, project, type, status, assignee, priority, progress, due, updated.

Настраиваются columns/order/width/sorting/pinning/density; state хранится в view.

Filters: `field + operator + value`; conditions AND, values within condition OR; nested logical groups нет. Поддерживаются text, reference, date, number/progress, user, empty/not empty. Активные условия видны.

Quick filters: open, mine, unassigned, overdue, due today, recently updated, unread. Quick list search — key/title.

Multi-sort и одна grouping: status, assignee, project, type, priority, due period. Hidden sorting нет.

Views personal/project. Personal создаёт любой пользователь; project требует права. Project view не перезаписывается автоматически: save, save personal copy или reset local changes.

View хранит scope, filters, sorting, grouping, columns/order/width, density, subprojects inclusion и list/board mode.

Server pagination 25/50/100; infinite scroll не основная модель. Back navigation сохраняет page/scroll/filters/sort/view.

CSV export по праву с access, filters, columns, sorting, localized headers и unambiguous dates. XLSX нет.

## 26. Bulk и board

Bulk: assignee, priority, due, progress, tags, workflow, move project, change type, soft delete. Каждая task проверяется отдельно; partial success подробно отображается; all filtered results selectable; large operation подтверждается и при необходимости queue.

Board по statuses. DnD выполняет transition; required comment/field/confirmation открывает form. DnD не единственный способ. Combined board доступна только при совместимых status sets/order.

## 27. Search

Global search доступен везде, ищет tasks/projects.

Task: exact key highest priority, title, description, comments, supported text custom fields. Project: name, code, description.

Access control строгий: no disclosure через IDs/snippets/counts/hints. Closed tasks searchable. Archived projects searchable только admins. Deleted tasks hidden; admin explicit filter.

Есть compact suggestions и full results page с filters/sort/pagination. Snippet показывает match source. Comment match по возможности открывает activity entry.

User search и command palette не входят.

## 28. REST API

Versioned JSON REST (`/api/v1`) и актуальный OpenAPI. Web/API используют одинаковые rights/workflow/validation/history/audit/notifications.

Service users, scopes + normal rights, expiring/revocable/hashed tokens shown once, rate limit, request ID, pagination, allowlisted filters/sorting, unified errors, partial-success bulk, file operations и source identity.

Idempotency обязательна для task/comment/time creation. `Idempotency-Key`: same key+same payload returns original; same key+different payload — 409. TTL/implementation выбирает исполнитель.

Optimistic locking: tasks, projects/important project settings, workflows, task types/form config и важные config entities. Не universal для каждой child record.

Webhooks, GraphQL, OAuth provider, full admin API и integration platform не входят.

## 29. Administration

UI: organization, users/admins, groups, roles/rights, task types, workflows/statuses, custom fields, work types, notification rules, file rules, service users/tokens, SMTP, queue/scheduler, audit, trash, health/status.

SMTP: host, port, encryption, username, password, sender, test send; env override allowed.

Diagnostics: queue availability, failed count, last error, last successful scheduler run, retry failed job. Worker control panel не требуется.

Health: app/DB readiness, queue/scheduler, storage writeability без secrets.

## 30. Localization, mobile, PWA, accessibility

Locales `ru` и `uk`; user chooses. Все UI/validation/email/dates/system labels localized. English optional.

Organization/user timezone; display in user timezone; reminders correct.

Current Chrome/Edge/iOS Safari/Android Chrome. Firefox best effort; Firefox-only issue не release blocker при несоразмерной отдельной архитектуре.

Mobile full scenarios: navigation, lists, create/edit, workflow, comments, files, filters, notifications, time. Touch-friendly controls/dialogs/actions.

Installable PWA. Offline editing/background sync/push excluded.

Accessibility: keyboard, visible focus, labels/names, contrast, non-color errors, basic screen-reader semantics.

## 31. Nonfunctional

Baseline: ~1000 active users, ~1000 projects, up to 1,000,000 tasks as architecture/index/pagination reference. Full load certification not required.

Representative documented environment targets: ordinary server response ~500 ms; lists/search ~1 s; UI loading states; heavy exports/bulk/email background. Benchmarks, not universal guarantees.

Security: modern password hashing, CSRF/session protection, secure cookies/HTTPS, validation/escaping, server authz, upload security, login/API rate limits, secrets outside repo, no secrets in logs/history/audit, dependency scans, confirmation/audit for critical actions.

Critical vulnerabilities block release. High normally block unless documented non-applicable/no-fix/compensated exception.

## 32. Tests и visual checks

Automated coverage: auth/users, roles/access/inheritance, projects/depth/cycles, tasks/subtasks/relations, workflow/close/reopen, custom fields, comments/history/audit, files, time, notifications, filters/views/search, API/scopes/idempotency/locking, bulk partial success, trash/restore.

E2E минимум: login, create task/form, assignment, comment+mention, upload, workflow, required transition comment, close/reopen, subtask/parent close block, filters/view, board, time, role differences, mobile workflow, lock conflict, exact key search, restore task, API create.

Visual viewports: 1440×900, 1024×768, 390×844, 360×800 if possible. Проверяются ru/uk, states, long text, dialogs, tables/cards, filters, board, admin, mobile и no unnecessary horizontal scroll.

Reproducible demo/seed data; production не создаёт demo автоматически.

## 33. CI, migrations, deployment, backup

PR checks: backend tests, frontend typecheck/tests, production build, lint/format, migration verification, dependency/security.

Migrations: clean DB, upgrade, no manual table edits, data preservation, large-table awareness, rollback/safe recovery plan.

Production deployment documented for runtime, DB, build, web server, queues, scheduler, storage, SMTP, HTTPS, permissions, health. Docker optional, не единственный обязательный способ.

До приёмки real backup/restore drill с проверкой data/files/tasks/comments/rights/history/time.

## 34. Documentation/report

Обязательны README, installation, production deployment, upgrade, backup/restore, queues/scheduler, SMTP, storage, API/OpenAPI, admin guide, short user guide, troubleshooting, architecture decisions, release notes, known limitations.

Final report: scope/stages; branch/commit/run; stack/architecture; data model/indexes; workflow/fields/history/rights/API; changed modules; test/build/lint/E2E results; visual checks; demo users; deployment/backup; limitations/deferred; compromises.

## 35. Definition of Done

Feature done: business rules, Web/API consistency, server rights, history/audit/notifications, tests, mobile/visual checks, docs, no critical defect.

Version 1.0 accepted: all mandatory scope, integrated stages, tests/build/E2E, clean install/upgrade, reproducible deployment, backup/restore, desktop/mobile scenarios, accurate OpenAPI, no critical vulnerabilities, high fixed or explicitly excepted, complete docs/limitations.

## 36. После 1.0

AD/LDAP/SAML/OAuth login; multi-tenancy; multiple assignees; private tasks/confidential fields; unlimited depth; mixed inheritance; offline/background sync/push; webhooks/integration platform; Telegram/SMS/Slack/Teams; email-to-task/replies; AI; universal CSV import; GraphQL/OAuth provider; full admin API; project-specific/user/task/file/computed custom fields; complex conditional forms; timers/screenshots/payroll/billing/budgets; XLSX; storage backend switching/migration UI; integrity/orphan diagnostic; Office external preview; OCR/document versions/collaborative editing; infrastructure control panel; auto-update UI; WORM audit.