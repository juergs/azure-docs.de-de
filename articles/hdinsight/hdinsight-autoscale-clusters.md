---
title: Automatisches Skalieren von Azure HDInsight-Clustern
description: Verwenden des Azure HDInsight-Features „Autoskalierung“ für die automatische Apache Hadoop-Skalierung von Clustern
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/22/2019
ms.openlocfilehash: 45804bd3e81e7363010979b7a6e028356b3a5080
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780061"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Automatisches Skalieren von Azure HDInsight-Clustern

> [!Important]
> Das Feature für die Autoskalierung funktioniert nur für Spark-, Hive-, LLAP- und HBase-Cluster, die nach dem 8. Mai 2019 erstellt wurden. 

Das Azure HDInsight-Feature „Autoskalierung“ skaliert die Anzahl der Workerknoten in einem Cluster automatisch zentral hoch oder herunter. Andere Arten von Knoten im Cluster können derzeit nicht skaliert werden.  Während der Erstellung eines neuen HDInsight-Clusters kann eine minimale und maximale Anzahl von Workerknoten festgelegt werden. Die automatische Skalierung überwacht dann die Ressourcenanforderungen der Analyselast und skaliert die Anzahl von Workerknoten dann zentral hoch oder herunter. Für dieses Feature fallen keine zusätzlichen Gebühren an.

## <a name="cluster-compatibility"></a>Clusterkompatibilität

Die folgende Tabelle beschreibt die Clustertypen und Versionen, die mit dem Feature „Autoskalierung“ kompatibel sind.

| Version | Spark | Hive | LLAP | hbase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| HDInsight 3.6 ohne ESP | Ja | Ja | Ja | Ja* | Nein | Nein | Nein |
| HDInsight 4.0 ohne ESP | Ja | Ja | Ja | Ja* | Nein | Nein | Nein |
| HDInsight 3.6 mit ESP | Ja | Ja | Ja | Ja* | Nein | Nein | Nein |
| HDInsight 4.0 mit ESP | Ja | Ja | Ja | Ja* | Nein | Nein | Nein |

\* HBase-Cluster können nur für zeitplanbasierte, aber nicht für auslastungsbasierte Skalierung konfiguriert werden.

## <a name="how-it-works"></a>Funktionsweise

Sie können zwischen lastbasierter Skalierung und zeitplanbasierter Skalierung für Ihren HDInsight-Cluster wählen. Die lastbasierte Skalierung ändert die Anzahl der Knoten in Ihrem Cluster innerhalb eines von Ihnen festgelegten Bereichs, um eine optimale CPU-Auslastung zu gewährleisten und die Betriebskosten zu minimieren.

Die zeitplanbasierte Skalierung ändert die Anzahl der Knoten in Ihrem Cluster basierend auf Bedingungen, die zu bestimmten Zeiten wirksam werden. Diese Bedingungen skalieren den Cluster auf eine gewünschte Anzahl von Knoten.

### <a name="metrics-monitoring"></a>Überwachen von Metriken

Die automatische Skalierung überwacht kontinuierlich die Cluster und sammelt die folgenden Metriken:

* **CPU insgesamt für ausstehende**: Die Gesamtanzahl von Kernen, die zum Starten der Ausführung aller ausstehenden Container erforderlich sind.
* **Arbeitsspeicher insgesamt für ausstehende**: Die Gesamtgröße des Arbeitsspeichers (in MB), die zum Starten der Ausführung aller ausstehenden Container erforderlich ist.
* **Freie CPUs insgesamt**: Die Summe aller nicht verwendeten Kerne auf den aktiven Workerknoten.
* **Freier Arbeitsspeicher insgesamt**: Die Summe aller nicht verwendeten Kerne (in MB) auf den aktiven Workerknoten.
* **Verwendeter Arbeitsspeicher pro Knoten**: Die Last auf einem Workerknoten. Ein Workerknoten, auf dem 10GB Arbeitsspeicher verwendet werden, ist höher ausgelastet als ein Workerknoten, auf dem 2GB verwendet werden.
* **Anzahl der Anwendungsmaster pro Knoten**: Die Anzahl der Anwendungsmastercontainer (Application Master, AM), die auf einem Workerknoten ausgeführt werden. Ein Workerknoten, der 2 AM-Container hostet, gilt als wichtiger als ein Workerknoten, der 0 AM-Container hostet.

Die oben aufgeführten Metriken werden alle 60 Sekunden überprüft. Die Autoskalierung trifft auf Basis dieser Metriken Entscheidungen zum zentralen Hoch- und Herunterskalieren.

### <a name="load-based-cluster-scale-up"></a>Zentrales Hochskalieren eines lastbasierten Clusters

Wenn die folgenden Bedingungen erkannt werden, fordert die automatische Skalierung ein zentrales Hochskalieren an:

* „CPU insgesamt für ausstehende“ ist länger als drei Minuten größer als die Anzahl der insgesamt freien CPUs.
* „Arbeitsspeicher insgesamt für ausstehende“ ist länger als drei Minuten größer als der insgesamt freie Arbeitsspeicher.

Der HDInsight-Dienst berechnet, wie viele neue Workerknoten benötigt werden, um die aktuellen CPU- und Speicheranforderungen zu erfüllen, und gibt dann eine zentrale Hochskalierungsanforderung aus, um die erforderliche Anzahl an Knoten hinzuzufügen.

### <a name="load-based-cluster-scale-down"></a>Zentrales Herunterskalieren eines lastbasierten Clusters

Wenn die folgenden Bedingungen erkannt werden, fordert die automatische Skalierung ein zentrales Herunterskalieren an:

* „CPU insgesamt für ausstehende“ ist länger als 10 Minuten kleiner als die Anzahl der insgesamt freien CPUs.
* „Arbeitsspeicher insgesamt für ausstehende“ ist länger als 10 Minuten kleiner als der insgesamt freie Arbeitsspeicher.

Basierend auf der Anzahl der AM-Container pro Knoten und der aktuellen CPU- und Arbeitsspeicheranforderungen gibt die Autoskalierung eine Anforderung zum Entfernen einer bestimmten Anzahl von Knoten aus. Der Dienst erkennt auch, welche Knoten aufgrund der aktuellen Auftragsausführung für die Entfernung in Frage kommen. Der Vorgang des zentralen Herunterskalierens deaktiviert zunächst die Knoten und entfernt sie dann aus dem Cluster.

## <a name="get-started"></a>Erste Schritte

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Erstellen eines Clusters mit lastbasierter Autoskalierung

Wenn Sie die automatische Skalierung in einem Cluster verwenden möchten, müssen Sie die Option **Automatische Skalierung aktivieren** beim Erstellen des Clusters aktivieren. 

Um das Feature „Autoskalierung“ mit lastbasierter Skalierung zu aktivieren, führen Sie als Teil des normalen Clustererstellungsvorgangs folgende Schritte aus:

1. Aktivieren Sie auf der Registerkarte **Konfiguration + Preise** das Kontrollkästchen **Automatische Skalierung aktivieren**.
1. Wählen Sie **Lastbasiert** unter **Autoskalierungstyp** aus.
1. Geben Sie die gewünschten Werte für die folgenden Eigenschaften ein:  

    * Anfängliche **Anzahl von Workerknoten** für **Workerknoten**.
    * **Minimale** Anzahl von Workerknoten.
    * **Maximale** Anzahl von Workerknoten.

    ![Aktivieren der lastbasierten Autoskalierung des Workerknotens](./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-autoscale.png)

Die anfängliche Anzahl der Workerknoten kann vom Mindest- bis zum Höchstwert reichen. Dieser Wert definiert die Anfangsgröße des Clusters bei der Erstellung. Die Mindestzahl der Workerknoten sollte auf drei oder mehr festgelegt werden. erforderlich. Die Skalierung des Clusters auf weniger als drei Knoten kann dazu führen, dass der Cluster aufgrund unzureichender Dateireplikation im abgesicherten Modus hängen bleibt. Weitere Informationen finden Sie unter [Hängenbleiben im abgesicherten Modus]( https://docs.microsoft.com/ azure/hdinsight/hdinsight-scaling-best-practices#getting-stuck-in-safe-mode).

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Erstellen eines Clusters mit zeitplanbasiert Autoskalierung

Um das Feature „Autoskalierung“ mit zeitplanbasierter Skalierung zu aktivieren, führen Sie als Teil des normalen Clustererstellungsvorgangs folgende Schritte aus:

1. Aktivieren Sie auf der Registerkarte **Konfiguration + Preise** das Kontrollkästchen **Automatische Skalierung aktivieren**.
1. Geben Sie die **Anzahl der Knoten** für **Workerknoten** ein. Damit wird begrenzt, wie hoch der Cluster skaliert werden kann.
1. Wählen Sie unter **Autoskalierungstyp** die Option **Zeitplanbasiert** aus.
1. Klicken Sie auf **Konfigurieren**, um das Fenster **Konfiguration der Autoskalierung** zu öffnen.
1. Wählen Sie Ihre Zeitzone aus, und klicken Sie dann auf **+ Bedingung hinzufügen**.
1. Wählen Sie die Tage der Woche, an denen die neue Bedingung angewendet werden soll.
1. Bearbeiten Sie die Zeit, zu der die Bedingung wirksam werden soll, und die Anzahl der Knoten, auf die der Cluster skaliert werden soll.
1. Fügen Sie gegebenenfalls weitere Bedingungen hinzu.

    ![Aktivieren der zeitplanbasierten Erstellung des Workerknotens](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

Die Anzahl der Knoten muss zwischen 3 und der maximalen Anzahl der Workerknoten liegen, die Sie vor dem Hinzufügen von Bedingungen eingegeben haben.

### <a name="final-creation-steps"></a>Letzte Erstellungsschritte

Wählen Sie für die last- und für die zeitplanbasierte Skalierung den VM-Typ für Workerknoten aus, indem Sie eine VM in der Dropdown-Liste unter **Knotengröße** auswählen. Nachdem Sie den VM-Typ für jeden Knotentyp ausgewählt haben, können Sie den Bereich der geschätzten Kosten für den gesamten Cluster sehen. Passen Sie die VM-Typen entsprechend Ihrem Budget an.

![Aktivieren der Knotengröße für die zeitplanbasierte Autoskalierung des Workerknotens](./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-vmsize.png)

Ihr Abonnement verfügt für jede Region über ein Kapazitätskontingent. Die Gesamtzahl der Kerne Ihrer Hauptknoten darf in Kombination mit der maximalen Anzahl von Workerknoten das Kapazitätskontingent nicht überschreiten. Dieses Kontingent ist jedoch eine weiche Grenze. Sie können immer ein Supportticket erstellen, um es problemlos erhöhen zu lassen.

> [!Note]  
> Wenn Sie die gesamte Kernkontingentgrenze überschreiten, informiert Sie eine Fehlermeldung darüber, dass die maximale Knotenzahl die Zahl der verfügbaren Kerne in dieser Region überschritten hat, und Sie werden aufgefordert, eine andere Region auszuwählen oder den Support um Erhöhung des Kontingents zu bitten.

Weitere Informationen zum Erstellen von HDInsight-Clustern mit dem Azure-Portal finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Erstellen eines Clusters mithilfe einer Resource Manager-Vorlage

#### <a name="load-based-autoscaling"></a>Lastbasierte Autoskalierung

Sie können einen HDInsight-Cluster mit lastbasierter Autoskalierung und einer Azure Resource Manager-Vorlage erstellen, indem Sie dem Abschnitt `computeProfile` > `workernode` einen `autoscale`-Knoten mit den Eigenschaften `minInstanceCount` und `maxInstanceCount` hinzufügen, wie im folgenden JSON-Codeausschnitt gezeigt.

```json
{
  "name": "workernode",
  "targetInstanceCount": 4,
  "autoscale": {
      "capacity": {
          "minInstanceCount": 3,
          "maxInstanceCount": 10
      }
  },
  "hardwareProfile": {
      "vmSize": "Standard_D13_V2"
  },
  "osProfile": {
      "linuxOperatingSystemProfile": {
          "username": "[parameters('sshUserName')]",
          "password": "[parameters('sshPassword')]"
      }
  },
  "virtualNetworkProfile": null,
  "scriptActions": []
}
```

Weitere Informationen zum Erstellen von Clustern mit Resource Manager-Vorlagen finden Sie unter [Erstellen von Apache Hadoop-Clustern in HDInsight mithilfe von Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  

#### <a name="schedule-based-autoscaling"></a>Zeitplanbasierte Autoskalierung

Sie können einen HDInsight-Cluster mit zeitplanbasierter Autoskalierung und einer Azure Resource Manager-Vorlage erstellen, indem Sie dem Abschnitt `computeProfile` > `workernode` einen `autoscale`-Knoten hinzufügen. Der `autoscale`-Knoten enthält ein `recurrence` mit einer `timezone` und einem `schedule`. Damit wird beschrieben, wann die Änderung erfolgen wird.

```json
{
  "autoscale": {
    "recurrence": {
      "timeZone": "Pacific Standard Time",
      "schedule": [
        {
          "days": [
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday"
          ],
          "timeAndCapacity": {
            "time": "11:00",
            "minInstanceCount": 10,
            "maxInstanceCount": 10
          }
        },
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4,
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Aktivieren und Deaktivieren der Autoskalierung für einen aktuell ausgeführten Cluster

#### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Zum Aktivieren der Autoskalierung in einem ausgeführten Cluster wählen **Clustergröße** unter **Einstellungen**. Klicken Sie dann auf **Autoskalierung aktivieren**. Wählen Sie die gewünschte Art der Autoskalierung, und geben Sie die Optionen für die last- oder zeitplanbasierte Skalierung ein. Klicken Sie abschließend auf **Speichern**.

![Aktivieren der zeitplanbasierte Autoskalierung des Workerknotens für einen aktiven Cluster](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-enable-running-cluster.png)

#### <a name="using-the-rest-api"></a>Verwenden der REST-API

Um die Autoskalierung für einen aktiven Cluster über die REST-API zu aktivieren oder zu deaktivieren, senden Sie eine POST-Anforderung an den Endpunkt der Autoskalierung, wie im folgenden Codeausschnitt gezeigt:

```
https://management.azure.com/subscriptions/{subscription Id}/resourceGroups/{resourceGroup Name}/providers/Microsoft.HDInsight/clusters/{CLUSTERNAME}/roles/workernode/autoscale?api-version=2018-06-01-preview
```

Verwenden Sie die geeigneten Parameter in der Anforderungsnutzlast. Die nachfolgende JSON-Nutzlast könnte verwendet werden, um Autoskalierung zu aktivieren. Verwenden Sie die Nutzlast `{autoscale: null}`, um Autoskalierung zu deaktivieren.

```json
{ autoscale: { capacity: { minInstanceCount: 3, maxInstanceCount: 2 } } }
```

Eine vollständige Beschreibung aller Nutzlastparameter finden Sie im vorherigen Abschnitt über [Aktivieren von lastenbasierter Autoskalierung](#load-based-autoscaling).

## <a name="best-practices"></a>Bewährte Methoden

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Auswählen von last- oder zeitplanbasierter Skalierung

Berücksichtigen Sie die folgenden Faktoren, bevor Sie sich für einen Modus entscheiden:

* Aktivieren Sie die automatische Skalierung während der Clustererstellung.
* Die Mindestanzahl von Knoten muss mindestens drei Knoten betragen.
* Lastvarianz: Folgt die Last des Clusters zu bestimmten Zeiten bzw. an bestimmten Tagen einem einheitlichen Muster? Wenn nicht, ist die lastbasierte Skalierung die besser geeignete Option.
* SLA-Anforderungen: Die Autoskalierung ist nicht vorausschauend (prädiktiv), sondern reaktiv. Besteht eine ausreichende Verzögerung zwischen dem Zeitpunkt, zu dem die Zunahme der Last beginnt, und dem Zeitpunkt, zu dem der Cluster die Zielgröße erreicht haben muss? Falls strenge SLA-Anforderungen gelten und die Last einem festen bekannten Muster folgt, ist „zeitplanbasiert“ die besser geeignete Option.

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Berücksichtigen der Latenz von Vorgängen zum zentralen Hoch- bzw. Herunterskalieren

Es kann 10 bis 20 Minuten dauern, bis ein Skalierungsvorgang abgeschlossen ist. Sie sollten diese Verzögerung einplanen, wenn Sie einen benutzerdefinierten Zeitplan einrichten. Wenn Sie beispielsweise um 9:00 Uhr eine Clustergröße von 20 benötigen, sollten Sie den Zeitplantrigger auf einen früheren Zeitpunkt festlegen, z. B. 8:30 Uhr, damit der Skalierungsvorgang um 9:00 Uhr abgeschlossen ist.

### <a name="preparation-for-scaling-down"></a>Vorbereitung für das zentrale Herunterskalieren

Beim zentralen Herunterskalieren des Clusters werden die Knoten von der Autoskalierung außer Betrieb genommen, um die Zielgröße zu erreichen. Falls auf diesen Knoten Aufgaben durchgeführt werden, wartet die Autoskalierung, bis sie abgeschlossen sind. Da jeder Workerknoten auch eine Rolle im Hadoop Distributed File System innehat, werden die temporären Daten auf die restlichen Knoten verschoben. Sie sollten also sicherstellen, dass auf den verbleibenden Knoten genügend Speicherplatz vorhanden ist, um alle temporären Daten zu hosten.

Die ausgeführten Aufträge werden weiterhin ausgeführt und abgeschlossen. Für die ausstehenden Aufträge wird auf die reguläre Einplanung mit weniger verfügbaren Workerknoten gewartet.

### <a name="minimum-cluster-size"></a>Minimale Clustergröße

Skalieren Sie den Cluster nicht auf weniger als drei Knoten herunter. Die Skalierung des Clusters auf weniger als drei Knoten kann dazu führen, dass der Cluster aufgrund unzureichender Dateireplikation im abgesicherten Modus hängen bleibt. Weitere Informationen finden Sie unter [Hängenbleiben im abgesicherten Modus]( https://docs.microsoft.com/ azure/hdinsight/hdinsight-scaling-best-practices#getting-stuck-in-safe-mode).

## <a name="monitoring"></a>Überwachung

### <a name="cluster-status"></a>Clusterstatus

Der im Azure-Portal aufgeführte Clusterstatus kann Ihnen helfen, die Aktivitäten der Autoskalierung zu überwachen.

![Aktivieren des Clusterstatus für die lastbasierte Autoskalierung des Workerknotens](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Alle Statusmeldungen des Clusters, die möglicherweise angezeigt werden, werden in der folgenden Liste erläutert.

| Clusterstatus | Erklärung |
|---|---|
| Wird ausgeführt | Der Cluster wird normal ausgeführt. Alle vorherigen Autoskalierungsaktivitäten wurde erfolgreich abgeschlossen. |
| Wird aktualisiert  | Die Autoskalierungskonfiguration für den Cluster wird aktualisiert.  |
| HDInsight-Konfiguration  | Es wird ein Vorgang für das zentral Hoch- oder Herunterskalieren des Clusters ausgeführt.  |
| Fehler beim Aktualisieren  | HDInsight hat beim Aktualisieren der Autoskalierungskonfiguration Fehler festgestellt. Kunden können wählen, ob sie den Aktualisierungsvorgang wiederholen oder die Autoskalierung deaktivieren möchten.  |
| Fehler  | Es gibt ein Problem mit dem Cluster, sodass er kann nicht verwendet werden kann. Löschen Sie diesen Cluster, und erstellen Sie einen neuen.  |

Ihrem Cluster anzuzeigen, gehen Sie zum Diagramm **Clustergröße** auf der Seite **Übersicht** für Ihren Cluster, oder klicken Sie unter **Einstellungen** auf **Clustergröße**.

### <a name="operation-history"></a>Vorgangsverlauf

Sie können den Verlauf des zentralen Hoch- und Herunterskalierens des Clusters als Teil der Clustermetriken anzeigen. Sie können auch alle Skalierungsaktionen des letzten Tags, der letzten Woche oder eines anderen Zeitraums auflisten.

Wählen Sie unter **Überwachung** **Metriken** aus. Klicken Sie dann im Dropdownfeld **Metrik** auf **Metrik hinzufügen** und auf **Anzahl der aktiven Worker**. Klicken Sie auf die Schaltfläche in der oberen rechten Ecke, um den Zeitbereich zu ändern.

![Aktivieren der Metrik für die zeitplanbasierte Autoskalierung des Workerknotens](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über bewährte Methoden für die manuelle Skalierung von Clustern in [Skalieren von HDInsight-Clustern](hdinsight-scaling-best-practices.md)
