---
title: Definieren eines technischen OAuth1-Profils in einer benutzerdefinierten Richtlinie
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie ein technisches OAuth 1.0-Profil in einer benutzerdefinierten Richtlinie in Azure Active Directory B2C definieren.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d97d908ddf5d55bf09d96a5ef16fa79a7afde7b4
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951104"
---
# <a name="define-an-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definieren eines technischen OAuth1-Profils in einer benutzerdefinierten Richtlinie in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) bietet Unterstützung für Identitätsanbieter mit dem [OAuth 1.0-Protokoll](https://tools.ietf.org/html/rfc5849). In diesem Artikel werden die Einzelheiten eines technischen Profils für die Interaktion mit einem Anspruchsanbieter thematisiert, der dieses standardisierte Protokoll unterstützt. Mit einem technischen OAuth1-Profil können Sie einen Verbund mit einem OAuth1-basierten Identitätsanbieter wie Twitter erstellen. Über einen Verbund mit dem Identitätsanbieter können sich Benutzer mit ihren vorhandenen Identitäten aus sozialen Netzwerken oder Unternehmen anmelden.

## <a name="protocol"></a>Protocol

Das **Name**-Attribut des **Protocol**-Elements muss auf `OAuth1` festgelegt werden. Das Protokoll für das technische Profil **Twitter-OAUTH1** ist z.B. `OAuth1`.

```XML
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...
```

## <a name="input-claims"></a>Eingabeansprüche

Die Elemente **InputClaims** und **InputClaimsTransformations** sind leer oder fehlen.

## <a name="output-claims"></a>Ausgabeansprüche

Das **OutputClaims**-Element enthält eine Liste von Ansprüchen, die vom OAuth1-Identitätsanbieter zurückgegeben wurden. Sie müssen den Namen des Anspruchs, der in Ihrer Richtlinie definiert ist, dem Namen, der für den Identitätsanbieter definiert wurde, zuordnen. Sie können auch Ansprüche, die nicht vom Identitätsanbieter zurückgegeben wurden, einfügen, sofern Sie das **DefaultValue**-Attribut festlegen.

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

Das folgende Beispiel zeigt die Ansprüche, die vom Identitätsanbieter Twitter zurückgegeben wurden:

- Der Anspruch **user_id** wird dem Anspruch **issuerUserId** zugeordnet.
- Der Anspruch **screen_name** wird dem Anspruch **displayName** zugeordnet.
- Dem Anspruch **email** wird kein Name zugeordnet.

Das technische Profil gibt auch Ansprüche zurück, die vom Identitätsanbieter nicht zurückgegeben werden:

- Der Anspruch **identityProvider** enthält den Namen des Identitätsanbieters.
- Der Anspruch **authenticationSource** enthält als Standardwert `socialIdpAuthentication`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metadaten

| Attribut | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| client_id | Ja | Die Anwendungs-ID des Identitätsanbieters. |
| ProviderName | Nein | Der Name des Identitätsanbieters. |
| request_token_endpoint | Ja | Die URL des Endpunkts für das Anforderungstoken gemäß RFC 5849. |
| authorization_endpoint | Ja | Die URL des Autorisierungsendpunkts gemäß RFC 5849. |
| access_token_endpoint | Ja | Die URL des Tokenendpunkts gemäß RFC 5849. |
| ClaimsEndpoint | Nein | Die URL des Endpunkts für Benutzerinformationen. |
| ClaimsResponseFormat | Nein | Das Antwortformat für Ansprüche.|

## <a name="cryptographic-keys"></a>Kryptografische Schlüssel

Das **CryptographicKeys**-Element enthält das folgende Attribut:

| Attribut | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| client_secret | Ja | Der geheime Clientschlüssel der Anwendung des Identitätsanbieters.   |

## <a name="redirect-uri"></a>Umleitungs-URI

Wenn Sie die Umleitungs-URL Ihres Identitätsanbieters konfigurieren, geben Sie `https://login.microsoftonline.com/te/tenant/policyId/oauth1/authresp` an. Ersetzen Sie **tenant** durch den Namen Ihres Mandanten (z.B. contosob2c.onmicrosoft.com) und **policyId** durch Ihre Richtlinien-ID (z.B. b2c_1_policy). Der Umleitungs-URI muss klein geschrieben sein. Fügen Sie eine Umleitungs-URL für alle Richtlinien hinzu, die die Anmeldung des Identitätsanbieters verwenden.

Achten Sie bei Verwendung der Domäne **b2clogin.com** anstelle von **login.microsoftonline.com** darauf, b2clogin.com anstelle von login.microsoftonline.com zu verwenden.

Beispiele:

- [Hinzufügen von Twitter als OAuth1-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien](active-directory-b2c-custom-setup-twitter-idp.md)













