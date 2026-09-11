# Установка avito-mcp-ru агентом

Документ для ИИ-агента, который ставит сервер за человека. Человеку удобнее
[README](README.md).

## 1. Проверить uv

```bash
uvx --version || curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 2. Прописать сервер

Claude Desktop: `~/Library/Application Support/Claude/claude_desktop_config.json`
(macOS) или `%APPDATA%\Claude\claude_desktop_config.json` (Windows).
Cline: `cline_mcp_settings.json`.

```json
{
  "mcpServers": {
    "avito": {
      "command": "uvx",
      "args": ["avito-mcp-ru"],
      "env": {
        "AVITO_CLIENT_ID": "<значение>",
        "AVITO_CLIENT_SECRET": "<значение>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## 3. Ключи

| переменная | тип | где взять |
|---|---|---|
| `AVITO_CLIENT_ID` | секрет | client_id из раздела Для бизнеса → Интеграции → API. |
| `AVITO_CLIENT_SECRET` | секрет | client_secret оттуда же, меняется на токен по OAuth2. |

Значения спрашиваются у человека и в репозиторий не пишутся. Второй путь, без
переменных окружения: запустить сервер и вызвать `avito_add_cabinet`,
ключи лягут в `~/.marketplace-mcp/cabinets.json` с правами 600.

## 4. Проверить

Перезапустить клиент и вызвать `avito_check_auth`. Ответ «ключей нет»
означает, что сервер поднялся, а ключи не дошли: смотреть шаг 3. Каталог
отвечает `avito_list_sections`, в нём 64 методов.

## Если не поднимается

- `uvx` не найден: шаг 1, потом перезапустить клиент, он читает PATH при старте.
- Пусто в списке инструментов: клиент не перечитал конфигурацию, нужен рестарт.
- Ошибка авторизации при верных ключах: активный кабинет в
  `~/.marketplace-mcp/cabinets.json` имеет приоритет над переменными окружения.
