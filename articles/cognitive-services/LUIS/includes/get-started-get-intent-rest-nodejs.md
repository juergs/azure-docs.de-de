---
title: Abrufen der Absicht mit einem REST-Aufruf in Node.js
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: b158f3738e5d5e33c831e7312c167e5185d19e95
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74414597"
---
## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [Node.js](https://nodejs.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Verwenden Sie Node.js, um den [Vorhersageendpunkt](https://aka.ms/luis-apim-v3-prediction) abzurufen und ein Vorhersageergebnis zu erhalten.

1. Kopieren Sie den folgenden Codeausschnitt in eine Datei namens `predict.js`:

    ```javascript
    var request = require('request');
    var requestpromise = require('request-promise');
    var querystring = require('querystring');
    
    // Analyze text
    //
    getPrediction = async () => {
    
        // YOUR-KEY - Language Understanding starter key
        var endpointKey = "YOUR-KEY";
    
        // YOUR-ENDPOINT Language Understanding endpoint URL, an example is westus2.api.cognitive.microsoft.com
        var endpoint = "YOUR-ENDPOINT";
    
        // Set the LUIS_APP_ID environment variable 
        // to df67dcdb-c37d-46af-88e1-8b97951ca1c2, which is the ID
        // of a public sample application.    
        var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";
    
        var utterance = "turn on all lights";
    
        // Create query string 
        var queryParams = {
            "show-all-intents": true,
            "verbose":  true,
            "query": utterance,
            "subscription-key": endpointKey
        }
    
        // append query string to endpoint URL
        var URI = `https://${endpoint}/luis/prediction/v3.0/apps/${appId}/slots/production/predict?${querystring.stringify(queryParams)}`
    
        // HTTP Request
        const response = await requestpromise(URI);
    
        // HTTP Response
        console.log(response);
    
    }
    
    // Pass an utterance to the sample LUIS app
    getPrediction().then(()=>console.log("done")).catch((err)=>console.log(err));
    ```

1. Ersetzen Sie die folgenden Werte:

    * `YOUR-KEY` durch Ihren Startschlüssel
    * `YOUR-ENDPOINT` durch Ihre Endpunkt-URL Beispiel: `westus2.api.cognitive.microsoft.com`.

1. Installieren Sie die Abhängigkeiten `request`, `request-promise` und `querystring` mit diesem Befehl: 

    ```console
    npm install request request-promise querystring
    ```

1. Führen Sie die App mit diesem Befehl aus:

    ```console
    node predict.js
    ```

 1. Überprüfen Sie die Vorhersageantwort, die im JSON-Format zurückgegeben wird:   
    
    ```console
    {"query":"turn on all lights","prediction":{"topIntent":"HomeAutomation.TurnOn","intents":{"HomeAutomation.TurnOn":{"score":0.5375382},"None":{"score":0.08687421},"HomeAutomation.TurnOff":{"score":0.0207554}},"entities":{"HomeAutomation.Operation":["on"],"$instance":{"HomeAutomation.Operation":[{"type":"HomeAutomation.Operation","text":"on","startIndex":5,"length":2,"score":0.724984169,"modelTypeId":-1,"modelType":"Unknown","recognitionSources":["model"]}]}}}}
    ```

    Die zur besseren Lesbarkeit formatierte JSON-Antwort: 

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren](../get-started-get-model-rest-apis.md)