# Отчёт по лабораторной работе №3
"Мониторинг с Prometheus и Grafana"

University: [ITMO University](https://itmo.ru/ru/)  
Faculty: FTMI  
Course: [introduction-in-web-tech](https://itmo-ict-faculty.github.io/introduction-in-web-tech)  
Year: 2025/2026  
Group: U4225  
Author: Gunin Nikita Alekseevich  
Lab: Lab3  
Date of create:  __.__.2025  
Date of finish:  

---

## Шаг 1. Подготовка проекта

Создана структура для лабораторной работы №3 в папке `lab3`, а также файл отчёта и директория для скриншотов.

devops-lab-gunin/
├── lab3/
│   ├── lab3_report.md
│   └── screenshots/

![project_structure](screenshots/project_structure.png)

---

## Шаг 2. Создание конфигурации Prometheus

Для настройки мониторинга был создан файл конфигурации `prometheus.yml`, в котором указаны цели (targets) для сбора метрик:
- сам Prometheus (порт 9090)
- Node Exporter (порт 9100)

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']


---

## Шаг 3. Запуск Node Exporter

Для сбора системных метрик был запущен контейнер `node-exporter`:

```bash
docker run -d \
  --name node-exporter \
  --restart=unless-stopped \
  -p 9100:9100 \
  -v "/proc:/host/proc:ro" \
  -v "/sys:/host/sys:ro" \
  -v "/:/rootfs:ro" \
  prom/node-exporter \
  --path.procfs=/host/proc \
  --path.rootfs=/rootfs \
  --path.sysfs=/host/sys \
  --collector.filesystem.mount-points-exclude="^/(sys|proc|dev|host|etc)($$|/)"

Проверка работы контейнера:
Node Exporter отдаёт метрики по HTTP на порту 9100


---

## Шаг 4. Запуск Prometheus

Prometheus используется как система мониторинга для сбора и хранения метрик.  
Для сохранения данных Prometheus был создан Docker volume:

```bash
docker volume create prometheus-data

Prometheus был запущен как Docker-контейнер с использованием предварительно созданного конфига prometheus.yml:

docker run -d \
  --name prometheus \
  -p 9090:9090 \
  -v $(pwd)/prometheus:/etc/prometheus \
  prom/prometheus \
  --config.file=/etc/prometheus/prometheus.yml

  Проверка работы контейнера:
  Веб-интерфейс Prometheus доступен по адресу http://localhost:9090, что подтверждает его успешный запуск:


  ![prometheus](lab3/screenshots/prometheus.png)


  ---

## Шаг 5. Запуск Grafana

Для визуализации метрик был запущен контейнер Grafana:

```bash
docker volume create grafana-data

docker run -d \
  --name grafana \
  --restart=unless-stopped \
  -p 3000:3000 \
  -v grafana-data:/var/lib/grafana \
  -e "GF_SECURITY_ADMIN_PASSWORD=admin" \
  grafana/grafana

  Проверка работы контейнера:
  Веб-интерфейс Grafana доступен по адресу http://localhost:3000:


  ---

## Шаг 6. Подключение Prometheus как источника данных в Grafana

Для подключения Prometheus к Grafana был добавлен новый Data Source:

1. Открыт интерфейс Grafana по адресу `http://localhost:3000`
2. Выбрано меню **Connections → Add new data source**
3. Выбран источник данных **Prometheus**
4. Указан URL подключения:
http://host.docker.internal:9090

(Адрес `localhost` не подходит, так как Grafana работает в Docker-контейнере и не может обратиться к Prometheus напрямую)

5. Нажата кнопка **Save & Test** — подключение выполнено успешно ✅

![grafana_prometheus_connected](screenshots/grafana_prometheus_connected.png)