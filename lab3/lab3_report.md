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