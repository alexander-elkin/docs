# Статусы виртуальной машины в группе ВМ

## Список статусов

Виртуальная машина в группе ВМ может находиться в одном из следующих статусов:

Статус | Описание
----- | -----
`CREATING` | Создание виртуальной машины в системе [!KEYREF compute-full-name].
`STARTING` | Запуск операционной системы и пользовательского приложения.
`OPENING` | Виртуальная машина, с точки зрения сетевого балансировщика, готова получать трафик.
`WARMING` | Прогрев пользовательского приложения.
`RUNNING` | Виртуальная машина работает.
`CLOSING` | Виртуальная машина, с точки зрения сетевого балансировщика, перестала получать трафик.
`STOPPING` | Остановка виртуальной машины.
`DELETING` | Удаление виртуальной машины в системе [!KEYREF compute-full-name].
`UPDATING` | Обновление базовых параметров или метаданных без пересоздания виртуальной машины.
`FAILED` | Сбой при выполнении операции. Вывести виртуальную машину из этого состояния вы должны самостоятельно. Проверьте не превышены ли [лимиты](limits.md) и пересоздайте группу.

## Операции с группой виртуальных машин

Операции, которые можно производить с группой виртуальных машин:

- [Создание](#create).
- [Удаление](#update).
- [Изменение](#update).

### Создание {#create}

При [создании группы](../operations/create-fixed-group.md) каждая виртуальная машина с учетом [политики развертывания](instance-group-policies.md#deploy-policy) проходит следующие стадии:

1. `CREATING` — виртуальной машине выделяются вычислительные ресурсы: количество и производительность ядер процессора (vCPU), количество памяти (RAM). Назначается IP-адрес и создаются диски.
1. `STARTING` — запуск виртуальной машины.
1. `OPENING` — виртуальная машина, с точки зрения сетевого балансировщика, готова получать трафик.
1. `WARMING` — виртуальная машина прогревается: наполняются кэши и происходят различные оптимизации.
1. `RUNNING` — виртуальная машина перешла в рабочий режим.

### Удаление {#delete}

При [удалении группы](../operations/delete.md) каждая виртуальная машина с учетом [политики развертывания](instance-group-policies.md#deploy-policy) проходит следующие стадии:

1. `CLOSING` — завершаются все текущие операции, и на машину прекращает подаваться трафик.
1. `STOPPING` — остановка виртуальной машины.
1. `DELETING` — виртуальная машина удаляется. Когда удаление завершится, виртуальная машина исчезнет из списка доступных ресурсов. После удаления всех виртуальных машин, группа также исчезнет из списка доступных ресурсов.

### Изменение {#update}

При [изменении группы](../operations/update.md) каждая виртуальная машина с учетом [политики развертывания](instance-group-policies.md#deploy-policy) проходит следующие стадии:

1. `CLOSING` — завершаются все текущие операции, и на машину прекращает подаваться трафик.
1. `STOPPING` — остановка виртуальной машины.
1. `DELETING` — виртуальная машина удаляется. Когда удаление завершится, машина исчезнет из списка доступных ресурсов.
1. `CREATING` — создается новая виртуальная машина по обновленной конфигурации. Машине выделяются вычислительные ресурсы: количество и производительность ядер процессора (vCPU), количество памяти (RAM). Назначается IP-адрес и создаются диски.
1. `STARTING` — запуск виртуальной машины.
1. `OPENING` — виртуальная машина, с точки зрения сетевого балансировщика, готова получать трафик.
1. `WARMING` — виртуальная машина прогревается: наполняются кэши и происходят различные оптимизации.
1. `RUNNING` — виртуальная машина перешла в рабочий режим.
