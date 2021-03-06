1. В консоли управления выберите каталог, в котором нужно создать группу виртуальных машин.
1. Выберите сервис **[!KEYREF compute-full-name]**.
1. На странице **Виртуальные машины** перейдите на вкладку **Группы виртуальных машин**.
1. Нажмите кнопку **Создать группу**.
1. В блоке **Базовые параметры** введите:
    - Имя группы в поле **Имя**. Имя группы должно быть уникальным в Облаке.

        [!INCLUDE [name-format](../../_includes/name-format.md)]

    - Описание группы в поле **Описание**.
1. В блоке **Распределение** выберите зоны доступности. Виртуальные машины группы могут находиться в разных зонах и регионах доступности. [Подробнее о географии Облака](../../overview/concepts/geo-scope.md).
1. В блоке **Шаблон виртуальной машины** нажмите кнопку **Добавить**:
    - Выберите нужный публичный [образ](../../compute/operations/images-with-pre-installed-software/get-list.md).
    - В блоке **Диски**:
        - Выберите [тип диска](../../compute/concepts/disk.md#concept_z5t_rtr_52b) (HDD или NVME).
        - Укажите размер диска.

            Чтобы добавить дополнительные диски, нажмите **Добавить диск**.
    - В блоке **Вычислительные ресурсы**:
        - Выберите [тип виртуальной машины](../../compute/concepts/vm-types.md) (стандартная или легкая).
        - Укажите необходимое количество vCPU и объем RAM.
    - В блоке **Сетевые настройки**:
        - Выберите [облачную сеть](../../compute/concepts/vm.md#network).
        - Отметьте необходимость в публичном IP-адресе.
    - В блоке **Доступ** укажите данные для доступа на виртуальную машину:
        - В поле **Логин** введите имя пользователя.
        - В поле **SSH ключ** вставьте содержимое файла открытого ключа.

            Пару ключей для подключения по SSH необходимо создать самостоятельно. Для создания ключей используйте сторонние инструменты, например утилиты `ssh-keygen` в Linux и macOS или [PuTTYgen](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) в Windows.
    - Нажмите кнопку **Добавить**.
1. В блоке **Развертывание** укажите:
    - Расширение размера группы.
    - Уменьшение размера группы.
    - Максимальное количество виртуальных машин при создании группы.
    - Максимальное количество виртуальных машин при удалении группы.

        Подробнее читайте в разделе [[!TITLE]](../../instance-groups/concepts/instance-group-policies.md#deploy-policy).
1. В блоке **Масштабирование**:
    - Выберите [тип группы](../../instance-groups/concepts/instance-group-types.md). На данный момент можно создавать только группы с фиксированным типом.
    - Укажите необходимое количество виртуальных машин.
1. Нажмите кнопку **Создать**.
