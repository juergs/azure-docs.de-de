---
title: Anwendung eines Reservierungsrabatts auf einen Azure Database for PostgreSQL-Einzelserver
description: Anwendung eines Reservierungsrabatts auf einen Azure Database for PostgreSQL-Einzelserver
author: kummanish
ms.author: manishku
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2019
ms.openlocfilehash: 2f1843690861a463c634f523e82facffc72b6743
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75986738"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-database-for-postgresql-single-server"></a>Anwendung eines Reservierungsrabatts auf einen Azure Database for PostgreSQL-Einzelserver

Nachdem Sie eine reservierte Kapazität für einen Azure Database for PostgreSQL-Einzelserver erworben haben, wird der Reservierungsrabatt automatisch auf Instanzen von PostgreSQL-Einzelservern angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt nur die Computekosten Ihres Azure Database for PostgreSQL-Einzelservers ab. Kosten für Speicher und Netzwerk werden zum regulären Satz abgerechnet. 

## <a name="how-reservation-discount-is-applied"></a>Wie der Reservierungsrabatt angewendet wird

Nicht in Anspruch genommener Reservierungsrabatt ***geht verloren***. Das heißt, wenn Sie für eine Stunde nicht über die entsprechenden Ressourcen verfügen, verlieren Sie eine Reservierungsmenge für diese Stunde. Sie können ungenutzte reservierte Stunden nicht übertragen.</br>

Wenn Sie eine Ressource beenden, wird der Reservierungsrabatt automatisch auf eine andere entsprechende Ressource im angegebenen Reservierungsumfang angewandt. Werden im angegebenen Reservierungsumfang keine entsprechenden Ressourcen gefunden, gehen die reservierten Stunden verloren.

## <a name="discount-applied-to-azure-database-for-postgresql-single-server"></a>Auf Azure Database for PostgreSQL-Einzelserver angewendeter Rabatt

Der Rabatt für reservierte Kapazität für Azure Database for PostgreSQL-Einzelserver wird auf Stundenbasis auf die Ausführung Ihres PostgreSQL-Einzelservers angewendet. Die erworbene Reservierung wird mit der Computenutzung abgeglichen, die von dem ausgeführten Azure Database for PostgreSQL-Einzelserver ausgegeben wird. Bei PostgreSQL-Einzelservern, die nicht über die gesamte Stunde hinweg ausgeführt werden, wird die Reservierung automatisch auf andere Azure Database for PostgreSQL-Einzelserver angewendet, die den Reservierungsattributen entsprechen. Der Rabatt kann auf gleichzeitig ausgeführte Azure Database for PostgreSQL-Einzelserver angewendet werden. Wenn Sie über keinen PostgreSQL-Einzelserver verfügen, der über die gesamte Stunde hinweg ausgeführt wird und den Reservierungsattributen entspricht, profitieren Sie nicht in vollem Umfang vom Reservierungsrabatt für diese Stunde.

Die folgenden Beispiele veranschaulichen, wie der Rabatt für reservierte Kapazität für Azure Database for PostgreSQL-Einzelserver angewendet wird – abhängig davon, wie viele Kerne Sie erworben haben und wann sie ausgeführt werden.

* **Beispiel 1:** Sie erwerben reservierte Kapazität für einen Azure Database for PostgreSQL-Einzelserver mit acht V-Kernen. Wenn Sie einen Azure Database for PostgreSQL-Einzelserver mit 16 V-Kernen ausführen, der den restlichen Attributen der Reservierung entspricht, werden Ihnen acht V-Kerne der Computenutzung Ihres PostgreSQL-Einzelservers zum Preis für die nutzungsbasierte Bezahlung in Rechnung gestellt, und Sie erhalten den Reservierungsrabatt für eine Stunde der Computenutzung von acht V-Kernen des PostgreSQL-Einzelservers.</br>

Bei den übrigen Beispielen wird davon ausgegangen, dass die reservierte Kapazität für Azure Database for PostgreSQL-Einzelserver, die Sie erwerben, für einen Azure Database for PostgreSQL-Einzelserver mit 16 V-Kernen gilt und die restlichen Reservierungsattribute den ausgeführten PostgreSQL-Einzelservern entsprechen.

* **Beispiel 2:** Sie führen eine Stunde lang zwei Azure Database for PostgreSQL-Einzelserver mit jeweils acht V-Kernen aus. Der Reservierungsrabatt für 16 V-Kerne wird auf die Computenutzung beider Azure Database for PostgreSQL-Einzelserver mit acht V-Kernen angewendet.

* **Beispiel 3:** Sie führen einen Azure Database for PostgreSQL-Einzelserver mit 16 V-Kernen von 13 Uhr bis 13:30 Uhr aus. Sie führen einen weiteren Azure Database for PostgreSQL-Einzelserver mit 16 V-Kernen von 13:30 Uhr bis 14 Uhr aus. Beide Instanzen sind durch den Reservierungsrabatt abgedeckt.

* **Beispiel 4:** Sie führen einen Azure Database for PostgreSQL-Einzelserver mit 16 V-Kernen von 13 Uhr bis 13:45 Uhr aus. Sie führen einen weiteren Azure Database for PostgreSQL-Einzelserver mit 16 V-Kernen von 13:30 Uhr bis 14 Uhr aus. Die zusätzlichen 15 Minuten werden Ihnen zu den Preisen der nutzungsbasierten Bezahlung in Rechnung gestellt. Für den restlichen Zeitraum wird der Reservierungsrabatt auf die Computenutzung angewendet.

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer Azure-Reservierungen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Grundlegendes zur Nutzung von Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt
Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).
