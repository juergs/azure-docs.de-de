---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 12/04/2019
ms.openlocfilehash: 06800e7bb18634f1cbe847ced5450172106ec6f9
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75840648"
---
**Computeziele können für mehrere Trainingsaufträge wiederverwendet werden.** Beispielsweise können Sie einen virtuellen Remotecomputer, den Sie an Ihren Arbeitsbereich anfügen, für mehrere Aufträge wiederverwenden.  Verwenden Sie für Machine Learning-Pipelines den entsprechenden [Pipelineschritt](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) für jedes Computeziel.

|Trainings&nbsp;ziele|[Automatisiertes Machine Learning](../articles/machine-learning/concept-automated-ml.md) | [ML-Pipelines](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning-Designer](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Lokaler Computer](../articles/machine-learning/how-to-set-up-training-targets.md#local)| ja | &nbsp; | &nbsp; |
|[Azure Machine Learning-Computeinstanz (Vorschau)](../articles/machine-learning/concept-compute-instance.md) | | ja |  |
|[Azure Machine Learning Compute-Cluster](../articles/machine-learning/how-to-set-up-training-targets.md#amlcompute)| Ja & <br/>Hyperparameteroptimierung&nbsp; | ja | ja |
|[Remote-VM](../articles/machine-learning/how-to-set-up-training-targets.md#vm) | Ja & <br/>Hyperparameteroptimierung | ja | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-create-your-first-pipeline.md#databricks)| Ja (nur lokaler SDK-Modus) | ja | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-create-your-first-pipeline.md#adla) | &nbsp; | ja | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-set-up-training-targets.md#hdinsight) | &nbsp; | ja | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-set-up-training-targets.md#azbatch) | &nbsp; | ja | &nbsp; |
