# Описание проекта

Проект "Docker Playbook" представляет собой набор Ansible playbook'ов и Dockerfile, предназначенных для автоматизации установки и настройки Docker, Nginx и других необходимых пакетов на серверах Ubuntu. Этот проект упрощает процесс развертывания и управления серверной инфраструктурой.

## Структура проекта

Проект состоит из следующих файлов:

- **docker-playbook.yaml**: Playbook для установки Docker на сервере Ubuntu.
- **playbook.yaml**: Основной playbook для начальной настройки сервера, включая установку Nginx и других пакетов.
- **playbook2.yaml**: Playbook для манипуляции файлами, включая копирование файлов и создание директорий.
- **role_playbook**: Playbook для установки Nginx с использованием ролей Ansible.
- **Dockerfile**: Файл, используемый для создания образа Docker с предустановленным SSH-сервером.

## Установка и использование

### Требования

Перед началом убедитесь, что у вас установлены следующие компоненты:

- [Ansible](https://www.ansible.com/)
- [Docker](https://www.docker.com/)
- Сервер на базе Ubuntu

### Шаги по установке

1. **Клонируйте репозиторий**:
   ```bash
   git clone <URL вашего репозитория>
   cd <имя_репозитория>
   ```

2. **Настройка инвентарного файла**:
   Убедитесь, что у вас есть файл инвентаризации Ansible, который содержит адреса ваших целевых серверов. Например:
   ```ini
   [ubuntu]
   ваш_сервер_ip
   ```

3. **Запуск playbook для установки Docker**:
   Выполните следующий команду для установки Docker:
   ```bash
   ansible-playbook -i inventory docker-playbook.yaml
   ```

4. **Запуск основного playbook для начальной настройки сервера**:
   Выполните следующий команду для настройки сервера:
   ```bash
   ansible-playbook -i inventory playbook.yaml
   ```

5. **Запуск playbook для манипуляции файлами**:
   Для выполнения операций с файлами используйте:
   ```bash
   ansible-playbook -i inventory playbook2.yaml
   ```

6. **Запуск role_playbook для установки Nginx**:
   Чтобы установить Nginx с использованием ролей, выполните:
   ```bash
   ansible-playbook -i inventory role_playbook.yaml
   ```

### Создание образа Docker

Чтобы создать образ Docker с предустановленным SSH-сервером, выполните следующие шаги:

1. Перейдите в директорию с вашим `Dockerfile`.
2. Выполните команду сборки образа:
   ```bash
   docker build -t my-ssh-server .
   ```

3. Запустите контейнер на основе созданного образа:
   ```bash
   docker run -d -p 2222:22 my-ssh-server
   ```

Теперь вы можете подключиться к вашему SSH-серверу на порту 2222.

## Описание файлов

### docker-playbook.yaml

Этот playbook устанавливает Docker и его зависимости на сервере Ubuntu:

```yaml
- name: Install Docker
  hosts: ubuntu
  become: yes
  tasks:
    - name: Install Packages
      apt:
        name: "{{item}}"
        state: present
      with_items:
        - curl
        - apt-transport-https
        - ca-certificates
        - software-properties-common
    - name: Add keys
      shell:
        cmd: curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
    - name: Add repo
      shell:
        cmd: add-apt-repository -y "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
    - name: Install Docker
      apt:
        name: docker-ce
        state: present
```

### playbook.yaml

Этот playbook выполняет начальную настройку сервера:

```yaml
- name: Initial server setup
  hosts: ubuntu
  gather_facts: no
  tasks:
    ...
```

### playbook2.yaml

Playbook для манипуляции файлами:

```yaml
- name: File manipulation
  hosts: ubuntu
  tasks:
    ...
```

### role_playbook

Playbook для установки Nginx с использованием ролей Ansible:

```yaml
- name: Nginx installation
  become: yes 
  hosts: ubuntu
  roles:
    - nginx
```

### Dockerfile

Файл для создания образа Docker с SSH-сервером:

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y openssh-server 
...
CMD ["/usr/sbin/sshd", "-D"]
```

## Заключение

Проект "Docker Playbook" предоставляет удобные инструменты для автоматизации установки и настройки серверов Ubuntu с использованием Ansible и Docker. Если у вас есть вопросы или предложения по улучшению проекта, пожалуйста, свяжитесь с нами!
