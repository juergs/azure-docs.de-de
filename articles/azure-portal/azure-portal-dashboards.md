---
title: Erstellen und Freigeben von Dashboards im Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Dashboards im Azure-Portal erstellt, angepasst, veröffentlicht und freigegeben werden.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/01/2019
ms.author: mblythe
ms.openlocfilehash: a3b4d7cb33bf0da0c4431d76a54644208ea6468f
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640455"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Erstellen und Freigeben von Dashboards im Azure-Portal

Mit Dashboards können Sie eine fokussierte und organisierte Ansicht Ihrer Cloudressourcen im Azure-Portal erstellen. Verwenden Sie Dashboards als Arbeitsbereich, in dem Sie schnell Aufgaben für alltägliche Vorgänge starten und Ressourcen überwachen können.  Erstellen Sie benutzerdefinierte Dashboards, die beispielsweise auf Projekten, Aufgaben oder Benutzerrollen basieren.  Das Azure-Portal stellt ein Standarddashboard als Ausgangspunkt bereit. Sie können das Standarddashboard bearbeiten, zusätzliche Dashboards erstellen und anpassen sowie Dashboards veröffentlichen und freigeben, um sie für andere Benutzer verfügbar zu machen. In diesem Artikel wird beschrieben, wie Sie ein neues Dashboard erstellen, die Benutzeroberfläche anpassen und Dashboards veröffentlichen und freigeben.

## <a name="create-a-new-dashboard"></a>Erstellen eines neuen Dashboards

In diesem Beispiel erstellen wir ein neues privates Dashboard und weisen ihm einen Namen zu. Gehen Sie dazu wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **Dashboard** im oberen Bereich der linken Randleiste aus. Die Standardansicht ist möglicherweise bereits auf das Dashboard festgelegt.
1. Wählen Sie **+ Neues Dashboard**aus.

    ![Screenshot des Standarddashboards](./media/azure-portal-dashboards/dashboard-new.png)

4. Mit dieser Aktion wird der **Kachelkatalog** geöffnet, aus dem Sie Kacheln auswählen, sowie ein leeres Raster, in dem Sie die Kacheln anordnen.

    ![Screenshot des Kachelkatalogs und eines leeren Rasters](./media/azure-portal-dashboards/dashboard-name.png)

5. Wählen Sie in der Dashboardbezeichnung den Text **Mein Dashboard** aus, und geben Sie einen Namen ein, mit dem Sie das benutzerdefinierte Dashboard leicht identifizieren können.
1. Wählen Sie **Anpassung abgeschlossen** in der Kopfzeile der Seite aus, um den Bearbeitungsmodus zu beenden.

Die Dashboardansicht zeigt nun Ihr leeres Dashboard an. Wählen Sie die Dropdownliste neben dem Dashboardnamen aus, um die für Sie verfügbaren Dashboards anzuzeigen – die Liste kann Dashboards enthalten, die andere Benutzer erstellt und freigegeben haben.

## <a name="edit-a-dashboard"></a>Bearbeiten eines Dashboards

Nun bearbeiten wir das Dashboard zum Hinzufügen, Ändern der Größe und Anordnen von Kacheln, die Ihre Azure-Ressourcen darstellen.

### <a name="add-tiles"></a>Hinzufügen von Kacheln

Führen Sie die folgenden Schritte aus, um einem Dashboard Kacheln hinzuzufügen:
1. Wählen Sie ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Bearbeiten** in der Kopfzeile der Seite aus.

    ![Screenshot des Dashboards der hervorgehobenen Bearbeitungsoption](./media/azure-portal-dashboards/dashboard-edit.png)

2. Durchsuchen Sie den **Kachelkatalog**, oder verwenden Sie das Suchfeld, um nach der gewünschten Kachel zu suchen.
1. Wählen Sie **Hinzufügen** aus, um die Kachel automatisch dem Dashboard mit einer Standardgröße und einer Standardposition hinzuzufügen. Sie können die Kachel auf das Raster ziehen und dann an der gewünschten Position platzieren.

Viele Ressourcenseiten (auch als „Blätter“ bezeichnet) enthalten ein Reißzweckensymbol in der Befehlsleiste. Wenn Sie das Symbol auswählen, wird eine Kachel, die die Quellseite darstellt, an das aktuell aktive Dashboard angeheftet. Diese Methode ist eine alternative Möglichkeit zum Hinzufügen von Kacheln zum Dashboard.

![Screenshot der Befehlsleiste der Seite mit Reißzweckensymbol](./media/azure-portal-dashboards/dashboard-pin-blade.png)

> [!TIP]
> Wenn Sie mit mehreren Organisationen arbeiten, fügen Sie dem Dashboard die Kachel **Organisationsidentität** hinzu, um die Organisation eindeutig anzuzeigen, zu der die Ressourcen gehören.
>
>
### <a name="resize-or-rearrange-tiles"></a>Ändern der Größe oder Neuanordnung von Kacheln
Führen Sie die folgenden Schritte aus, um die Größe einer Kachel zu ändern oder die Kacheln auf einem Dashboard neu anzuordnen:

1. Wählen Sie ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Bearbeiten** in der Kopfzeile der Seite aus.
1. Wählen Sie in der oberen rechten Ecke einer Kachel das Kontextmenü aus. Wählen Sie dann eine Kachelgröße aus. Kacheln, die eine beliebige Größe unterstützen, enthalten auch einen „Ziehpunkt“ in der unteren rechten Ecke, mit dem Sie die Kachel auf die gewünschte Größe ziehen können.

   ![Screenshot des Dashboards mit geöffneter Menü „Kachelgröße“](./media/azure-portal-dashboards/dashboard-tile-resize.png)

3. Wählen Sie eine Kachel aus, und ziehen Sie sie an eine neue Position im Raster, um das Dashboard anzuordnen.

### <a name="additional-tile-configuration"></a>Zusätzliche Kachelkonfiguration

Einige Kacheln benötigen möglicherweise weitere Konfiguration, um die gewünschten Informationen anzuzeigen. Beispielsweise muss die Kachel **Metrikdiagramm** so eingerichtet werden, dass eine Metrik aus **Azure Monitor** angezeigt wird. Sie können Kacheldaten auch anpassen, um die Standardzeiteinstellungen des Dashboards außer Kraft zu setzen.

Für alle Kacheln, die eingerichtet werden müssen, wird ein Banner **Kachel konfigurieren** angezeigt, bis Sie die Kachel anpassen. Wählen Sie dieses Banner aus, und nehmen Sie dann die erforderliche Einrichtung vor.

![Screenshot einer Kachel, die konfiguriert werden muss](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Mithilfe einer Markdownkachel können Sie benutzerdefinierte, statische Inhalte in Ihrem Dashboard anzeigen. Dabei kann es sich um grundlegende Anweisungen, ein Bild, eine Reihe von Links oder sogar um Kontaktinformationen handeln. Weitere Informationen zum Verwenden einer Markdownkachel finden Sie unter [Verwenden einer benutzerdefiniertem Markdownkachel](azure-portal-markdown-tile.md).
>
>
### <a name="customize-tile-data"></a>Anpassen von Kacheldaten

Die Daten im Dashboard zeigen automatisch Aktivitäten der letzten 24 Stunden an. Gehen Sie folgendermaßen vor, um eine andere Zeitspanne nur für diese Kachel anzuzeigen:

1. Wählen Sie in der oberen linken Ecke der Kachel das ![Filtersymbol](./media/azure-portal-dashboards/dashboard-filter.png) Filtersymbol aus, oder wählen Sie im Kontextmenü **Kacheldaten anpassen** aus.

   ![Screenshot des Kachelkontextmenüs](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

2. Aktivieren Sie das Kontrollkästchen zum **Außerkraftsetzen der Zeiteinstellungen für das Dashboard auf Kachelebene**.

   ![Screenshot des Dialogfelds zum Konfigurieren der Zeiteinstellungen der Kachel](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

3. Wählen Sie die Zeitspanne aus, die für diese Kachel angezeigt werden soll. Sie können zwischen den letzten 30 Minuten bis zu den letzten 30 Tagen wählen oder einen benutzerdefinierten Bereich definieren.
1. Wählen Sie die anzuzeigende Zeitgranularität aus. Sie können ein beliebiges Inkrement zwischen einer Minute und einem Monat anzeigen.
1. Wählen Sie **Übernehmen**.

## <a name="delete-a-tile"></a>Löschen einer Kachel

Um eine Kachel aus einem Dashboard zu entfernen, führen Sie die folgenden Schritte aus:

* Wählen Sie das Kontextmenü in der oberen rechten Ecke der Kachel aus, und wählen Sie dann **Aus Dashboard entfernen** aus. Oder
* Wählen Sie ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Bearbeiten** aus, um den Anpassungsmodus einzugeben. Platzieren Sie den Mauszeiger in der oberen rechten Ecke der Kachel, und wählen Sie dann das ![Löschsymbol](./media/azure-portal-dashboards/dashboard-delete-icon.png) Löschsymbol aus, um die Kachel aus dem Dashboard zu entfernen.

   ![Screenshot: Entfernen einer Kachel aus dem Dashboard](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Klonen eines Dashboards

Führen Sie die folgenden Schritte aus, um ein vorhandenes Dashboard als Vorlage für ein neues Dashboard zu verwenden:

1. Stellen Sie sicher, dass in der Dashboardansicht das Dashboard angezeigt wird, das Sie kopieren möchten.
1. Wählen Sie in der Kopfzeile der Seite ![Klonsymbol](./media/azure-portal-dashboards/dashboard-clone.png) **Klonen** aus.
1. Eine Kopie des Dashboards mit dem Namen „Klon von *Ihr Dashboardname*“ wird im Bearbeitungsmodus geöffnet. Verwenden Sie die weiter oben beschriebenen Schritte in diesem Artikel, um das Dashboard umzubenennen und anzupassen.

## <a name="publish-and-share-a-dashboard"></a>Veröffentlichen und Freigeben eines Dashboards

Wenn Sie ein Dashboard erstellen, ist es standardmäßig ein privates Dashboard. Dies bedeutet, dass Sie die einzige Person sind, die es anzeigen kann. Um Dashboards für andere Benutzer verfügbar zu machen, können Sie sie für andere Benutzer freigeben. Zunächst müssen Sie das Dashboard als Azure-Ressource veröffentlichen. Gehen Sie folgendermaßen vor, um ein benutzerdefiniertes Dashboard zu veröffentlichen und freizugeben:

1. Wählen Sie ![Freigabesymbol](./media/azure-portal-dashboards/dashboard-share-icon.png) **Freigeben** in der Kopfzeile der Seite aus. Das Formular **Freigabe und Zugriffssteuerung** wird angezeigt.
1. Vergewissern Sie sich, dass der richtige Dashboardname angezeigt wird.
1. Wählen Sie einen **Abonnementnamen** aus. Benutzer mit Zugriff auf das Abonnement können das freigegebene Dashboard verwenden. Der Zugriff auf die Ressourcen, die von den einzelnen Kacheln dargestellt werden, wird durch die rollenbasierte Zugriffssteuerung in Azure festgelegt.
1. Aktivieren Sie das Kontrollkästchen, um dieses Dashboard in der Ressourcengruppe „Dashboards“ für das ausgewählte Abonnement zu veröffentlichen. Oder deaktivieren Sie das Kontrollkästchen, und wählen Sie stattdessen die Veröffentlichung in einer vorhandenen Ressourcengruppe aus.
1. Wählen Sie einen Speicherort für die Dashboardressource aus. Es wird empfohlen, das Dashboard zusammen mit anderen Ressourcen zu speichern. Hinweis: Wenn Sie eine vorhandene Ressourcengruppe auswählen, wird das Dashboard automatisch mit dieser Ressourcengruppe gespeichert.
1. Wählen Sie **Veröffentlichen**.

   ![Screenshot des Dialogfelds zum Veröffentlichen des Dashboards](./media/azure-portal-dashboards/dashboard-publish.png)

### <a name="set-access-control-on-a-shared-dashboard"></a>Festlegen der Zugriffssteuerung für ein freigegebenes Dashboard

Nachdem das Dashboard veröffentlicht wurde, können Sie verwalten, wer Zugriff auf das Dashboard besitzt, indem Sie die folgenden Schritte ausführen:

1. Wählen Sie im Bereich **Freigabe und Zugriffssteuerung** die Option **Benutzer verwalten** aus.

   ![Screenshot des Dialogfelds „Dashboardfreigabe und Zugriffssteuerung“](./media/azure-portal-dashboards/dashboard-share-access-control.png)

2. Die Seite **Zugriffssteuerung** wird geöffnet. Auf dieser Seite können Sie die Zugriffsebene für eine Person überprüfen oder eine neue Rollenzuweisung hinzufügen. Wenn Sie hier eine Rollenzuweisung hinzufügen, erteilen Sie Berechtigungen für das Dashboard.

> [!NOTE]
> Kacheln sind repräsentative Ansichten von Ressourcen in Ihrer Organisation. Der Zugriff auf Ressourcen wird über die rollenbasierte Zugriffssteuerungszuweisung verwaltet, und Berechtigungen werden vom Abonnement an die Ressource vererbt. Wenn Sie den Zugriff auf ein Dashboard gewähren, werden für die im Dashboard angezeigten Ressourcen nicht automatisch Berechtigungen zugewiesen. Weitere Informationen zu Berechtigungen für freigegebene Dashboards und zur rollenbasierten Zugriffssteuerung für Ressourcen finden Sie unter [Freigeben von Dashboards mit rollenbasierter Zugriffssteuerung](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Öffnen eines freigegebenen Dashboards

Gehen Sie folgendermaßen vor, um ein freigegebenes Dashboard zu suchen und zu öffnen:

1. Wählen Sie die Dropdownliste neben dem Dashboardnamen aus.
1. Wählen Sie ein Dashboard aus der angezeigten Liste der Dashboards aus, oder **durchsuchen Sie alle Dashboards**, wenn das Dashboard, das Sie öffnen möchten, nicht aufgelistet ist.

   ![Screenshot des Menüs „Dashboardauswahl“](./media/azure-portal-dashboards/dashboard-browse.png)

3. Wählen Sie im Feld **Typ** die Option **Freigegebene Dashboards** aus.
1. Wählen Sie mindestens ein Abonnement aus. Sie können auch Text eingeben, um Dashboards nach Namen zu filtern.
1. Wählen Sie ein Dashboard aus der Liste der freigegebenen Dashboards aus.

## <a name="delete-a-dashboard"></a>Löschen eines Dashboards

Führen Sie die folgenden Schritte aus, um ein privates oder freigegebenes Dashboard dauerhaft zu löschen:

1. Wählen Sie das zu löschende Dashboard aus der Dropdownliste neben dem Namen des Dashboards aus.
1. Wählen Sie ![Löschsymbol](./media/azure-portal-dashboards/dashboard-delete-icon.png) **Löschen** in der Kopfzeile der Seite aus.
1. Wählen Sie für ein privates Dashboard im Bestätigungsdialogfeld **OK** aus, um das Dashboard zu entfernen. Aktivieren Sie für ein freigegebenes Dashboard im Bestätigungsdialogfeld das Kontrollkästchen, um zu bestätigen, dass das veröffentlichte Dashboard von anderen Benutzern nicht mehr angezeigt werden kann. Wählen Sie anschließend **OK** aus.

   ![Screenshot der Löschbestätigung](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Nächste Schritte

* [Freigeben von Dashboards mit rollenbasierter Zugriffssteuerung](azure-portal-dashboard-share-access.md)
* [Programmgesteuertes Erstellen von Azure-Dashboards](azure-portal-dashboards-create-programmatically.md)
