---
title: Konfigurieren und Lesen von Protokollen mit einem Azure Functions-Trigger für Cosmos DB
description: Hier erfahren Sie, wie Sie bei Verwendung eines Azure Functions-Triggers für Cosmos DB die Protokolle für die Azure Functions-Protokollierungspipeline verfügbar machen.
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 5ff747b225f8984bcaafd80015e85a9f014bdb50
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75441826"
---
# <a name="how-to-configure-and-read-the-logs-when-using-azure-functions-trigger-for-cosmos-db"></a>Konfigurieren und Lesen der Protokolle bei Verwendung eines Azure Functions-Triggers für Cosmos DB

In diesem Artikel erfahren Sie, wie Sie Ihre Azure Functions-Umgebung so konfigurieren, dass die Protokolle für Azure Functions-Trigger für Cosmos DB an Ihre konfigurierte [Überwachungslösung](../azure-functions/functions-monitoring.md) gesendet werden.

## <a name="included-logs"></a>Enthaltene Protokolle

Der Azure Functions-Trigger für Cosmos DB nutzt intern die [Änderungsfeed-Prozessorbibliothek](./change-feed-processor.md), und die Bibliothek generiert eine Reihe von Integritätsprotokollen, die zur Überwachung interner Vorgänge für die [Problembehandlung](./troubleshoot-changefeed-functions.md) verwendet werden können.

Die Integritätsprotokolle geben Aufschluss über das Verhalten des Azure Functions-Triggers für Cosmos DB bei Vorgängen in Lastenausgleichsszenarien oder während der Initialisierung.

## <a name="enabling-logging"></a>Aktivieren der Protokollierung

Suchen Sie bei Verwendung des Azure Functions-Triggers für Cosmos DB zum Aktivieren der Protokollierung in Ihrem Azure Functions-Projekt oder in Ihrer Azure Functions-App nach der Datei `host.json`, und [konfigurieren Sie den erforderlichen Protokolliergrad](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson). Sie müssen die Ablaufverfolgungen für `Host.Triggers.CosmosDB` aktivieren, wie im folgenden Beispiel zu sehen:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Nachdem Sie die Azure-Funktion mit der aktualisierten Konfiguration bereitgestellt haben, werden die Protokolle für Azure Functions-Trigger für Cosmos DB im Rahmen Ihrer Ablaufverfolgungen angezeigt. Die Protokolle finden Sie in Ihrem konfigurierten Protokollierungsanbieter in der *Kategorie* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Abfragen der Protokolle

Führen Sie die folgende Abfrage aus, um die vom Azure Functions-Trigger für Cosmos DB generierten Protokolle in der [Azure Application Insights-Analyse](../azure-monitor/app/analytics.md) abzufragen:

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Nächste Schritte

* [Aktivieren Sie die Überwachung](../azure-functions/functions-monitoring.md) in Ihren Azure Functions-Anwendungen.
* Informieren Sie sich über die Vorgehensweise zum [Diagnostizieren und Behandeln allgemeiner Probleme](./troubleshoot-changefeed-functions.md) bei Verwendung des Azure Functions-Triggers für Cosmos DB.