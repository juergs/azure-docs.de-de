---
title: Konfigurieren von DNS-Namen mit Traffic Manager
description: Erfahren Sie, wie Sie eine benutzerdefinierte Domäne für eine Azure App Service-App konfigurieren, die für den Lastenausgleich mit Traffic Manager integriert ist.
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.topic: article
ms.date: 08/17/2016
ms.custom: seodec18
ms.openlocfilehash: 9139b83f1f2920da47b4a0d440f622626d41c938
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689276"
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet.
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dieser Artikel enthält allgemeine Anweisungen zur Verwendung eines benutzerdefinierten Domänennamens mit einer [App Service](overview.md)-App mit [Traffic Manager](../traffic-manager/traffic-manager-overview.md)-Integration für den Lastenausgleich.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Interpretation von DNS-Datensätzen
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurieren Ihrer Web-Apps für den Modus "Standard"
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Hinzufügen eines DNS-Datensatzes zu Ihrer benutzerdefinierten Domäne
> [!NOTE]
> Wenn Sie eine Domäne über Azure App Service-Web-Apps erworben haben, überspringen Sie die folgenden Schritte, und lesen Sie den letzten Schritt des Artikels zum [Erwerb einer Domäne für Web-Apps](manage-custom-dns-buy-domain.md) .
> 
> 

Um Ihre benutzerdefinierte Domäne mit einer Web-App in Azure App Service zu verknüpfen, müssen Sie einen neuen Eintrag in die DNS-Tabelle für Ihre benutzerdefinierte Domäne einfügen. Verwenden Sie hierzu die von Ihrem Domänenanbieter bereitgestellten Tools.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Die Details variieren von Domänenanbieter zu Domänenanbieter. Generell erfolgt die Zuordnung jedoch *von* Ihrem benutzerdefinierten Domänennamen (etwa **contoso.com**) *zum* Traffic Manager-Domänennamen (**contoso.trafficmanager.net**), der in Ihre Web-App integriert ist.

> [!NOTE]
> Wenn ein Eintrag bereits verwendet wird und Sie Ihre Apps präemptiv an ihn binden müssen, können Sie einen zusätzlichen CNAME-Eintrag erstellen. Um beispielsweise **www\.contoso.com** präemptiv an Ihre Web-App zu binden, erstellen Sie einen CNAME-Eintrag zur Verknüpfung von **awverify.www** mit **contoso.trafficmanager.net**. Anschließend können Sie „www\.contoso.com“ zu Ihrer Web-App hinzufügen, ohne den CNAME-Eintrag „www“ zu ändern. Weitere Informationen finden Sie unter [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne][CREATEDNS].

Speichern Sie die Änderungen, sobald Sie die DNS-Datensätze bei Ihrem Domänenanbieter hinzugefügt oder geändert haben.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Aktivieren des Traffic Manager
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie im [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
