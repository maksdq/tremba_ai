# tremba_ai 🤖

Telegram і Twitch бот який спілкується в стилі українського стрімера [thetremba](https://twitch.tv/thetremba).

---

## Можливості

- 💬 **Telegram** — відповідає в особистих повідомленнях та групах (по тригерним словам)
- 📺 **Twitch** — відповідає в чатах стримерів на `@tremba_ai`
- 🎤 **Голосові повідомлення та кружечки** — транскрибує через Groq Whisper і відповідає
- 🖼️ **Фото** — розпізнає зображення через vision модель
- 🧠 **Пам'ять розмови** — зберігає контекст останніх 20 повідомлень
- 🔄 **Ротація API ключів** — до 10 Groq ключів, автоматичне переключення при вичерпанні ліміту
- 🛡️ **Захист від prompt injection** — ігнорує спроби змінити поведінку бота
- 🈲 **Фільтр мов** — тільки українська/англійська, без китайських/чеських артефактів
- ⛔ **Банворди Twitch** — автоматична цензура заборонених слів в Twitch чаті
- 🤖 **Ігнорування ботів** — не реагує на Nightbot, Streamlabs, Moobot тощо

---

## Технології

- Python 3.10+
- [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot)
- [Groq API](https://console.groq.com) — LLM (llama-3.3-70b) + Whisper
- Twitch IRC через asyncio (без зовнішніх бібліотек)

---

## Встановлення

```bash
git clone https://github.com/maksdq/tremba_ai
cd tremba_ai
pip install python-telegram-bot aiohttp
```

---

## Налаштування

Відкрий `bot.py` і заміни токени:

```python
TELEGRAM_TOKEN = "токен_від_BotFather"
TWITCH_TOKEN = "oauth_токен"  # отримати на twitchapps.com/tmi
```

Groq ключі (до 10 штук) — впиши в список:

```python
keys = [
    "gsk_ключ1",
    "gsk_ключ2",
    # ...
]
```

### Отримання токенів

| Токен | Де отримати |
|-------|-------------|
| Telegram | [@BotFather](https://t.me/BotFather) → `/newbot` |
| Groq API | [console.groq.com](https://console.groq.com) → API Keys |
| Twitch OAuth | [twitchapps.com/tmi](https://twitchapps.com/tmi) |

---

## Запуск

```bash
python bot.py
```

### Як systemd сервіс

```bash
cp tremba_bot.service /etc/systemd/system/
systemctl daemon-reload
systemctl enable --now tremba_bot
```

---

## Команди бота

| Команда | Опис |
|---------|------|
| `/start` | Привітання та список команд |
| `/twitch нік` | Додати бота в Twitch чат |
| `/twitch_remove нік` | Видалити бота з Twitch чату |
| `/twitch_list` | Список підключених каналів |

> Після `/twitch нік` — напиши у своєму Twitch чаті `/mod tremba_ai`

---

## Тригери

**Telegram групи** — бот реагує якщо повідомлення містить:
`філіп` `фєліп`

або якщо хтось відповів на повідомлення бота.

**Twitch чат** — бот реагує тільки на `@tremba_ai`.

---

## Структура файлів

```
tremba_ai/
├── bot.py               # основний файл бота
├── requirements.txt     # залежності
├── tremba_bot.service   # systemd сервіс
├── twitch_channels.json # список підключених twitch каналів (auto)
└── faces/               # еталонні фото для розпізнавання облич (опційно)
    ├── filip.jpg
    └── maria.jpg
```

---

## Ліміти Groq (безкоштовний план)

| Модель | Ліміт |
|--------|-------|
| llama-3.3-70b-versatile | 100,000 токенів/день |
| llama-3.1-8b-instant | 500,000 токенів/день |
| whisper-large-v3-turbo | 7,200 хв аудіо/день |

Бот автоматично переключається між ключами при вичерпанні ліміту.
