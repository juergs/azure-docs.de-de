---
title: Häufig gestellte Fragen zu Azure Monitor für VMs (allgemein verfügbare Version, GA) | Microsoft-Dokumentation
description: Azure Monitor for VMs ist eine Lösung in Azure, die Integritäts- und Leistungsüberwachung des Azure VM-Betriebssystems mit der automatischen Erkennung von Anwendungskomponenten und Abhängigkeiten mit anderen Ressourcen kombiniert und die Kommunikation unter ihnen als Zuordnung darstellt. In diesem Artikel werden häufig gestellte Fragen zum allgemein verfügbaren Release beantwortet.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/05/2019
ms.openlocfilehash: 4833b8a1835bd5da3327c73058f170fb0a5738a8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450704"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Monitor für VMs in der allgemein verfügbaren Version (GA)

Diese häufig gestellten Fragen zur allgemeinen Verfügbarkeit betreffen Änderungen, die im Zug unserer Vorbereitung auf die Version für allgemeine Verfügbarkeit in Azure Monitor für VMs auftreten. 

## <a name="updates-for-azure-monitor-for-vms"></a>Updates für Azure Monitor für VMs

Wir planen, im Januar 2020 eine neue Version von Azure Monitor für VMs zu veröffentlichen. Kunden, die Azure Monitor für VMs nach diesem Release aktivieren, erhalten automatisch die neue Version. Bestandskunden, die Azure Monitor für VMs bereits verwenden, werden zum Upgrade aufgefordert. Diese häufig gestellten Fragen und unsere Dokumentation bieten Anleitungen zum Durchführen eines Upgrades in der erforderlichen Größenordnung, wenn Sie über große Bereitstellungen mit mehreren Arbeitsbereichen verfügen.

Mit diesem Upgrade werden Leistungsdaten von Azure Monitor für VMs in derselben Tabelle `InsightsMetrics` wie bei [Azure Monitor für Container](container-insights-overview.md) gespeichert. So können Sie diese beiden Datasets einfacher abfragen. Außerdem können Sie vielfältigere Datasets speichern, die in der zuvor verwendeten Tabelle nicht gespeichert werden konnten. Die Leistungsansichten werden ebenfalls für die Verwendung dieser neuen Tabelle aktualisiert.

Es erfolgt ein Wechsel zu neuen Datentypen für unsere Verbindungsdatasets. Diese Änderung erfolgt im Dezember 2019 und wird in einem Azure Update-Blogbeitrag bekannt gegeben. Daten, die derzeit in den benutzerdefinierten Protokolltabellen `ServiceMapComputer_CL` und `ServiceMapProcess_CL` gespeichert sind, werden in dedizierte Datentypen mit den Namen `VMComputer` und `VMProcess` verschoben. Durch den Wechsel zu dedizierten Datentypen erhalten sie eine höhere Priorität für die Datenerfassung, und das Tabellenschema wird für alle Kunden standardisiert.

Uns ist bewusst, dass die Aufforderung zum Upgrade für bestehende Kunden eine Unterbrechung ihres Workflows bedeutet. Deshalb haben wir uns entschieden, dies zum jetzigen Zeitpunkt in der Public Preview und nicht erst später nach der allgemeinen Verfügbarkeit durchzuführen.

## <a name="what-is-changing"></a>Was ändert sich?

Wenn Sie derzeit den Onboardingprozess für Azure Monitor für VMs durchführen, aktivieren Sie die Dienstzuordnungslösung in dem Arbeitsbereich, den Sie zum Speichern Ihrer Überwachungsdaten ausgewählt haben, und konfigurieren dann die Leistungsindikatoren für die Daten, die wir von Ihren VMs sammeln. Wir veröffentlichen eine neue Lösung mit dem Namen **VMInsights**, die zusätzliche Funktionen für die Datensammlung sowie einen neuen Speicherort für diese Daten in Ihrem Log Analytics-Arbeitsbereich umfasst.

Bei unserem aktuellen Verfahren unter Verwendung von Leistungsindikatoren in Ihrem Log Analytics-Arbeitsbereich werden die Daten an die Tabelle `Perf` gesendet. Bei dieser neuen Lösung werden die Daten an die Tabelle `InsightsMetrics` gesendet, die auch von Azure Monitor für Container verwendet wird. Aufgrund dieses Tabellenschemas können wir zusätzliche Metriken und Dienstdatasets speichern, die nicht mit dem Format der Tabelle „Perf“ kompatibel sind.

## <a name="what-should-i-do-about-the-performance-counters-in-my-workspace-if-i-install-the-vminsights-solution"></a>Was soll ich mit den Leistungsindikatoren in meinem Arbeitsbereich machen, wenn ich die VMInsights-Lösung installiere?

Bei der aktuellen Methode zum Aktivieren von Azure Monitor für VMs werden Leistungsindikatoren in Ihrem Arbeitsbereich verwendet. Bei der neuen Methode werden diese Daten in einer neuen Tabelle namens `InsightsMetrics` gespeichert.

Sobald wir unsere Benutzeroberfläche für die Verwendung der Daten in der Tabelle `InsightsMetrics` aktualisiert haben, aktualisieren wir auch unsere Dokumentation und verbreiten diese Ankündigung über mehrere Kanäle, einschließlich eines Banners im Azure-Portal. Zu diesem Zeitpunkt können Sie die [Leistungsindikatoren](vminsights-enable-overview.md#performance-counters-enabled) in Ihrem Arbeitsbereich deaktivieren, wenn Sie sie nicht mehr benötigen. 

>[!NOTE]
>Wenn Sie über Warnungsregeln verfügen, die auf diese Leistungsindikatoren in der Tabelle `Perf` verweisen, müssen Sie sie aktualisieren, damit sie auf die neuen Daten in der Tabelle `InsightsMetrics` verweisen. In unserer Dokumentation finden Sie Beispiele für Protokollabfragen, die Sie verwenden können und die auf diese Tabelle verweisen.
>

Wenn Sie die Leistungsindikatoren aktiviert lassen, werden Ihnen die erfassten und in der Tabelle `Perf` gespeicherten Daten basierend auf den [Log Analytics-Preisen](https://azure.microsoft.com/pricing/details/monitor/) in Rechnung gestellt.

## <a name="how-will-this-change-affect-my-alert-rules"></a>Wie wirkt sich diese Änderung auf meine Warnungsregeln aus?

Wenn Sie [Protokollwarnungen](../platform/alerts-unified-log.md) erstellt haben, die die Tabelle `Perf` für die im Arbeitsbereich aktivierten Leistungsindikatoren abfragen, sollten Sie diese Regeln aktualisieren, damit sie stattdessen auf die Tabelle `InsightsMetrics` verweisen. Diese Anweisung betrifft auch alle Regeln für die Protokollsuche, die `ServiceMapComputer_CL` und `ServiceMapProcess_CL` verwenden, da diese Datasets in die Tabellen `VMComputer` und `VMProcess` verschoben werden.

Diese häufig gestellten Fragen und unsere Dokumentation werden noch aktualisiert, damit Beispiele für Warnungsregeln für die Protokollsuche für die von uns gesammelten Datasets enthalten sind.

## <a name="how-will-this-affect-my-bill"></a>Welche Folgen hat das für meine Rechnung?

Die Abrechnung basiert weiterhin auf Daten, die erfasst und in Ihrem Log Analytics-Arbeitsbereich gespeichert werden.

Die von uns gesammelten Leistungsdaten auf Computerebene sind identisch, haben eine ähnliche Größe wie die in der Tabelle `Perf` gespeicherten Daten und weisen ungefähr dieselben Kosten auf.

## <a name="what-if-i-only-want-to-use-service-map"></a>Kann ich auch nur die Dienstzuordnung verwenden?

Das geht. Beim Anzeigen von Azure Monitor für VMs werden im Azure-Portal entsprechende Aufforderungen zum bevorstehenden Update angezeigt. Nach der Veröffentlichung erhalten Sie eine Aufforderung zum Update auf die neue Version. Wenn Sie nur das [Zuordnungsfeature](vminsights-maps.md) verwenden möchten, müssen Sie kein Upgrade durchführen und können das Zuordnungsfeature von Azure Monitor für VMs sowie die Dienstzuordnungslösung, auf die Sie über Ihren Arbeitsbereich oder die Dashboardkachel zugreifen, weiterhin verwenden.

Wenn Sie die Leistungsindikatoren in Ihrem Arbeitsbereich manuell aktivieren, können Sie möglicherweise Daten in einigen unserer Leistungsdiagramme sehen, die über Azure Monitor angezeigt werden. Nach der Veröffentlichung der neuen Lösung nehmen wir eine Aktualisierung unserer Leistungsdiagramme vor, um die in der `InsightsMetrics`-Tabelle gespeicherten Daten abzufragen. Wenn Sie Daten aus dieser Tabelle in diesen Diagrammen anzeigen möchten, müssen Sie ein Upgrade auf die neue Version von Azure Monitor für VMs durchführen.

Die Änderungen zum Verschieben von Daten aus `ServiceMapComputer_CL` und `ServiceMapProcess_CL` wirken sich sowohl auf die Dienstzuordnung als auch Azure Monitor für VMs aus, sodass Sie dieses Update trotzdem einplanen müssen.

Wenn Sie kein Upgrade auf die **VMInsights**-Lösung durchführen, werden wir weiterhin Legacyversionen unserer Arbeitsmappen zur Leistung bereitstellen, die sich auf Daten in der `Perf`-Tabelle beziehen.  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>Werden die Datasets der Dienstzuordnung auch in InsightsMetrics gespeichert?

Die Datasets werden nicht dupliziert, wenn Sie beide Lösungen verwenden. Beide Angebote nutzen die Datasets gemeinsam, die in den Tabellen `VMComputer` (zuvor „ServiceMapComputer_CL“), `VMProcess` (zuvor „ServiceMapProcess_CL“), `VMConnection` und `VMBoundPort` zum Speichern der von uns gesammelten Zuordnungsdatasets gespeichert werden.  

In der Tabelle `InsightsMetrics` werden die von uns gesammelten VM-, Prozess- und Dienstdatasets gespeichert, und sie wird nur aufgefüllt, wenn Sie Azure Monitor für VMs verwenden. Die Dienstzuordnungslösung erfasst keine Daten und speichert nichts in der Tabelle `InsightsMetrics`.

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>Wenn in meinem Arbeitsbereich sowohl die Dienstzuordnungslösung als auch die VMInsights-Lösung vorhanden ist, werden mir die Daten dann doppelt in Rechnung gestellt?

Nein. Die beiden Lösungen nutzen die Zuordnungsdatasets gemeinsam, die wir in `VMComputer` (zuvor „ServiceMapComputer_CL“), `VMProcess` (zuvor „ServiceMapProcess_CL“), `VMConnection` und `VMBoundPort` speichern. Diese Daten werden Ihnen nicht doppelt in Rechnung gestellt, wenn beide Lösungen in Ihrem Arbeitsbereich vorhanden sind.

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>Wenn ich die Dienstzuordnungslösung oder die VMInsights-Lösung entferne, werden dann auch meine Daten entfernt?

Nein. Die beiden Lösungen nutzen die Zuordnungsdatasets gemeinsam, die wir in `VMComputer` (zuvor „ServiceMapComputer_CL“), `VMProcess` (zuvor „ServiceMapProcess_CL“), `VMConnection` und `VMBoundPort` speichern. Wenn Sie eine der Lösungen entfernen, erkennen diese Datasets, dass immer noch eine Lösung vorhanden ist, die die Daten verwendet, und die Daten verbleiben im Log Analytics-Arbeitsbereich. Sie müssen beide Lösungen aus Ihrem Arbeitsbereich entfernen, damit die Daten entfernt werden.

## <a name="when-will-this-update-be-released"></a>Wann wird dieses Update veröffentlicht?

Wir gehen davon aus, dass das Update für Azure Monitor für VMs Anfang Januar 2020 veröffentlicht wird. Wenn das Veröffentlichungsdatum näher rückt, werden wir hier Updates veröffentlichen und Benachrichtigungen im Azure-Portal einblenden, wenn Sie Azure Monitor öffnen.

## <a name="health-feature-is-in-limited-public-preview"></a>Integritätsfeature in der eingeschränkten Public Preview

Wir haben von Kunden viel wertvolles Feedback zu unserem VM-Integritätsfeature erhalten. Es besteht starkes Interesse an diesem Feature und seinen Möglichkeiten für die Unterstützung von Überwachungsworkflows. Wir planen eine Reihe von Änderungen, um Funktionen hinzuzufügen und das erhaltene Feedback umzusetzen. 

Um die Auswirkungen dieser Änderungen auf neue Kunden möglichst gering zu halten, haben wir dieses Feature in eine **eingeschränkte Public Preview** verschoben. Dieses Update fand im Oktober 2019 statt.

Wir planen, dieses Integritätsfeature 2020 erneut zu veröffentlichen, nachdem Azure Monitor für VMs allgemein verfügbar ist.

## <a name="how-do-existing-customers-access-the-health-feature"></a>Wie greifen vorhandene Kunden auf das Integritätsfeature zu?

Vorhandene Kunden, die das Integritätsfeature verwenden, können weiterhin darauf zugreifen, doch wird es neuen Kunden nicht angeboten.  

Für den Zugriff auf das Feature können Sie der Azure-Portal-URL [https://portal.azure.com](https://portal.azure.com) das Featureflag `feature.vmhealth=true` hinzufügen. Beispiel: `https://portal.azure.com/?feature.vmhealth=true`.

Sie können auch diese kurze URL verwenden, mit der das Featureflag automatisch gesetzt wird: [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview).

Als vorhandener Kunde können Sie das Integritätsfeature weiterhin für VMs verwenden, die mit einer bestehenden Arbeitsbereichseinrichtung mit Integritätsfunktion verbunden sind.  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>Ich verwende VM-Integrität derzeit für eine Umgebung und möchte das Feature für eine neue bereitstellen

Wenn Sie bereits ein Kunde sind, der das Integritätsfeature verwendet, und Sie das Feature für ein neues Rollout verwenden möchten, nehmen Sie unter vminsights@microsoft.com Kontakt mit uns auf, um Anweisungen anzufordern.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den Anforderungen und Methoden, die Ihnen beim Überwachen Ihrer virtuellen Computer helfen, finden Sie unter [Bereitstellen von Azure Monitor für VMs](vminsights-enable-overview.md).
