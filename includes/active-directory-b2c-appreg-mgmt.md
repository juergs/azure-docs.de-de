---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: 0ab9302909f86ff4559ece217f8d702710733691
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75367999"
---
Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden. [Weitere Informationen zur neuen Oberfläche](https://aka.ms/b2cappregintro)

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.
1. Wählen Sie unter **Verwalten** die Option **App-Registrierungen (Legacy)** aus.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
1. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *managementapp1*.
1. Wählen Sie für **Anwendungstyp** die Option **Web-App/API** aus.
1. Geben Sie im Feld **Anmelde-URL** eine gültige URL ein. Beispiel: `https://localhost`. Der Endpunkt muss nicht erreichbar sein, aber es muss sich dabei um eine gültige URL handeln.
1. Klicken Sie auf **Erstellen**.
1. Notieren Sie sich die **Anwendungs-ID**, die auf der Übersichtsseite **Registrierte App** angezeigt wird. Sie verwenden diesen Wert in einem späteren Schritt.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** und dann **Neue Registrierung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Zum Beispiel *managementapp1*.
1. Wählen Sie **Nur Konten in diesem Organisationsverzeichnis** aus.
1. Deaktivieren Sie unter **Berechtigungen** das Kontrollkästchen *Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen*.
1. Wählen Sie **Registrieren**.
1. Notieren Sie die **Anwendungs-ID (Client)** , die auf der Übersichtsseite der Anwendung angezeigt wird. Sie verwenden diesen Wert in einem späteren Schritt.