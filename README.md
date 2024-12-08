```markdown
# Ansible Server Automation and Docker Deployment

## Описание проекта

Данный репозиторий содержит Ansible-плейбуки и Dockerfile для автоматизации серверных задач, включая установку Docker, настройку Nginx, базовую настройку серверов, управление файлами и создание контейнера Ubuntu с SSH. Это решение подходит для быстрой настройки окружения, автоматизации задач DevOps и управления инфраструктурой.

---

## Структура репозитория

```
.
├── docker-playbook.yaml     # Плейбук для установки Docker
├── playbook.yaml            # Плейбук для настройки сервера (Nginx, пакеты, пользователь)
├── playbook2.yaml           # Плейбук для работы с файлами
├── role_playbook.yaml       # Плейбук для установки Nginx через роль
├── roles/
│   └── nginx/               # Роль для установки Nginx
├── Dockerfile               # Dockerfile для создания контейнера Ubuntu с SSH
```

---

## Установка и использование

### 1. Настройка Ansible

Убедитесь, что на управляющей машине установлен Ansible. Целевые серверы должны быть доступны через SSH. Вы можете запускать плейбуки, указывая IP-адрес сервера в команде.

Пример команды:
```bash
ansible-playbook -i "192.168.1.100," -u your_user --private-key your_key.pem <playbook>.yaml
```

### 2. Плейбуки

#### Установка Docker
Для установки Docker на сервер:
```bash
ansible-playbook -i "192.168.1.100," -u your_user --private-key your_key.pem docker-playbook.yaml
```

#### Настройка сервера (Nginx и пакеты)
Для базовой настройки сервера:
```bash
ansible-playbook -i "192.168.1.100," -u your_user --private-key your_key.pem playbook.yaml
```

#### Работа с файлами
Для копирования файлов и создания директорий:
```bash
ansible-playbook -i "192.168.1.100," -u your_user --private-key your_key.pem playbook2.yaml
```

#### Установка Nginx через роль
Для установки и настройки Nginx с использованием роли:
```bash
ansible-playbook -i "192.168.1.100," -u your_user --private-key your_key.pem role_playbook.yaml
```

### 3. Dockerfile
Для создания контейнера Ubuntu с поддержкой SSH:
1. Постройте образ Docker:
   ```bash
   docker build -t ubuntu-ssh .
   ```
2. Запустите контейнер:
   ```bash
   docker run -d -p 2222:22 ubuntu-ssh
   ```
3. Подключитесь к контейнеру через SSH:
   ```bash
   ssh root@localhost -p 2222
   # Пароль: admin
   ```

---

## Содержание файлов

### `docker-playbook.yaml`
Плейбук для установки Docker CE:
- Установка необходимых пакетов.
- Добавление ключей GPG и репозитория Docker.
- Установка Docker CE.

### `playbook.yaml`
Базовая настройка сервера:
- Установка Nginx и дополнительных пакетов.
- Настройка сетевого интерфейса.
- Создание пользователя `test-user`, если Nginx запущен.

### `playbook2.yaml`
Работа с файлами:
- Копирование файлов на сервер.
- Создание директорий.
- Перемещение файлов в созданные директории.

### `role_playbook.yaml`
Использование роли `nginx`:
- Установка и настройка Nginx.

### `roles/nginx/`
Роль для установки Nginx:
- Устанавливает и запускает Nginx.
- Гибко настраивается через переменные.

### `Dockerfile`
Dockerfile для создания контейнера:
- Базовый образ Ubuntu.
- Установка и настройка SSH.
- Позволяет подключаться к контейнеру через `root` с паролем `admin`.

---

## Заключение

Этот репозиторий предоставляет инструменты для автоматизации задач серверной настройки и развёртывания приложений. Использование Ansible и Docker облегчает управление инфраструктурой, позволяя быстро настраивать серверы и контейнеры для ваших приложений.
```