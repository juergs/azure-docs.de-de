---
title: Zeichentools-Modul | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Microsoft Azure Maps Web SDK Daten für Zeichenoptionen festlegen.
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 2f23d4d7962fc4a01ac2f9d20dc834bcd2f08be5
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910584"
---
# <a name="use-the-drawing-tools-module"></a>Verwenden des Zeichentools-Moduls

Das Web SDK für Azure Maps stellt ein *Zeichentools-Modul* bereit. Dieses Modul vereinfacht das Zeichnen und Bearbeiten von Formen auf der Karte mit einem Eingabegerät wie einer Maus oder einem Touchscreen. Die Kernklasse dieses Moduls ist der [Zeichnungs-Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-), der alle Funktionen zur Verfügung stellt, die zum Zeichnen und Bearbeiten von Formen auf der Karte benötigt werden. Der Zeichnungs-Manager kann direkt verwendet und in eine benutzerdefinierte Symbolleisten-Benutzeroberfläche integriert werden, oder Sie nutzen die integrierte [Zeichnen-Symbolleisten](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)-Klasse. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Laden des Zeichentools-Moduls auf eine Webseite

1. Erstellen Sie eine neue HTML-Datei, und [implementieren Sie die Karte wie üblich](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).
2. Laden Sie das Azure Maps-Zeichentools-Modul. Hierzu stehen zwei Möglichkeiten zur Verfügung:
    - Verwenden Sie die global gehostete Azure Content Delivery Network-Version des Azure Maps-Moduls „Dienste“. Fügen Sie einen Verweis auf das JavaScript und das CSS-Stylesheet im `<head>`-Element der Datei hinzu:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
        ```

    - Laden Sie alternativ das Zeichentools-Modul für den Quellcode des Azure Maps Web SDK lokal mithilfe des npm-Pakets [azure-maps-drawing-tools](https://www.npmjs.com/package/azure-maps-drawing-tools), und hosten Sie es anschließend zusammen mit Ihrer App. Dieses Paket enthält außerdem TypeScript-Definitionen. Verwenden Sie diesen Befehl:
    
        > **npm install azure-maps-drawing-tools**
    
        Fügen Sie anschließend einen Verweis auf das JavaScript und das CSS-Stylesheet im `<head>`-Element der Datei hinzu:

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Direktes Verwenden des Zeichnungs-Managers

Jetzt, da das Zeichentools-Modul in Ihre Anwendung geladen wurde, können Sie den [Zeichnungs-Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) verwenden, um die Zeichen- und Bearbeitungsfunktionen innerhalb der Karte zu aktivieren. Sie können Optionen für den Zeichnungs-Manager festlegen, während Sie ihn instanziieren, oder alternativ die Funktion `drawingManager.setOptions()` verwenden.

### <a name="set-the-drawing-mode"></a>Festlegen des Zeichenmodus

Mit dem folgenden Code wird eine Instanz des Zeichnungs-Managers erstellt und die Option für den **Zeichnungsmodus** festgelegt. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

Der nachstehende Code ist ein vollständiges, ausführbares Beispiel zum Festlegen eines Zeichnungsmodus für den Zeichnungs-Manager. Klicken Sie auf die Karte, um mit dem Zeichnen eines Polygons zu beginnen.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Zeichnen eines Polygons" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>Draw a polygon</a> (Zeichnen eines Polygons) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="set-the-interaction-type"></a>Festlegen des Interaktionstyps

Der Zeichnungs-Manager unterstützt drei verschiedene Arten der Interaktion mit der Karte zum Zeichnen von Formen.

* `click`: Koordinaten werden hinzugefügt, wenn mit der Maus oder per Toucheingabe geklickt wird.
* `freehand `: Koordinaten werden hinzugefügt, wenn die Maus auf die Karte gezogen bzw. die gleiche Geste per Toucheingabe vollzogen wird. 
* `hybrid`: Koordinaten werden hinzugefügt, wenn mit der Maus oder per Toucheingabe geklickt oder gezogen wird.

Der folgende Code aktiviert den Polygon-Zeichenmodus und legt den Typ von Zeichnungsinteraktion, dem der Zeichnungs-Manager entsprechen soll, auf `freehand` fest. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

Nachstehend finden Sie ein Codebeispiel, das eine Funktion zum freien Zeichnen eines Polygons auf der Karte durch Drücken der linken Maustaste und Bewegen der Maus implementiert. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Freihandzeichnung" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>Free-hand drawing</a> (Freihandzeichnung) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="customizing-drawing-options"></a>Anpassen von Zeichnungsoptionen

In den vorherigen Beispielen wurde gezeigt, wie Zeichnungsoptionen beim Instanziieren des Zeichnungs-Managers angepasst werden. Sie können die Optionen für den Zeichnungs-Manager auch mithilfe der `drawingManager.setOptions()`-Funktion festlegen. Nachstehend finden Sie ein Tool zum Testen der Anpassung aller Optionen für den Zeichnungs-Manager mit der setOptions-Funktion.

<br/>

<iframe height="685" title="Anpassen des Zeichnungs-Managers" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Get shape data</a> (Abrufen von Formdaten) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie die weiteren Funktionen des Zeichentools-Moduls verwendet werden:

> [!div class="nextstepaction"]
> [Hinzufügen einer Zeichnen-Symbolleiste](map-add-drawing-toolbar.md)

> [!div class="nextstepaction"]
> [Abrufen von Formdaten](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Reagieren auf Zeichnungsereignisse](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Interaktionstypen und Tastenkombinationen](drawing-tools-interactions-keyboard-shortcuts.md)

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Zeichnungs-Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Zeichnungssymbolleiste](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
