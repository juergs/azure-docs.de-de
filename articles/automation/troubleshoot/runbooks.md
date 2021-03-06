---
title: Beheben von Fehlern bei Azure Automation-Runbooks
description: Erfahren Sie, wie Sie Probleme beheben, die bei Azure Automation-Runbooks auftreten können.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 10152087b45a4048f30f382b237017efbbb63787
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769879"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Beheben von Fehlern bei Runbooks

Wenn beim Ausführen von Runbooks in Azure Automation Fehler auftreten, können Sie die folgenden Schritte ausführen, um das Problem zu diagnostizieren.

1. **Stellen Sie sicher, dass Ihr Runbook-Skript auf Ihrem lokalen Computer erfolgreich ausgeführt wird:**  Sprachreferenzinformationen und Lernmodule finden Sie in der [PowerShell-Dokumentation](/powershell/scripting/overview) oder der [Python-Dokumentation](https://docs.python.org/3/).

   Mit der lokalen Ausführung Ihres Skripts können Sie häufige Fehler erkennen und beheben, z. B.:

   - **Fehlende Module**
   - **Syntaxfehler**
   - **Logikfehler**

2. **Untersuchen Sie Rundbook**-[Fehlerdatenströme](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#runbook-output) auf bestimmte Nachrichten, und vergleichen Sie sie mit den folgenden Fehlern.

3. **Stellen Sie sicher, dass Ihre Knoten und der Automation-Arbeitsbereich über die erforderlichen Module verfügen:** Wenn Ihr Runbook Module importiert, stellen Sie sicher, dass sie in Ihrem Automatisierungskonto vorhanden sind, indem Sie die unter [Importieren von Modulen](../shared-resources/modules.md#import-modules) aufgeführten Schritte ausführen. Aktualisieren Sie Ihre Module auf die neueste Version, indem Sie die Anleitungen unter [Aktualisieren von Azure-Modulen in Azure Automation](..//automation-update-azure-modules.md) befolgen. Weitere Problembehandlungsinformationen finden Sie unter [Problembehandlung von Modulen](shared-resources.md#modules).

Wenn das Runbook angehalten oder unerwartet fehlgeschlagen ist

* Unter [Auftragsstatuswerte prüfen](https://docs.microsoft.com/azure/automation/automation-runbook-execution#job-statuses) werden einige Statuswerte für Runbooks sowie einige mögliche Ursachen beschrieben.
* [Fügen Sie dem Runbook eine zusätzliche Ausgabe hinzu](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#message-streams), um zu ermitteln, was passiert, bevor das Runbook angehalten wird.
* [Behandeln Sie alle Ausnahmen](https://docs.microsoft.com/azure/automation/automation-runbook-execution#handling-exceptions), die von Ihrem Auftrag ausgelöst werden.

## <a name="login-azurerm"></a>Szenario: Ausführen von Login-AzureRMAccount zum Anmelden

### <a name="issue"></a>Problem

Beim Ausführen eines Runbooks wird die folgende Fehlermeldung angezeigt:

```error
Run Login-AzureRMAccount to login.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann auftreten, wenn Sie kein RunAs-Konto verwenden oder wenn das RunAs-Konto abgelaufen ist. Informationen finden Sie unter [Verwalten von „RunAs“-Konten in Azure Automation](https://docs.microsoft.com/azure/automation/manage-runas-account).

Dieser Fehler hat zwei Hauptursachen:

* Verschiedene Versionen von AzureRM-Modulen.
* Sie versuchen, auf Ressourcen in einem separaten Abonnement zuzugreifen.

### <a name="resolution"></a>Lösung

Wenn diese Fehlermeldung nach dem Aktualisieren eines AzureRM-Moduls angezeigt wird, sollten Sie alle Ihre AzureRM-Module auf die gleiche Version aktualisieren.

Wenn Sie versuchen, auf Ressourcen in einem anderen Abonnement zuzugreifen, können Sie die folgenden Schritte ausführen, um die Berechtigungen zu konfigurieren.

1. Wechseln Sie zum ausführenden Konto des Automation-Kontos, und kopieren Sie die Anwendungs-ID und den Fingerabdruck.
  ![Kopieren der Anwendungs-ID und des Fingerabdrucks](../media/troubleshoot-runbooks/collect-app-id.png)
1. Wechseln Sie zur Zugriffssteuerung des Abonnements, in dem das Automation-Konto NICHT gehostet wird, und fügen Sie eine neue Rollenzuweisung hinzu.
  ![Zugriffssteuerung](../media/troubleshoot-runbooks/access-control.png)
1. Fügen Sie die Anwendungs-ID hinzu, die Sie im vorherigen Schritt kopiert haben. Wählen Sie die Berechtigungen für „Mitwirkender“ aus.
   ![Hinzufügen der Rollenzuweisung](../media/troubleshoot-runbooks/add-role-assignment.png)
1. Kopieren Sie den Namen des Abonnements für den nächsten Schritt.
1. Sie können nun den folgenden Runbookcode verwenden, um die Berechtigungen Ihres Automation-Kontos für das andere Abonnement zu testen.

    Ersetzen Sie \<CertificateThumbprint\> durch den Wert, den Sie in Schritt 1 kopiert haben, und \<SubscriptionName\> durch den Wert, den Sie in Schritt 4 kopiert haben.

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint "<CertificateThumbprint>"
    #Select the subscription you want to work with
    Select-AzureRmSubscription -SubscriptionName '<YourSubscriptionNameGoesHere>'

    #Test and get outputs of the subscriptions you granted access.
    $subscriptions = Get-AzureRmSubscription
    foreach($subscription in $subscriptions)
    {
        Set-AzureRmContext $subscription
        Write-Output $subscription.Name
    }
    ```

## <a name="unable-to-find-subscription"></a>Szenario: Azure-Abonnement nicht gefunden

### <a name="issue"></a>Problem

Beim Arbeiten mit den Cmdlets `Select-AzureSubscription` oder `Select-AzureRmSubscription` wird der folgende Fehler angezeigt:

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>Fehler

Mögliche Ursachen für diesen Fehler:

* Der Abonnementname ist ungültig

* Der Azure Active Directory-Benutzer, der die Abonnementdetails abrufen möchte, ist nicht als Administrator des Abonnements konfiguriert.

### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um zu ermitteln, ob die Authentifizierung in Azure erfolgt ist und ob Sie auf das gewünschte Abonnement zugreifen können:

1. Testen Sie Ihr Skript außerhalb von Azure Automation, um sicherzustellen, dass es eigenständig verwendet werden kann.
2. Achten Sie darauf, dass Sie das Cmdlet `Add-AzureAccount` vor dem Cmdlet `Select-AzureSubscription` ausführen.
3. Fügen Sie am Anfang Ihres Runbooks `Disable-AzureRmContextAutosave –Scope Process` hinzu. Dieses Cmdlet stellt sicher, dass alle Anmeldeinformationen nur für die Ausführung des aktuellen Runbooks gelten.
4. Wenn diese Fehlermeldung weiterhin angezeigt wird, sollten Sie Ihren Code ändern, indem Sie den Parameter **AzureRmContext** nach dem Cmdlet `Add-AzureAccount` hinzufügen und dann den Code ausführen.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

## <a name="auth-failed-mfa"></a>Szenario: Fehler beim Authentifizieren bei Azure, da die mehrstufige Authentifizierung aktiviert ist

### <a name="issue"></a>Problem

Sie erhalten bei der Authentifizierung bei Azure mit Ihrem Azure-Benutzernamen und -Kennwort die folgende Fehlermeldung:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>Ursache

Falls für Ihr Azure-Konto die mehrstufige Authentifizierung eingerichtet ist, können Sie für die Authentifizierung bei Azure keinen Azure Active Directory-Benutzer verwenden. Stattdessen müssen Sie ein Zertifikat oder einen Dienstprinzipal für die Authentifizierung gegenüber Azure verwenden.

### <a name="resolution"></a>Lösung

Informationen zum Verwenden eines Zertifikats mit den klassischen Azure-Bereitstellungsmodell finden Sie unter [Managing Azure Services with the Microsoft Azure Automation Preview Service](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) (Verwalten von Azure Services mit dem Microsoft Azure Automation Preview-Dienst). Informationen zum Verwenden eines Dienstprinzipals mit Azure Resource Manager-Cmdlets finden Sie unter [Erstellen eines Dienstprinzipals mit dem Azure-Portal](../../active-directory/develop/howto-create-service-principal-portal.md) und [Authentifizieren eines Dienstprinzipals mit dem Azure Resource Manager](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="get-serializationsettings"></a>Szenario: In Ihren Auftragsdatenströmen wird ein Fehler zur get_SerializationSettings-Methode angezeigt

### <a name="issue"></a>Problem

Sie sehen einen Fehler in Ihren Auftragsdatenströmen für ein Runbook mit der folgenden Nachricht:

```error
Connect-AzureRMAccount : Method 'get_SerializationSettings' in type
'Microsoft.Azure.Management.Internal.Resources.ResourceManagementClient' from assembly
'Microsoft.Azure.Commands.ResourceManager.Common, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
does not have an implementation.
At line:16 char:1
+ Connect-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Connect-AzureRmAccount], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

### <a name="cause"></a>Ursache

Dieser Fehler wird durch die kombinierte Verwendung von AzureRM- und Az-Cmdlets in einem Runbook verursacht. Er tritt auf, wenn Sie `Az` vor dem Importieren von `AzureRM` importieren.

### <a name="resolution"></a>Lösung

Az- und AzureRM-Cmdlets können nicht im gleichen Runbook importiert und verwendet werden. Weitere Informationen zur Az-Unterstützung in Azure Automation finden Sie unter [Unterstützung für Az-Module in Azure Automation](../az-modules.md).

## <a name="task-was-cancelled"></a>Szenario: Beim Runbook tritt folgender Fehler auf: Eine Aufgabe wurde abgebrochen.

### <a name="issue"></a>Problem

Für Ihr Runbook tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt:

```error
Exception: A task was canceled.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann durch veraltete Azure-Module verursacht werden.

### <a name="resolution"></a>Lösung

Dieser Fehler kann behoben werden, indem Sie Ihre Azure-Module auf die neueste Version aktualisieren.

Klicken Sie in Ihrem Automation-Konto auf **Module** und anschließend auf **Azure-Module aktualisieren**. Das Update dauert ungefähr 15 Minuten. Führen Sie anschließend das Runbook, bei dem der Fehler aufgetreten ist, erneut aus. Weitere Informationen zum Aktualisieren Ihrer Module finden Sie unter [Aktualisieren von Azure-Modulen in Azure Automation](../automation-update-azure-modules.md).

## <a name="runbook-auth-failure"></a>Szenario: Fehler bei Runbooks, wenn mehrere Abonnements verwendet werden

### <a name="issue"></a>Problem

Beim Ausführen von Runbooks mit kann das Runbook Azure-Ressourcen nicht verwalten.

### <a name="cause"></a>Ursache

Das Runbook verwendet beim Ausführen nicht den richtigen Kontext.

### <a name="resolution"></a>Lösung

Wenn Sie mit mehreren Abonnements arbeiten, geht beim Aufrufen von Runbooks unter Umständen der Abonnementkontext verloren. Um sicherzustellen, dass der Abonnementkontext an die Runbooks übergeben wird, fügen Sie dem Cmdlet den Parameter `AzureRmContext` hinzu und übergeben darin den Kontext. Außerdem wird empfohlen, das Cmdlet `Disable-AzureRmContextAutosave` mit dem Bereich **Prozess** zu verwenden. So wird sichergestellt, dass die von Ihnen genutzten Anmeldeinformationen nur für das aktuelle Runbook verwendet werden.

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

Weitere Informationen finden Sie unter [Arbeiten mit mehreren Abonnements](../automation-runbook-execution.md#working-with-multiple-subscriptions).

## <a name="not-recognized-as-cmdlet"></a>Szenario: Die Benennung wird nicht als Name eines Cmdlets, einer Funktion oder eines Skripts erkannt.

### <a name="issue"></a>Problem

Für Ihr Runbook tritt ein Fehler auf, der dem im folgenden Beispiel ähnelt:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* Das Modul, das das Cmdlet enthält, wird nicht in das Automationkonto importiert.
* Das Modul, das das Cmdlet enthält, wird zwar importiert, ist jedoch veraltet.

### <a name="resolution"></a>Lösung

Dieser Fehler kann behoben werden, indem Sie eine der folgenden Aufgaben ausführen:

Wenn es sich bei dem Modul um ein Azure-Modul handelt, finden Sie weitere Informationen zum Aktualisieren Ihrer Module in Ihrem Automation-Konto unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md).

Wenn es ein separates Modul ist, sollten Sie sicherstellen, dass es in Ihr Automationkonto importiert wird.

## <a name="job-attempted-3-times"></a>Szenario: Es wurde dreimal erfolglos versucht, den Runbookauftrag zu starten

### <a name="issue"></a>Problem

Ihr Runbook schlägt mit dem folgenden Fehler fehl:

```error
The job was tried three times but it failed
```

### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* Arbeitsspeicherlimit. Die Dokumentation der Grenzwerte, die für das Zuordnen von Arbeitsspeicher zu einer Sandbox gelten, finden Sie unter [Automatisierungsgrenzwerte](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits). Für einen Auftrag kann ein Fehler auftreten, wenn dafür mehr als 400 MB Arbeitsspeicher verwendet werden.

* Netzwerksockets. Azure-Sandboxes sind auf 1.000 gleichzeitige Netzwerksockets beschränkt, wie unter [Automatisierungsgrenzwerte](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) beschrieben.

* Modul inkompatibel. Dieser Fehler kann auftreten, wenn Modulabhängigkeiten nicht korrekt sind. Wenn dies der Fall ist, gibt Ihr Runbook in der Regel die Benachrichtigung „Befehl wurde nicht gefunden“ oder „Der Parameter kann nicht gebunden werden“.

* Ihr Runbook versucht, eine ausführbare Datei oder einen Unterprozess in einem Runbook aufzurufen, das in einer Azure-Sandbox ausgeführt wird. Dieses Szenario wird in Azure-Sandboxes nicht unterstützt.

* Vom Runbook wurden zu viele Ausnahmedaten in den Ausgabestream geschrieben.

### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Die empfohlenen Methoden für die Einhaltung des Arbeitsspeichergrenzwerts sind die Aufteilung der Workload zwischen mehreren Runbooks und die Verarbeitung von weniger Daten im Arbeitsspeicher, die Vermeidung des Schreibens von unnötigen Ausgaben aus den Runbooks oder die Ermittlung, wie viele Prüfpunkte Sie in Ihre PowerShell-Workflow-Runbooks schreiben. Sie können die clear-Methode verwenden, z.B. `$myVar.clear()`, um die Variable zu löschen, und dann `[GC]::Collect()` verwenden, um sofort die Garbage Collection durchzuführen. Durch diese Aktionen wird der Speicherbedarf Ihres Runbooks während der Laufzeit reduziert.

* Aktualisieren Sie Ihre Azure-Module, indem Sie die Schritte unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) befolgen.

* Eine andere Lösung ist das Ausführen des Runbooks in einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Hybrid Worker unterliegen nicht den vom Arbeitsspeicher und Netzwerk vorgegebenen Grenzwerten, die für Azure-Sandboxes gelten.

* Wenn Sie einen Prozess (z.B. .exe oder subprocess.call) in einem Runbook aufrufen müssen, müssen Sie das Runbook auf einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md) ausführen.

* Für den Auftragsausgabestream gilt ein Grenzwert von 1 MB. Achten Sie darauf, Aufrufe einer ausführbaren Datei oder eines Teilprozesses in einen Try/Catch-Block einzuschließen. Wenn diese eine Ausnahme auslösen, schreiben Sie den Meldungstext dieser Ausnahme in eine Automation-Variable. Dadurch wird verhindert, dass sie in den Auftragsausgabestream geschrieben wird.

## <a name="sign-in-failed"></a>Szenario: Fehler beim Anmelden beim Azure-Konto

### <a name="issue"></a>Problem

Beim Arbeiten mit den Cmdlets `Add-AzureAccount` oder `Connect-AzureRmAccount` wird einer der folgenden Fehler angezeigt:

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn der Objektname für die Anmeldeinformationen ungültig ist. Er kann auch auftreten, wenn Benutzername und Kennwort, die Sie zur Einrichtung des Automation-Anmeldeinformationsobjekts verwendet haben, nicht gültig sind.

### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um zu ermitteln, wo der Fehler liegt:

1. Stellen Sie sicher, dass Sie keine Sonderzeichen eingegeben haben. Dazu zählt auch das **\@** -Zeichen im Namen des Automation-Anmeldeinformationsobjekts, mit dem Sie die Verbindung zu Azure herstellen.
2. Überprüfen Sie, ob Sie den Benutzernamen und das Kennwort aus den Azure Automation-Anmeldeinformationen in Ihrem lokalen PowerShell ISE-Editor verwenden können. Sie können überprüfen, ob Benutzername und Kennwort korrekt sind, indem Sie die folgenden Cmdlets in der PowerShell ISE ausführen:

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred
   #Using Azure Resource Manager
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Wenn die Authentifizierung lokal fehlschlägt, bedeutet dies, dass Sie Ihre Azure Active Directory-Anmeldeinformationen nicht richtig eingerichtet haben. Informationen zur richtigen Einrichtung des Azure Active Directory-Kontos finden Sie im Blogbeitrag [Authenticating to Azure using Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) (Authentifizieren in Azure mit Azure Active Directory).

4. Wenn es sich um einen vorübergehenden Fehler zu handeln scheint, versuchen Sie, eine Wiederholungslogik zu Ihrer Authentifizierungsroutine hinzuzufügen, um die Authentifizierung zuverlässiger zu gestalten.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       #Logging in to Azure...
       $connectionResult = Connect-AzureRmAccount `
                              -ServicePrincipal `
                              -TenantId $servicePrincipalConnection.TenantId `
                              -ApplicationId $servicePrincipalConnection.ApplicationId `
                              -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

## <a name="child-runbook-object"></a>Der Objektverweis wurde nicht auf eine Objektinstanz festgelegt.

### <a name="issue"></a>Problem

Die folgende Fehlermeldung wird angezeigt, wenn Sie ein untergeordnetes Runbook mit dem `-Wait`-Switch aufrufen und der Ausgabestream ein Objekt enthält:

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>Ursache

Es gibt ein bekanntes Problem, bei dem [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) den Ausgabestream nicht ordnungsgemäß behandelt, wenn er Objekte enthält.

### <a name="resolution"></a>Lösung

Implementieren Sie eine Abruflogik, und verwenden Sie das Cmdlet [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput), um die Ausgabe abzurufen. Nachfolgend sehen Sie ein Beispiel für diese Logik.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

## <a name="fails-deserialized-object"></a>Szenario: Runbookfehler aufgrund eines deserialisierten Objekts

### <a name="issue"></a>Problem

Ihr Runbook schlägt mit dem folgenden Fehler fehl:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>Ursache

Wenn es sich bei Ihrem Runbook um einen PowerShell-Workflow handelt, werden komplexe Objekte in einem deserialisierten Format gespeichert, um den Runbookstatus beizubehalten, sobald der Workflow angehalten wird.

### <a name="resolution"></a>Lösung

Sie können dieses Problem mit einer der folgenden drei Lösungen beheben:

* Wenn Sie komplexe Objekte von einem Cmdlet an ein anderes übergeben, sollten Sie diese Cmdlets mit einem InlineScript umschließen.
* Übergeben Sie den Namen oder Wert, den Sie aus dem komplexen Objekt benötigen, anstatt das gesamte Objekt zu übergeben.
* Verwenden Sie anstelle eines PowerShell-Workflow-Runbooks ein PowerShell-Runbook.

## <a name="quota-exceeded"></a>Szenario: Fehler beim Runbookauftrag, da das zugeordnete Kontingent überschritten wurde

### <a name="issue"></a>Problem

Ihr Runbookauftrag schlägt mit dem folgenden Fehler fehl:

```error
The quota for the monthly total job run time has been reached for this subscription
```

### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn die Auftragsausführung das kostenlose Kontingent von 500 Minuten für Ihr Konto überschreitet. Dieses Kontingent gilt für alle Arten von Auftragsausführungsaufgaben. Zu diesen Aufgaben zählen: Testen eines Auftrags, Starten eines Auftrags im Portal, Ausführen eines Auftrags per Webhook und Planen der Ausführung eines Auftrags per Azure-Portal oder in Ihrem Rechenzentrum. Weitere Informationen zu den Preisen für Automation finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).

### <a name="resolution"></a>Lösung

Wenn Sie mehr als 500 Minuten an Verarbeitungszeit pro Monat nutzen möchten, müssen Sie Ihr Abonnement vom Tarif „Free“ auf den Tarif „Basic“ umstellen. Sie können das Upgrade auf den Tarif „Basic“ mit den folgenden Schritten durchführen:

1. Melden Sie sich bei Ihrem Azure-Abonnement an.
2. Wählen Sie das Automation-Konto aus, das Sie aktualisieren möchten.
3. Klicken Sie auf **Einstellungen** > **Preise**.
4. Klicken Sie am unteren Seitenrand auf **Aktivieren**, um Ihr Konto mit dem Tarif **Basic** zu aktualisieren.

## <a name="cmdlet-not-recognized"></a>Szenario: Cmdlet wird beim Ausführen eines Runbooks nicht erkannt

### <a name="issue"></a>Problem

Ihr Runbookauftrag schlägt mit dem folgenden Fehler fehl:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

### <a name="cause"></a>Ursache

Dieser Fehler wird verursacht, wenn die PowerShell-Engine das Cmdlet nicht findet, das Sie in Ihrem Runbook verwenden. Das kann daran liegen, weil das Modul mit dem Cmdlet unter dem Konto nicht vorhanden ist, ein Namenskonflikt mit einem Runbooknamen besteht oder das Cmdlet auch in einem anderen Modul vorhanden ist und Automation den Namen nicht auflösen kann.

### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Überprüfen Sie, ob Sie den Cmdlet-Namen richtig eingegeben haben.
* Stellen Sie sicher, dass das Cmdlet unter Ihrem Automation-Konto vorhanden ist und dass keine Konflikte bestehen. Überprüfen Sie wie folgt, ob das Cmdlet vorhanden ist: Öffnen Sie ein Runbook im Bearbeitungsmodus, und suchen Sie in der Bibliothek nach dem gewünschten Cmdlet, oder führen Sie `Get-Command <CommandName>` aus. Nachdem Sie überprüft haben, dass das Cmdlet für das Konto verfügbar ist und dass keine Namenskonflikte mit anderen Cmdlets oder Runbooks bestehen, sollten Sie es der Canvas hinzufügen und sicherstellen, dass Sie in Ihrem Runbook einen gültigen Parametersatz verwenden.
* Falls ein Namenskonflikt vorliegt und das Cmdlet in zwei unterschiedlichen Modulen verfügbar ist, können Sie das Problem beheben, indem Sie den vollqualifizierten Namen für das Cmdlet verwenden. Sie können beispielsweise **ModuleName\CmdletName** verwenden.
* Wenn Sie das Runbook lokal in einer Hybrid Worker-Gruppe ausführen, stellen Sie sicher, dass das Modul und das Cmdlet auf dem Computer installiert sind, auf dem der Hybrid Worker gehostet wird.

## <a name="long-running-runbook"></a>Szenario: Ein Runbook mit langer Ausführungszeit kann nicht abgeschlossen werden

### <a name="issue"></a>Problem

Ihr Runbook befindet sich nach 3 Stunden Laufzeit im Status **Angehalten**. Möglicherweise wird diese Fehlermeldung angezeigt:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Dies ist das in Azure-Sandboxes vorgesehene Verhalten aufgrund der Überwachung der Prozesse in Azure Automation auf gleichmäßige Verteilung. Wenn es länger als drei Stunden ausgeführt wird, wird das Runbook von der gleichmäßigen Verteilung automatisch beendet. Der Status eines Runbooks, der das Zeitlimit für die gleichmäßigen Verteilung überschreitet, ist je nach Runbooktyp unterschiedlich. PowerShell- und Python-Runbooks werden auf einen Status **Angehalten** festgelegt. PowerShell-Workflow-Runbooks werden auf **Fehlerhaft** festgelegt.

### <a name="cause"></a>Ursache

Das Runbook hat den von der gleichmäßigen Verteilung in einer Azure-Sandbox vorgegebenen Grenzwert von 3 Stunden überschritten.

### <a name="resolution"></a>Lösung

Die empfohlene Lösung ist das Ausführen des Runbooks in einem [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md).

Hybrid Worker unterliegen nicht dem von der [gleichmäßigen Verteilung](../automation-runbook-execution.md#fair-share) in einer Azure-Sandbox vorgegebenen Grenzwert von 3 Stunden. Runbooks, die auf Hybrid Runbook Workers ausgeführt werden, sollten weiterentwickelt werden, um das Neustartverhalten bei unerwarteten lokalen Infrastrukturproblemen zu unterstützen.

Eine weitere Möglichkeit ist das Optimieren des Runbooks durch die Erstellung von [untergeordneten Runbooks](../automation-child-runbooks.md). Wenn Ihr Runbook für mehrere Ressourcen eine Schleife durch die gleiche Funktion durchführt, z.B. für einen Datenbankvorgang in mehreren Datenbanken, können Sie diese Funktion in ein untergeordnetes Runbook verschieben. Jedes dieser untergeordneten Runbooks wird parallel in separaten Prozessen ausgeführt. Dieses Verhalten führt dazu, dass die Gesamtdauer der Verarbeitung für das übergeordnete Runbook verringert wird.

Die PowerShell-Cmdlets für dieses Szenario, die das untergeordnete Runbook aktivieren, sind:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) – Mit diesem Cmdlet können Sie ein Runbook starten und Parameter an das Runbook übergeben.

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) – Wenn Vorgänge nach Abschluss des untergeordneten Runbooks ausgeführt werden müssen, können Sie mit diesem Cmdlet den Auftragsstatus für jedes untergeordnete Element überprüfen.

## <a name="expired webhook"></a>Szenario: Status: 400 (ungültige Anforderung) beim Aufrufen eines Webhooks

### <a name="issue"></a>Problem

Beim Versuch, einen Webhook für ein Azure Automation-Runbook aufzurufen, tritt der folgende Fehler auf:

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>Ursache

Der Webhook, den Sie aufrufen möchten, ist deaktiviert oder abgelaufen.

### <a name="resolution"></a>Lösung

Falls der Webhook deaktiviert ist, können Sie ihn über das Azure-Portal wieder aktivieren. Wenn ein Webhook abgelaufen ist, muss er gelöscht und neu erstellt werden. Das [Verlängern eines Webhooks](../automation-webhooks.md#renew-webhook) ist nur möglich, solange er noch nicht abgelaufen ist.

## <a name="429"></a>Szenario: 429: The request rate is currently too large. Please try again (Die Anforderungsrate ist derzeit zu hoch. Versuchen Sie es noch mal.)

### <a name="issue"></a>Problem

Beim Ausführen des Cmdlets `Get-AzureRmAutomationJobOutput` wird folgende Fehlermeldung angezeigt:

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>Ursache

Dieser Fehler kann beim Abrufen der Auftragsausgabe eines Runbooks auftreten, das über zahlreiche [ausführliche Datenströme](../automation-runbook-output-and-messages.md#verbose-stream) verfügt.

### <a name="resolution"></a>Lösung

Es gibt zwei Möglichkeiten, diesen Fehler zu beheben:

* Bearbeiten Sie das Runbook, und verringern Sie die Anzahl ausgegebener Auftragsdatenströme.
* Verringern Sie die Anzahl von Datenströmen, die beim Ausführen des Cmdlets abgerufen werden. Wenn Sie dieses Verhalten nutzen möchten, können Sie den Parameter `-Stream Output` für das Cmdlet `Get-AzureRmAutomationJobOutput` angeben, um ausschließlich Ausgabedatenströme abzurufen. 

## <a name="cannot-invoke-method"></a>Szenario: Bei einem PowerShell-Auftrag wird folgender Fehler ausgegeben: Cannot invoke method (Methode kann nicht aufgerufen werden)

### <a name="issue"></a>Problem

Beim Starten eines PowerShell-Auftrags in einem in Azure ausgeführten Runbook wird die folgende Fehlermeldung angezeigt:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann auftreten, wenn Sie einen PowerShell-Auftrag in einem in Azure ausgeführten Runbook starten. Dieses Verhalten kann auftreten, da Runbooks, die in einer Azure-Sandbox ausgeführt werden, möglicherweise nicht im [vollständigen Sprachmodus](/powershell/module/microsoft.powershell.core/about/about_language_modes) ausgeführt werden.

### <a name="resolution"></a>Lösung

Es gibt zwei Möglichkeiten, diesen Fehler zu beheben:

* Verwenden Sie `Start-AzureRmAutomationRunbook` anstelle von `Start-Job` zum Starten eines Runbooks.
* Wenn diese Fehlermeldung für Ihr Runbook angezeigt wird, führen Sie es in einem Hybrid Runbook Worker aus.

Weitere Informationen zu diesem Verhalten und anderen Verhaltensmerkmalen von Azure Automation-Runbooks finden Sie unter [Runbook-Verhalten](../automation-runbook-execution.md#runbook-behavior).

## <a name="other"></a>Mein Problem ist oben nicht aufgeführt.

In den folgenden Abschnitten werden zusätzlich zur unterstützenden Dokumentation weitere häufige Fehler aufgeführt, die Ihnen bei der Behebung Ihres Problems helfen.

## <a name="hybrid-runbook-worker-doesnt-run-jobs-or-isnt-responding"></a>Hybrid Runbook Worker führt keine Aufträge aus oder reagiert nicht

Wenn Sie Aufträge nicht mit Azure Automation, sondern mithilfe eines Hybrid Workers ausführen, ist ggf. eine [Problembehandlung für den Hybrid Worker selbst](https://docs.microsoft.com/azure/automation/troubleshoot/hybrid-runbook-worker) erforderlich.

## <a name="runbook-fails-with-no-permission-or-some-variation"></a>Beim Runbook trifft der Fehler „Keine Berechtigung“ oder ein ähnlicher Fehler auf

RunAs-Konten verfügen unter Umständen nicht über die gleichen Berechtigungen für Azure-Ressourcen wie Ihr aktuelles Konto. Vergewissern Sie sich, dass Ihr RunAs-Konto über [Berechtigungen für den Zugriff auf alle Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) verfügt, die in Ihrem Skript verwendet werden.

## <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbooks haben funktioniert, wurden aber plötzlich angehalten

* Wenn Runbooks zuvor ausgeführt, dann aber angehalten wurden, [stellen Sie sicher, dass das RunAs-Konto nicht abgelaufen ist](https://docs.microsoft.com/azure/automation/manage-runas-account#cert-renewal).
* Falls Sie zum Starten von Runbooks Webhooks verwenden, [vergewissern Sie sich, dass der Webhook nicht abgelaufen ist](https://docs.microsoft.com/azure/automation/automation-webhooks#renew-webhook).

## <a name="issues-passing-parameters-into-webhooks"></a>Probleme beim Übergeben von Parametern an Webhooks

Wenn Sie Unterstützung beim Übergeben von Parametern an Webhooks benötigen, lesen Sie den Artikel [Starten eines Azure Automation-Runbooks mit einem Webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#parameters).

## <a name="issues-using-az-modules"></a>Probleme beim Verwenden von Az-Modulen

Die Verwendung von Az-Modulen und AzureRM-Modulen im gleichen Automation-Konto wird nicht unterstützt. Weitere Informationen finden Sie unter [Az-Module in Runbooks](https://docs.microsoft.com/azure/automation/az-modules).

## <a name="inconsistent-behavior-in-runbooks"></a>Inkonsistentes Verhalten in Runbooks

Führen Sie die entsprechenden Schritte in [Ausführen von Runbooks](https://docs.microsoft.com/azure/automation/automation-runbook-execution#runbook-behavior) aus, um Probleme mit gleichzeitigen Aufträgen, mit Ressourcen, die mehrmals erstellt werden, oder mit sonstiger zeitabhängiger Logik in Runbooks zu vermeiden.

## <a name="runbook-fails-with-the-errors-no-permission-forbidden-403-or-some-variation"></a>Beim Runbook treten folgende Fehler auf: „Keine Berechtigung“, „Verboten“, „403“ oder ein ähnlicher Fehler

RunAs-Konten verfügen unter Umständen nicht über die gleichen Berechtigungen für Azure-Ressourcen wie Ihr aktuelles Konto. Vergewissern Sie sich, dass Ihr RunAs-Konto über [Berechtigungen für den Zugriff auf alle Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) verfügt, die in Ihrem Skript verwendet werden.

## <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbooks haben funktioniert, wurden aber plötzlich angehalten

* Wenn Runbooks zuvor ausgeführt, dann aber angehalten wurden, [stellen Sie sicher, dass das RunAs-Konto nicht abgelaufen ist](https://docs.microsoft.com/azure/automation/manage-runas-account#cert-renewal).
* Falls Sie zum Starten von Runbooks Webhooks verwenden, [sollten Sie sich vergewissern, dass der Webhook nicht abgelaufen ist](https://docs.microsoft.com/azure/automation/automation-webhooks#renew-webhook).

## <a name="passing-parameters-into-webhooks"></a>Übergeben von Parametern an Webhooks

Wenn Sie Unterstützung beim Übergeben von Parametern an Webhooks benötigen, lesen Sie den Artikel [Starten eines Azure Automation-Runbooks mit einem Webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#parameters).

## <a name="using-az-modules"></a>Verwenden von Az-Modulen

Die Verwendung von Az-Modulen und AzureRM-Modulen im gleichen Automation-Konto wird nicht unterstützt. Weitere Informationen finden Sie unter [Az-Module in Runbooks](https://docs.microsoft.com/azure/automation/az-modules).

## <a name="using-self-signed-certificates"></a>Verwenden selbstsignierter Zertifikate

Um selbstsignierte Zertifikate zu verwenden, müssen Sie sich an den Leitfaden unter [Erstellen eines neuen Zertifikats](https://docs.microsoft.com/azure/automation/shared-resources/certificates#creating-a-new-certificate) halten.

## <a name="recommended-documents"></a>Empfohlene Dokumente

* [Starten eines Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/automation-starting-a-runbook)
* [Ausführen von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/automation-runbook-execution)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
