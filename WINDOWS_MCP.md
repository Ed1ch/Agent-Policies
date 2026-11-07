# Выдача общих Agent Policies через Windows MCP

**Статус:** будущий интерфейс; существующий Windows MCP не объявлен защищённым.

- Legacy MCP: сервер может возвращать `instructions` при `initialize`.
- MCP 2026-07-28: `server/discover` может отдавать `instructions`, но клиент может не вызывать discovery и не показывать инструкции модели.
- Поэтому инструкции в MCP не гарантируют чтение или соблюдение правил.

## Возможная реализация (отложена)

1. Проверенный локальный policy bundle, привязанный к одобренной ревизии Git и доверенной подписи/корню проверки; не исполнять GitHub `main` автоматически.
2. `get_agent_rules` (read-only): эффективные общие правила, профиль хоста, `policy_version`, `policy_revision`, `policy_digest`, `verification`, `enforcement_mode`, `server_build`, `host_id`, `principal`, `effective_scope`.
3. Broker проверяет **каждую** операцию по host, scope, правам и approval, включая PowerShell, GUI и FileSystem. Недействительная policy → запрет изменяющих действий.
4. Журнал разрешений/отказов с версией политики, без секретов. Проверять также клиент, который игнорирует `get_agent_rules`.

Для Windows MCP пока используется Context-First: Project Instructions + [GENERAL.md](GENERAL.md) + [TOOL_SELECTION.md](TOOL_SELECTION.md); это правила поведения, а не sandbox.

Спецификация: [MCP lifecycle 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle), [MCP discovery 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/server/discover).
