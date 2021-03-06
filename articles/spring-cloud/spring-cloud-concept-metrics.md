---
title: Grundlegendes zu Metriken für Azure Spring Cloud
description: Hier erfahren Sie, wie Sie Metriken in Azure Spring Cloud überprüfen.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: brendm
ms.openlocfilehash: e6517f1a7374b3960c3b749e63a90fe9eb21e7b0
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277942"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Grundlegendes zu Metriken für Azure Spring Cloud

Der Metrik-Explorer von Azure ist eine Komponente des Microsoft Azure-Portals, die das Zeichnen von Diagrammen, das visuelle Korrelieren von Trends und das Untersuchen von Spitzen und Tiefen in Metriken ermöglicht. Verwenden Sie den Metrik-Explorer zum Untersuchen der Integrität und Auslastung Ihrer Ressourcen. 

In Azure Spring Cloud gibt es zwei Ansichten für Metriken.
* Übersichtsseite mit Diagrammen für jede Anwendung
* Seite mit allgemeinen Metriken

 ![Metrikdiagramme](media/metrics/metrics-1.png)

Über die Diagramme auf der Seite **Übersicht** einer Anwendung kann der Status einer Anwendung schnell überprüft werden. Auf der allgemeinen Seite **Metriken** können alle Metriken eingesehen werden. Sie können auf der allgemeinen Seite „Metriken“ Ihre eigenen Diagramme erstellen und an das Dashboard anheften.

## <a name="application-overview-page"></a>Übersichtsseite der Anwendung
Wählen Sie in der **App-Verwaltung** eine App aus, damit Diagramme für sie auf der Seite „Übersicht“ angezeigt werden.  

 ![Anwendungsmetriken in der App-Verwaltung](media/metrics/metrics-2.png)

Die Seite **Anwendungsübersicht** jeder Anwendung enthält ein Metrikdiagramm, das eine schnelle Überprüfung des Anwendungsstatus ermöglicht.  

 ![Die Seite „Übersicht“ der Anwendungsmetriken](media/metrics/metrics-3.png)

In Azure Spring Cloud stehen Ihnen diese fünf Metrikdiagramme zur Verfügung, die minütlich aktualisiert werden:

* **HTTP-Serverfehler**: Fehleranzahl für an Ihre App gesendete HTTP-Anforderungen
* **Eingehende Daten**: Von Ihrer App empfangene Byte
* **Ausgehende Daten**: Von Ihrer App gesendete Byte
* **Anforderungen**: Von Ihrer App empfangene Anforderungen
* **Durchschnittliche Antwortzeit**: Durchschnittliche Antwortzeit Ihrer App

Für die Diagramme kann ein Zeitbereich von einer Stunde bis zu sieben Tagen ausgewählt werden.

## <a name="common-metrics-page"></a>Seite mit allgemeinen Metriken

Wenn Sie im linken Navigationsbereich auf **Metriken** klicken, werden Sie auf die Seite mit allgemeinen Metriken weitergeleitet.

Wählen Sie zuerst Metriken aus, die Sie sich ansehen möchten:

![Auswählen einer Metrikansicht](media/metrics/metrics-4.png)

Wählen Sie dann für jede Metrik einen Aggregationstyp aus:

![Metrikaggregation](media/metrics/metrics-5.png)

Der Aggregationstyp gibt an, wie die Zeit aggregiert werden soll. Für Metriken wird ein Zeitintervall von einer Minute verwendet.
* Gesamt: Summieren aller Metriken als Zielausgabe
* Durchschnitt: Verwenden des Durchschnittswerts im Zeitraum als Zielausgabe
* Maximum/Minimum: Verwenden des maximalen/minimalen Werts im Zeitraum als Zielausgabe

Der Zeitbereich, der angezeigt werden soll, kann auch geändert werden.  Dabei kann zwischen den letzten 30 Minuten bis zu den letzten 30 Tagen oder ein benutzerdefinierter Zeitbereich ausgewählt werden.

![Ändern des Zeitbereichs für Metriken](media/metrics/metrics-6.png)

In der Standardansicht werden die Metriken aller Anwendungen des Azure Spring Cloud-Diensts angezeigt. Für die Anzeige können dann Metriken für eine App oder Instanz gefiltert werden.  Klicken Sie auf **Filter hinzufügen**, legen Sie die Eigenschaft auf **App** fest, und wählen Sie im Textfeld **Werte** die zu überwachende Zielanwendung aus. 

Sie können zwei Arten von Filtern (Eigenschaften) verwenden:
* App: Nach App-Namen filtern
* Instanz: Nach App-Instanz filtern

![Filter für Metriken](media/metrics/metrics-7.png)

Sie können auch die Option **Apply splitting** (Aufteilung anwenden) verwenden. Dabei werden mehrere Linien für eine App gezeichnet:

![Aufteilung von Metriken](media/metrics/metrics-8.png)

>[!TIP]
> Sie können auf der Seite „Metriken“ Ihre eigenen Diagramme erstellen und an das **Dashboard** anheften. Geben Sie dem Diagramm zunächst einen Namen.  Wählen Sie anschließend oben rechts **An Dashboard anheften** aus. Sie können nun Ihre Anwendung auf dem **Dashboard** im Portal überprüfen.

## <a name="user-portal-metrics-options"></a>Optionen für Benutzerportalmetriken

Die verfügbaren Metriken und ihre Details sind in der nachfolgenden Tabelle aufgeführt.
>[!div class="mx-tdBreakAll"]
>| Name | Anzeigename | Name der Metrik für den Azure Spring Cloud-Aktuator | Einheit | Details |
>|----|----|----|----|------------|
>| SystemCpuUsagePercentage | System CPU Usage Percentage (Prozentualer Anteil der System-CPU-Auslastung) | system.cpu.usage | Percent | Aktuelle CPU-Auslastung für das gesamte System Dieser Wert ist im [0.0,1.0]-Intervall ein Double. Ein Wert von 0.0 bedeutet, dass sich alle CPUs während des aktuell beobachteten Zeitraums im Leerlauf befanden. Ein Wert von 1.0 bedeutet, dass alle CPUs während 100 Prozent der Zeit des aktuell beobachteten Zeitraums aktiv ausgeführt wurden. Alle Werte zwischen 0.0 und 1.0 sind möglich, je nach Aktivitäten, die im System ausgeführt werden. Wenn die aktuelle CPU-Auslastung des Systems nicht verfügbar ist, gibt die Methode einen negativen Wert zurück. |
>| AppCpuUsagePercentage | App CPU Usage Percentage (Prozentualer Anteil der App-CPU-Auslastung) | App CPU Usage Percentage (Prozentualer Anteil der App-CPU-Auslastung) | Percent | Aktuelle CPU-Auslastung für den Java Virtual Machine-Prozess. Dieser Wert ist im [0.0,1.0]-Intervall ein Double. Ein Wert von 0.0 bedeutet, dass keine der CPUs während des aktuell beobachteten Zeitraums Threads des JVM-Prozesses ausgeführt hat. Ein Wert von 1.0 bedeutet, dass alle CPUs während 100 Prozent der Zeit des aktuell beobachteten Zeitraums aktiv JVM-Threads ausgeführt haben. Zu den JVM-Threads gehören Anwendungsthreads sowie interne JVM-Threads. Alle Werte zwischen 0.0 und 1.0 sind möglich, je nach Aktivitäten, die im JVM-Prozess und im gesamten System ausgeführt werden. Wenn die aktuelle CPU-Auslastung für den Java Virtual Machine-Prozess nicht verfügbar ist, gibt die Methode einen negativen Wert zurück. |
>| AppMemoryCommitted | App Memory Assigned (Zugewiesener App-Arbeitsspeicher) | jvm.memory.committed | Byte | Die Menge des Arbeitsspeichers (in Byte), die garantiert für die Verwendung durch die Java-VM zur Verfügung steht. Die Menge zugesicherten Speichers kann sich mit der Zeit ändern (zunehmen oder abnehmen). Die Java-VM gibt möglicherweise Arbeitsspeicher für das System frei, und „Zugesichert“ könnte niedriger als „Init“ sein. „Zugesichert“ ist immer größer oder gleich „Verwendet“. |
>| AppMemoryUsed | App Memory Used (Verwendeter App-Arbeitsspeicher) | jvm.memory.used | Byte | Die derzeit verwendete Arbeitsspeichermenge in Byte. |
>| AppMemoryMax | App Memory Max (Maximaler App-Arbeitsspeicher) | jvm.memory.max | Byte | Die maximale Speichermenge (in Byte), die für die Arbeitsspeicherverwaltung verwendet werden kann. Der Wert kann undefiniert sein. Wenn die maximale Arbeitsspeichermenge definiert ist, kann sie sich mit der Zeit ändern. Die Menge des verwendeten und zugesicherten Arbeitsspeichers ist immer kleiner oder gleich dem maximalen Wert, wenn er definiert ist. Eine Speicherzuordnung kann jedoch fehlschlagen, wenn sie versucht, den verwendeten Arbeitsspeicher so zu vergrößern, dass „Verwendet“ > „Zugesichert“ ist, selbst wenn „Verwendet <= „Max“ weiterhin zutrifft, z. B. wenn dem System der virtuelle Arbeitsspeicher ausgeht. |
>| MaxOldGenMemoryPoolBytes | Max Available Old Generation Data Size (Maximal verfügbare Tenured-Datengröße) | jvm.gc.max.data.size | Byte | Die maximale Arbeitsspeicherauslastung des Tenured-Speicherpools ab dem Zeitpunkt, an dem die Java-VM gestartet wurde. |
>| OldGenMemoryPoolBytes | Old Generation Data Size (Tenured-Datengröße) | jvm.gc.live.data.size | Byte | Größe des Tenured-Arbeitsspeicherpools nach einer vollständigen Garbage Collection. |
>| OldGenPromotedBytes | Promote to Old Generation Data Size (Hochstufung auf die Tenured-Datengröße) | jvm.gc.memory.promoted | Byte | Anzahl der positiven Erhöhungen der Größe des Tenured-Speicherpools vor der Garbage Collection gegenüber nach der Garbage Collection. |
>| YoungGenPromotedBytes | Promote to Young Generation Data Size (Hochstufung auf die Eden-Datengröße) | jvm.gc.memory.allocated | Byte | Inkrementiert zur Erhöhung der Größe des Eden-Speicherpools nach einer Garbage Collection gegenüber dem Zeitpunkt vor der nächsten. |
>| GCPauseTotalCount | GC Pause Count (Anzahl der Garbage Collection-Pausen) | jvm.gc.pause (total-count) | Anzahl | Anzahl aller Garbage Collections nach dem Start dieser Java-VM, einschließlich Eden- und Tenured-Garbage Collections. |
>| GCPauseTotalTime | GC Pause Total Time (Gesamtzeit der Garbage Collection-Pausen) | jvm.gc.pause (total-time) | Millisekunden | Gesamte für Garbage Collections verwendete Zeit nach dem Start dieser Java-VM, einschließlich Eden- und Tenured-Garbage Collections. |
>| TomcatSentBytes | Tomcat Total Sent Bytes (Insgesamt gesendete Bytes für Tomcat) | tomcat.global.sent | Byte | Menge der Daten in Byte, die vom Tomcat-Webserver gesendet wurden. |
>| TomcatReceivedBytes | Tomcat Total Received Bytes (Insgesamt empfangene Bytes für Tomcat) | tomcat.global.received | Byte | Menge der Daten in Byte, die vom Tomcat-Webserver empfangen wurden. |
>| TomcatRequestTotalTime | Tomcat Request Total Time (Gesamtzeit für Tomcat-Anforderungen) | tomcat.global.request (total-time) | Millisekunden | Insgesamt vom Tomcat-Webserver in Anspruch genommene Zeit für die Verarbeitung von Anforderungen. |
>| TomcatRequestTotalCount | Tomcat Request Total Count (Gesamtzahl von Tomcat-Anforderungen) | tomcat.global.request (total-count) | Anzahl | Gesamtzahl aller vom Tomcat-Webserver verarbeiteten Anforderungen. |
>| TomcatRequestMaxTime | Tomcat Request Max Time (Maximale für Tomcat-Anforderung verwendete Zeit) | tomcat.global.request.max | Millisekunden | Maximale vom Tomcat-Webserver in Anspruch genommene Zeit für die Verarbeitung einer Anforderung. |
>| TomcatErrorCount | Tomcat Global Error (Globale Tomcat-Fehler) | tomcat.global.error | Anzahl | Anzahl der bei der Verarbeitung von Anforderungen aufgetretenen Fehler. |
>| TomcatSessionActiveMaxCount | Tomcat Session Max Active Count (Maximale Anzahl aktiver Tomcat-Sitzungen) | tomcat.sessions.active.max | Anzahl | Die maximale Anzahl von Sitzungen, die zur selben Zeit aktiv waren. |
>| TomcatSessionAliveMaxTime | Tomcat Session Max Alive Time (Maximale aktive Zeit für Tomcat-Sitzungen) | tomcat.sessions.alive.max | Millisekunden | Der längste Zeitraum (in Sekunden), während der eine abgelaufene Sitzung noch aktiv war. |
>| TomcatSessionCreatedCount | Tomcat Session Created Count (Anzahl erstellter Tomcat-Sitzungen) | tomcat.sessions.created | Anzahl | Gesamtzahl aller erstellten Sitzungen. |
>| TomcatSessionExpiredCount | Tomcat Session Expired Count (Anzahl abgelaufener Tomcat-Sitzungen) | tomcat.sessions.expired | Anzahl | Anzahl der abgelaufenen Sitzungen. |
>| TomcatSessionRejectedCount | Tomcat Session Rejected Count (Anzahl abgewiesener Tomcat-Sitzungen) | tomcat.sessions.rejected | Anzahl | Anzahl der Sitzungen, die nicht erstellt wurden, da die maximal mögliche Anzahl aktiver Sitzungen bereits erreicht war. |

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte mit dem Azure-Metrik-Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Nächste Schritte
* [Tutorial: Überwachen von Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Kontingente und Servicepläne für Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)

