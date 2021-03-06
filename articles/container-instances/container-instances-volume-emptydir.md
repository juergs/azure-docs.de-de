---
title: Einbinden eines emptyDir-Volumes in eine Containergruppe
description: Hier erfahren Sie, wie Sie ein emptyDir-Volume einbinden, um Daten zwischen den Containern in einer Containergruppe in Azure Container Instances freizugeben.
ms.topic: article
ms.date: 02/08/2018
ms.openlocfilehash: 955423b685ebb3979271c7c2dc7e835a16100c2b
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552456"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Einbinden eines emptyDir-Volumes in Azure Container Instances

Hier erfahren Sie, wie Sie ein *emptyDir*-Volume einbinden, um Daten zwischen den Containern in einer Containergruppe in Azure Container Instances freizugeben. Verwenden Sie *emptyDir-* -Volumes als kurzlebige Caches für Ihre containerisierten Workloads.

> [!NOTE]
> Zurzeit ist das Einbinden eines *emptyDir*-Volumes auf Linux-Container beschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede in der [Übersicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="emptydir-volume"></a>emptyDir-Volume

Das *emptyDir*-Volume bietet ein Verzeichnis, in das geschrieben und auf das von jedem Container in einer Containergruppe zugegriffen werden kann. Container in der Gruppe können die gleichen Dateien im Volume lesen und schreiben, und das Volume kann mit den gleichen oder unterschiedlichen Pfaden in jedem Container eingebunden werden.

Beispiele für die Verwendung eines *emptyDir*-Volumes:

* Temporärer Speicherbereich
* Setzen von Prüfpunkten bei Tasks mit langer Ausführungsdauer
* Speicherdaten, die von einem Sidecar-Container abgerufen und von einem Anwendungscontainer bereitgestellt werden

Daten in einem *emptyDir*-Volume bleiben bei Containerabstürzen gespeichert. Bei neu gestarteten Container kann jedoch nicht garantiert werden, dass die Daten in einem *emptyDir*-Volume beibehalten werden. Wenn Sie eine Containergruppe anhalten, wird das *emptyDir*-Volume nicht persistent gespeichert.

## <a name="mount-an-emptydir-volume"></a>Einbinden eines emptyDir-Volumes

Sie können ein emptyDir-Volume in eine Containerinstanz einbinden, indem Sie zum Bereitstellen eine [Azure Resource Manager-Vorlage](/azure/templates/microsoft.containerinstance/containergroups) verwenden.

Füllen Sie in der Vorlage zunächst das `volumes`-Array im Abschnitt `properties` für die Containergruppe auf. Füllen Sie als Nächstes für jeden Container in der Containergruppe, in die Sie das *emptyDir*-Volume einbinden möchten, das Array `volumeMounts` im Abschnitt `properties` der Containerdefinition auf.

Mit der folgenden Resource Manager-Vorlage wird beispielsweise eine Containergruppe mit zwei Containern erstellt, die jeweils das *emptyDir*-Volume einbinden:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Ein Beispiel für die Bereitstellung der Containerinstanzen mit einer Azure Resource Manager-Vorlage finden Sie unter [Bereitstellen von Gruppen mit mehreren Containern in Azure Container Instances](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie andere Volumetypen in Azure Container Instances bereitgestellt werden:

* [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](container-instances-volume-azure-files.md)
* [Einbinden eines gitRepo-Volumes in Azure Container Instances](container-instances-volume-gitrepo.md)
* [Einbinden eines geheimen Volumes in Azure Container Instances](container-instances-volume-secret.md)