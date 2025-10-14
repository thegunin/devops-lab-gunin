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