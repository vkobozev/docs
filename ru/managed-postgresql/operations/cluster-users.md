# Управление пользователями БД

Вы можете добавлять и удалять пользователей, а также управлять их индивидуальными настройками.

## Получить список пользователей {#list-users}

{% list tabs %}

- Консоль управления
  
  1. Перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Пользователи**.
  
- CLI
  
  {% include [cli-install](../../_includes/cli-install.md) %}
  
  {% include [default-catalogue](../../_includes/default-catalogue.md) %}
  
  Чтобы получить список пользователей кластера, выполните команду:
  
  ```
  $ yc managed-postgresql user list
       --cluster-name=<имя кластера>
  ```
  
  Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md).
  
{% endlist %}

## Добавить пользователя {#adduser}

При добавлении пользователя {{ mpg-short-name }} по умолчанию резервирует для него 50 подключений к {{ PG }}-кластеру (параметр `connlimit`). Минимальное количество подключений на пользователя — 10.

{% include [note-pg-user-connections.md](../../_includes/mdb/note-pg-user-connections.md) %}

{% list tabs %}

- Консоль управления
  
  1. Перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **Пользователи**.
  1. Нажмите кнопку **Добавить**.
  1. Введите имя пользователя БД и пароль (от 8 до 128 символов).
  
- CLI
  
  {% include [cli-install](../../_includes/cli-install.md) %}
  
  {% include [default-catalogue](../../_includes/default-catalogue.md) %}
  
  Чтобы создать пользователя в кластере, выполните команду:
  
  ```
  $ yc managed-postgresql user create <имя пользователя>
       --cluster-name=<имя кластера>
       --password=<пароль для пользователя>
       --permissions=<список баз, к которым пользователь должен иметь доступ>
       --conn-limit=<максимальное количество соединений для пользователя>
  ```
  
  Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md).
  
{% endlist %}

## Изменить пользователя {#updateuser}

Для пользователя можно изменить:

- имя и пароль;
- список баз, к которым у него есть доступ;
- ограничение на количество подключений к базам.

{% list tabs %}

- Консоль управления
  
  В консоли управления пока можно изменить только пароль пользователя БД:
  
  1. Перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **Пользователи**.
  1. Нажмите значок ![image](../../_assets/vertical-ellipsis.svg) и выберите пункт **Изменить пароль**.
  
- CLI
  
  {% include [cli-install](../../_includes/cli-install.md) %}
  
  {% include [default-catalogue](../../_includes/default-catalogue.md) %}
  
  Чтобы изменить пароль пользователя или список доступных ему баз данных, выполните команду:
  
  ```
  $ yc managed-postgresql user update <имя пользователя>
       --cluster-name=<имя кластера>
       --password=<пароль для пользователя>
       --permissions=<список баз, к которым пользователь должен иметь доступ>
       --conn-limit=<максимальное количество соединений для пользователя>
  ```
  
  Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md).
  
{% endlist %}

## Удалить пользователя {#removeuser}

{% list tabs %}

- Консоль управления
  
  1. Перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **Пользователи**.
  1. Нажмите значок ![image](../../_assets/vertical-ellipsis.svg) и выберите пункт **Удалить**.
  
- CLI
  
  {% include [cli-install](../../_includes/cli-install.md) %}
  
  {% include [default-catalogue](../../_includes/default-catalogue.md) %}
  
  Чтобы удалить пользователя, выполните команду:
  
  ```
  $ yc managed-postgresql user delete <имя пользователя>
       --cluster-name=<имя кластера>
  ```
  
  Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md).
  
{% endlist %}
