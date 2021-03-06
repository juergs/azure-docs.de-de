---
title: Ablaufsteuerung des Unterhaltungslernmoduls – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Informationen zur Ablaufsteuerung des Unterhaltungslernmoduls.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f28b60d67e84e3e2e39cc647045a6dfca473b810
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932096"
---
# <a name="control-flow"></a>Ablaufsteuerung

Dieses Dokument beschreibt die Ablaufsteuerung des Unterhaltungslernmoduls (Conversation Learner, CL), wie im Diagramm unten dargestellt.

![](media/controlflow.PNG)

1. Benutzer geben einen Begriff oder eine Formulierung in den Bot ein, z.B. „Wie ist das Wetter in Düsseldorf?“
1. CL übergibt die Benutzereingabe einem Machine Learning-Modell, das Entitäten extrahiert
   - Dieses Modell wird vom Unterhaltungslernmodul erstellt und auf www.luis.ai gehostet
1. Alle extrahierten Entitäten und der Eingabetext des Benutzers werden an die Rückrufmethode zur Entitätserkennung im Code des Bots übergeben.
    - Dieser Code kann Entitätswerte festlegen/löschen/bearbeiten
1. Das neuronale Netzwerk des Unterhaltungslernmoduls übernimmt dann die Ausgabe der Entitätsextrahierung und die Benutzereingabe und bewertet alle im Bot definierten Aktionen
   - In diesem Beispiel ist die Aktion mit der höchsten Wahrscheinlichkeit die Bereitstellung der Wettervorhersage:

     ![](media/controlflow_forecast.PNG)

1. Die ausgewählte Aktion erfordert in diesem Fall einen API-Aufruf, um die Wettervorhersage abzurufen. 
1. Diese API, die zuvor mithilfe der CL.AddCallback-Methode registriert wurde, wird dann aufgerufen.  Das Ergebnis dieser API wird dann als Nachricht an den Benutzer zurückgegeben – z.B. „Sonnig mit einer Höchsttemperatur von 19 Grad“.
1. Dann erfolgt ein Aufruf des neuronalen Netzwerks, um auf der Grundlage des vorherigen Schritts die nächste Aktion zu bestimmen.
1. Das neuronale Netzwerk sagt dann den nächsten Satz möglicher Aktionen vorher, und die ausgewählte Aktion wird dem Benutzer angezeigt, in diesem Fall „Haben Sie weitere Wünsche?“

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einsetzen von Conversation Learner](./how-to-teach-cl.md)
