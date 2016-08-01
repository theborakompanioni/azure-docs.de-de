<properties
	pageTitle="Neuerungen in Machine Learning | Microsoft Azure"
	description="Neue Features, die in Azure Machine Learning verfügbar sind."
	services="machine-learning"
	documentationCenter=""
	authors="vDonGlover"
	manager="raymondl"
	editor=""/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/13/2016"
	ms.author="v-donglo"/>

# Neuerungen in Azure Machine Learning

Die Updates von Microsoft Azure Machine Learning (ML) vom Juli 2016 bieten die folgenden Features:

* Webdienste werden jetzt als Azure-Ressourcen über [Azure Resource Manager](../azure-portal/resource-group-overview.md)-Schnittstellen (ARM) verwaltet, was die folgenden Verbesserungen ermöglicht:
* Es gibt neue [REST-APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) zum Bereitstellen und Verwalten Ihrer ARM-basierten Webdienste.
* Es gibt ein neues [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/)-Portal, das als Zentrale zum Verwalten aller Aspekte Ihres Webdiensts fungiert. I
* Es gibt ein neues abonnementbasiertes Bereitstellungsmodell für Webdienste für mehrere Regionen, das ARM-basierte APIs verwendet, die den ARM-Ressourcenanbieter für Webdienste nutzen.
* Es gibt neue [Preispläne](https://azure.microsoft.com/pricing/details/machine-learning/) und Planverwaltungsfunktionen, die den neuen ARM-Ressourcenanbieter für die Abrechnung nutzen.
	* Sie können nun [Ihren Webdienst in mehreren Regionen bereitstellen](machine-learning-how-to-deploy-to-multiple-regions.md), ohne eine neues Abonnement für jede Region erstellen zu müssen.
* Der Webdienst [Usage Statistics](machine-learning-manage-new-webservice.md) wird geboten.
* Das Testen der RRS-Funktion von Azure ML mithilfe von Beispieldaten wurde vereinfacht.
* Es gibt eine neue BES-Testseite mit Beispieldaten und einem Auftragsübermittlungsverlauf.

Darüber hinaus wurde Machine Learning Studio so aktualisiert, dass Sie die Bereitstellung gemäß dem neuen Webdienstmodell oder weiter gemäß dem klassischen Webdienstmodell vornehmen können.

<!---HONumber=AcomDC_0720_2016-->