---
title: "Entwicklerhandbuch für Azure Government"
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: 
cloud: gov
documentationcenter: 
author: Joharve2
manager: Chrisnie
editor: 
ms.assetid: 6e04e9aa-1a73-442c-a46c-2e4ff87e58d5
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 10/29/2015
ms.author: jharve
translationtype: Human Translation
ms.sourcegitcommit: 08de3cb0d62e800db88238764e4f9f221d2add06
ms.openlocfilehash: 42c1b022a22cefae15d32690856bdb80b0eaffa9


---
# <a name="microsoft-azure-government-developer-guide"></a>Microsoft Azure Government-Entwicklerhandbuch
<p> Bei der Azure Government-Umgebung handelt es sich um eine physische Instanz, die vom Rest des Microsoft-Netzwerks getrennt ist.  Dieses Entwicklerhandbuch bietet Informationen zu den Unterschieden, die für Entwickler und Administratoren erforderlich sind, um mit diesen unterschiedlichen Bereichen von Azure interagieren und arbeiten zu können.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>In diesem Thema
* [Übersicht](#Overview)
* [Anleitung für Entwickler](#Guidance)
* [Derzeit in Microsoft Azure Government verfügbare Funktionen](#Features)
* [Endpunktzuordnung](#Endpoint)
* [Nächste Schritte](#next)

## <a name="a-nameoverviewaoverview"></a><a name="Overview"></a>Übersicht
Microsoft Azure Government ist eine separate Instanz des Microsoft Azure Service, die auf die Anforderungen an die Sicherheit und Kompatibilität von Bundes-, Landes- und kommunalen Behörden sowie deren Lösungsanbietern ausgerichtet ist. Azure Government bietet die physische Isolation und Netzwerkisolation von nicht behördlichen Bereitstellungen sowie überprüftes US-Personal.

Microsoft bietet eine Reihe von Tools zum Erstellen und Bereitstellen von Cloud-Anwendungen für den globalen Microsoft Azure Service ("globaler Dienst") und Microsoft Azure Government.

Beim Erstellen und Bereitstellen von Anwendungen für Azure Government-Dienste müssen die Entwickler anders als beim globalen Dienst die wichtigsten Unterschiede zwischen den beiden Diensten kennen.  Dies gilt insbesondere bei der Einrichtung und Konfiguration der Programmierumgebung, beim Konfigurieren der Endpunkte, beim Erstellen von Anwendungen sowie bei deren Bereitstellung als Dienste für Azure Government.

Die Informationen in diesem Dokument fassen diese Unterschiede zusammen und ergänzen die Informationen, die auf der [Azure Government](http://www.azure.com/gov "Azure Government")-Website und in der [Technischen Bibliothek für Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") auf der MSDN-Website verfügbar sind. Möglicherweise sind auch an vielen anderen Orten wie dem [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center"/), dem [Azure-Dokumentationscenter](https://azure.microsoft.com/documentation/) und in [Azure-Blogs](https://azure.microsoft.com/blog/ "Azure Blogs"/) offizielle Informationen verfügbar.

Dieser Inhalt ist für Partner und Entwickler für Bereitstellungen mit Microsoft Azure Government konzipiert.

## <a name="a-nameguidanceaguidance-for-developers"></a><a name="Guidance"></a>Anleitung für Entwickler
Da die meisten technischen Inhalte, die derzeit verfügbar sind, davon ausgehen, dass Anwendungen für den globalen Dienst und nicht für Microsoft Azure Government entwickelt werden, müssen Sie sicherstellen, dass Entwickler die wichtigsten Unterschiede für Anwendungen beachten, die in Azure Government gehostet werden.

* Zunächst gibt es Unterschiede bei den Diensten und Funktionen. Daher sind bestimmte Funktionen, die in bestimmten Regionen des globalen Diensts verfügbar sind, in Azure Government möglicherweise nicht enthalten.
* Für Funktionen in Azure Government gibt es zudem hinsichtlich der Konfiguration Unterschiede zum globalen Dienst.  Aus diesem Grund sollten Sie Ihren Beispielcode, Ihre Konfigurationen und Schritte überprüfen, um sicherzustellen, dass Sie innerhalb der Azure Government Cloud Services-Umgebung erstellen und ausführen.

## <a name="a-namefeaturesa-features-currently-available-in-microsoft-azure-government"></a><a name="Features"></a> Derzeit in Microsoft Azure Government verfügbare Funktionen
Azure Government verfügt derzeit in den Regionen US GOV IOWA und US GOV VIRGINIA über die folgenden Dienste:

* Virtual Machines
* Skalierungsgruppen für virtuelle Computer
* Containerdienste
* Batch-Konten
* Remote App-Sammlungen
* Verfügbarkeitsgruppen
* Virtuelle Netzwerke
* Load Balancer
* Anwendungsgateways
* Virtual Network-Gateways
* Lokale Netzwerkgateways
* Routentabellen
* Traffic Manager-Profile
* ExpressRoute-Verbindungen
* Netzwerksicherheitsgruppe
* Netzwerkschnittstellen
* Öffentliche IP-Adressen
* Verbindungen
* Speicherkonten
* StorSimple Manager
* App-Dienste
* Media Services
* SQL-DATENBANKEN
* SQL Data Warehouse-Instanzen
* SQL Server Stretch-Datenbanken
* Redis Caches
* Elastische SQL-Pools
* SQL-Server
* Log Analytics
* Event Hubs
* Service Bus-Namespaces
* Azure Active Directory
* Multi-Factor Authentication
* Rights Management
* Automation-Konten
* Marketplace

Es sind auch noch andere Dienste verfügbar und es werden zudem kontinuierlich weitere Dienste hinzugefügt.  Die aktuelle Liste der Dienste finden Sie in der [Regionsübersicht](https://azure.microsoft.com/regions/#services) , in der die einzelnen verfügbaren Regionen und ihre Dienste hervorgehoben sind.

Derzeit wird Azure Government von den Rechenzentren von US GOV Iowa und US GOV Virginia unterstützt.  Informationen zu aktuell verfügbaren Rechenzentren und Diensten finden Sie in der obigen Regionsübersicht.

## <a name="a-nameendpointaendpoint-mapping"></a><a name="Endpoint"></a>Endpunktzuordnung
Verwenden Sie die folgende Tabelle als Anleitung für die Zuordnung von öffentlichen Microsoft Azure- und SQL-Datenbank-Endpunkten zu Azure Government-spezifischen Endpunkten.

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

* Weitere Informationen zur Azure Resource Manager-Authentifizierung über Azure AD finden Sie unter [Authentifizieren von Anforderungen des Azure Resource Managers](https://msdn.microsoft.com/library/azure/dn790557.aspx).

## <a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte
Wenn Sie mehr über Azure Government erfahren möchten, nutzen Sie einige der unten aufgeführten Links.

* **[Registrieren Sie sich für eine Testversion](https://azuregov.microsoft.com/trial/azuregovtrial)**
* **[Erwerben von und Zugreifen auf Azure Government](http://azure.com/gov)**
* **[Azure Government – Übersicht](/azure-government-overview)**
* **[Azure Government – Blog](http://blogs.msdn.com/b/azuregov/)**
* **[Azure – Compliance](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md



<!--HONumber=Dec16_HO2-->


