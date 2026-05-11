# Прототип changelog · 2026-05-06 · Khloe Daily Task — full redesign

**Экраны:** `sKhloeTask` (sidebar 84/85/86 — три варианта: volume / buy / deposit)
**Один DOM-экран, три режима через `data-task-type` + `applyKhloeTaskMode()`.**

**Live:** https://stanislavs-ui.github.io/simpleworld-prototypes/main_prototype/
Sidebar → группа «Хлоя задание» → 84 (Объём) / 85 (Покупка) / 86 (Пополнение).
Прямые URL: `…#79:volume` / `…#79:buy` / `…#79:deposit`.

---

## TL;DR

1. Полный визуальный редизайн карточки задания (3 разных блока вместо одного).
2. Новые иконки (volume = ₿↔$ swap, buy = shopping cart, deposit = wallet+USDT).
3. Раскрывающийся FAQ (4 Q&A) вместо одного абзаца «Подробнее».
4. **Новый per-task progress bar** над CTA — нужен endpoint от бэка.
5. **Bug-fix:** URL hash теперь содержит тип задания (`#79:buy`) — после hard-refresh правильный variant восстанавливается.

---

## 1. Новый layout: 3 отдельных блока

Было: одна карточка (hero + body + CTA).
Стало:

```
┌──────────────────────────────┐
│ Block 1 (карточка):          │
│   ТОРГОВЫЙ ОБЪЁМ             │  ← type-label, big bold uppercase
│   [icon, hero zone]          │
│   Жители, чей объём…         │  ← описание условий
└──────────────────────────────┘
       (gap 12px)
┌──────────────────────────────┐
│ Block 2 (карточка):          │
│   1. Открой некастодиальный…│
│   2. Сделай обмен…           │
│   3. Вернись к Хлое…         │
│   Подробнее ▾                │  ← FAQ (раскрывается)
└──────────────────────────────┘
       (gap 4px)
┌──────────────────────────────┐
│ CTA блок (без бордера):      │
│   ПРОГРЕСС    $12 / $20      │  ← НОВОЕ: per-task прогресс
│   ████░░░░░░░  60%           │
│   [Открыть Simple Wallet]    │
│   [Проверить]                │
└──────────────────────────────┘
```

Hero zone выросла: 116px → 144px. SVG render: 148×100 → 200×135 (иконка крупнее).

---

## 2. Тексты (Лира)

### Type label (большой жирный uppercase, заменяет старый subtitle)

| Тип | Label |
|-----|-------|
| volume | **ТОРГОВЫЙ ОБЪЁМ** |
| buy | **ПОКУПКА КРИПТЫ** |
| deposit | **ПОПОЛНЕНИЕ БАЛАНСА** |

### Описание (gender-neutral, plural)

| Тип | Текст |
|-----|-------|
| volume | Жители, чей объём торговли составит $20 и более, получат один ритуал. |
| buy | Жители, чья покупка криптовалюты составит $10 и более, получат один ритуал. |
| deposit | Жители, чьё пополнение баланса составит $10 и более, получат один ритуал. |

### Шаги (унифицированы)

| # | Шаг |
|---|-----|
| 1 | Открой некастодиальный кошелёк в Simple Wallet |
| 2 | volume: «Сделай обмен любой криптовалютной пары кроме стейблкоинов на сумму от $20»<br>buy: «Купи любую криптовалюту на сумму от $10»<br>deposit: «Пополни баланс на сумму от $10» |
| 3 | Вернись к Хлое и нажми «Проверить» |

---

## 3. FAQ блок (раскрывается из «Подробнее»)

Кнопка «Подробнее» — по центру, белым жирноватым шрифтом (13px, 600). При раскрытии — 4 Q&A блока (gap 20px), каждый Q жирный (14px, 700 white), A нормальный (13px, hsl(0 0% 80%)).

### volume (4 вопроса)

| Q | A |
|---|---|
| Какие сделки засчитываются? | Любой обмен криптовалюты — например, USDC на ETH или SOL на BTC. |
| Можно разбить на несколько сделок? | Да. Главное — общая сумма от $20. |
| Какие обмены не засчитываются? | Обмены между стейблкоинами (USDC ↔ USDT и т.п.). |
| Задание не засчиталось — что делать? | Подожди несколько минут — иногда есть задержка. Если не помогло — напиши в поддержку Simple Wallet или в чат комьюнити. |

### buy (4 вопроса)

| Q | A |
|---|---|
| Какие покупки засчитываются? | Любая покупка криптовалюты на сумму от $10 — например, ETH, BTC, SOL. |
| А если стоимость купленного упадёт? | Не важно. Считается сумма покупки на момент сделки, а не текущая стоимость. |
| Что делать, если на балансе нет средств? | Сначала пополни баланс — например, с внешнего кошелька или с биржи. |
| Задание не засчиталось — что делать? | (тот же текст) |

### deposit (4 вопроса)

| Q | A |
|---|---|
| Откуда можно пополнить? | С внешнего кошелька или с биржи (Binance, Bybit и другие). Любая криптовалюта, например USDC, USDT, Bitcoin, ETH и другие. |
| Какая минимальная сумма? | $10. Можно одним переводом или несколькими — суммируется. |
| Внутренние переводы засчитываются? | Нет. Переводы между своими счетами Simple не считаются. |
| Задание не засчиталось — что делать? | Подожди несколько минут — может быть задержка сети. Если не помогло — напиши в поддержку Simple Wallet или в чат комьюнити. |

---

## 4. Иконки (monochrome white + minor accents)

Все иконки — line-style, белые, размер ~120-148px в SVG (рендер 200×135).

| Тип | Иконка |
|-----|--------|
| volume | Два круга-монеты с символами **₿** (слева) и **$** (справа), между ними — 2 swap-стрелки в противоположных направлениях (top: → , bottom: ←). DeFi-стандарт «token-pair swap». |
| buy | Shopping cart (handle + basket trapezoid + 2 wheels) с ₿-монеткой внутри. Универсальная «buy crypto» иконография. |
| deposit | Wallet (line-style, Tabler Icons geometry — bifold с card pocket) + USDT (₮) монета чуть выше, частично перекрывает верх кошелька — как «падает в кошелёк». |

Иконки рендерил в PNG через Chrome headless и проверял visually перед deploy (новое правило `feedback_svg_render_self_check.md`).

---

## 5. **🆕 Per-task progress bar — нужен endpoint от бэка**

Над кнопкой «Открыть Simple Wallet» — прогресс юзера по конкретному заданию (current/target), визуальный progress bar.

**В прототипе сейчас демо-значения hardcoded в config:**

```js
volume:  { progressCurrent: 12, progressTarget: 20 }   // $12/$20 → 60%
buy:     { progressCurrent: 4,  progressTarget: 10 }   // $4/$10 → 40%
deposit: { progressCurrent: 0,  progressTarget: 10 }   // $0/$10 → 0%
```

**Запрос к бэку:** возвращать реальные значения current/target.

### Предложение по API

**Вариант A** — расширить существующий `GET /arena/today`:
```json
{
  "khloeMet": true,
  "todayClaimed": false,
  "burntYesterday": false,
  "currentDay": 5,
  "stack": "5/7",
  "taskType": "volume",                  // ← уже есть из D194
  "taskAmountUsd": 20,                   // ← уже есть
  "taskStatus": "in_progress",           // ← уже есть
  "taskProgress": {                      // ← НОВОЕ
    "currentUsd": 12.50,
    "targetUsd": 20.00
  }
}
```

**Вариант B** — отдельный `GET /arena/task-progress` (если refresh progress нужен чаще чем `today`).

UI пересчитывает % сам (`Math.round(current/target * 100)`).

### Источник данных

- volume → `Wallet.PartnerApi.checkTradeVolume(clientId, sinceUtc=todayMidnightUtc, untilUtc=now)` — `volumeUsd`
- buy → `checkBuyAmount` — `purchaseUsd`
- deposit → `checkExternalDeposit` — `depositUsd`

Это ровно те же 3 endpoint'а из Wallet Partner API 2.16.0 (D194), что используются для финального `/arena/check-task`. Можно дёргать их read-only без сайд-эффектов в любой момент.

### Когда дёргать

1. На открытии экрана задания (init).
2. После возврата с `Открыть Simple Wallet` (visibilitychange / focus event на клиенте).
3. После клика «Проверить» (response того endpoint'а уже может вернуть `progress` объект).

Cache: 30s server-side per `userId+taskType+day` чтобы не спамить Wallet Partner API при быстром флипе вкладок.

---

## 6. Bug-fix: URL hash теперь хранит тип задания

**Было:** `location.hash = current + 1` (просто индекс экрана). После hard-refresh sKhloeTask открывался на default-варианте (volume), даже если юзер был на buy/deposit.

**Стало:** для sKhloeTask hash имеет вид `{idx}:{type}` — например `#79:buy`. На init скрипт парсит `:type`, кладёт в `localStorage('sw-khloe-task-current-type')`, и restore-логика применяет правильный config.

```js
// goTo() — preserve type suffix when on sKhloeTask:
var hashStr = String(current + 1);
if (screens[current] && screens[current].id === 'sKhloeTask') {
  var t = localStorage.getItem('sw-khloe-task-current-type') || '';
  if (t) hashStr = hashStr + ':' + t;
}
location.hash = hashStr;

// On hash restore (boot):
const [hashScreenStr, hashTaskType] = location.hash.replace('#','').split(':');
if (hashTaskType && /^(volume|buy|deposit)$/.test(hashTaskType)) {
  localStorage.setItem('sw-khloe-task-current-type', hashTaskType);
}
goTo(parseInt(hashScreenStr));
```

Только клиент-сайд изменение. Бэку делать ничего не нужно.

---

## 7. Мелкое

- «Проверить» button: добавлен `text-align: center` (наследование от `.btn-cta` было left).
- Текст в раскрытом FAQ — белый на чуть более тёмном bg (раньше: серый внутри отдельного бокса).
- Hero zone height: 116px → 144px. Все 3 SVG отцентрованы геометрически (volume — без сдвига, buy — translate(0,-7), deposit — coin/wallet groups сдвинуты внутри).

---

## Файлы

- `prototypes/active/newprototype/index.html`
  - Markup `sKhloeTask`: ~lines 4326-4380
  - Config `khloeTaskTypes`: ~lines 6357-6490
  - Logic `applyKhloeTaskMode()`: ~lines 6510-6560
  - `goTo()` wrap + hash preserve: ~lines 4616-4630
  - Hash restore on boot: ~lines 7104-7120

- `_bmad-output/project-status.md` — D213 в Decision Log

---

## Open Q для бэка

1. **Прогресс-эндпоинт:** расширяем `GET /arena/today` (Вариант A) или делаем отдельный `GET /arena/task-progress` (Вариант B)? Решай ты — зависит от реального cadence запросов.
2. **Стейблкоин-фильтр для volume:** где зашит список «стейблкоинов которые НЕ считаются» в обмене для volume-задания? Минимум: USDC, USDT, DAI. Если в `economics-params.yaml` уже есть — ок; если нет — нужно добавить и зашить в `Wallet.PartnerApi.checkTradeVolume`.
3. **`target` сумма ($20/$10/$10):** статика из `economics-params.yaml` или может меняться в БД? Если может — сразу включи в response.

---

🤖 Подготовил Макс (продуктовик SW) после сессии с Стасом. Вопросы — в DM или в `#simple-world-dev`.
