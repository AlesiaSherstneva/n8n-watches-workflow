# n8n-watches-workflow

Рабочий workflow для обработки клиентских сообщений, связанных с элитными часами ("Rolex", "Omega", "Patek"). По 
условию задания требовалась LLM-обработка запроса с помощью DeepSeek. Я пыталась оформить соответствующую ноду, но,
к сожалению, после регистрации API-ключа DeepSeek вернул мне статус "Payment Required", то есть потребовал оплату.

Поэтому вместо LLM-ноды используется нода-заглушка, возвращающая стандартный ответ "Ваше сообщение принято, спасибо!".

## Настройка API перед началом работы

1. Открыть файл `watches.json` в приложении [n8n.cloud](https://n8n.cloud/).

2. Открыть любую из трёх нод `Google Sheets`(Leads Table, Watches Table, Errors Table). В поле `Credential to connect with` 
добавить сервисный JSON-ключ от аккаунта Google Cloud, либо, если ключа нет, войти с помощью основного Google-аккаунта. 
Оставшиеся две ноды просто открыть и закрыть, права доступа подгрузятся автоматически.

3. Создать бота в `Telegram` с помощью [@BotFather](https://t.me/BotFather), скопировать его токен. Открыть ноду 
`Telegram`, вставить токен в поле `Credential to connect with`. В поле `Chat ID` вставить свой Id (получить можно 
у бота [@userinfobot](https://t.me/userinfobot)).

4. Открыть ноду `Webhook`, найти `test URL` и `production URL`.

## Тестирование API
Чтобы отправить запрос на `test URL`, нужно предварительно активировать кнопку `Test workflow`.

Для работы с `production URL` - перевести переключатель `Inactive` в положение `Active`.

Пример валидного запроса:
```bash
curl -X POST <WEBHOOK_URL> \
   -H "Content-Type: application/json" \
   -d '{
      "name": "Анна",
      "email": "anna@mail.ru",
      "message": "Интересует Rolex Submariner"
   }'
```

Пример невалидного запроса:
```bash
curl -X POST <WEBHOOK_URL> \
   -H "Content-Type: application/json" \
   -d '{
      "name": "Анна",
      "email": "invalid-email",
      "message": "Интересует Rolex Submariner"
   }'
```

[Ссылка на документ Leads_Test в Google Sheets](https://docs.google.com/spreadsheets/d/1MBcmkwdjGBBn8jT2TdzczLzCZSgS0FgouOkTyUXq1oY)

[Ссылка на демо-видео](https://drive.google.com/file/d/1jYLtxLwf5ZFHHZjczs95Tk2bmEegPfUM/view?usp=sharing)