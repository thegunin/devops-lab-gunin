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

## Этап 2. Настройка конфигурации MkDocs

На данном этапе был настроен сайт с использованием темы **Material**.  
Файл `mkdocs.yml` был изменён для настройки внешнего вида и структуры сайта.

### Конфигурационный файл `mkdocs.yml`

```yaml
site_name: Кампус ИТМО в Москве
site_description: Вымышленный учебный проект – демонстрационный сайт для курсовой работы
site_author: Gunin Nikita
site_url: https://thegunin.github.io/devops-lab-gunin/coursework/

theme:
  name: material
  language: ru
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.top
    - toc.integrate
    - search.highlight
    - search.share
    - content.code.copy
  palette:
    scheme: default
    primary: blue
    accent: deep purple

nav:
  - Главная: index.md
  - О проекте: about.md
  - Архитектура кампуса: architecture.md
  - Технологии: technologies.md
  - Контакты: contacts.md

markdown_extensions:
  - admonition
  - codehilite
  - footnotes
  - tables
  - toc:
      permalink: true

extra:
  generator: false
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/thegunin
```

Создана структура страниц сайта:

```
docs/
├── index.md
├── about.md
├── architecture.md
├── technologies.md
└── contacts.md
```

Также на главной странице добавлено предупреждение о вымышленности проекта:

```markdown
!!! warning "Важно"
    Данный сайт является **учебным проектом** и описывает **вымышленный проект Кампуса ИТМО в Москве**.
```

Скриншоты этапа расположены в папке `screenshots/`:
- `mkdocs_config.png` — файл конфигурации MkDocs
- `mkdocs_pages.png` — структура страниц сайта

---