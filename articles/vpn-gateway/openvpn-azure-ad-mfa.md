---
title: 'Aktivieren Sie MFA für VPN-Benutzer: Azure AD-Authentifizierung'
description: Aktivieren der mehrstufigen Authentifizierung für VPN-Benutzer
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: b22581d012b2c69081bc7b4eee093227c060b4c2
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76169708"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Aktivieren von Azure Multi-Factor Authentication (MFA) für VPN-Benutzer

Wenn Sie möchten, dass Benutzer vor der Gewährung des Zugriffs zur Angabe eines zweiten Authentifizierungsfaktor aufgefordert werden, können Sie Azure Multi-Factor Authentication (MFA) für Ihren Azure AD-Mandanten konfigurieren. Die Schritte in diesem Artikel helfen Ihnen, eine Anforderung für die zweistufige Überprüfung zu aktivieren.

## <a name="prereq"></a>Voraussetzung

Voraussetzung für diese Konfiguration ist ein konfigurierter Azure AD-Mandant mithilfe der Schritte in [Konfigurieren eines Mandanten](openvpn-azure-ad-tenant.md).

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="enablesign"></a> Konfigurieren von Anmeldeeinstellungen

Konfigurieren Sie auf der Seite **Azure-VPN – Eigenschaften** die Anmeldeeinstellungen.

1. Legen Sie **Aktiviert für die Benutzeranmeldung?** auf **Ja** fest. Dadurch können sich alle Benutzer im AD-Mandanten erfolgreich mit dem VPN verbinden.
2. Legen Sie **Benutzerzuweisung erforderlich?** auf **Ja** fest, wenn Sie die Anmeldung auf Benutzer beschränken möchten, die über Berechtigungen für das Azure-VPN verfügen.
3. Speichern Sie die Änderungen.

   ![Berechtigungen](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).
