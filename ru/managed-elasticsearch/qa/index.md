---
title: "{{ mes-name }}. Ответы на вопросы"
description: "Как получить логи моей работы в управляемом сервисе Elasticsearch?  Ответы на этот и другие вопросы в данной статье."
---

# Вопросы и ответы о {{ mes-short-name }}

{% include [Elasticsearch-end-of-service](../../_includes/mdb/mes/note-end-of-service.md) %}

## Общие вопросы {#general}

#### Как происходит обслуживание кластеров {{ ES }}? {#service-window}

Под обслуживанием в {{ mes-short-name }} понимается:

* автоматическая установка обновлений и исправлений {{ ES }} для ваших хостов;
* изменение класса хостов и объема хранилища;
* другие сервисные работы {{ mes-short-name }}.

Подробнее в разделе [{#T}](../concepts/maintenance.md).

#### Включено ли резервное копирование кластеров по умолчанию? {#default-backup}

Да, по умолчанию резервное копирование включено. Для кластеров {{ mes-name }} выполняется полное резервное копирование один раз в час и сохраняются все индексы. Это позволяет восстановить состояние кластера из любой доступной резервной копии.

Резервные копии хранятся 7 дней.

#### Какую версию {{ ES }} использует {{ mes-short-name }}? {#dbms-version}

В {{ mes-short-name }} доступны версии {{ ES }}, поддерживаемые производителем. Подробнее см. в разделе [{#T}](../concepts/update-policy.md).


#### Что происходит, когда выпускается новая версия {{ ES }}? {#new-version}

При выходе новых версий, содержащих только исправления ошибок (_maintenance release_), программное обеспечение кластеров автоматически обновляется после короткого периода тестирования.

Владельцы затронутых кластеров БД получают предварительное оповещение о сроках проведения работ и доступности баз данных.


#### Что происходит, когда версия {{ ES }} становится неподдерживаемой? {#dbms-deprecated}

{{ mes-short-name }} автоматически оповещает владельцев кластеров о том, что срок поддержки используемой версии {{ ES }} истекает, по электронной почте.

Кластеры с неподдерживаемой версией {{ ES }} обновляются в соответствии с [политикой управления версиями](../concepts/update-policy.md).

Владельцы затронутых кластеров получают предварительное оповещение о сроках проведения работ и доступности баз данных.

{% include [logs](../../_qa/logs.md) %}

{% include [log-duration](../../_includes/mdb/log-duration-qa.md) %}

#### Как настроить алерт, который срабатывает при заполнении определенного процента дискового пространства? {#disk-space-percentage}

[Создайте алерт](../../managed-elasticsearch/operations/monitoring.md#monitoring-integration) с метрикой `disk.used_bytes` в сервисе {{ monitoring-full-name }}. Метрика показывает размер использованного дискового пространства в кластере {{ mes-name }}.

Для `disk.used_bytes` используются пороги для оповещения. Их рекомендуемые значения:

* `{{ ui-key.yacloud_monitoring.monitoring-alerts.status.alarm }}` — 90% дискового пространства.
* `{{ ui-key.yacloud_monitoring.monitoring-alerts.status.warn }}` — 80% дискового пространства.

Значения порогов задаются только в байтах. Например, рекомендуемые значения для диска размером в 100 ГБ:

* `{{ ui-key.yacloud_monitoring.monitoring-alerts.status.alarm }}` — `96636764160` байтов (90%).
* `{{ ui-key.yacloud_monitoring.monitoring-alerts.status.warn }}` — `85899345920` байтов (80%).

#### Почему кластер работает медленно, хотя вычислительные ресурсы использованы не до предела? {#throttling}

{% include [throttling](../../_qa/throttling.md) %}

Чтобы увеличить максимальные значения IOPS и bandwidth и снизить вероятность троттлинга, расширьте размер хранилища при [изменении кластера](../operations/cluster-update.md#change-disk-size).

Если вы используете хранилище с типом диска `network-hdd`, рассмотрите возможность перехода на `network-ssd` или `network-ssd-nonreplicated` путем [восстановления кластера](../operations/cluster-backups.md#restore) из резервной копии.
