---
title: Übernehmen des Abrechnungsbesitzes für Azure-Abonnements von anderen Benutzern
description: Hier erfahren Sie, wie Sie von anderen Benutzern den Abrechnungsbesitz für Azure-Abonnements anfordern.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: f7d6c6cbe5b99cb429b5399df7ba9765c1553901
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75985386"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>Übernehmen des Abrechnungsbesitzes für Azure-Abonnements von anderen Konten

Die Übernahme des Azure-Abonnementbesitzes kann beispielsweise erforderlich sein, wenn der aktuelle Abrechnungsbesitzer die Organisation verlässt oder Sie die Abonnements über Ihr Abrechnungskonto bezahlen möchten. Bei der Besitzübernahme gehen die Abrechnungspflichten für das Abonnement auf Ihr Konto über.

Dieser Artikel bezieht sich auf ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. [Überprüfen Sie, ob Sie Zugriff auf eine Microsoft-Kundenvereinbarung haben.](#check-for-access)

Der Abrechnungsbesitz kann nur mit der Rolle **Rechnungsabschnitt (Besitzer)** oder **Rechnungsabschnitt (Mitwirkender)** angefordert werden. Weitere Informationen finden Sie unter [Rollen und Aufgaben für einen Rechnungsabschnitt](understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="request-billing-ownership"></a>Anfordern des Abrechnungsbesitzes

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) als Rechnungsabschnitt-Besitzer oder -Mitwirkender eines Abrechnungskontos für die Microsoft-Kundenvereinbarung an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Wählen Sie auf der Abrechnungsbereicheseite das Abrechnungskonto aus, das verwendet wird, um die Nutzung der Abonnements zu bezahlen. Das Abrechnungskonto sollte vom Typ **Microsoft-Kundenvereinbarung** sein.

    ![Screenshot: Suche nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/mca-request-billing-ownership/list-of-scopes.png)

    > [!NOTE]
    >
    > Das Azure-Portal speichert den letzten Abrechnungsbereich, auf den Sie zugreifen, und zeigt den Bereich an, wenn Sie das nächste Mal auf die Seite „Kostenverwaltung + Abrechnung“ gelangen. Wenn Sie „Kostenverwaltung + Abrechnung“ bereits besucht haben, wird die Abrechnungsbereicheseite nicht angezeigt. Wenn dies der Fall ist, überprüfen Sie, ob Sie sich im [richtigen Bereich](#check-for-access) befinden. Wenn nicht, [wechseln Sie den Bereich](view-all-accounts.md#switch-billing-scope-in-the-azure-portal), um das Abrechnungskonto für eine Microsoft-Kundenvereinbarung auszuwählen.

4. Wählen Sie auf der linken Seite die Option **Abrechnungsprofile** aus.

    ![Screenshot: Auswählen von Abrechnungsprofilen](./media/mca-request-billing-ownership/mca-select-profiles.png)     

    > [!Note]
    >
    > Wenn keine Abrechnungsprofile angezeigt werden, befinden Sie sich nicht im richtigen Abrechnungsbereich. Sie müssen ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung und dann Abrechnungsprofile auswählen. Weitere Informationen zum Ändern von Bereichen finden Sie unter [Wechseln des Abrechnungsbereichs im Azure-Portal](view-all-accounts.md#switch-billing-scope-in-the-azure-portal).

5. Wählen Sie ein **Abrechnungsprofil** in der Liste aus. Nachdem Sie den Besitz der Abonnements übernommen haben, wird deren Nutzung über dieses Abrechnungsprofil abgerechnet.

6. Wählen Sie links die Option **Rechnungsabschnitte**.

    ![Screenshot: Auswahl von Rechnungsabschnitten](./media/mca-request-billing-ownership/mca-select-invoice-sections.png)   

7. Wählen Sie in der Liste einen Rechnungsabschnitt aus. Nachdem Sie den Besitz der Abonnements übernommen haben, wird deren Nutzung diesem Abschnitt der Rechnung für dieses Abrechnungsprofil zugewiesen.

8. Wählen Sie links unten die Option **Übertragungsanforderungen** und anschließend **Neue Anforderung hinzufügen** aus.

    ![Screenshot: Auswahl von Übertragungsanforderungen](./media/mca-request-billing-ownership/mca-select-transfer-requests.png)

9. Geben Sie die E-Mail-Adresse des Benutzers ein, dessen Abrechnungsbesitz Sie übernehmen möchten. Der Benutzer muss ein Kontoadministrator für ein Abrechnungskonto eines Microsoft Online Services-Programms oder ein Kontobesitzer für ein Enterprise Agreement sein. Weitere Informationen finden Sie unter [Anzeigen von sämtlichen Abrechnungskonten im Azure-Portal](view-all-accounts.md). Wählen Sie **Übertragungsanforderung senden** aus.

    ![Screenshot: Senden einer Übertragungsanforderung](./media/mca-request-billing-ownership/mca-send-transfer-requests.png)

10. Der Benutzer erhält eine E-Mail mit der Aufforderung, Ihre Übertragungsanforderung zu prüfen.

    ![Screenshot: E-Mail mit Aufforderung zur Prüfung der Übertragungsanforderung](./media/mca-request-billing-ownership/mca-review-transfer-request-email.png)

11. Um die Übertragungsanforderung zu genehmigen, muss der Benutzer auf den Link in der E-Mail klicken und den Anweisungen folgen.

    ![Screenshot: E-Mail mit Aufforderung zur Prüfung der Übertragungsanforderung](./media/mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>Überprüfen des Status der Übertragungsanforderung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Wählen Sie auf der Abrechnungsbereicheseite das Abrechnungskonto aus, für das die Anforderung übertragen wurde.

4. Wählen Sie auf der linken Seite die Option **Abrechnungsprofile** aus.

    ![Screenshot: Auswählen von Abrechnungsprofilen](./media/mca-request-billing-ownership/mca-select-profiles.png)     

5. Wählen Sie das **Abrechnungsprofil** aus, für das die Übertragungsanforderung gesendet wurde.

6. Wählen Sie links die Option **Rechnungsabschnitte**.

    ![Screenshot: Auswahl von Rechnungsabschnitten](./media/mca-request-billing-ownership/mca-select-invoice-sections.png)   

7. Wählen Sie den Rechnungsabschnitt, für den die Übertragungsanforderung gesendet wurde, in der Liste aus.

8. Wählen Sie links unten die Option **Übertragungsanforderungen** aus. Auf der Seite „Übertragungsanforderungen“ werden folgende Informationen angezeigt:

    ![Screenshot: Liste mit Übertragungsanforderungen](./media/mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

   |Column|Definition|
   |---------|---------|
   |Anforderungsdatum|Das Datum, an dem die Übertragungsanforderung gesendet wurde.|
   |Recipient|Die E-Mail-Adresse des Benutzers, an den Sie die Anforderung zur Übertragung des Abrechnungsbesitzes gesendet haben.|
   |Ablaufdatum|Das Datum, bis zu dem die Anforderung gültig ist.|
   |Status|Der Status der Übertragungsanforderung.|

    Die Übertragungsanforderung kann folgende Statuswerte besitzen:

   |Status|Definition|
   |---------|---------|
   |In Bearbeitung|Der Benutzer hat die Übertragungsanforderung nicht akzeptiert.|
   |Verarbeitung|Der Benutzer hat die Übertragungsanforderung genehmigt. Die Abrechnung wird für die vom Benutzer ausgewählten Abonnements auf Ihren Rechnungsabschnitt übertragen.|
   |Abgeschlossen| Die Abrechnung wurde für die vom Benutzer ausgewählten Abonnements auf Ihren Rechnungsabschnitt übertragen.|
   |Mit Fehlern abgeschlossen|Die Anforderung wurde abgeschlossen, für einige vom Benutzer ausgewählte Abonnements konnte die Abrechnung jedoch nicht übertragen werden.|
   |Abgelaufen|Die Anforderung ist abgelaufen, da der Benutzer sie nicht rechtzeitig akzeptiert hat.|
   |Canceled|Die Anforderung wurde von einem Benutzer mit Zugriff auf die Übertragungsanforderung abgebrochen.|
   |Abgelehnt|Der Benutzer hat die Übertragungsanforderung abgelehnt.|

9. Wählen Sie eine Übertragungsanforderung aus, um Details anzuzeigen. Auf der Seite mit den Übertragungsdetails werden folgende Informationen angezeigt:

    ![Screenshot: Liste mit übertragenen Abonnements](./media/mca-request-billing-ownership/mca-transfer-completed.png)

   |Column  |Definition|
   |---------|---------|
   |ID der Übertragungsanforderung|Die eindeutige ID für Ihre Übertragungsanforderung. Wenn Sie eine Supportanfrage senden, teilen Sie dem Azure-Support die ID mit, um die Bearbeitung Ihrer Supportanfrage zu beschleunigen.|
   |Übertragung angefordert am|Das Datum, an dem die Übertragungsanforderung gesendet wurde.|
   |Übertragung angefordert von|Die E-Mail-Adresse des Benutzers, der die Übertragungsanforderung gesendet hat.|
   |Übertragungsanforderung läuft ab am| Das Datum, an dem die Übertragungsanforderung abläuft.|
   |E-Mail-Adresse des Empfängers|Die E-Mail-Adresse des Benutzers, an den Sie die Anforderung zur Übertragung des Abrechnungsbesitzes gesendet haben.|
   |An Empfänger gesendeter Übertragungslink|Die URL, die zur Prüfung der Übertragungsanforderung an den Benutzer gesendet wurde.|

## <a name="supported-subscription-types"></a>Unterstützte Abonnementtypen

Sie können für die unten angegebenen Abonnementtypen den Abrechnungsbesitz anfordern.

- [Action Pack](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [Azure in Open-Lizenzierung](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass-Sponsorship](https://azure.microsoft.com/offers/azure-pass/)\*
- [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure-Plan](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Von Microsoft Azure gesponsertes Angebot](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [Visual Studio Enterprise-Abonnenten (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [Visual Studio Enterprise-Abonnenten (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enterprise-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio Test Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\* Alle Gutschriften für das Abonnement sind unter dem neuen Konto nach der Übertragung nicht mehr verfügbar.

\*\* Wird nur für Abonnements in Konten unterstützt, die bei der Registrierung auf der Azure-Website erstellt werden.


## <a name="additional-information"></a>Zusätzliche Informationen

Der folgende Abschnitt enthält zusätzliche Informationen zur Übertragung von Abonnements.

### <a name="no-service-downtime"></a>Keine Dienstunterbrechung

Die Azure-Dienste in dem Abonnement werden ohne Unterbrechung weiter ausgeführt. Wir ändern lediglich die Abrechnungsbeziehung für die Azure-Abonnements, die der Benutzer für die Übertragung auswählt.

### <a name="disabled-subscriptions"></a>Deaktivierte Abonnements

Deaktivierte Abonnements können nicht übertragen werden. Abonnements müssen sich im Zustand „Aktiv“ befinden, um den Abrechnungsbesitz übertragen zu können.

### <a name="azure-resources-transfer"></a>Übertragung von Azure-Ressourcen

Alle Ressourcen aus den Abonnements (wie etwa virtuelle Computer, Datenträger und Websites) werden übertragen.

### <a name="azure-marketplace-products-transfer"></a>Übertragung von Azure Marketplace-Produkten

Azure Marketplace-Produkte werden zusammen mit den entsprechenden Abonnements übertragen.

### <a name="azure-reservations-transfer"></a>Übertragung von Azure-Reservierungen

Azure-Reservierungen werden nicht automatisch zusammen mit Abonnements verschoben. [Wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), wenn Sie Reservierungen verschieben möchten.

### <a name="access-to-azure-services"></a>Zugriff auf Azure-Dienste

Der Zugriff für vorhandene Benutzer, Gruppen oder Dienstprinzipale, der per (rollenbasierter Azure-Zugriffssteuerung (RBAC))[../role-based-access-control/overview.md] zugewiesen wurde, ist von dieser Übertragung nicht betroffen.

### <a name="azure-support-plan"></a>Azure-Supportplan

Der Azure-Support wird nicht zusammen mit den Abonnements übertragen. Wenn der Benutzer alle Azure-Abonnements überträgt, bitten Sie ihn, den Supportplan zu kündigen.

### <a name="charges-for-transferred-subscription"></a>Gebühren für übertragene Abonnements

Der ursprüngliche Abrechnungsbesitzer des Abonnements ist für alle Gebühren verantwortlich, die bis zum Abschluss der Übertragung gemeldet wurden. Gebühren, die nach Abschluss der Übertragung gemeldet werden, werden Ihrem Abrechnungsabschnitt zugeordnet. Es kann vorkommen, dass Gebühren, die vor der Übertragung angefallen sind, erst nach der Übertragung gemeldet werden. Diese Gebühren werden in Ihrem Rechnungsabschnitt aufgeführt.

### <a name="cancel-a-transfer-request"></a>Abbrechen einer Übertragungsanforderung

Eine Übertragungsanforderung kann abgebrochen werden, bis sie entweder genehmigt oder abgelehnt wurde. Wenn Sie eine Übertragungsanforderung abbrechen möchten, navigieren Sie zur [Seite mit den Übertragungsdetails](#check-the-transfer-request-status), und wählen Sie am unteren Seitenrand die Option „Abbrechen“.

### <a name="software-as-a-service-saas-transfer"></a>SaaS-Übertragung (Software-as-a-Service)

SaaS-Produkte werden nicht zusammen mit den Abonnements übertragen. Bitten Sie den Benutzer, sich zwecks Übertragung des Abrechnungsbesitzes von SaaS-Produkten [an den Azure-Support zu wenden](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Zusammen mit dem Abrechnungsbesitz kann der Benutzer auch den Ressourcenbesitz übertragen. Der Ressourcenbesitz ermöglicht das Ausführen von Verwaltungsvorgängen (etwa das Löschen und Anzeigen der Produktdetails). Bei dem Benutzer muss es sich um einen Ressourcenbesitzer für das SaaS-Produkt handeln, um den Ressourcenbesitz übertragen zu können.

## <a name="check-for-access"></a>Überprüfen des Zugriffs
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

## <a name="next-steps"></a>Nächste Schritte

- Der Abrechnungsbesitz für die Azure-Abonnements geht auf Ihren Rechnungsabschnitt über. Überwachen Sie die Gebühren für die entsprechenden Abonnements über das [Azure-Portal](https://portal.azure.com).
- Weisen Sie anderen Benutzern Berechtigungen zum Anzeigen und Verwalten der Abrechnung für diese Abonnements zu. Weitere Informationen finden Sie unter [Rollen und Aufgaben für einen Rechnungsabschnitt](understand-mca-roles.md#invoice-section-roles-and-tasks).
