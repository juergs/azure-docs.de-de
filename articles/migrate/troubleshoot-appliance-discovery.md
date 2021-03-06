---
title: Behandeln von Problemen bei der Bereitstellung und Ermittlung der Azure Migrate-Appliance
description: Erhalten Sie Hilfe bei der Bereitstellung der Azure Migrate-Appliance und bei der Ermittlung von Computern.
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: d66e792b901742f903dccf7a0e7999db4d02e26a
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76289525"
---
# <a name="troubleshoot-the-azure-migrate-appliance-and-discovery"></a>Behandeln von Problemen bei der Azure Migrate-Appliance und der Ermittlung

Dieser Artikel hilft Ihnen bei der Behebung von Problemen bei der Bereitstellung der [Azure Migrate](migrate-services-overview.md)-Appliance und bei der Verwendung der Appliance zur Erkennung von lokalen Computern.


## <a name="whats-supported"></a>Was wird unterstützt?

[Überprüfen](migrate-appliance.md) Sie die Anforderungen an die Applianceunterstützung.


## <a name="invalid-ovf-manifest-entry"></a>„Ungültiger OVF-Manifesteintrag“

Wenn Sie den Fehler „Die angegebene Manifestdatei ist ungültig: Ungültiger OVF-Manifesteintrag“ erhalten, gehen Sie wie folgt vor:

1. Überprüfen Sie, ob die OVA-Datei für die Azure Migrate-Appliance ordnungsgemäß heruntergeladen wird, indem Sie deren Hashwert überprüfen. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware) Wenn der Hashwert nicht übereinstimmt, laden Sie die OVA-Datei erneut herunter, und wiederholen Sie die Bereitstellung.
2. Wenn immer noch Fehler bei der Bereitstellung auftreten und Sie den VMware vSphere-Client zum Bereitstellen der OVF-Datei verwenden, versuchen Sie, die Bereitstellung über den vSphere-Webclient vorzunehmen. Wenn die Bereitstellung weiterhin fehlschlägt, versuchen Sie es mit einem anderen Webbrowser.
3. Wenn Sie den vSphere-Webclient verwenden und die Bereitstellung auf vCenter Server 6.5 oder 6.7 ausführen möchten, versuchen Sie, die OVA-Datei direkt auf dem ESXi-Host bereitzustellen:
   - Stellen Sie mit dem Webclient (https://<*Host-IP-Adresse*>/ui) eine direkte Verbindung mit dem ESXi-Host her (anstelle von vCenter Server).
   - Wählen Sie unter **Startseite** > **Bestand** die Optionen **Datei** > **OVF-Vorlage bereitstellen** aus. Navigieren Sie zur OVA-Datei, und schließen Sie die Bereitstellung ab.
4. Wenn weiterhin ein Fehler bei der Bereitstellung auftritt, wenden Sie sich an den Azure Migrate-Support.

## <a name="cant-connect-to-the-internet"></a>Verbindung mit dem Internet kann nicht hergestellt werden

Dies kann der Fall sein, wenn sich der Appliancecomputer hinter einem Proxy befindet.

- Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
- Wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Verbindungen verwenden, fügen Sie die folgenden URLs einer Zulassungsliste hinzu:

    - [URLs für VMware-Bewertung](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)
    - [URLs für Hyper-V-Bewertung](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v)
    - [URLs für VMware-Migration ohne Agent](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)
    - [URLs für VMware-Migration mit Agent](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)
    - [URLs für Hyper-V-Migration](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v)

- Wenn Sie für die Internetverbindung einen abfangenden Proxy verwenden, importieren Sie das Proxyzertifikat [mit diesen Schritten](https://docs.microsoft.com/azure/migrate/concepts-collector) in die Appliance-VM.

##  <a name="datetime-synchronization-error"></a>Datums-/Zeitsynchronisierungsfehler

Ein Fehler bei der Datums- und Zeitsynchronisierung (Fehler 802) weist darauf hin, dass die Serveruhr möglicherweise um mehr als fünf Minuten von der aktuellen Uhrzeit abweicht. Passen Sie die Uhrzeit auf dem virtuellen Collector-Computer an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
2. Führen Sie **w32tm /tz** aus, um die Zeitzone zu überprüfen.
3. Führen Sie **w32tm /resync** aus, um die Zeit zu synchronisieren.


## <a name="unabletoconnecttoserver"></a>„UnableToConnectToServer“

Wenn dieser Verbindungsfehler angezeigt wird, können Sie möglicherweise keine Verbindung mit vCenter Server „*Servername*.com:9443“ herstellen. Die Fehlerdetails weisen darauf hin, dass unter „https://*Servername*.com:9443/sdk“ kein Endpunkt lauscht, der die Nachricht akzeptieren kann.

- Überprüfen Sie, ob Sie die neueste Version der Appliance ausführen. Ist dies nicht der Fall, führen Sie ein Upgrade der Appliance auf die [neueste Version](https://docs.microsoft.com/azure/migrate/concepts-collector) durch.
- Wenn das Problem bei der neuesten Version weiterhin auftritt, ist die Appliance möglicherweise nicht in der Lage, den angegebenen vCenter Server-Namen aufzulösen, oder der angegebene Port ist eventuell falsch. Wenn der Port nicht angegeben ist, versucht Collector standardmäßig, eine Verbindung mit Port 443 herzustellen.

    1. Versuchen Sie, „*Servername*.com“ von der Appliance aus per Ping zu erreichen.
    2. Wenn Schritt 1 nicht erfolgreich ist, können Sie versuchen, eine Verbindung mit vCenter Server über die IP-Adresse herzustellen.
    3. Ermitteln Sie die richtige Portnummer für die Verbindung mit vCenter Server.
    4. Überprüfen Sie, ob vCenter Server ausgeführt wird und betriebsbereit ist.


## <a name="error-6005260039-appliance-might-not-be-registered"></a>Fehler 60052/60039: Die Appliance ist möglicherweise nicht registriert

- Der Fehler 60052 „Möglicherweise wurde die Appliance nicht erfolgreich beim Azure Migrate-Projekt registriert“ tritt auf, wenn das zum Registrieren der Appliance verwendete Azure-Konto nicht über ausreichende Berechtigungen verfügt.
    - Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt.
    - Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware).
- Der Fehler 60039 „Möglicherweise wurde die Appliance nicht erfolgreich beim Azure Migrate-Projekt registriert" kann auftreten, wenn die Registrierung fehlschlägt, da das zum Registrieren der Appliance verwendete Azure Migrate Projekt nicht gefunden werden kann.
    - Überprüfen Sie im Azure-Portal, ob das Projekt in der Ressourcengruppe vorhanden ist.
    - Sollte das Projekt nicht vorhanden sein, erstellen Sie in Ihrer Ressourcengruppe ein neues Azure Migrate-Projekt, und registrieren Sie die Appliance erneut. Informationen zum Erstellen eines neuen Projekts finden Sie [hier](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool).

## <a name="error-6003060031-key-vault-management-operation-failed"></a>Fehler 60030/60031: Ein Key Vault-Verwaltungsvorgang war nicht erfolgreich

Wenn der Fehler 60030 oder 60031 „Ein Azure Key Vault-Verwaltungsvorgang war nicht erfolgreich“ auftritt, gehen Sie wie folgt vor:
- Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt.
- Vergewissern Sie sich, dass das Konto Zugriff auf den in der Fehlermeldung angegebenen Key Vault hat, und wiederholen Sie dann den Vorgang.
- Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.
- Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware).

## <a name="error-60028-discovery-couldnt-be-initiated"></a>Fehler 60028: Die Ermittlung konnte nicht initiiert werden

Fehler 60028: „Die Ermittlung konnte aufgrund eines Fehlers nicht initiiert werden. Der Vorgang war für die angegebene Liste von Hosts oder Clustern nicht erfolgreich“. Dieser Fehler weist darauf hin, dass die Ermittlung aufgrund eines Problems beim Zugreifen auf oder Abrufen von VM-Informationen auf den in der Fehlermeldung aufgelisteten Hosts nicht gestartet werden konnte. Die übrigen Hosts wurden erfolgreich hinzugefügt.

- Fügen Sie die in der Fehlermeldung angegebenen Hosts mithilfe der Option **Host hinzufügen** erneut hinzu.
- Gehen Sie im Falle eines Überprüfungsfehlers gemäß der Korrekturanleitung vor, um den Fehler zu beheben, und verwenden Sie dann erneut die Option **Speichern und Ermittlung starten**.

## <a name="error-60025-azure-ad-operation-failed"></a>Fehler 60025: Fehler bei einem Azure AD-Vorgang 
Fehler 60025: „Fehler bei einem Azure AD-Vorgang. Der Fehler trat beim Erstellen oder Aktualisieren der Azure AD-Anwendung auf“. Dieser Fehler tritt auf, wenn zum Initiieren der Ermittlung ein anderes Azure-Benutzerkonto als für die Registrierung der Appliance verwendet wird. Führen Sie eines der folgenden Verfahren aus:

- Stellen Sie sicher, dass zum Initiieren der Ermittlung dasselbe Benutzerkonto verwendet wird wie zum Registrieren der Appliance.
- Weisen Sie dem Benutzerkonto, bei dem der Ermittlungsvorgang fehlgeschlagen ist, Zugriffsberechtigungen für die Azure Active Directory-Anwendung zu.
- Löschen Sie die Ressourcengruppe, die zuvor für das Azure Migrate-Projekt erstellt wurde. Erstellen Sie eine andere Ressourcengruppe, um neu zu beginnen.
- Weitere Informationen zu den Azure Active Directory-Anwendungsberechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware).


## <a name="error-50004-cant-connect-to-host-or-cluster"></a>Fehler 50004: Verbindung mit Host oder Cluster nicht möglich

Fehler 50004: „Es ist keine Verbindung mit einem Host oder Cluster möglich, da der Servername nicht aufgelöst werden kann. WinRM-Fehlercode: 0x803381B9“. Dieser Fehler tritt auf, wenn der Azure DNS-Dienst für die Appliance den von Ihnen angegebenen Cluster- oder Hostnamen nicht auflösen kann.

- Wenn Sie diesen Fehler auf dem Cluster sehen, versuchen Sie, den vollqualifizierten Domänennamen (FQDN) des Clusters anzugeben.
- Sie sehen diesen Fehler eventuell auch für Hosts in einem Cluster. In diesem Fall kann die Appliance zwar eine Verbindung mit dem Cluster herstellen, doch gibt der Cluster Hostnamen zurück, bei denen es sich nicht um FQDNs handelt. Um diesen Fehler zu beheben, aktualisieren Sie die „hosts“-Datei auf der Appliance und fügen Sie ein Mapping von IP-Adresse und Hostnamen hinzu:
    1. Öffnen Sie Notepad als Administrator.
    2. Öffnen Sie die Datei „C:\Windows\System32\Drivers\etc\hosts“.
    3. Fügen Sie die IP-Adresse und den Hostnamen in einer Zeile hinzu. Wiederholen Sie dies für jeden Host oder Cluster, bei dem dieser Fehler angezeigt wird.
    4. Speichern und schließen Sie die Datei „hosts“.
    5. Überprüfen Sie mithilfe der Applianceverwaltungs-App, ob die Appliance eine Verbindung mit den Hosts herstellen kann. Nach 30 Minuten sollten die neuesten Informationen für diese Hosts im Azure-Portal angezeigt werden.

## <a name="discovered-vms-not-in-portal"></a>Im Portal befinden sich keine ermittelten virtuellen Computer

Wenn der Erkennungszustand „Erkennung wird ausgeführt“ lautet, die virtuellen Computer im Portal aber noch nicht angezeigt werden, warten Sie einige Minuten:
- Es dauert etwa 15 Minuten für eine VMware-VM.
- Es dauert etwa zwei Minuten für jeden hinzugefügten Host für die Hyper-V-VM-Erkennung.

Wenn Sie warten und der Zustand sich nicht ändert, wählen Sie **Aktualisieren** auf der Registerkarte **Server** aus. Dadurch sollte in Azure Migrate die Anzahl der ermittelten Server angezeigt werden: Serverbewertung und Azure Migrate: Servermigration.

Wenn dies nicht funktioniert und Sie VMware-Server erkennen:

- Stellen Sie sicher, dass das von Ihnen angegebene vCenter-Konto über ordnungsgemäß festgelegte Berechtigungen mit Zugriff auf mindestens einen virtuellen Computer verfügt.
- Azure Migrate kann keine VMware-VMs erkennen, wenn dem vCenter-Konto auf Ebene des vCenter-VM-Ordners Zugriff gewährt wird. [Erfahren Sie mehr](tutorial-assess-vmware.md#set-the-scope-of-discovery) über das Beschränken der Ermittlung.

## <a name="vm-data-not-in-portal"></a>VM-Daten nicht im Portal

Wenn ermittelte VMs nicht im Portal angezeigt werden, warten Sie einige Minuten. Es dauert bis zu 30 Minuten, bis die ermittelten Daten im Portal angezeigt werden. Wenn nach 30 Minuten keine Daten vorhanden sind, versuchen Sie, die Daten wie folgt zu aktualisieren:

1. Wechseln Sie zu **Server** > **Azure Migrate-Serverbewertung**, und wählen Sie **Übersicht** aus.
2. Wählen Sie unter **Verwalten** die Option **Agent-Integritätsdiagnose** aus.
3. Wählen Sie **Agent aktualisieren** aus.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen sehen.

## <a name="deleted-vms-appear-in-portal"></a>Gelöschte VMs werden im Portal angezeigt

Wenn Sie virtuelle Computer löschen und sie weiterhin im Portal angezeigt werden, warten Sie 30 Minuten. Wenn sie weiterhin angezeigt werden, aktualisieren Sie sie wie oben beschrieben.

## <a name="common-app-discovery-errors"></a>Häufige Fehler bei der App-Ermittlung

Azure Migrate unterstützt die Erkennung von Anwendungen, Rollen und Features mithilfe von Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. Die App-Ermittlung wird derzeit nur für VMware unterstützt. [Erfahren Sie mehr](how-to-discover-applications.md) über die Anforderungen und Schritte bei der Einrichtung der App-Ermittlung.

Typische Fehler bei der App-Ermittlung sind in der Tabelle zusammengefasst.

**Fehler** | **Ursache** | **Aktion**
--- | --- | --- | ---
10000: „Die auf dem Server installierten Anwendungen können nicht ermittelt werden“. | Dies kann vorkommen, wenn das Betriebssystem des Computers nicht Windows oder Linux ist. | Verwenden Sie nur die App-Ermittlung für Windows/Linux.
10001: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Interner Fehler – in der Appliance fehlen einige Dateien. | Wenden Sie sich an den Microsoft Support.
10002: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Der Discovery Agent auf der Appliance funktioniert möglicherweise nicht ordnungsgemäß. | Wenn sich das Problem nicht innerhalb von 24 Stunden von selbst löst, wenden Sie sich an den Support.
10003: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Der Discovery Agent auf der Appliance funktioniert möglicherweise nicht ordnungsgemäß. | Wenn sich das Problem nicht innerhalb von 24 Stunden von selbst löst, wenden Sie sich an den Support.
10004: „Installierte Anwendungen für <Windows-/Linux->Computer können nicht ermittelt werden“. |  In der Appliance wurden keine Anmeldeinformationen für den Zugriff auf <Windows-/Linux->Computer angegeben.| Fügen Sie der Appliance entsprechende Anmeldeinformationen mit Zugriff auf die <Windows-/Linux->Computer hinzu.
10005: „Der Zugriff auf den lokalen Server ist nicht möglich“. | Die Anmeldeinformationen für den Zugriff könnten falsch sein. | Aktualisieren Sie die Anmeldeinformationen für die Appliance, um sicherzustellen, dass Sie mit ihnen auf den betreffenden Computer zugreifen können. 
10006: „Der Zugriff auf den lokalen Server ist nicht möglich“. | Dies kann vorkommen, wenn das Betriebssystem des Computers nicht Windows oder Linux ist.|  Verwenden Sie nur die App-Ermittlung für Windows/Linux.
9000/9001/9002: „Die auf dem Server installierten Anwendungen können nicht ermittelt werden“. | Die VMware-Tools sind möglicherweise nicht installiert oder beschädigt. | Installieren Sie die VMware-Tools auf dem betreffenden Computer, oder installieren Sie diese Tools erneut. Überprüfen Sie dann, ob sie funktionieren.
9003: „Die auf dem Server installierten Anwendungen können nicht ermittelt werden“. | Dies kann vorkommen, wenn das Betriebssystem des Computers nicht Windows oder Linux ist. | Verwenden Sie nur die App-Ermittlung für Windows/Linux.
9004: „Die auf dem Server installierten Anwendungen können nicht ermittelt werden“. | Dies kann vorkommen, wenn die VM ausgeschaltet ist. | Stellen Sie für die Ermittlung sicher, dass die VM eingeschaltet ist.
9005: „Die auf der VM installierten Anwendungen können nicht ermittelt werden“. | Dies kann vorkommen, wenn das Betriebssystem des Computers nicht Windows oder Linux ist. | Verwenden Sie nur die App-Ermittlung für Windows/Linux.
9006/9007: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Der Discovery Agent auf der Appliance funktioniert möglicherweise nicht ordnungsgemäß. | Wenn sich das Problem nicht innerhalb von 24 Stunden von selbst löst, wenden Sie sich an den Support.
9008: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Das könnte ein interner Fehler sein.  | Wenn sich das Problem nicht innerhalb von 24 Stunden von selbst löst, wenden Sie sich an den Support.
9009: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Dies kann auftreten, wenn die Einstellungen der Windows-Benutzerkontensteuerung (UAC) auf dem Server restriktiv sind und die Ermittlung installierter Anwendungen verhindern. | Suchen Sie auf dem Server nach den Einstellungen für die Benutzerkontensteuerung, und konfigurieren Sie die UAC-Einstellung auf dem Server so, dass eine der beiden niedrigeren Stufen eingestellt ist.
9010: „Die auf dem Server installierten Anwendungen können nicht abgerufen werden“. | Das könnte ein interner Fehler sein.  | Wenn sich das Problem nicht innerhalb von 24 Stunden von selbst löst, wenden Sie sich an den Support.
8084: „Anwendungen können aufgrund eines VMware-Fehlers nicht ermittelt werden:  <Exception from VMware>“ | Die Azure Migrate-Appliance verwendet VMware-APIs zum Ermitteln von Anwendungen. Das Problem kann aufgrund einer Ausnahme auftreten, die von vCenter Server beim Versuch der Ermittlung von Anwendungen ausgelöst wurde. Die Fehlermeldung von VMware wird in der Fehlermeldung im Portal angezeigt. | Suchen Sie nach der Nachricht in der [VMware-Dokumentation](https://pubs.vmware.com/vsphere-51/topic/com.vmware.wssdk.apiref.doc/index-faults.html), und folgen Sie den Schritten zur Behebung. Wenn Sie das Problem nicht beheben können, wenden Sie sich an den Microsoft-Support.




## <a name="next-steps"></a>Nächste Schritte
Richten Sie eine Appliance für [VMware](how-to-set-up-appliance-vmware.md), [Hyper-V](how-to-set-up-appliance-hyper-v.md) oder [physische Server](how-to-set-up-appliance-physical.md) ein.
