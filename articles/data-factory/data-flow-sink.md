---
title: Senkentransformation in einem Zuordnungsdatenfluss
description: Hier erfahren Sie, wie Sie im Zuordnungsdatenfluss eine Senkentransformation konfigurieren.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 1c65a456270cdca345504c07b927a7ef7e1f725b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75440268"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>Senkentransformation in einem Zuordnungsdatenfluss

Nach der Transformation Ihrer Daten können Sie die Daten in ein Zieldataset als Senke übertragen. Jeder Datenfluss erfordert mindestens eine Senkentransformation. Sie können aber so viele Senken wie erforderlich hinzufügen, um Ihren Transformationsfluss zu realisieren. Um in weitere Senken zu schreiben, erstellen Sie über neue Verzweigungen und bedingte Teilungen neue Datenströme.

Jede Senkentransformation ist genau einem Data Factory-Dataset zugeordnet. Das Dataset definiert die Form und Position der Daten, in die geschrieben werden soll.

## <a name="supported-sink-connectors-in-mapping-data-flow"></a>Unterstützte Senkenconnectors im Zuordnungsdatenfluss

Derzeit können die folgenden Datasets in einer Senkentransformation verwendet werden:
    
* [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL-Datenbank](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

Die für diese Connectors spezifischen Einstellungen befinden sich auf der Registerkarte **Einstellungen**. Informationen zu diesen Einstellungen finden Sie in der Connector-Dokumentation. 

Azure Data Factory hat Zugriff auf über [90 native Connectors](connector-overview.md). Um Daten in diese anderen Quellen ausgehend von Ihrem Datenfluss zu schreiben, verwenden Sie die Kopieraktivität, um die Daten nach Abschluss des Datenflusses aus einem der unterstützten Stagingbereiche zu laden.

## <a name="sink-settings"></a>Senkeneinstellungen

Nachdem Sie eine Senke hinzugefügt haben, konfigurieren Sie die Senke über die Registerkarte **Senke**. Hier können Sie das Dataset auswählen oder erstellen, in das die Senke schreibt. 

![Senkeneinstellungen](media/data-flow/sink-settings.png "Senkeneinstellungen")

**Schemaabweichung**: [Schemaabweichung](concepts-data-flow-schema-drift.md) ist die Fähigkeit von Data Factory, flexible Schemas in Ihren Datenflüssen nativ zu verarbeiten, ohne explizit Spaltenänderungen definieren zu müssen. Aktivieren Sie **Schemaabweichung zulassen**, wenn Sie zusätzliche Spalten zusätzlich zur Definition im Datenschema der Senke schreiben möchten.

**Schema überprüfen**: Ist „Schema überprüfen“ aktiviert, schlägt der Datenfluss fehl, wenn eine Spalte im definierten Schema des Datasets nicht gefunden wurde.

## <a name="field-mapping"></a>Feldzuordnung

Ähnlich wie bei einer Auswahltansformation können Sie auf der Registerkarte **Zuordnung** der Senke festlegen, welche eingehenden Spalten geschrieben werden. Standardmäßig werden alle Eingabespalten – auch abweichende Spalten – zugeordnet. Dies wird als **automatische Zuordnung** bezeichnet.

Wenn Sie die automatische Zuordnung deaktivieren, können Sie eine feste spaltenbasierte Zuordnung oder eine regelbasierte Zuordnung hinzuzufügen. Die regelbasierte Zuordnung ermöglicht das Schreiben von Ausdrücken mit Musterabgleich, während bei der festen Zuordnung logische und physische Spaltennamen zugeordnet werden. Weitere Informationen zur regelbasierten Zuordnung finden Sie unter [Spaltenmuster im Zuordnungsdatenfluss](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink).

## <a name="data-preview-in-sink"></a>Datenvorschau in Senken

Beim Abrufen einer Datenvorschau in einem Debugcluster werden keine Daten in die Senke geschrieben. Es wird eine Momentaufnahme der Daten zurückgegeben, aber nichts in das Ziel geschrieben. Um das Schreiben von Daten in die Senke zu überprüfen, führen Sie einen Pipelinedebugvorgang in der Pipelinecanvas aus.

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun Ihren Datenfluss erstellt haben, fügen Sie [Ihrer Pipeline eine Aktivität zur Ausführung eines Datenflusses](concepts-data-flow-overview.md) hinzu.
