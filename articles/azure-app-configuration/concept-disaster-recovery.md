---
title: Resilienz und Notfallwiederherstellung für Azure App Configuration
description: Erfahren Sie, wie Sie Resilienz und Notfallwiederherstellung mit Azure App Configuration implementieren.
author: yegu-ms
ms.author: yegu
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: cd706e42eff19ebacf92b77d2438af80dc16a5fb
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028249"
---
# <a name="resiliency-and-disaster-recovery"></a>Resilienz und Notfallwiederherstellung

Azure App Configuration ist momentan ein regionaler Dienst. Jeder Konfigurationsspeicher wird in einer bestimmten Azure-Region erstellt. Ein regionsweiter Ausfall betrifft alle Speicher in dieser Region. App Configuration bietet kein automatisches Failover auf eine andere Region. Dieser Artikel enthält allgemeine Richtlinien zur Verwendung mehrerer Konfigurationsspeicher in mehreren Azure-Regionen, um die geografische Resilienz Ihrer Anwendung zu erhöhen.

## <a name="high-availability-architecture"></a>Hochverfügbarkeitsarchitektur

Um eine regionsübergreifende Redundanz zu erzielen, müssen Sie mehrere App Configuration-Speicher in verschiedenen Regionen erstellen. Mit diesem Setup kann die Anwendung auf mindestens einen zusätzlichen Konfigurationsspeicher zurückgreifen, wenn der primäre Speicher nicht mehr zugänglich ist. Das folgende Diagramm veranschaulicht die Topologie zwischen Ihrer Anwendung und dem primären und sekundären Konfigurationsspeicher:

![Georedundante Speicher](./media/geo-redundant-app-configuration-stores.png)

Ihre Anwendung lädt ihre Konfiguration parallel aus dem primären und sekundären Speicher. Dadurch erhöht sich die Wahrscheinlichkeit, dass die Konfigurationsdaten erfolgreich abgerufen werden können. Sie sind selbst dafür verantwortlich, die Daten in beiden Speichern zu synchronisieren. In den folgenden Abschnitten wird erläutert, wie Sie Georesilienz in Ihrer Anwendung implementieren können.

## <a name="failover-between-configuration-stores"></a>Failover zwischen Konfigurationsspeichern

In technischer Hinsicht führt Ihre Anwendung kein Failover aus. Sie versucht, den gleichen Satz von Konfigurationsdaten gleichzeitig aus zwei App Configuration-Speichern abzurufen. Gestalten Sie Ihren Code so, dass die Daten zuerst aus dem sekundären Speicher und anschließend aus dem primären Speicher geladen werden. Dadurch wird sichergestellt, dass die Konfigurationsdaten im primären Speicher Vorrang haben, wenn sie verfügbar sind. Der folgende Codeausschnitt zeigt, wie Sie dies über die .NET Core-CLI implementieren:

#### <a name="net-core-2xtabcore2x"></a>[.NET Core 2.x](#tab/core2x)

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    
```

#### <a name="net-core-3xtabcore3x"></a>[.NET Core 3.x](#tab/core3x)

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                    .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
            })
            .UseStartup<Startup>());
```
---

Beachten Sie den `optional`-Parameter, der an die `AddAzureAppConfiguration`-Funktion übergeben wird. Wenn dieser Parameter auf `true` festgelegt wird, sorgt er dafür, dass die Anwendung fortgesetzt wird, wenn die Funktion keine Konfigurationsdaten laden kann.

## <a name="synchronization-between-configuration-stores"></a>Synchronisierung zwischen Konfigurationsspeichern

Es ist wichtig, dass all Ihre georedundanten Konfigurationsspeicher denselben Satz von Daten verwenden. Mit der Funktion **Exportieren** in App Configuration können Sie Daten nach Bedarf aus dem primären Speicher in den sekundären Speicher kopieren. Diese Funktion ist sowohl über das Azure-Portal als auch über die CLI verfügbar.

Im Azure-Portal können Sie eine Änderung wie folgt an einen anderen Konfigurationsspeicher pushen:

1. Wählen Sie auf der Registerkarte **Import/Export** Folgendes aus: **Exportieren** > **App-Konfiguration** > **Ziel** > **Ressource auswählen**.

2. Geben Sie auf dem neuen Blatt, dass geöffnet wird, das Abonnement, die Ressourcengruppe und den Ressourcennamen Ihres sekundären Speichers an, und wählen Sie anschließend **Anwenden** aus.

3. Die Benutzeroberfläche wird aktualisiert, sodass Sie auswählen können, welche Konfigurationsdaten Sie in den sekundären Speicher exportieren möchten. Sie können den Standardzeitwert unverändert lassen sowie **Quellbezeichnung** und **Zielbezeichnung** auf den gleichen Wert festlegen. Wählen Sie **Übernehmen**.

4. Wiederholen Sie die vorherigen Schritte für alle Konfigurationsänderungen.

Dieser Exportprozess kann mithilfe der Azure-Befehlszeilenschnittstelle automatisiert werden. Der folgenden Befehl zeigt, wie Sie eine einzelne Konfigurationsänderung aus dem primären in den sekundären Speicher exportieren:

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Ihre Anwendung zum Erzielen von Georesilienz für App Configuration während der Laufzeit erweitern. Konfigurationsdaten aus App Configuration können auch zur Erstellungs- oder Bereitstellungszeit eingebettet werden. Weitere Informationen finden Sie unter [Integrieren in eine CI/CD-Pipeline](./integrate-ci-cd-pipeline.md).

