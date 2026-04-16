# X-UI Grafana Dashboard

Мониторинг пользователей x-ui через стек **Loki + Promtail + Grafana**.

## Структура

```
xui-grafana/
├── docker-compose.yml
├── loki/
│   └── loki-config.yml
├── promtail/
│   └── promtail-config.yml
└── grafana/
    ├── dashboards/
    │   └── xui-dashboard.json
    └── provisioning/
        ├── datasources/loki.yml
        └── dashboards/dashboard.yml
```

## Запуск

```bash
# 1. Скопируй папку на сервер
scp -r xui-grafana/ user@your-server:~/

# 2. Зайди на сервер и запусти
ssh user@your-server
cd xui-grafana
docker compose up -d

# 3. Проверь что всё работает
docker compose ps
docker compose logs promtail --follow
```

## Доступ

- **Grafana**: http://YOUR_SERVER_IP:3000
- Логин: `admin`
- Пароль: `admin123` (смени после первого входа!)

## Что отображается на дашборде

| Панель | Описание |
|--------|----------|
| Всего подключений | Суммарное кол-во за выбранный период |
| Активные пользователи | Кто был активен в последние 5 минут |
| TCP vs UDP | Соотношение протоколов (donut chart) |
| График по времени | Активность каждого пользователя |
| Топ пользователей | Кто больше всего подключался |
| Топ доменов | Какие сайты/сервисы используют |
| Live лог | Последние события в реальном времени |

## Требования к серверу

- Docker + Docker Compose v2
- Открытый порт 3000 (или настрой nginx reverse proxy)
- Путь к логам x-ui: `/usr/local/x-ui/access.log`

## Если логи находятся в другом месте

Поменяй путь в `promtail/promtail-config.yml`:

```yaml
volumes:
  - /твой/путь/к/логам:/var/log/x-ui:ro
```

## Безопасность

⚠️ Рекомендуется:
- Сменить пароль admin после первого входа
- Закрыть порт 3000 через firewall, использовать nginx с HTTPS
- Порт 3100 (Loki) не открывать наружу
