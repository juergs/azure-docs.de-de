---
title: Was ist die Bing-Entitätssuche-API?
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die Bing-Entitätssuche-API zum Extrahieren und Suchen von Entitäten und Orten in Suchabfragen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 2f374e29f4dc5406956cd56d1bb0bd1466e65773
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75384518"
---
# <a name="what-is-bing-entity-search-api"></a>Was ist die Bing-Entitätssuche-API?

Die Bing-Entitätssuche-API sendet eine Suchabfrage an Bing und erhält Ergebnisse, die Entitäten und Orte umfassen. Zu Orten gehören Restaurants, Hotels oder andere lokale Geschäfte. Bing gibt Orte zurück, wenn die Abfrage den Namen des lokalen Geschäfts angibt oder nach einer bestimmten Art von Geschäft fragt (z.B. „Restaurants in meiner Nähe“). Bing gibt Entitäten zurück, wenn die Abfrage bekannte Personen, Orte (Touristenattraktionen, Länder/Regionen, Bundesländer usw.) oder Dinge zurückgibt.

|Funktion  |BESCHREIBUNG  |
|---------|---------|
|[Suchvorschläge in Echtzeit](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Stellen Sie Suchvorschläge bereit, die wie eine Dropdownliste angezeigt werden können, während der Benutzer etwas eingibt.       | 
| [Mehrdeutigkeit bei Entitäten](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Rufen Sie mehrere Entitäten für Abfragen mit mehreren möglichen Bedeutungen ab. |
| [Suchen nach Orten](concepts/search-for-entities.md#find-places) | Suchen und Zurückgeben von Informationen zu lokalen Unternehmen und Entitäten  |

## <a name="workflow"></a>Workflow

Die Bing-Entitätssuche-API ist ein RESTful-Webdienst und kann somit problemlos in jeder Programmiersprache aufgerufen werden, die HTTP-Anforderungen beherrscht und JSON analysieren kann. Der Dienst kann entweder über die REST-API oder über das SDK verwendet werden.

1. Erstellen Sie ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit Zugriff auf die Bing-Suche-APIs. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses [Konto erstellen](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Senden Sie eine Anforderung mit einer gültigen Suchabfrage an die API.
3. Analysieren Sie die zurückgegebene JSON-Nachricht, um die API-Antwort zu verarbeiten.

## <a name="next-steps"></a>Nächste Schritte

* Testen Sie die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) für die Bing-Entitätssuche-API. 
* Ein [Schnellstart](quickstarts/csharp.md) ermöglicht einen schnellen Einstieg in die Verwendung Ihrer ersten Anforderung.
* Der Abschnitt mit der Referenz zur [Bing-Entitätssuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
* In den [Verwendungs- und Anzeigeanforderungen für Bing](./use-display-requirements.md) erfahren Sie, wie Inhalte und Informationen verwendet werden dürfen, die über die Bing-Suche-APIs gefunden wurden.
* Besuchen Sie die Seite [Was ist die Bing-Websuche-API?](../bing-web-search/search-the-web.md), um die anderen verfügbaren APIs zu untersuchen.