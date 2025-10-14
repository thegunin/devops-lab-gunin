# Курсовая работа
**Тема:** Создание персонального сайта на MkDocs  
**Проект:** Вымышленный проект "Кампус ИТМО в Москве"  
**Автор:** Gunin Nikita Alekseevich  
**Группа:** U4225  
**Курс:** Introduction in Web Technologies  
**Год:** 2025/2026

---

## Этап 1. Подготовка проекта

Для выполнения курсовой работы создана папка проекта `coursework` внутри репозитория.  
Было настроено виртуальное окружение Python и установлены инструменты **MkDocs** и **Material for MkDocs**.

### Команды настройки:

```bash
python3 -m venv venv
source venv/bin/activate
pip install mkdocs mkdocs-material
mkdocs new .
```

### Структура проекта:

```
coursework/
├── docs/
│   └── index.md
├── mkdocs.yml
├── venv/
└── coursework_report.md
```

Скриншоты этапа расположены в папке `screenshots/`:
- `screenshots/project_structure.png` — структура проекта
- `screenshots/mkdocs_serve.png` — результат запуска локального сервера `mkdocs serve`

---