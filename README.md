# LabRes

## Окружение разработчика

### Базовая настройка
0. Установить WSL2 (опционально)
1. Установить docker, python3, poetry
2. В директории server выполнить команду: poetry install
3. Создать файл .env на основе .env-example

### Запуск сервисов
1. Выполнить команду: docker compose up --build

### Выполнение миграции БД
1. Выполнить команду: docker compose run server ./manage.py migrate
