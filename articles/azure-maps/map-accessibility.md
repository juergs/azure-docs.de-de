---
title: Erstellen einer barrierefreien Kartenanwendung mit Azure Maps | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Microsoft Azure Maps eine Anwendung mit Barrierefreiheitsfunktionen erstellen.
services: azure-maps
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2019
ms.topic: conceptual
ms.service: azure-maps
manager: cpendleton
ms.openlocfilehash: 739322feb8e844a197f2943f4ff050cacc0f2274
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911297"
---
# <a name="building-an-accessible-application"></a>Erstellen einer zugänglichen Anwendung

Mehr als 20 % der Internetbenutzer benötigen barrierefreie Webanwendungen. Daher muss Ihre Anwendung so konzipiert sein, dass sie von jedem Benutzer problemlos verwendet werden kann. Anstatt die Barrierefreiheit als eine Reihe von zu erledigenden Aufgaben zu betrachten, sollten Sie sie als Teil der allgemeinen Benutzerfreundlichkeit ansehen. Je zugänglicher Ihre Anwendung ist, desto mehr Personen können sie verwenden. 

Bei umfassenden interaktiven Inhalten wie einer Karte gelten einige allgemeine Überlegungen zur Barrierefreiheit:
- Unterstützung der Sprachausgabe für Benutzer, die Probleme beim Betrachten der Webanwendung haben.
- Es gibt verschiedene Methoden zum Interagieren mit Webanwendungen und dem Navigieren darin, z. B. die Verwendung einer Maus oder Tastatur oder das Berühren.
- Richten Sie den Farbkontrast so ein, dass die Farben nicht verschwimmen, sodass sie schwierig voneinander unterschieden werden können. 

Das Azure Maps Web SDK enthält zahlreiche Funktionen zur Barrierefreiheit wie die folgenden:
- Beschreibungen bei der Sprachausgabe, wenn die Karte verschoben wird und der Benutzer auf ein Steuerelement oder ein Popup fokussiert
- Maus-, Berührungs- und Tastaturunterstützung
- Unterstützung für Farbkontraste beim Stil von Straßenkarten

Sämtliche Details zur Konformität bei der Barrierefreiheit zu allen Microsoft-Produkten finden Sie [hier](https://cloudblogs.microsoft.com/industry-blog/government/2018/09/11/accessibility-conformance-reports/). Suchen Sie nach „Azure Maps Web“, um das spezielle Dokument zum Azure Maps Web SDK zu finden. 

## <a name="navigating-the-map"></a>Navigieren auf einer Karte
Es gibt verschiedene Möglichkeiten für das Zoomen, Schwenken, Drehen und Neigen der Karte. Im Folgenden werden die verschiedenen Möglichkeiten zum Navigieren auf der Karte erläutert.

**Zoomen der Karte**
- Doppelklicken Sie mit der Maus auf die Karte, um eine Ebene zu vergrößern.
- Mit einer Maus können Sie die Karte über das Mausrad zoomen.
- Bei einem Touchscreen können Sie die Karte mit zwei Fingern berühren und diese zusammenführen, um zu vergrößern, und auseinanderführen, um zu verkleinern.
- Doppeltippen Sie auf dem Touchscreen auf die Karte, um eine Ebene zu vergrößern.
- Verwenden Sie bei fokussierter Karte das Pluszeichen (`+`) oder das *Gleichheitszeichen (`=`), um eine Ebene zu vergrößern.
- Verwenden Sie bei fokussierter Karte das Minuszeichen (`-`) oder den Unterstrich (`_`), um eine Ebene zu verkleinern.
- Verwenden Sie das Zoomsteuerelement mit einer Maus, der Berührungssteuerung oder auf einer Tastatur der TAB- oder EINGABETASTE.
- Halten Sie die Schaltfläche `Shift` und die linke Maustaste auf der Karte gedrückt, und ziehen Sie einen Bereich der Karte, um die Karte zu vergrößern.

**Schwenken der Karte**
- Halten Sie die linke Maustaste auf der Karte gedrückt, und ziehen Sie sie in eine beliebige Richtung.
- Berühren Sie bei einem Touchscreen die Karte, und ziehen Sie sie in eine beliebige Richtung.
- Wenn die Karte den Fokus hat, können Sie sie mit den Pfeiltasten verschieben.

**Drehen der Karte**
- Halten Sie die rechte Maustaste auf der Karte gedrückt, und ziehen Sie sie nach links oder rechts. 
- Berühren Sie auf einem Touchscreen die Karte mit zwei Fingern, um sie zu drehen.
- Verwenden Sie bei fokussierter Karte die Tastenkombination UMSCHALTTASTE und NACH-LINKS- oder NACH-RECHTS-TASTE.
- Verwenden Sie das Drehungssteuerelement mit einer Maus, der Berührungssteuerung oder auf einer Tastatur der TAB- oder EINGABETASTE.

**Neigen der Karte**
- Halten Sie die rechte Maustaste auf der Karte gedrückt, und ziehen Sie sie nach oben oder unten. 
- Berühren Sie auf einem Touchscreen die Karte mit zwei Fingern, und ziehen Sie diese zusammen nach oben oder unten.
- Verwenden Sie bei fokussierter Karte die Tastenkombination UMSCHALTTASTE und NACH-OBEN- oder NACH-UNTEN-TASTE. 
- Verwenden Sie das Neigungssteuerelement mit einer Maus, der Berührungssteuerung oder auf einer Tastatur der TAB- oder EINGABETASTE.

**Ändern des Kartenstils** Nicht alle Entwickler stellen sämtliche mögliche Kartenstile in ihrer Anwendung zur Verfügung. Der Entwickler kann den Kartenstil programmgesteuert festlegen und nach Wunsch ändern. Wenn ein Entwickler das Steuerelement für die Auswahl des Kartenstils anzeigt, kann der Benutzer den Kartenstil mithilfe der Maus, der Berührungssteuerung oder der Tastatur (mit der TAB- oder EINGABETASTE) ändern. Der Entwickler kann angeben, welche Kartenstile über das Auswahlsteuerelement für den Kartenstil verfügbar gemacht werden. 

## <a name="keyboard-shortcuts"></a>Tastenkombinationen

In die Karte sind einige Tastenkombinationen integriert, die die Verwendung der Karte vereinfachen. Diese Tastenkombinationen sind funktionsfähig, wenn die Karte den Fokus hat.

| Key      | Action                            |
|----------|-----------------------------------|
| `Tab` | Navigieren Sie zwischen den Steuerelementen und Popups auf der Karte. |
| `ESC` | Wechseln Sie den Fokus von einem beliebigen Element auf der Karte zum Kartenelement der obersten Ebene. |
| `Ctrl` + `Shift` + `D` | Schalten Sie zwischen den Detailebenen für die Sprachausgabe um.  |
| NACH-LINKS-TASTE | Schwenken der Karte um 100 Pixel nach links |
| NACH-RECHTS-TASTE | Schwenken der Karte um 100 Pixel nach rechts |
| NACH-UNTEN-TASTE | Schwenken der Karte um 100 Pixel nach unten |
| NACH-OBEN-TASTE | Schwenken der Karte um 100 Pixel nach oben |
| `Shift` + NACH-OBEN-TASTE | Erhöhen der Kartenneigung um 10 Grad |
| `Shift` + NACH-UNTEN-TASTE | Verringern der Kartenneigung um 10 Grad |
| `Shift` + NACH-RECHTS-TASTE | Drehen der Karte um 15 Grad im Uhrzeigersinn |
| `Shift` + NACH-LINKS-TASTE | Drehen der Karte um 15 Grad gegen den Uhrzeigersinn |
| Pluszeichen (`+`) oder <sup>*</sup>Gleichheitszeichen (`=`) | Vergrößern |
| Minuszeichen, Bindestrich (`-`) oder <sup>*</sup>Unterstrich (`_`) | Verkleinern | 
| `Shift` + Ziehen der Maus auf der Karte, um einen Bereich zu markieren | Vergrößern eines Bereichs |

<sup>*</sup> Für diese Tastenkombinationen wird normalerweise dieselbe Taste auf einer Tastatur verwendet. Sie wurden hinzugefügt, um die Bedienfreundlichkeit zu verbessern, sodass es keine Rolle spielt, ob der Benutzer die UMSCHALTTASTE für diese Verknüpfungen verwendet.

## <a name="screen-reader-support"></a>Unterstützung der Sprachausgabe

Benutzer können über die Tastatur auf der Karte navigieren. Wenn eine Sprachausgabe ausgeführt wird, benachrichtigt die Karte den Benutzer über Änderungen an ihrem Zustand. So werden die Benutzer z. B. über Kartenänderungen informiert, wenn die Karte geschwenkt oder vergrößert bzw. verkleinert wird. Standardmäßig enthält die Karte vereinfachte Beschreibungen, die weder die Zoomebene noch die Koordinaten der Kartenmitte enthalten. Der Benutzer kann die Detailebene dieser Beschreibungen mithilfe der Tastenkombination `Ctrl` + `Shift` + `D` ändern.

Zusätzliche Informationen, die auf die allgemeine Karte platziert werden, sollten über entsprechende Textinformationen für Sprachausgabenbenutzer verfügen. Fügen Sie, wo es angebracht ist, unbedingt [ARIA- (Accessible Rich Internet Applications)](https://www.w3.org/WAI/standards-guidelines/aria/), alt- und title-Attribute hinzu. 

## <a name="make-popups-keyboard-accessible"></a>Verfügbarmachen von Popups für die Tastatursteuerung

Häufig wird ein Marker oder Symbol verwendet, um einen Standort auf der Karte darzustellen. Weitere Informationen zu diesem Standort werden in der Regel in einem Popup angezeigt, wenn der Benutzer mit dem Marker interagiert. Bei den meisten Anwendungen werden Popups angezeigt, wenn ein Benutzer auf einen Marker klickt oder tippt. Dies erfordert jedoch die Verwendung der Maus oder des Touchscreens durch den Benutzer. Es empfiehlt sich, Popups auch bei Verwendung der Tastatur zugänglich zu machen. Dies können Sie erreichen, indem Sie für jeden Datenpunkt ein Popup erstellen und der Karte hinzufügen. 

Im folgenden Beispiel werden Points of Interest (POI) auf der Karte mithilfe einer Symbolebene geladen. Außerdem wird der Karte für jeden Point of Interest ein Popup hinzugefügt. Ein Verweis auf die einzelnen Popups wird in den Eigenschaften der Datenpunkte gespeichert, sodass sie auch für einen Marker abgerufen werden können, wenn z. B. auf diesen geklickt wird. Wenn die Karte den Fokus hat, kann der Benutzer die einzelnen Popups auf der Karte durch Drücken der TAB-TASTE einzeln durchlaufen.

<br/>

<iframe height='500' scrolling='no' title='Erstellen einer zugänglichen Anwendung' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen erhalten Sie über den Stift <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Erstellen einer zugänglichen Anwendung</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) unter <a href='https://codepen.io'>CodePen</a>. </iframe>

<br/>

## <a name="additional-accessibility-tips"></a>Weitere Tipps zur Barrierefreiheit

Im Folgenden finden Sie einige zusätzliche Tipps, wie Sie Ihre Webkartenanwendung noch barrierefreier machen können.

- Wenn viele interaktive Punktdaten auf der Karte angezeigt werden, sollten Sie die Übersichtlichkeit per Clustering erhöhen. 
- Sorgen Sie für einen ausreichenden Farbkontrast zwischen Text/Symbolen und den Hintergrundfarben von 4:5:1 oder höher.
- Halten Sie die Meldungen der Sprachausgabe (ARIA-, alt- und title-Attribute) kurz und aussagekräftig. Vermeiden Sie unnötigen Jargon und Akronyme.
- Versuchen Sie die an die Sprachausgabe gesendeten Meldungen zu optimieren, um kurze und aussagekräftige Informationen bereitzustellen, die für den Benutzer einfach zu verstehen sind. Wenn Sie z. B. die Sprachausgabe häufig aktualisieren möchten (etwa beim Verschieben der Karte), sollten Sie wie folgt vorgehen:
    - Warten Sie, bis das Verschieben der Karte abgeschlossen ist, bevor Sie die Sprachausgabe aktualisieren.
    - Geben Sie Aktualisierungen nur alle paar Sekunden aus. 
    - Kombinieren Sie die Meldungen auf logische Weise. 
- Vermeiden Sie die Verwendung von Farben als einzige Methode zum Übermitteln von Informationen. Verwenden Sie Text, Symbole oder Muster, um die Farben zu ergänzen oder zu ersetzen. Einige Überlegungen:
    - Wenn Sie eine Blasenebene verwenden, um den relativen Wert zwischen Datenpunkten anzuzeigen, sollten Sie den Radius der einzelnen Blasen zusätzlich oder als Alternative zur Farbgebung skalieren. 
    - Verwenden Sie ggf. eine Symbolebene mit verschiedenen Symbolen für unterschiedliche Metrikkategorien, wie z. B. Dreiecke, Sterne und Quadrate. Die Symbolebene unterstützt auch das Skalieren der Größe von Symbolen. Sie können auch eine Beschriftung anzeigen.
    - Wenn Liniendaten angezeigt werden, kann die Gewichtung oder Größe über die Breite dargestellt werden. Mit einem Strichmuster können verschiedene Linienkategorien dargestellt werden. Eine Symbolebene kann auch zusammen mit einer Linie verwendet werden, um Symbole entlang der Linie zu überlagern. Die Verwendung eines Pfeilsymbols ist hilfreich, um die Richtung der Linie anzuzeigen.
    - Wenn Polygondaten angezeigt werden, kann als Alternative zur Farbe ein Muster (z. B. Streifen) verwendet werden. 
- Einige Visualisierungen wie Wärmebilder, Kachelebenen und Bildebenen sind für Benutzer mit Sehbehinderungen nicht zugänglich. Einige Überlegungen:
    - Lassen Sie über die Sprachausgabe beschreiben, was auf einer Ebene angezeigt wird, wenn sie der Karte hinzugefügt wird. Wenn z. B. eine Kachelebene mit einem Wetterradar angezeigt wird, können Sie über die Sprachausgabe beispielsweise „Die Karte wird mit Wetterradardaten überlagert“ ausgeben.
- Schränken Sie die Menge an Funktionen ein, für die ein Mauszeiger erforderlich ist. Sie sind für Benutzer unzugänglich, die eine Tastatur oder ein Touchgerät für die Interaktion mit Ihrer Anwendung verwenden. Beachten Sie, dass dennoch empfohlen wird, für interaktive Inhalte wie klickbare Symbole, Links und Schaltflächen einen Stil für die Maussteuerung bereitzustellen.
- Versuchen Sie selbst, mit der Tastatur in Ihrer Anwendung zu navigieren. Sorgen Sie für eine logische Aktivierreihenfolge.
- Wenn Sie Tastenkombinationen erstellen, versuchen Sie, diese auf zwei Tasten oder weniger zu beschränken. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Barrierefreiheit in den Web SDK-Modulen.

> [!div class="nextstepaction"]
> [Barrierefreiheit bei Zeichenwerkzeugen](drawing-tools-interactions-keyboard-shortcuts.md)

Erfahren Sie bei Microsoft Learn, wie Sie barrierefreie Apps entwickeln:

> [!div class="nextstepaction"]
> [Barrierefreiheit beim Lernpfad für den Badge „Action Digital“](https://ready.azurewebsites.net/learning/track/2940)

Sehen Sie sich diese nützlichen Tools für Barrierefreiheit an:
> [!div class="nextstepaction"]
> [Entwickeln von barrierefreien Apps](https://developer.microsoft.com/windows/accessible-apps)

> [!div class="nextstepaction"]
> [WAI-ARIA – Übersicht](https://www.w3.org/WAI/standards-guidelines/aria/)

> [!div class="nextstepaction"]
> [Web Accessibility Evaluation-Tool (WAVE)](https://wave.webaim.org/)

> [!div class="nextstepaction"]
> [WebAim-Farbkontrastprüfung](https://webaim.org/resources/contrastchecker/)

> [!div class="nextstepaction"]
> [Anzeigesimulator No Coffee](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl?hl=en-US)
