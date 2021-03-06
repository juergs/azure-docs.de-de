---
title: Erstellen einer iOS-App
description: Befolgen Sie dieses Tutorial, um sich grundlegende Kenntnisse hinsichtlich der Verwendung mobiler Azure-App-Back-Ends für die iOS-Entwicklung in Objective-C oder Swift anzueignen.
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d933319d22fe7622f0409e8931c41a801fcd18ca
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668788"
---
# <a name="create-an-ios-app"></a>Erstellen einer iOS-App

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center unterstützt End-to-End- und integrierte Dienste, die für die Entwicklung mobiler Apps von zentraler Bedeutung sind. Entwickler können **Build**-, **Test**- und **Verteilungs**dienste nutzen, um eine Pipeline für Continuous Integration und Delivery einzurichten. Nach der Bereitstellung der App können Entwickler den Status und die Nutzung ihrer App mithilfe der **Analyse**- und **Diagnose**dienste überwachen und mit Benutzern über den **Push**dienst interagieren. Entwickler können auch den **Authentifizierung**sdienst nutzen, um ihre Benutzer zu authentifizieren, und den **Daten**dienst, um App-Daten dauerhaft in der Cloud zu speichern und zu synchronisieren.
>
> Falls Sie Clouddienste in Ihre mobile Anwendung integrieren möchten, sollten Sie sich noch heute für [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) registrieren.

## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie [Azure Mobile App Service-Apps](app-service-mobile-value-prop.md), einen Cloud-Back-End-Dienst, zu einer iOS-App hinzufügen können. Der erste Schritt besteht in der Erstellung eines neuen mobilen Back-Ends in Azure. Anschließend laden Sie eine einfache iOS-Beispiel-App vom Typ *Aufgabenliste* herunter, die Daten in Azure speichert.

Sie benötigen einen Mac und ein [Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen eines neuen mobilen Azure-App-Back-Ends

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Erstellen einer Datenbankverbindung und Konfigurieren des Client- und Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-ios-app"></a>Ausführen der iOS-App

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
