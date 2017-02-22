---
title: Microsoft Azure Government-Entwicklerhandbuch | Microsoft-Dokumentation
description: "Dieser Artikel enthält einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government."
services: azure-government
cloud: gov
documentationcenter: 
author: smichelotti
manager: zakramer
ms.assetid: 6e04e9aa-1a73-442c-a46c-2e4ff87e58d5
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/13/2017
ms.author: stemi
translationtype: Human Translation
ms.sourcegitcommit: 0bab116e977a8490bf0c114c8b829aa1ce07e923
ms.openlocfilehash: 41af18426ce3f8124458a61c3af280c3dd17cfcd


---
# <a name="azure-government-developer-guide"></a>Azure Government-Entwicklerhandbuch
Bei der Azure Government-Umgebung handelt es sich um eine physische Instanz, die vom Rest des Microsoft-Netzwerks getrennt ist. Dieses Handbuch erläutert die Unterschiede, die Anwendungsentwickler und -administratoren kennen müssen, um mit separaten Regionen von Azure zu interagieren und zu arbeiten.

## <a name="overview"></a>Übersicht
Azure Government ist eine separate Instanz des Microsoft Azure-Diensts. Sie ist für die Anforderungen an Sicherheit und Compliance von Bundes-, Landes- und Kommunalbehörden der USA und deren Lösungsanbieter vorgesehen. Azure Government bietet die physische Isolation und Netzwerkisolation von nicht behördlichen Bereitstellungen sowie überprüftes US-Personal.

Microsoft bietet verschiedene Tools, die Entwicklern beim Erstellen und Bereitstellen von Cloudanwendungen für den globalen Microsoft Azure-Dienst („globaler Dienst“) und Microsoft Azure Government-Dienste helfen.

Wenn Entwickler Anwendungen für Azure Government-Dienste erstellen und bereitstellen, müssen sie anders als beim globalen Dienst die wichtigsten Unterschiede zwischen den beiden Diensten kennen. Dies gilt insbesondere für die folgenden Bereiche: Einrichtung und Konfiguration der Programmierumgebung, Konfigurieren der Endpunkte, Erstellen von Anwendungen sowie deren Bereitstellung als Dienste für Azure Government.

Die Informationen in diesem Dokument fassen die Unterschiede zwischen den beiden Diensten zusammen. Das Dokument ergänzt die Informationen, die auf der [Azure Government](http://www.azure.com/gov "Azure Government")-Website und in der [technischen Bibliothek für Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") auf der MSDN-Website verfügbar sind. Möglicherweise sind auch an vielen anderen Orten wie dem [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center"), [Azure-Dokumentationscenter](https://azure.microsoft.com/documentation/) und in [Azure-Blogs](https://azure.microsoft.com/blog/ "Azure-Blogs") offizielle Informationen verfügbar.

Dieser Inhalt ist für Partner und Entwickler für Bereitstellungen mit Microsoft Azure Government konzipiert.

## <a name="guidance-for-developers"></a>Anleitung für Entwickler
Bei den meisten der derzeit verfügbaren technischen Inhalte wird davon ausgegangen, dass Anwendungen für den globalen Dienst und nicht für Azure Government entwickelt werden. Aus diesem Grund ist es wichtig, zwei Hauptunterschiede bei Anwendungen zu kennen, die Sie zum Hosten in Azure Government entwickeln.

* Bestimmte Dienste und Funktionen, die in bestimmten Regionen des globalen Diensts vorhanden sind, sind möglicherweise in Azure Government nicht verfügbar sein.
* Die Featurekonfigurationen in Azure Government können sich von denen im globalen Dienst unterscheiden. Aus diesem Grund sollten Sie Ihren Beispielcode, Ihre Konfigurationen und Schritte überprüfen, um sicherzustellen, dass die Erstellung und Ausführung innerhalb der Azure Government Cloud Services-Umgebung erfolgt.

Derzeit wird Azure Government von den Rechenzentren von US Gov Iowa und US Gov Virginia unterstützt. Aktuelle Rechenzentren und verfügbare Dienste finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services).


## <a name="endpoint-mapping"></a>Endpunktzuordnung
In der folgenden Tabelle finden Sie Informationen für die Zuordnung von öffentlichen Azure- und SQL-Datenbank-Endpunkten zu Azure Government-spezifischen Endpunkten:

| Name | Azure Government-Endpunkt |
| --- | --- |
| ActiveDirectoryServiceEndpointResourceId  | https://management.core.usgovcloudapi.net/ |
| GalleryUrl | https://gallery.usgovcloudapi.net/ |
| ManagementPortalUrl | https://manage.windowsazure.us |
| ServiceManagementUrl | https://management.core.usgovcloudapi.net/ |
| PublishSettingsFileUrl | https://manage.windowsazure.us/publishsettings/index |
| ResourceManagerUrl | https://management.usgovcloudapi.net/ |
| SqlDatabaseDnsSuffix | .database.usgovcloudapi.net |
| StorageEndpointSuffix | core.usgovcloudapi.net |
| ActiveDirectoryAuthority | https://login-us.microsoftonline.com/ |
| GraphUrl | https://graph.windows.net/ |
| GraphEndpointResourceId | https://graph.windows.net/ |
| TrafficManagerDnsSuffix | usgovtrafficmanager.net |
| AzureKeyVaultDnsSuffix | vault.usgovcloudapi.net |
| AzureKeyVaultServiceEndpointResourceId | https://vault.usgovcloudapi.net |



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Government finden Sie in den folgenden Ressourcen:

* [Registrieren Sie sich für eine Testversion](https://azuregov.microsoft.com/trial/azuregovtrial)
* [Erwerben von und Zugreifen auf Azure Government](http://azure.com/gov)
* [Azure Government – Übersicht](/azure-government-overview)
* [Azure Government – Blog](http://blogs.msdn.microsoft.com/azuregov/)
* [Azure – Compliance](https://www.microsoft.com/trustcenter/compliance/complianceofferings)



<!--HONumber=Feb17_HO3-->


