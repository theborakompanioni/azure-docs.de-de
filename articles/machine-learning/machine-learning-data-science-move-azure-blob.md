<properties
	pageTitle="Verschieben von Daten in den und aus dem Azure-Blobspeicher | Microsoft Azure"
	description="Verschieben von Daten in den und aus dem Azure-Blobspeicher"
	services="machine-learning,storage"
	documentationCenter=""
	authors="bradsev"
	manager="jhubbard"
	editor="cgronlun" />

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/14/2016"
	ms.author="bradsev;sachouks" />

# Verschieben von Daten in den und aus dem Azure-Blobspeicher

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Nachstehend finden Sie Links zu Anleitungen für Technologien zum Verschieben von Daten in den und aus dem Azure-BLOB-Speicher:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Es hängt vom jeweiligen Szenario ab, welche Methode für Sie am besten geeignet ist. Mithilfe der Informationen im Artikel [Szenarien für erweiterte Analysen in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) können Sie die Ressourcen ermitteln, die Sie für verschiedene, im erweiterten Analyseprozess verwendeten Data Science-Workflows benötigen.

> [AZURE.NOTE] Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Erste Schritte mit Azure Blob Storage](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Als Alternative können Sie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) für folgende Zwecke verwenden:

- Erstellen und Planen einer Pipeline, die Daten aus Azure- Blobspeicher herunterlädt
- Übergeben dieser Daten an einen veröffentlichten Azure Machine Learning-Webdienst
- Abrufen der Predictive Analytics-Ergebnisse und
- Hochladen der Ergebnisse in den Speicher

Weitere Informationen finden Sie unter [Erstellen von Vorhersagepipelines mithilfe von Azure Data Factory und Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## Voraussetzungen

In diesem Dokument wird davon ausgegangen, dass Sie über ein Azure-Abonnement, ein Speicherkonto und den zugehörigen Speicherschlüssel für dieses Konto verfügen. Bevor Sie Daten hoch- und herunterladen können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen.

- Informationen zum Einrichten eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
- Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

<!---HONumber=AcomDC_0921_2016-->