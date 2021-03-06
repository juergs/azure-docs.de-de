---
title: 'Tutorial: Überwachen von Azure Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Spring Cloud-Warnungen verwenden.
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 12/29/2019
ms.openlocfilehash: 49fea7d568e356169f8bbf0dfd1f4ce5c80a7223
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75690225"
---
# <a name="tutorial-monitor-spring-cloud-resources-using-alerts-and-action-groups"></a>Tutorial: Überwachen von Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen

Mit Azure Spring Cloud-Warnungen können Sie Ressourcen auf der Grundlage von Bedingungen (etwa dem verfügbaren Speicher, der Anforderungsrate oder der Datennutzung) überwachen Eine Warnung sendet eine Benachrichtigung, wenn Raten oder Bedingungen die definierten Spezifikationen erfüllen.

Zum Einrichten einer Warnungspipeline müssen zwei Schritte ausgeführt werden: 
1. Richten Sie eine Aktionsgruppe mit den Aktionen (E-Mail, SMS, Runbook oder Webhook) ein, die beim Auslösen einer Warnung ausgeführt werden sollen. Aktionsgruppen können für unterschiedliche Warnungen erneut verwendet werden.
2. Richten Sie Warnungsregeln ein. Die Regeln binden Metrikmuster basierend auf Zielressource, Metrik, Bedingung, Zeitaggregation usw. an die Aktionsgruppen.

## <a name="prerequisites"></a>Voraussetzungen
Zusätzlich zu den erforderlichen Azure Spring-Komponenten werden für dieses Tutorial die folgenden Ressourcen benötigt:

* Eine bereitgestellte Azure Spring Cloud-Instanz.  Befolgen Sie unseren [Schnellstart](spring-cloud-quickstart-launch-app-cli.md), um zu beginnen.

* Eine zu überwachende Azure-Ressource. In diesem Beispiel wird eine Spring Cloud-Instanz überwacht.
 
Die folgenden Prozeduren initialisieren sowohl **Aktionsgruppe** als auch **Warnung** ausgehend von der Option **Warnungen** im linken Navigationsbereich einer Spring Cloud-Instanz. (Die Prozedur kann auch über die Seite **Monitor – Übersicht** im Azure-Portal gestartet werden.) 

Navigieren Sie von einer Ressourcengruppe zu Ihrer Spring Cloud-Instanz. Wählen Sie im linken Bereich **Warnungen** und dann **Aktionen verwalten** aus.

![Screenshot der Seite „Ressourcengruppe“ im Portal](media/alerts-action-groups/action-1-a.png)

## <a name="set-up-action-group"></a>Einrichten einer Aktionsgruppe

Wählen Sie **+ Aktionsgruppe hinzufügen** aus, um die Prozedur zum Initialisieren einer neuen **Aktionsgruppe** zu starten.

![Screenshot der Option „Aktionsgruppe hinzufügen“ im Portal](media/alerts-action-groups/action-1.png)

Gehen Sie auf der Seite **Aktionsgruppe hinzufügen** wie folgt vor:

 1. Geben Sie unter **Name der Aktionsgruppe** und **Kurzname** jeweils einen Namen ein.

 1. Geben Sie **Abonnement** und **Ressourcengruppe** an.

 1. Geben Sie unter **Aktionsname** einen Namen ein.

 1. Wählen Sie unter **Aktionstyp** eine Option aus.  Dadurch wird ein weiterer Bereich auf der rechten Seite geöffnet, in dem Sie die Aktion definieren können, die bei Aktivierung ausgeführt wird.

 1. Definieren Sie die Aktion mithilfe der Optionen im rechten Bereich.  In diesem Fall wird eine E-Mail-Benachrichtigung verwendet.

 1. Klicken Sie im rechten Aktionsbereich auf **OK**.

 1. Klicken Sie im Dialogfeld **Aktionsgruppe hinzufügen** auf **OK**. 

  ![Screenshot: Definieren einer Aktion im Portal](media/alerts-action-groups/action-2.png)

## <a name="set-up-alert"></a>Einrichten einer Warnung 

In den vorherigen Schritten wurde eine **Aktionsgruppe** erstellt, die eine Benachrichtigung per E-Mail verwendet. Sie können auch Telefonbenachrichtigungen, Webhooks, Azure Functions usw. verwenden.  

Navigieren Sie zum Konfigurieren einer **Warnung** zurück zur Seite **Warnungen**, und klicken Sie auf **Warnungsregeln verwalten**.

  ![Screenshot: Definieren einer Warnung im Portal](media/alerts-action-groups/alerts-2.png)

1. Wählen Sie die **Ressource** für die Warnung aus.

1. Klicken Sie auf **+ Neue Warnungsregel**.

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-3.png)

1. Geben Sie auf der Seite **Regel erstellen** einen Wert für **RESSOURCE** an.

1. Die Einstellung **BEDINGUNG** bietet zahlreiche Optionen für die Überwachung Ihrer **Spring Cloud**-Ressourcen.  Klicken Sie auf **Hinzufügen**, um den Bereich **Signallogik konfigurieren** zu öffnen.

1. Wählen Sie eine Bedingung aus. In diesem Beispiel wird **System CPU Usage Percentage** (System-CPU-Auslastung in Prozent) verwendet.

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-3-1.png)

1. Scrollen Sie im Bereich **Signallogik konfigurieren** nach unten, um den zu überwachenden **Schwellenwert** festzulegen.

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-3-2.png)

1. Klicken Sie auf **Fertig**.

Ausführliche Informationen zu den für die Überwachung verfügbaren Bedingungen finden Sie unter [Metriken für Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-concept-metrics#user-portal-metrics-options).

 Klicken Sie unter **AKTIONEN** auf **Aktionsgruppe auswählen**. Wählen Sie im Bereich **AKTIONEN** die zuvor definierte **Aktionsgruppe** aus.

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-3-3.png) 

1. Scrollen Sie nach unten, und geben Sie unter **WARNUNGSDETAILS** einen Namen für die Warnungsregel ein.

1. Legen Sie den **Schweregrad** fest.

1. Klicken Sie auf **Warnungsregel erstellen**.

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-3-4.png)

Vergewissern Sie sich, dass die neue Warnungsregel aktiviert ist.

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-4.png)

Eine Regel kann auch über die Seite **Metriken** erstellt werden:

   ![Screenshot: „Neue Warnungsregel“ im Portal](media/alerts-action-groups/alerts-5.png)

## <a name="next-steps"></a>Nächste Schritte
* [Metriken für Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-concept-metrics#user-portal-metrics-options)
* [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* [SMS-Warnungsverhalten in Aktionsgruppen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-sms-behavior)
* [Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
