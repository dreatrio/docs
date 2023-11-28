# Ограничения в зоне доступности {{ region-id }}-d

В зоне доступности `{{ region-id }}-d` ряд сервисов работает с ограничениями.

Наши инженеры работают над их устранением. Информация на странице будет обновляться по мере снятия ограничений.

## {{ data-transfer-full-name }} {#data-transfer}

Полноценная работа сервиса {{ data-transfer-name }} внутри зоны `{{ region-id }}-d` еще не поддержана.

Для корректной работы {{ data-transfer-name }} требуется, чтобы источник или приемник данных размещался вне зоны `{{ region-id }}-d`. 

См. [рекомендации](../../data-transfer/operations/endpoint/migration-to-an-availability-zone.md) в документации сервиса.

## {{ cloud-desktop-name }} {#cloud-desktop}

Создание рабочих столов в зоне `{{ region-id }}-d` еще не поддержано.

## {{ ydb-name }} {#managed-ydb}

Создание баз в Dedicated-режиме в зоне `{{ region-id }}-d` еще не поддержано.