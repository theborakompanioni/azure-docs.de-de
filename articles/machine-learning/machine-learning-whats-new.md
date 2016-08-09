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
	ms.date="07/15/2016"
	ms.author="v-donglo"/>

# Neuerungen in Azure Machine Learning

Die Updates von Microsoft Azure Machine Learning (ML) vom Juli 2016 bieten die folgenden Features:

* Webdienste werden jetzt als Azure-Ressourcen über [Azure Resource Manager](../resource-group-overview.md)-Schnittstellen verwaltet, wodurch sich folgende Verbesserungen und Erweiterungen ergeben:
	* Es gibt neue [REST-APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) zum Bereitstellen und Verwalten Ihrer Resource Manager-basierten Webdienste.
	* Es gibt ein neues [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/)-Portal, das als Zentrale zum Verwalten aller Aspekte Ihres Webdiensts fungiert.
* Es gibt ein neues abonnementbasiertes Bereitstellungsmodell für Webdienste für mehrere Regionen, das Resource Manager-basierte APIs verwendet, die den Resource Manager-Ressourcenanbieter für Webdienste nutzen.
* Es gibt neue [Preispläne](https://azure.microsoft.com/pricing/details/machine-learning/) und Planverwaltungsfunktionen, die den neuen Resource Manager-Ressourcenanbieter für die Abrechnung nutzen.
	* Sie können nun [Ihren Webdienst in mehreren Regionen bereitstellen](machine-learning-how-to-deploy-to-multiple-regions.md), ohne für jede Region ein neues Abonnement erstellen zu müssen.
* Es wird ein Webdienst für [Verwendungsstatistiken](machine-learning-manage-new-webservice.md) bereitgestellt.
* Das Testen der RRS-Funktion von Azure ML mithilfe von Beispieldaten wurde vereinfacht.
* Es gibt eine neue BES-Testseite mit Beispieldaten und einem Auftragsübermittlungsverlauf.

Darüber hinaus wurde Machine Learning Studio so aktualisiert, dass Sie die Bereitstellung gemäß dem neuen Webdienstmodell oder weiter gemäß dem klassischen Webdienstmodell vornehmen können.

<!---HONumber=AcomDC_0727_2016-->