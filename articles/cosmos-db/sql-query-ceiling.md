---
title: CEILING in der Abfragesprache für Azure Cosmos DB
description: Hier erfahren Sie mehr über die SQL-Systemfunktion CEILING in Azure Cosmos DB, die die kleinste ganze Zahl zurückgibt, die größer oder gleich dem angegebenen numerischen Ausdruck ist.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2da7820a6c9f1f90585b4deb605bb99c7580b0e5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444804"
---
# <a name="ceiling-azure-cosmos-db"></a>CEILING (Azure Cosmos DB)
 Gibt den kleinsten ganzzahligen Wert zurück, der größer oder gleich dem angegebenen numerischen Ausdruck ist.  
  
## <a name="syntax"></a>Syntax
  
```sql
CEILING (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel werden positive numerische, negative und Nullwerte mit der `CEILING`-Funktion angezeigt.  
  
```sql
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Hier ist das Resultset.  
  
```json
[{c1: 124, c2: -123, c3: 0}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
