---
title: 'Schnellstart: Synthetisieren von Sprache in eine Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 9b13d8fc3b77426a59dea5399223b79c4bb4b1a1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467387"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache in einer Audiodatei umgewandelt. Nach der Erfüllung einiger Voraussetzungen sind für die Sprachsynthese in eine Audiodatei lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Audiokonfigurationsobjekts zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ ````SpeechSynthesizer```` unter Verwendung der obigen Konfigurationsobjekte
> * Konvertieren Ihres Texts unter Verwendung des Objekts ````SpeechSynthesizer```` in synthetisierte Sprache und Speichern der synthetisierten Sprache in der angegebenen Audiodatei
> * Überprüfen des zurückgegebenen Objekts ````SpeechSynthesizer```` auf Fehler
