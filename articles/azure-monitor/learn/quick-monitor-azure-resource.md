---
title: Überwachen einer Azure-Ressource mit Azure Monitor
description: Hier erfahren Sie, wie Sie Daten für eine Azure-Ressource in Azure Monitor erfassen und analysieren.
ms.service: azure-monitor
ms. subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: 3081c0f8fa8450643a992c8e82840a44db263f76
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75863977"
---
# <a name="quickstart-monitor-an-azure-resource-with-azure-monitor"></a>Schnellstart: Überwachen einer Azure-Ressource mit Azure Monitor
[Azure Monitor](../overview.md) beginnt in dem Moment, in dem Azure-Ressourcen erstellt werden, mit dem Sammeln von Daten aus diesen Ressourcen. Diese Schnellstartanleitung bietet eine kurze Übersicht darüber, welche Daten für eine Ressource automatisch gesammelt werden, und erläutert, wie Sie die Daten für eine bestimmte Ressource im Azure-Portal anzeigen können. Später können Sie Einstellungen für die Sammlung weiterer Daten konfigurieren und zum Azure Monitor-Menü wechseln, um mit denselben Tools auf Daten zuzugreifen, die für alle Ressourcen in Ihrem Abonnement gesammelt wurden.

Eine genaue Beschreibung der Überwachungsdaten, die aus Azure-Ressourcen gesammelt werden, finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../insights/monitor-azure-resource.md).


## <a name="sign-in-to-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an. 


## <a name="overview-page"></a>Seite „Übersicht“
Viele Dienste stellen auf der Seite **Übersicht** Überwachungsdaten bereit, die einen kurzen Einblick in ihren Betrieb erlauben. Die Informationen basieren in der Regel auf einer Teilmenge der Plattformmetriken, die in Azure Monitor-Metriken gespeichert werden.

1. Suchen Sie in Ihrem Abonnement nach einer Azure-Ressource.
2. Wechseln Sie zur Seite **Übersicht**, und prüfen Sie, ob dort Leistungsdaten angezeigt werden. Diese Daten werden von Azure Monitor bereitgestellt. Das folgende Beispiel ist die Seite **Übersicht** für ein Azure-Speicherkonto, und wie Sie sehen, werden mehrere Metriken angezeigt.

    ![Seite „Übersicht“](media/quick-monitor-azure-resource/overview.png)

3. Sie können auf ein beliebiges Diagramm klicken, um die Daten im Metrik-Explorer zu öffnen. Dieser wird weiter unten beschrieben.

## <a name="view-the-activity-log"></a>Anzeigen des Aktivitätsprotokolls
Das Aktivitätsprotokoll bietet Einblicke in die Vorgänge, die für die einzelnen Azure-Ressourcen im Abonnement durchgeführt wurden. Hierzu gehören Informationen beispielsweise dazu, wann eine Ressource erstellt oder geändert wird, wann ein Auftrag gestartet wird oder wann ein bestimmter Vorgang ausgeführt wird.

1. Wählen Sie oben im Menü für Ihre Ressource die Option **Aktivitätsprotokoll** aus.
2. Der aktuelle Filter ist so eingestellt, dass Ereignisse in Bezug auf Ihre Ressource angezeigt werden. Wenn keine Ereignisse angezeigt werden, ändern Sie den **Zeitraum**, um den Zeitbereich zu vergrößern.

    ![Aktivitätsprotokoll](media/quick-monitor-azure-resource/activity-log-resource.png)

4. Wenn Sie Ereignisse aus anderen Ressourcen in Ihrem Abonnement anzeigen möchten, ändern Sie Kriterien im Filter, oder entfernen Sie Filtereigenschaften.

    ![Aktivitätsprotokoll](media/quick-monitor-azure-resource/activity-log-all.png)



## <a name="view-metrics"></a>Anzeigen von Metriken
Metriken sind numerische Werte, die einen Aspekt Ihrer Ressource zu einem bestimmten Zeitpunkt beschreiben. Azure Monitor sammelt Plattformmetriken automatisch aus allen Azure-Ressourcen in Intervallen von einer Minute. Sie können diese Metriken im Metrik-Explorer anzeigen.

1. Wählen Sie im Menü Ihrer Ressource im Abschnitt **Überwachung** die Option **Metriken** aus. Damit wird der Metrik-Explorer geöffnet, und der Bereich ist auf Ihre Ressource festgelegt.
2. Klicken Sie auf **Metrik hinzufügen**, um dem Diagramm eine Metrik hinzuzufügen.
   
   ![Metrik-Explorer](media/quick-monitor-azure-resource/metrics-explorer-01.png)
   
4. Wählen Sie aus der Liste eine **Metrik** und dann eine **Aggregation** aus. Damit wird definiert, wie die Werte für jedes Zeitintervall erfasst werden.

    ![Metrik-Explorer](media/quick-monitor-azure-resource/metrics-explorer-02.png)

5. Klicken Sie auf **Metrik hinzufügen**, um weitere Kombinationen aus Metrikwerten und Aggregationen zum Diagramm hinzuzufügen.

    ![Metrik-Explorer](media/quick-monitor-azure-resource/metrics-explorer-03.png)



## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie das Aktivitätsprotokoll und die Metriken für eine Azure-Ressource angezeigt, die von Azure Monitor automatisch erfasst werden. Ressourcenprotokolle bieten Einblicke in die genauen Vorgänge einer Ressource, müssen aber konfiguriert werden, damit sie gesammelt werden. Fahren Sie mit dem Tutorial zum Sammeln von Ressourcenprotokollen in einem Log Analytics-Arbeitsbereich fort, in dem sie mithilfe von Protokollabfragen analysiert werden können.

> [!div class="nextstepaction"]
> [Sammeln und Analysieren von Ressourcenprotokollen mit Azure Monitor](tutorial-resource-logs.md)
