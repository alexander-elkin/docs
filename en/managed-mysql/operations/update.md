# Changing cluster settings

After creating a cluster, you can:

* [Change the host class](#change-resource-preset).

* [Increase the storage size](#change-disk-size) (available only for network storage, `network-hdd` and `network-nvme`).

* [Configure the servers](#change-postgresql-config) [!KEYREF MY] as described in the [documentation [!KEYREF MY]](https://www.postgresql.org/docs/10/runtime-config.html).

## Change the host class {#change-resource-preset}

---

**[!TAB CLI]**

[!INCLUDE [cli-install](../../_includes/cli-install.md)]

[!INCLUDE [default-catalogue](../../_includes/default-catalogue.md)]

To change the [host class](../concepts / instance-types.md) for the cluster:

1. View the description of the CLI's update cluster command:

    ```
    $ [!KEYREF yc-mdb-my] cluster update --help
    ```

2. Request a list of available host classes (the `ZONES` column specifies the availability zones where you can select the appropriate class):

    ```
    $ [!KEYREF yc-mdb-my] resource-preset list
    
    +-----------+--------------------------------+-------+----------+
    |    ID     |            ZONE IDS            | CORES |  MEMORY  |
    +-----------+--------------------------------+-------+----------+
    | s1.nano   | ru-central1-a, ru-central1-b,  |     1 | 4.0 GB   |
    |           | ru-central1-c                  |       |          |
    | s1.micro  | ru-central1-a, ru-central1-b,  |     2 | 8.0 GB   |
    |           | ru-central1-c                  |       |          |
    | ...                                                           |
    +-----------+--------------------------------+-------+----------+
    ```

3. Specify the class in the update cluster command:

    ```
    $ [!KEYREF yc-mdb-my] cluster update <cluster name>
         --resource-preset <class ID>
    ```

    [!KEYREF mmy-short-name] will run the update host class command for the cluster.

**[!TAB API]**

You can change the [host class](../concepts/instance-types.md) using the API [update](../api-ref/Cluster/update.md) method: pass the necessary value in the request parameter `configSpec.clickhouse.resources.resourcePresetId`.

To request a list of supported values, use the [list](../api-ref/ResourcePreset/list.md) method for the `ResourcePreset` resources.

---

## Increasing the storage size {#change-disk-size}

---

**[!TAB CLI]**

[!INCLUDE [cli-install](../../_includes/cli-install.md)]

[!INCLUDE [default-catalogue](../../_includes/default-catalogue.md)]

To increase the storage size for a cluster:

1. View the description of the CLI's update cluster command:

    ```
    $ [!KEYREF yc-mdb-my] cluster update --help
    ```

2. Make sure the cloud's quota is sufficient to increase the storage size: open the [Quotas](https://console.cloud.yandex.ru/?section=quotas) for your cloud and check that the [!KEYREF mmy-full-name] section still has space remaining in the **space** line.

3. Make sure the required cluster is using network storage (it is not yet possible to increase the size of local storage). To do this, request information about the cluster and find the `disk_type_id` field: it should be set to `network-hdd` or `network-nvme`:

    ```
    $ [!KEYREF yc-mdb-my] cluster get <cluster name>
    
    id: c7qkvr3u78qiopj3u4k2
    folder_id: b1g0ftj57rrjk9thribv
    ...
    config:
      ...
      resources:
        resource_preset_id: s1.nano
        disk_size: "10737418240"
        disk_type_id: network-nvme
    ...
    ```

4. Specify the required amount of storage in the update cluster command (it must be at least as large as `disk_size` in the cluster properties):

    ```
    $ [!KEYREF yc-mdb-my] cluster update <cluster name>
         --disk-size <storage size in GB>
    ```

    If all requirements are met, [!KEYREF mmy-short-name] runs the operation to increase the storage size.

**[!TAB API]**

You can change the storage size for a cluster using the API [update](../api-ref/Cluster/update.md) method: pass the appropriate values in the request parameter `configSpec.resources.diskSize`.

Make sure the cloud's quota is sufficient to increase the storage size: open the [Quotas](https://console.cloud.yandex.ru/?section=quotas) for your cloud and check that the [!KEYREF mmy-full-name] section still has space remaining in the **space** line.

---

## Changing [!KEYREF MY] {#change-postgresql-config} settings

You can change the DBMS settings of the hosts in your cluster. All supported settings are described [in the API reference](../api-ref/Cluster/update.md).

---

**[!TAB CLI]**

[!INCLUDE [cli-install](../../_includes/cli-install.md)]

[!INCLUDE [default-catalogue](../../_includes/default-catalogue.md)]

To change [!KEYREF MY] server settings:

1. View the description of the CLI's update cluster configuration command:

    ```
    $ [!KEYREF yc-mdb-my] cluster update-config --help
    ```

2. Set the required parameter values.

    All supported parameters are listed in [the request format for the update method](../api-ref/Cluster/update.md), in the `mysql_config_5_7` field. To specify the parameter name in the CLI's call, convert the name from <q>lowerCamelCase</q> to <q>snake_case</q>. For example, the `logMinDurationStatement` parameter from an API request should be converted to `log_min_duration_statement` for the CLI command:

    ```
    $ [!KEYREF yc-mdb-my] cluster update-config <cluster name>
         --set log_min_duration_statement=100,<parameter name>=<value>,...
    ```

    [!KEYREF mmy-short-name] will run the operation for changing the cluster settings.

**[!TAB API]**

You can change the DBMS settings for a cluster using the API [update](../api-ref/Cluster/update.md) method: pass the appropriate values in the request parameter `configSpec.postgresqlConfig_10.config`.

---

