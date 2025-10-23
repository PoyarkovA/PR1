# 📢 Prometheus Alertmanager конфигурация

## 📘 Описание проекта
**Alertmanager** — это компонент экосистемы Prometheus, предназначенный для обработки оповещений (alerts), поступающих от Prometheus.  
Он группирует, подавляет и отправляет уведомления через различные каналы: Email, Slack, Telegram и др.

> 🧠 *"Alertmanager обеспечивает централизованное управление оповещениями в системах мониторинга, помогая вовремя реагировать на инциденты."*

---

## ⚙️ Основные правила оповещений

| Название правила | Условие срабатывания | Действие | Канал уведомления |
|------------------|----------------------|----------|-------------------|
| HighCPUUsage     | CPU > 80% за 5 мин   | Отправка алерта | Slack |
|InstanceDown     | Экземпляр не отвечает | Уведомление дежурного | Email |
|MemoryLeakAlert  | Memory > 90%         | Эскалация | Telegram |

<span style="color:red; font-weight:bold;">❗Важно:</span> корректная настройка маршрутов (routes) в Alertmanager влияет на доставку уведомлений.

---

## 📄 Пример конфигурационного файла `alertmanager.yml`

```yaml
global:
  resolve_timeout: 5m

route:
  receiver: 'default-receiver'
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 1h

receivers:
  - name: 'default-receiver'
    email_configs:
      - to: 'admin@example.com'

inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname']
