---
title: host.json-Referenz für Azure Functions 2.x
description: Referenzdokumentation für die host.json-Datei von Azure Functions mit der v2 Runtime.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 782998e49b9af3bf4d2ae5a561faaca399c6809f
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75978816"
---
# <a name="hostjson-reference-for-azure-functions-2x-and-later"></a>host.json-Referenz für Azure Functions 2.x oder höher 

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version der Azure Functions-Runtime aus: "]
> * [Version 1](functions-host-json-v1.md)
> * [Version 2](functions-host-json.md)

Die Metadatendatei *host.json* enthält globale Konfigurationsoptionen, die sich auf alle Funktionen einer Funktionen-App auswirken. In diesem Artikel werden die Einstellungen aufgelistet, die ab Version 2.x der Azure Functions-Laufzeit verfügbar sind.  

> [!NOTE]
> Dieser Artikel gilt für Azure Functions 2.x oder höher.  Eine Referenz für „host.json“ in Functions 1.x finden Sie unter [host.json-Referenz für Azure Functions 1.x](functions-host-json-v1.md).

Weitere Konfigurationsoptionen Ihrer Funktions-App werden in den [Anwendungseinstellungen](functions-app-settings.md) verwaltet.

Einige Einstellungen in host.json werden nur bei lokaler Ausführung in der [local.settings.json](functions-run-local.md#local-settings-file)-Datei verwendet.

## <a name="sample-hostjson-file"></a>host.json-Beispieldatei

In der folgenden Beispieldatei *host.json* sind alle möglichen Optionen angegeben (mit Ausnahme der Optionen, die nur für die interne Verwendung vorgesehen sind).

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
    },
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 20,
              "evaluationInterval": "01:00:00",
              "initialSamplingPercentage": 1.0, 
              "samplingPercentageIncreaseTimeout" : "00:00:01",
              "samplingPercentageDecreaseTimeout" : "00:00:01",
              "minSamplingPercentage": 0.1,
              "maxSamplingPercentage": 0.1,
              "movingAverageRatio": 1.0
            },
            "samplingExcludedTypes" : "Dependency;Event",
            "samplingIncludedTypes" : "PageView;Trace",
            "enableLiveMetrics": true,
            "enableDependencyTracking": true,
            "enablePerformanceCountersCollection": true,            
            "httpAutoCollectionOptions": {
                "enableHttpTriggerExtendedInfoCollection": true,
                "enableW3CDistributedTracing": true,
                "enableResponseHeaderInjection": true
            },
            "snapshotConfiguration": {
                "agentEndpoint": null,
                "captureSnapshotMemoryWeight": 0.5,
                "failedRequestLimit": 3,
                "handleUntrackedExceptions": true,
                "isEnabled": true,
                "isEnabledInDeveloperMode": false,
                "isEnabledWhenProfiling": true,
                "isExceptionSnappointsEnabled": false,
                "isLowPrioritySnapshotUploader": true,
                "maximumCollectionPlanSize": 50,
                "maximumSnapshotsRequired": 3,
                "problemCounterResetInterval": "24:00:00",
                "provideAnonymousTelemetry": true,
                "reconnectInterval": "00:15:00",
                "shadowCopyFolder": null,
                "shareUploaderProcess": true,
                "snapshotInLowPriorityThread": true,
                "snapshotsPerDayLimit": 30,
                "snapshotsPerTenMinutesLimit": 1,
                "tempFolder": null,
                "thresholdForSnapshotting": 1,
                "uploaderProxy": null
            }
        }
    },
    "managedDependency": {
        "enabled": true
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

In den folgenden Abschnitten dieses Artikels werden die einzelnen allgemeinen Eigenschaften erläutert. Alle Eigenschaften sind optional, sofern nicht anders angegeben.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

Diese Einstellung ist ein untergeordnetes Element von [logging](#logging).

Sie steuert Optionen für Application Insights, einschließlich [Stichprobenoptionen](./functions-monitoring.md#configure-sampling).

Die vollständige JSON-Struktur finden Sie in der obigen [Beispieldatei „host.json“](#sample-hostjson-file).

> [!NOTE]
> Protokollsampling kann dazu führen, dass einige Ausführungen möglicherweise nicht auf dem Application Insights-Blatt für Überwachen angezeigt werden. Fügen Sie `samplingExcludedTypes: "Request"` zum Wert `applicationInsights` hinzu, um die Protokollstichprobenentnahme zu vermeiden.

| Eigenschaft | Standard | Beschreibung |
| --------- | --------- | --------- | 
| samplingSettings | – | Siehe [applicationInsights.samplingSettings](#applicationinsightssamplingsettings). |
| samplingExcludedTypes | NULL | Eine durch Strichpunkte getrennte Liste von Typen, für die keine Stichproben erstellt werden sollen. Anerkannte Typen sind: Dependency, Event, Exception, PageView, Request, Trace. Alle Instanzen der angegebenen Typen werden übertragen. Für nicht angegebene Typen werden Stichproben erstellt. |
| samplingIncludedTypes | NULL | Eine durch Semikolons getrennte Liste von Typen, für die Stichproben erstellt werden sollen. Eine leere Liste gibt an, dass Stichproben für alle Typen erstellt werden sollen. In `samplingExcludedTypes` aufgeführte Typen überschreiben die hier aufgeführten Typen. Anerkannte Typen sind: Dependency, Event, Exception, PageView, Request, Trace. Alle Instanzen der angegebenen Typen werden übertragen. Für nicht angegebene Typen werden Stichproben erstellt. |
| enableLiveMetrics | true | Aktiviert die Sammlung mit Livemetriken. |
| enableDependencyTracking | true | Aktiviert die Abhängigkeitsüberwachung. |
| enablePerformanceCountersCollection | true | Aktiviert die Sammlung mit Kudu-Leistungsindikatoren. |
| liveMetricsInitializationDelay | 00:00:15 | Nur zur internen Verwendung. |
| httpAutoCollectionOptions | – | Siehe [applicationInsights.httpAutoCollectionOptions](#applicationinsightshttpautocollectionoptions). |
| snapshotConfiguration | – | Siehe [applicationInsights.snapshotConfiguration](#applicationinsightssnapshotconfiguration). |

### <a name="applicationinsightssamplingsettings"></a>applicationInsights.samplingSettings

|Eigenschaft | Standard | Beschreibung |
| --------- | --------- | --------- | 
| isEnabled | true | Aktiviert oder deaktiviert die Stichprobenentnahme. | 
| maxTelemetryItemsPerSecond | 20 | Die Zielanzahl der Telemetrieelemente, die pro Sekunde auf den einzelnen Serverhosts protokolliert werden. Reduzieren Sie diesen Wert, wenn Ihre App auf vielen Hosts ausgeführt wird, um Ihre Zielgeschwindigkeit für den gesamten Datenverkehr einzuhalten. | 
| evaluationInterval | 01:00:00 | Das Intervall, in dem die aktuelle Telemetrierate erneut ausgewertet wird. Die Auswertung wird als gleitender Durchschnitt ausgeführt. Wenn bei Ihrer Telemetrie plötzliche Spitzen auftreten, sollten Sie dieses Intervall verkürzen. |
| initialSamplingPercentage| 1.0 | Der anfängliche Prozentsatz für die Stichprobenentnahme, der zu Beginn der Stichprobenentnahme angewendet wird, um den Prozentsatz dynamisch zu variieren. Reduzieren Sie den Wert nicht, solange Sie debuggen. |
| samplingPercentageIncreaseTimeout | 00:00:01 | Wenn der Prozentwert für die Stichprobenentnahme geändert wird, bestimmt diese Eigenschaft, wie schnell Application Insights den Prozentsatz für die Stichprobenentnahme danach erhöhen darf, um mehr Daten zu erfassen. |
| samplingPercentageDecreaseTimeout | 00:00:01 | Wenn der Prozentwert für die Stichprobenentnahme geändert wird, bestimmt diese Eigenschaft, wie schnell Application Insights den Prozentsatz für die Stichprobenentnahme danach reduzieren darf, um weniger Daten zu erfassen. |
| minSamplingPercentage | 0,1 | Diese Eigenschaft bestimmt den mindestens zulässigen Wert für den variierenden Prozentsatz der Stichprobenentnahme. |
| maxSamplingPercentage | 0,1 | Diese Eigenschaft bestimmt den maximal zulässigen Wert für den variierenden Prozentsatz der Stichprobenentnahme. |
| movingAverageRatio | 1.0 | Die Gewichtung, die bei der Berechnung des gleitenden Durchschnitts dem jüngsten Wert beigemessen wird. Verwenden Sie einen Wert kleiner oder gleich 1. Bei einem kleineren Wert reagiert der Algorithmus langsamer auf plötzliche Veränderungen. |

### <a name="applicationinsightshttpautocollectionoptions"></a>applicationInsights.httpAutoCollectionOptions

|Eigenschaft | Standard | Beschreibung |
| --------- | --------- | --------- | 
| enableHttpTriggerExtendedInfoCollection | true | Diese Eigenschaft aktiviert oder deaktiviert erweiterte HTTP-Anforderungsinformationen für HTTP-Trigger: eingehende Anforderungskorrelationsheader, Unterstützung für mehrere Instrumentierungsschlüssel, HTTP-Methoden, Pfade und Antworten. |
| enableW3CDistributedTracing | true | Diese Eigenschaft aktiviert oder deaktiviert die Unterstützung des W3C-Protokolls für die verteilte Ablaufverfolgung (und aktiviert das alte Korrelationsschema). Sie ist standardmäßig aktiviert, wenn `enableHttpTriggerExtendedInfoCollection` den Wert TRUE aufweist. Wenn `enableHttpTriggerExtendedInfoCollection` den Wert FALSE aufweist, gilt dieses Flag nur für ausgehende Anforderungen, nicht für eingehende Anforderungen. |
| enableResponseHeaderInjection | true | Diese Eigenschaft aktiviert oder deaktiviert die Injektion von Korrelationsheadern mit mehreren Komponenten in Antworten. Wenn die Injektion aktiviert wird, kann Application Insights eine Anwendungszuordnung erstellen, wenn mehrere Instrumentierungsschlüssel verwendet werden. Sie ist standardmäßig aktiviert, wenn `enableHttpTriggerExtendedInfoCollection` den Wert TRUE aufweist. Diese Einstellung ist nicht gültig, wenn `enableHttpTriggerExtendedInfoCollection` den Wert FALSE aufweist. |

### <a name="applicationinsightssnapshotconfiguration"></a>applicationInsights.snapshotConfiguration

Weitere Informationen zu Momentaufnahmen finden Sie unter [Debugmomentaufnahmen von Ausnahmen in .NET-Apps](/azure/azure-monitor/app/snapshot-debugger) und [Behandeln von Problemen beim Aktivieren des Application Insights-Momentaufnahmedebuggers oder Anzeigen von Momentaufnahmen](/azure/azure-monitor/app/snapshot-debugger-troubleshoot).

|Eigenschaft | Standard | Beschreibung |
| --------- | --------- | --------- | 
| agentEndpoint | NULL | Der Endpunkt wird zum Herstellen einer Verbindung mit dem Application Insights-Momentaufnahmedebuggerdienst verwendet. Wenn der Wert NULL vorliegt, wird ein Standardendpunkt verwendet. |
| captureSnapshotMemoryWeight | 0.5 | Die Gewichtung der Größe des aktuellen Prozessarbeitsspeichers für die Überprüfung, ob ausreichend Arbeitsspeicher zum Erstellen einer Momentaufnahme verfügbar ist. Es wird ein Wert erwartet, der größer als 0 (null) und ein echter Bruch ist (0 < CaptureSnapshotMemoryWeight < 1). |
| failedRequestLimit | 3 | Der Schwellenwert für die Anzahl fehlgeschlagener Anforderungen von Momentaufnahmen, ab der der Telemetrieprozessor deaktiviert wird.|
| handleUntrackedExceptions | true | Diese Eigenschaft aktiviert oder deaktiviert die Nachverfolgung von Ausnahmen, die nicht von der Application Insights-Telemetrie nachverfolgt werden. |
| isEnabled | true | Diese Eigenschaft aktiviert oder deaktiviert die Erfassung von Momentaufnahmen. | 
| isEnabledInDeveloperMode | false | Diese Eigenschaft aktiviert oder deaktiviert die Erfassung von Momentaufnahmen im Entwicklermodus. |
| isEnabledWhenProfiling | true | Diese Eigenschaft aktiviert oder deaktiviert die Erstellung von Momentaufnahmen, selbst wenn eine ausführliche Profilerstellungssitzung von Application Insights Profiler erfasst wird. |
| isExceptionSnappointsEnabled | false | Diese Eigenschaft aktiviert oder deaktiviert die Filterung von Ausnahmen. |
| isLowPrioritySnapshotUploader | true | Diese Eigenschaft bestimmt, ob der SnapshotUploader-Prozess mit einer niedrigeren Priorität als normal ausgeführt werden soll. |
| maximumCollectionPlanSize | 50 | Diese Eigenschaft gibt die maximale Anzahl von Problemen von 1 bis 9.999 an, die gleichzeitig nachverfolgt werden können. |
| maximumSnapshotsRequired | 3 | Diese Eigenschaft gibt die maximale Anzahl erfasster Momentaufnahmen für ein einzelnes Problem von 1 bis 999 an. Sie können ein Problem als individuelle throw-Anweisung in Ihrer Anwendung betrachten. Sobald die Anzahl der erfassten Momentaufnahmen für ein Problem diesen Wert erreicht, werden keine weiteren Momentaufnahmen für das Problem erfasst, bis der Problemzähler zurückgesetzt (siehe `problemCounterResetInterval`) und der `thresholdForSnapshotting`-Schwellenwert erneut erreicht wird. |
| problemCounterResetInterval | 24:00:00 | Diese Eigenschaft gibt an, wie oft die Problemzähler in einem Zeitraum von 1 Minute bis 7 Tage zurückgesetzt werden. Sobald dieses Intervall erreicht wird, werden alle Problemzähler auf 0 (null) gesetzt. Vorhandene Probleme, die den Schwellenwert für die Momentaufnahmenerstellung bereits erreicht haben, aber noch nicht die Anzahl der Momentaufnahmen in `maximumSnapshotsRequired` generiert haben, sind weiterhin aktiv. |
| provideAnonymousTelemetry | true | Diese Eigenschaft bestimmt, ob anonyme Nutzungs- und Fehlertelemetriedaten an Microsoft gesendet werden. Diese Telemetrie kann verwendet werden, wenn Sie Microsoft für die Behandlung von Problemen mit dem Momentaufnahmedebugger kontaktieren. Diese Daten werden auch für die Überwachung von Nutzungsmustern verwendet. |
| reconnectInterval | 00:15:00 | Diese Eigenschaft gibt an, wie oft die Verbindung zum Endpunkt des Momentaufnahmedebuggers wiederhergestellt wird. Das zulässige Intervall liegt zwischen 1 Minute und 1 Tag. |
| shadowCopyFolder | NULL | Diese Eigenschaft gibt den Ordner für Schattenkopien von Binärdateien an. Wenn sie nicht festgelegt wird, werden die folgenden Umgebungsvariablen der Reihe nach versucht: Fabric_Folder_App_Temp, LOCALAPPDATA, APPDATA, TEMP. |
| shareUploaderProcess | true | Wenn diese Eigenschaft den Wert TRUE aufweist, werden Momentaufnahmen für mehrere Apps mit dem gleichen InstrumentationKey-Wert von einer SnapshotUploader-Instanz erfasst und hochgeladen. Wenn der Wert FALSE vorliegt, wird eine eindeutige SnapshotUploader-Instanz für jedes Tupel (ProcessName, InstrumentationKey) verwendet. |
| snapshotInLowPriorityThread | true | Diese Eigenschaft bestimmt, ob Momentaufnahmen in einem Thread mit niedriger E/A-Priorität verarbeitet werden. Das Erstellen einer Momentaufnahme ist ein schneller Vorgang, aber um eine Momentaufnahme in den Momentaufnahmedebuggerdienst hochzuladen, muss sie erst als Minidump auf den Datenträger geschrieben werden. Dies geschieht im SnapshotUploader-Prozess. Wenn TRUE für diesen Wert festgelegt wird, wird eine niedrige E/A-Priorität zum Schreiben des Minidumps verwendet, sodass der Prozess nicht mit Ihrer Anwendung um Ressourcen konkurriert. Wenn FALSE für diesen Wert festgelegt wird, erfolgt die Erstellung des Minidumps schneller, jedoch wird Ihre Anwendung verlangsamt. |
| snapshotsPerDayLimit | 30 | Diese Eigenschaft legt die maximal zulässige Anzahl von Momentaufnahmen pro Tag (24 Stunden) fest. Dieser Schwellenwert wird auch für den Application Insights-Dienst festgelegt. Die Uploads sind pro Anwendung auf 50 pro Tag beschränkt (d. h. pro Instrumentierungsschlüssel). Mit diesem Wert wird verhindert, dass zusätzliche Momentaufnahmen erstellt werden, die letztendlich während des Uploads abgelehnt werden. Wenn der Wert „0“ (null) festgelegt wird, wird der Schwellenwert vollständig aufgehoben, was nicht empfohlen wird. |
| snapshotsPerTenMinutesLimit | 1 | Diese Eigenschaft legt die maximal zulässige Anzahl von Momentaufnahmen für einen Zeitraum von 10 Minuten fest. Obwohl es für diesen Wert keine Obergrenze gibt, sollten Sie ihn für Produktionsworkloads nur mit Bedacht erhöhen, da er sich auf die Leistung Ihrer Anwendung auswirken kann. Zwar ist die Erstellung einer Momentaufnahme ein schneller Prozess, jedoch ist das Erstellen eines Minidumps des Snapshots und Hochladen in den Momentaufnahmedebuggerdienst ein deutlich langsamerer Vorgang, der mit Ihrer Anwendung um Ressourcen konkurriert (sowohl CPU- als auch E/A-Ressourcen). |
| tempFolder | NULL | Diese Eigenschaft legt den Ordner fest, in den Minidumps und Uploaderprotokolldateien geschrieben werden. Wenn kein Wert festgelegt wird, wird *%TEMP%\Dumps* verwendet. |
| thresholdForSnapshotting | 1 | Diese Eigenschaft gibt an, wie viele Ausnahmen Application Insights ermitteln muss, bevor Momentaufnahmen angefordert werden. |
| uploaderProxy | NULL | Diese Eigenschaft überschreibt den Proxyserver, der vom SnapshotUploader-Prozess verwendet wird. Sie müssen diese Einstellung möglicherweise verwenden, wenn Ihre Anwendung eine Internetverbindung über einen Proxyserver herstellt. Snapshot Collector wird im Prozess Ihrer Anwendung ausgeführt und verwendet die gleichen Proxyeinstellungen. Der SnapshotUploader-Prozess wird jedoch separat ausgeführt, und Sie müssen den Proxyserver möglicherweise manuell konfigurieren. Wenn diese Eigenschaft den Wert NULL aufweist, versucht Snapshot Collector die Proxyadresse automatisch zu ermitteln, indem System.Net.WebRequest.DefaultWebProxy untersucht und der Wert an den SnapshotUploader-Prozess übergeben wird. Wenn nicht der Wert NULL vorliegt, wird die automatische Ermittlung nicht verwendet und der hier angegebene Proxyserver wird im SnapshotUploader-Prozess verwendet. |

## <a name="cosmosdb"></a>cosmosDb

Die Konfigurationseinstellung finden Sie in [Cosmos DB-Trigger und -Bindungen](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

Die Konfigurationseinstellung finden Sie in [Bindungen für Durable Functions](durable/durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>eventHub

Die Konfigurationseinstellung finden Sie in [Event Hub-Trigger und -Bindungen](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>Erweiterungen

Eigenschaft, die ein Objekt zurückgibt, das alle die bindungsspezifischen-Einstellungen enthält, z.B. [http](#http) und [eventhub](#eventhub).

## <a name="extensionbundle"></a>extensionBundle 

Mit Erweiterungsbündeln können Sie kompatible Bindungserweiterungen von Azure Functions zu Ihrer Funktions-App hinzufügen. Weitere Informationen finden Sie unter [Erweiterungsbündel für die lokale Entwicklung](functions-bindings-register.md#extension-bundles).

[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="functions"></a>functions

Eine Liste der Funktionen, die vom Auftragshost ausgeführt werden. Ein leeres Array bedeutet, dass alle Funktionen ausgeführt werden. Nur bei [lokaler Ausführung](functions-run-local.md) für die Verwendung vorgesehen. In Funktions-Apps in Azure sollten Sie anstelle dieser Einstellung die in [How to disable functions in Azure Functions](disable-function.md) (Deaktivieren von Funktionen in Azure Functions) aufgeführten Schritte ausführen, um bestimmte Funktionen zu deaktivieren.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Gibt die Timeoutdauer für alle Funktionen an. Die Angabe erfolgt im TimeSpan-Zeichenfolgenformat. Bei einem serverlosen Verbrauchsplan liegt der gültige Bereich zwischen 1 Sekunde und 10 Minuten, wobei der Standardwert bei 5 Minuten liegt.  

Im Premium-Tarif liegt der gültige Bereich zwischen 1 Sekunde und 60 Minuten, wobei der Standardwert bei 30 Minuten liegt.

Bei einem dedizierten App Service-Plan gibt es keine allgemeine Beschränkung, und der Standardwert beträgt 30 Minuten. Der Wert `-1` gibt eine unbegrenzte Ausführung an, es wird jedoch empfohlen, eine festgelegte Obergrenze beizubehalten.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Konfigurationseinstellungen für [Host Health Monitor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor) (Überwachung der Hostintegrität).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------| 
|enabled|true|Gibt an, ob die Funktion aktiviert ist. | 
|healthCheckInterval|10 Sekunden|Das Zeitintervall zwischen den regelmäßigen Integritätsüberprüfungen im Hintergrund. | 
|healthCheckWindow|2 Minuten|Ein variables Zeitfenster, das in Zusammenhang mit der `healthCheckThreshold`-Einstellung verwendet wird.| 
|healthCheckThreshold|6|Maximale Anzahl von fehlerhaften Integritätsüberprüfungen, bevor ein Neustart des Hosts initiiert wird.| 
|counterThreshold|0.80|Der Schwellenwert, an dem ein Leistungsindikator als fehlerhaft betrachtet wird.| 

## <a name="http"></a>http

Die Konfigurationseinstellung finden Sie in [HTTP-Trigger und -Bindungen](functions-bindings-http-webhook.md#hostjson-settings).

## <a name="logging"></a>logging

Steuert das Protokollierungsverhalten der Funktions-App, einschließlich Application Insights.

```json
"logging": {
    "fileLoggingMode": "debugOnly"
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "console": {
        ...
    },
    "applicationInsights": {
        ...
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Definiert, welche Stufe der Dateiprotokollierung aktiviert ist.  Optionen sind `never`, `always`, `debugOnly`. |
|logLevel|–|Objekt, das das Filtern der Protokollkategorie nach Funktionen in der App definiert. Version 2.x oder höher entspricht bei der Filterung der Protokollkategorie dem Layout von ASP.NET Core. Mit dieser Einstellung können Sie die Protokollierung nach bestimmten Funktionen filtern. Weitere Informationen finden Sie unter [Protokollfilterung](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) in der Dokumentation zu ASP.NET Core. |
|console|–| Die [Konsolen](#console)protokollierungseinstellung. |
|applicationInsights|–| Die [applicationInsights](#applicationinsights)-Einstellung. |

## <a name="console"></a>console

Diese Einstellung ist ein untergeordnetes Element von [logging](#logging). Sie steuert die Konsolenprotokollierung, wenn nicht im Debugmodus.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------| 
|isEnabled|false|Aktiviert oder deaktiviert die Konsolenprotokollierung.| 

## <a name="manageddependency"></a>managedDependency

Verwaltete Abhängigkeit ist ein Vorschaufeature, das derzeit nur mit PowerShell-basierten Funktionen unterstützt wird. Sie ermöglicht, dass Abhängigkeiten automatisch vom Dienst verwaltet werden können. Wenn die `enabled`-Eigenschaft auf `true` festgelegt ist, wird die Datei `requirements.psd1` verarbeitet. Abhängigkeiten werden aktualisiert, wenn Nebenversionen veröffentlicht werden. Weitere Informationen finden Sie unter [Verwaltete Abhängigkeit](functions-reference-powershell.md#dependency-management) im PowerShell-Artikel.

```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

## <a name="queues"></a>queues

Die Konfigurationseinstellungen finden Sie in [Trigger und Bindungen der Speicherwarteschlange](functions-bindings-storage-queue.md#host-json).  

## <a name="sendgrid"></a>sendGrid

Die Konfigurationseinstellung finden Sie in [SendGrid-Trigger und -Bindungen](functions-bindings-sendgrid.md#host-json).

## <a name="servicebus"></a>serviceBus

Die Konfigurationseinstellung finden Sie in [Service Bus-Trigger und -Bindungen](functions-bindings-service-bus.md#host-json).

## <a name="singleton"></a>singleton

Konfigurationseinstellungen für das Singleton-Sperrverhalten. Weitere Informationen finden Sie unter [GitHub-Problem zur Singleton-Unterstützung](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Eigenschaft  |Standard | Beschreibung |
|---------|---------|---------| 
|lockPeriod|00:00:15|Der Zeitraum, für den Sperren auf Funktionsebene gelten. Die Sperren werden automatisch verlängert.| 
|listenerLockPeriod|00:01:00|Der Zeitraum, für den Listenersperren gelten.| 
|listenerLockRecoveryPollingInterval|00:01:00|Das Zeitintervall für die Wiederherstellung der Listenersperre, wenn eine Listenersperre nicht beim Start abgerufen werden konnte.| 
|lockAcquisitionTimeout|00:01:00|Die maximale Zeitspanne, in der die Laufzeit versucht, eine Sperre abzurufen.| 
|lockAcquisitionPollingInterval|–|Das Intervall zwischen den Versuchen, eine Sperre abzurufen.| 

## <a name="version"></a>version

Die Versionszeichenfolge `"version": "2.0"` ist für Funktions-App mit der v2-Runtime als Ziel erforderlich.

## <a name="watchdirectories"></a>watchDirectories

Eine Reihe von [Verzeichnissen mit freigegebenem Code](functions-reference-csharp.md#watched-directories), die auf Änderungen überwacht werden sollte.  Dadurch wird sichergestellt, dass Änderungen am Code in diesen Verzeichnissen von Ihren Funktionen übernommen werden.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Aktualisieren der host.json-Datei](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Weitere Informationen finden Sie unter den globalen Einstellungen in Umgebungsvariablen](functions-app-settings.md)
