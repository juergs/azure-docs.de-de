---
title: Azure Service Fabric-CLI – sfctl mesh secretvalue
description: Erfahren Sie mehr über sfctl, die Azure Service Fabric-Befehlszeilenschnittstelle. Enthält eine Liste der Befehle zum Abrufen und Löschen Service Fabric Mesh-secretvalue-Ressourcen.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: a29d32dff9ad51942acb30dd834ad6fbd362ac65
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646108"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Ruft die Mesh-secretvalue-Ressourcen ab bzw. löscht sie.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| delete | Löscht den angegebenen Wert der benannten Geheimnisressource. |
| list | Listet die Namen aller Werte der angegebenen Geheimnisressource auf. |
| show | Listet den angegebenen Wert der Geheimnisressource auf. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue delete
Löscht den angegebenen Wert der benannten Geheimnisressource.

Löscht die Geheimniswertressource, die anhand des Namens identifiziert wird. Der Name der Ressource ist normalerweise die Version, die diesem Wert zugeordnet ist. Beim Löschen tritt ein Fehler auf, wenn der angegebene Wert verwendet wird.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |
| --version -v     [Erforderlich] | Der Name der Geheimnisversion. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue list
Listet die Namen aller Werte der angegebenen Geheimnisressource auf.

Ruft Informationen zu allen Geheimniswertressourcen der angegebenen Geheimnisressource ab. Die Informationen enthalten den Namen der Geheimniswertressourcen, aber nicht die tatsächlichen Werte.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue show
Listet den angegebenen Wert der Geheimnisressource auf.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |
| --version -v     [Erforderlich] | Der Name der Geheimnisversion. |
| --show-value | Zeigt den tatsächlichen Wert der Geheimnisversion an. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen. |
| --help -h | Zeigen Sie diese Hilfemeldung an, und schließen Sie sie. |
| --output -o | Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden. |


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)