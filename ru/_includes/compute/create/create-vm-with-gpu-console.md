Чтобы создать виртуальную машину:
1. В [консоли управления](https://console.cloud.yandex.ru) выберите каталог, в котором будет создана виртуальная машина.
1. В списке сервисов выберите **{{ compute-name }}**.
1. Нажмите кнопку **Создать ВМ**.
1. В блоке **Базовые параметры**:
    - Введите имя и описание ВМ.

        {% include [name-fqdn](../name-fqdn.md) %}
        
    - (опционально) Выберите или создайте [сервисный аккаунт](../../../iam/concepts/users/service-accounts.md). Использование сервисного аккаунта позволяет гибко настраивать права доступа к ресурсам.

    - Выберите [зону доступности](../../../overview/concepts/geo-scope.md), в которой будет находиться виртуальная машина.
1. В блоке **Публичные образы** выберите один из GPU-ориентированных образов и версию операционной системы.

   {% include [gpu-os](../gpu-os.md) %}
   
1. (опционально) В блоке **Диски** настройте загрузочный диск:
      - Укажите нужный размер диска.
      - Выберите [тип диска](../../../compute/concepts/disk.md#disks_types).
   Если вы хотите создать виртуальную машину из существующего диска, в блоке **Диски** [добавьте диск](../../../compute/operations/vm-create/create-from-disks.md).
1. В блоке **Вычислительные ресурсы**:
    - Выберите [платформу](../../../compute/concepts/vm-platforms.md#gpu-platforms) Intel Broadwell with NVIDIA Tesla v100.
    - Выберите [конфигурацию](../../../compute/concepts/gpus.md#config) виртуальной машины.
      - Укажите необходимое количество GPU.
1. В блоке **Сетевые настройки**:
    - Укажите идентификатор подсети или выберите [облачную сеть](../../../vpc/concepts/network.md#network) из списка. Если сети нет, нажмите кнопку **Создать новую сеть** и создайте ее:
        - В открывшемся окне укажите имя новой сети и выберите, к какой подсети необходимо подключить виртуальную машину. У каждой сети должна быть как минимум одна [подсеть](../../../vpc/concepts/network.md#subnet) (если подсети нет, создайте ее). Затем нажмите кнопку **Создать**.
    - В поле **Публичный адрес** выберите способ назначения адреса:
        - **Автоматически** — чтобы назначить случайный IP-адрес из пула адресов Яндекс.Облака.
        - **Список** — чтобы выбрать публичный IP-адрес из списка зарезервированных заранее статических адресов. Подробнее читайте в разделе [{#T}](../../../vpc/operations/set-static-ip.md).
        - **Без адреса** — чтобы не назначать публичный IP-адрес.
    - (опционально) Выберите опцию [защиты от DDoS-атак](../../../vpc/ddos-protection/).
1. В блоке **Доступ** укажите данные для доступа на виртуальную машину:
    - В поле **Логин** введите имя пользователя.
      
      {% note alert %}
    
      Не используйте логин `root` или другие имена, зарезервированные операционной системой. Для выполнения операций, требующих прав суперпользователя, используйте команду `sudo`. 
    
      {% endnote %}
      
    - В поле **SSH-ключ** вставьте содержимое файла [открытого ключа](../../../compute/quickstart/quick-create-linux.md#create-ssh).
1. Нажмите кнопку **Создать ВМ**.

Виртуальная машина появится в списке. При создании виртуальной машине назначаются [IP-адрес](../../../vpc/concepts/address) и [имя хоста](../../../vpc/concepts/address#imya-hosta-(fqdn)) (FQDN).
