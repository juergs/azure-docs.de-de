---
title: Protokolle für langsame Abfragen – Azure Database for MariaDB
description: Beschreibt die Protokolle, die in Azure Database for MariaDB verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren verschiedener Protokolliergrade.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 651094f043162cdc5f6d522c90c7567ae94a4274
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75746661"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>Protokolle für langsame Abfragen in Azure Database for MariaDB
In Azure Database for MariaDB ist das Protokoll für langsame Abfragen für Benutzer verfügbar. Der Zugriff auf das Transaktionsprotokoll wird jedoch nicht unterstützt. Das Protokoll für langsame Abfragen kann verwendet werden, um Leistungsengpässe für die Problembehandlung zu erkennen.

Weitere Informationen zum Protokoll für langsame Abfragen finden Sie in der MariaDB-Dokumentation zum [Protokoll für langsame Abfragen](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-slow-query-logs"></a>Zugreifen auf Protokolle für langsame Abfragen
Sie können Protokolle für langsame Abfragen von Azure Database for MariaDB über das Azure-Portal und die Azure CLI auflisten und herunterladen.

Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Server aus. Wählen Sie unter der Überschrift **Überwachung** die Seite **Serverprotokolle** aus.

Weitere Informationen zur Azure CLI finden Sie unter [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-cli.md).

Auf ähnliche Weise können Sie die Protokolle mithilfe von Diagnoseprotokollen an Azure Monitor weiterreichen. Weitere Informationen finden Sie [unten](concepts-server-logs.md#diagnostic-logs).

## <a name="log-retention"></a>Protokollaufbewahrung
Protokolle sind für bis zu sieben Tage nach ihrer Erstellung verfügbar. Wenn die Gesamtgröße der verfügbaren Protokolle 7 GB überschreitet, werden die ältesten Dateien gelöscht, bis Speicherplatz verfügbar ist.

Die Protokolle werden alle 24 Stunden oder bei Erreichen einer Größe von 7 GB rotiert, je nachdem, welches Ereignis früher eintritt.

## <a name="configure-slow-query-logging"></a>Konfigurieren der Protokollierung für langsame Abfragen
Das Protokoll für langsame Abfragen ist standardmäßig deaktiviert. Legen Sie „slow_query_log“ auf „ON“ fest, um dieses Feature zu aktivieren.

Weitere Parameter, die Sie anpassen können:

- **long_query_time**: Die Abfrage wird protokolliert, wenn sie länger als „long_query_time“ (in Sekunden) dauert. Der Standardwert ist 10 Sekunden.
- **log_slow_admin_statements**: Wenn „ON“, sind administrative Anweisungen wie ALTER_TABLE und ANALYZE_TABLE in den Anweisungen enthalten, die in „slow_query_log“ geschrieben werden.
- **log_queries_not_using_indexes**: Bestimmt, ob Abfragen, die keine Indizes verwenden, in „slow_query_log“ protokolliert werden.
- **log_throttle_queries_not_using_indexes**: Dieser Parameter begrenzt die Anzahl der Nichtindexabfragen, die in das Protokoll für langsame Abfragen geschrieben werden können. Dieser Parameter wird wirksam, wenn „log_queries_not_using_indexes“ auf „ON“ festgelegt ist.
- **log_output**: Wenn „file“, kann das Protokoll für langsame Abfragen sowohl in den lokalen Serverspeicher als auch in Azure Monitor-Diagnoseprotokolle geschrieben werden. Wenn der Parameter „None“ lautet, wird das Protokoll für langsame Abfragen nur in Azure Monitor-Diagnoseprotokolle geschrieben. 

Vollständige Beschreibungen der Parameter des Protokolls für langsame Abfragen finden Sie in der [MariaDB-Dokumentation zum Protokoll für langsame Abfragen](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Azure Database for MariaDB ist in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie die Protokolle zu langsamen Abfragen auf Ihrem MariaDB-Server aktiviert haben, können Sie sie an Azure Monitor-Protokolle, Event Hubs oder Azure Storage ausgeben. Weitere Informationen zum Aktivieren von Diagnoseprotokollen finden Sie im Gewusst-wie-Abschnitt der [Dokumentation zu Diagnoseprotokollen](../azure-monitor/platform/platform-logs-overview.md).

> [!IMPORTANT]
> Dieses Diagnosefeature für Serverprotokolle steht nur in den [Tarifen](concepts-pricing-tiers.md) „Universell“ und „Arbeitsspeicheroptimiert“ zur Verfügung.

In der folgenden Tabelle wird der Inhalt der einzelnen Protokolle beschrieben. Je nach Ausgabemethode können die enthaltenen Felder und ihre Reihenfolge variieren.

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Name des Servers |
| `start_time_t` [UTC] | Uhrzeit, zu der die Abfrage begann |
| `query_time_s` | Gesamtzeit, die die Abfrage zur Ausführung benötigte |
| `lock_time_s` | Gesamtzeit, über die die Abfrage gesperrt war |
| `user_host_s` | Username |
| `rows_sent_s` | Anzahl gesendeter Zeilen |
| `rows_examined_s` | Anzahl untersuchter Zeilen |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | Einfüge-ID |
| `sql_text_s` | Vollständige Abfrage |
| `server_id_s` | Server-ID |
| `thread_id_s` | Thread-ID |
| `\_ResourceId` | Ressourcen-URI |

## <a name="next-steps"></a>Nächste Schritte
- [Gewusst wie: Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe des Azure-Portals](howto-configure-server-logs-portal.md).
