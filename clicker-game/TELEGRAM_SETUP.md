# Настройка Clicker Game для Telegram

## 1. Создание бота в Telegram

1. Открой [@BotFather](https://t.me/BotFather) в Telegram
2. Отправь `/newbot` и следуй инструкциям
3. Сохрани **токен бота** (понадобится для Mini App)

## 2. Хостинг Mini App

Mini App должен быть доступен по HTTPS. Варианты:

### Вариант A: GitHub Pages (бесплатно)

1. Создай репозиторий на GitHub
2. Залей файлы: `index.html`, `config.js`
3. Settings → Pages → Source: Deploy from branch
4. Выбери ветку `main` и папку `/root`
5. URL будет: `https://ТВОЙ_ЮЗЕР.github.io/РЕПОЗИТОРИЙ/`

### Вариант B: Vercel / Netlify

1. Зарегистрируйся на [vercel.com](https://vercel.com) или [netlify.com](https://netlify.com)
2. Подключи репозиторий или загрузи папку с проектом
3. Получи URL вида `https://твой-проект.vercel.app`

## 3. Подключение Mini App к боту

1. Открой [@BotFather](https://t.me/BotFather)
2. Отправь `/mybots` → выбери своего бота
3. **Bot Settings** → **Menu Button** → **Configure menu button**
4. Введи URL твоего Mini App (например: `https://твой-сайт.com/`)
5. Введи текст кнопки: `🎮 Играть`

Или через API:
```
https://api.telegram.org/bot<ТОКЕН>/setChatMenuButton
POST: {"menu_button": {"type": "web_app", "text": "🎮 Играть", "web_app": {"url": "https://твой-url.com"}}}
```

## 4. Настройка Firebase (для реального рейтинга)

1. Зайди на [Firebase Console](https://console.firebase.google.com)
2. **Create project** → придумай имя
3. В проекте: **Build** → **Firestore Database** → **Create database** → Start in test mode
4. **Project Settings** (иконка шестерёнки) → **Your apps** → добавь Web app
5. Скопируй конфиг и вставь в `config.js`:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "твой-apiKey",
  authDomain: "твой-проект.firebaseapp.com",
  projectId: "твой-projectId",
  storageBucket: "твой-проект.appspot.com",
  messagingSenderId: "твой-senderId",
  appId: "твой-appId"
};

const FIREBASE_ENABLED = true;
```

6. В Firestore создай коллекцию `players` (создастся автоматически при первом сохранении)

## 5. Проверка

1. Открой своего бота в Telegram
2. Нажми на кнопку меню (или введи команду `/start` и нажми на кнопку)
3. Mini App должен открыться
4. В профиле отобразятся: Имя, @username, ID из Telegram
5. Рейтинг покажет реальных игроков (если Firebase настроен)

## Важно

- Mini App работает **только** когда открыт из Telegram
- В браузере напрямую будут отображаться "Гость" и локальный режим
- Для теста в браузере используй: `https://твой-url.com?tgWebAppData=...` (можно получить через [@userinfobot](https://t.me/userinfobot) для отладки)
