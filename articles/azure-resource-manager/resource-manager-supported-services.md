---
title: "Von Resource Manager unterstützte Dienste | Microsoft Docs"
description: "Beschreibt die Ressourcenanbieter, die den Ressourcen-Manager, die zugehörigen Schemas und verfügbaren API-Versionen sowie die Regionen unterstützen, die die Ressourcen hosten können."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: magoedte;tomfitz
translationtype: Human Translation
ms.sourcegitcommit: dabe7d9796ab24a257ea904bc5d978cb71d7e149
ms.openlocfilehash: af96c2b6063f7200cdeb6b51e5c729aa4643127f


---
# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Anbieter, Regionen, API-Versionen und Schemas für den Ressourcen-Manager
Der Azure-Ressourcen-Manager bietet eine neue Möglichkeit, die Dienste, aus denen Ihre Anwendungen bestehen, bereitzustellen und zu verwalten. Die meisten (jedoch nicht alle) Dienste unterstützen den Ressourcen-Manager, und einige Dienste unterstützen ihn nur teilweise. Dieses Thema enthält eine Liste der unterstützten Ressourcenanbieter für den Azure-Ressourcen-Manager.

Beim Bereitstellen der Ressourcen müssen Sie auch wissen, welche Bereiche diese Ressourcen unterstützen und welche API-Versionen für die Ressourcen verfügbar sind. Der Abschnitt [Unterstützte Regionen](#supported-regions) zeigt, wie Sie herausfinden, in welchen Regionen Ihr Abonnement und Ihre Ressourcen verwendet werden können. Im Abschnitt [Unterstützte API-Versionen](#supported-api-versions) wird gezeigt, wie Sie ermitteln, welche API-Versionen Sie verwenden können.

Informationen dazu, welche Dienste im Azure-Portal und im klassischen Portal unterstützt werden, finden Sie im [Verfügbarkeitsdiagramm für die Azure-Portale](https://azure.microsoft.com/features/azure-portal/availability/). Informationen dazu, welche Dienste das Verschieben von Ressourcen ermöglichen, finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

Die folgenden Tabellen zeigen, welche Microsoft-Dienste Bereitstellung und Verwaltung über Resource Manager unterstützen und welche nicht. Der Link in der Spalte **Schnellstartvorlagen** sendet eine Abfrage an das Azure-Repository mit Schnellstartvorlagen für den angegebenen Ressourcenanbieter. Schnellstartvorlagen werden häufig hinzugefügt und aktualisiert. Dass ein Link für einen bestimmten Dienst vorhanden ist, bedeutet nicht unbedingt, dass durch die Abfrage Vorlagen aus dem Repository zurückgegeben werden. Darüber hinaus gibt es zahlreiche Ressourcendrittanbieter, die Resource Manager unterstützen. Im Abschnitt [Ressourcenanbieter und -typen](#resource-providers-and-types) wird erläutert, wie Sie alle Ressourcenanbieter anzeigen.

## <a name="compute"></a>Compute
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| Batch |Ja |[Batch REST (in englischer Sprache)](/rest/api/batchservice) |[Batch-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json) | |
| Container |Ja |[Container Service REST](/rest/api/containerregistry) |[Containerschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) |[Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics Lifecycle Services |Ja | | | |
| Skalierungsgruppen |Ja |[Scale Set REST](/rest/api/compute/virtualmachinescalesets) |[Skalierungsgruppenschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |[virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) |
| Service Fabric |Ja |[Service Fabric REST](/rest/api/servicefabric) | [Service Fabric-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-09-01/Microsoft.ServiceFabric.json) |[Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtual Machines |Ja |[VM REST](/rest/api/compute/virtualmachines) |[VM-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |[virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuelle Computer (Klassisch) |Eingeschränkt |- |- |- |
| Remote App |Nein |- |- |- |
| Cloud Services (klassisch) |Eingeschränkt (siehe unten) |- |- |- |

"Virtuelle Computer (klassisch)" bezieht sich auf Ressourcen, die über das klassische Bereitstellungsmodell statt über das Ressourcen-Manager-Bereitstellungsmodell bereitgestellt wurden. Im Allgemeinen unterstützen diese Ressourcen keine Ressourcen-Manager-Vorgänge, es wurden jedoch einige Vorgänge aktiviert. Weitere Informationen zu diesen Bereitstellungsmodellen finden Sie unter [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](resource-manager-deployment-model.md). 

Clouddienste (klassisch) können mit anderen klassischen Ressourcen verwendet werden. Klassische Ressourcen nutzen allerdings nicht alle Resource Manager-Funktionen und sind keine gute Option für zukünftige Lösungen. Ändern Sie stattdessen Ihre Infrastruktur, um Ressourcen aus den Namespaces Microsoft.Compute, Microsoft.Storage und Microsoft.Network zu verwenden.

## <a name="networking"></a>Netzwerk
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| Application Gateway |Ja |[Application Gateway REST](https://msdn.microsoft.com/library/azure/mt684939.aspx) | |[applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS |Ja |[DNS REST](/rest/api/dns) |[DNS-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) |[dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute |Ja |[ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx) | |[expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Lastenausgleichsmodul |Ja |[Load Balancer REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) |[Lastenausgleichsschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |[loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Traffic Manager |Ja |[Traffic Manager REST](https://msdn.microsoft.com/library/azure/mt163667.aspx) |[Traffic Manager-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json) |[trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuelle Netzwerke |Ja |[Virtual Network REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) |[Virtuelles Netzwerkschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |[virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN Gateway |Ja |[Netzwerkgateway – REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) | |[virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[connections](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |

## <a name="storage"></a>Speicher
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- | --- |
| Speicher |Ja |[Storage REST](/rest/api/storagerp) |[Speicherkonto](resource-manager-template-storage.md) |[Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple |Ja | | | |

## <a name="databases"></a>Datenbanken
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- | --- |
| DocumentDB |Ja |[DocumentDB REST](/rest/api/documentdbresourceprovider) |[DocumentDB-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json) |[Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis-Cache |Ja | [Redis Cache REST](/rest/api/redis) |[Redis-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) |[Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-Datenbank |Ja |[SQL-Datenbank REST](/rest/api/sql) |[SQL-Datenbankschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) |[Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL Data Warehouse |Ja | | | |

## <a name="web--mobile"></a>Web- und mobile Anwendungen
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| API-Apps |Ja | [App Service REST](/rest/api/appservice) |[API-Apps-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[API-Apps](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API Management |Ja |[API Management REST](/rest/api/apimanagement) |[API Management-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json) |[Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) |
| Content Moderator |Ja | | | |
| Funktionen-App |Ja | [Funktionen-App-REST-API](/rest/api/appservice) | |[functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logik-Apps |Ja |[Logic Apps-REST-API](/rest/api/logic) |[Logic Apps-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) |[Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobile Apps |Ja | [App Service REST](/rest/api/appservice) |[Mobile Apps-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code) |
| Mobile Engagements |Ja |[Mobile Engagement REST](https://msdn.microsoft.com/library/azure/mt683754.aspx) | |[Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Suche |Ja |[Azure-Suchdienst-REST-API](/rest/api/searchservice) | [Suchschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-19/Microsoft.Search.json) |[Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web-Apps |Ja | [Web-Apps-REST-API](/rest/api/appservice/webapps) |[Web-Apps-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="intelligence--analytics"></a>Daten und Analyse
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| Analysis Services | Ja | [Analysis Services-REST-API](/rest/api/analysisservices) | [Analysis Services-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-16/Microsoft.AnalysisServices.json) | |
| Cognitive Services |Ja | [Cognitive Services REST](/rest/api/cognitiveservices) |[Cognitive Services-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) | |
| Datenkatalog |Ja |[Data Catalog REST](/rest/api/datacatalog) |[Data Catalog-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) | |
| Data Factory |Ja |[Data Factory REST](/rest/api/datafactory) | |[Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Data Lake Analytics |Ja | [Data Lake REST](/rest/api/datalakeanalytics) |[Data Lake-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) |[Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Data Lake-Speicher |Ja |[Data Lake Store REST](/rest/api/datalakestore) |[Data Lake-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) |[Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights |Ja |[HDInsights REST](/rest/api/hdinsight) | |[Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Machine Learning |Ja |[Machine Learning REST](/rest/api/machinelearning) |[Machine Learning-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) | |
| Stream Analytics |Ja |[Stream Analytics REST](/rest/api/streamanalytics) | | |
| Power BI |Ja |[Power BI Embedded REST](/rest/api/powerbiembedded) |[Power BI-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) | |


## <a name="internet-of-things"></a>Internet der Dinge
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| Event Hub |Ja |[Event Hub REST](/rest/api/eventhub) |[Event Hub-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) |[Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code) |
| IoTHubs |Ja |[IoT Hub REST](/rest/api/iothub) |[IoT Hub-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) |[Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Notification Hubs |Ja |[Notification Hub REST](/rest/api/notificationhubs) |[Notification Hub-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) |[Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Medien und CDN
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| CDN |Ja |[CDN REST](/rest/api/cdn) |[CDN-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) |[Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Mediendienst |Ja |[Media Services REST](/rest/api/media) |[Medienschema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) | |

## <a name="hybrid-integration"></a>Hybridintegration
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| BizTalk Services |Ja | |[BizTalk-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) | |
| Wiederherstellungsdienst |Ja |[Recovery Services-REST-API](/rest/api/recoveryservices) |[Recovery Services-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) |[Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Service Bus |Ja |[Service Bus REST](/rest/api/servicebus) |[Service Bus-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json) |[Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identitäts- und Zugriffsverwaltung
Azure Active Directory arbeitet mit dem Ressourcen-Manager zusammen, um die rollenbasierte Access Control für Ihr Abonnement zu aktivieren. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Entwicklerdienste
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| Überwachen |Ja |[Monitor-REST-API](/rest/api/monitor) |[Insights-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) |[Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing Maps |Ja | | | |
| DevTest Labs |Ja | [DevTest REST](/rest/api/dtl) |[DevTest Lab-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) |[Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code) |
| Visual Studio-Konto |Ja | |[Visual Studio-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) | |

## <a name="management-and-security"></a>Verwaltung und Sicherheit
| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- |
| Automation |Ja |[Automation REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) |[Automation-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json) |[Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Schlüsseltresor |Ja |[Referenz zur Schlüsseltresor-REST-API](/rest/api/keyvault) |[Key vault](resource-manager-template-keyvault.md)<br />[Geheimer Schlüssel im Tresor](resource-manager-template-keyvault-secret.md) |[Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Operational Insights |Ja | | | |
| Scheduler |Ja |[Scheduler REST](/rest/api/scheduler) |[Scheduler-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) |[Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Sicherheit (Vorschau) |Ja |[Security REST](https://msdn.microsoft.com/library/azure/mt704034.aspx) | |[Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code) |

## <a name="resource-manager"></a>Ressourcen-Manager
| Funktion | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstartvorlagen |
| --- | --- | --- | --- | --- | --- |
| Autorisierung |Ja |[Autorisierungs-REST-API](/rest/api/authorization) |[Ressourcensperre](resource-manager-template-lock.md)<br />[Rollenzuweisungen](resource-manager-template-role.md) |[Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Ressourcen |Ja |[Ressourcen-REST-API](/rest/api/resources) |[Ressourcenlinks](resource-manager-template-links.md) |[Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |

## <a name="resource-providers-and-types"></a>Ressourcenanbieter und -typen
Beim Bereitstellen von Ressourcen müssen Sie häufig Informationen zu den Ressourcenanbietern und -typen abrufen. Sie können diese Informationen über die REST-API, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle abrufen.

Damit Sie mit einem Ressourcenanbieter arbeiten können, muss dieser Ressourcenanbieter in Ihrem Konto registriert werden. Viele Ressourcenanbieter werden standardmäßig automatisch registriert, dennoch müssen Sie unter Umständen einige Ressourcenanbieter manuell registrieren. In den nachfolgenden Beispielen wird gezeigt, wie der Registrierungsstatus eines Ressourcenanbieters abgerufen und der Ressourcenanbieter bei Bedarf registriert wird.

### <a name="portal"></a>Portal
Sie können eine Liste der unterstützten Ressourcenanbieter anzeigen, indem Sie auf dem Abonnementblatt **Ressourcenanbieter** auswählen. Zum Registrieren des Abonnements bei einem Ressourcenanbieter wählen Sie den Link **Registrieren**.
   
![Liste der Ressourcenanbieter](./media/resource-manager-supported-services/view-resource-providers.png)

### <a name="rest-api"></a>REST-API
Verwenden Sie zum Abrufen aller verfügbaren Ressourcenanbieter, einschließlich Typen, Speicherorten, API-Versionen und Registrierungsstatus, den Vorgang zum [Auflisten aller Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_List). Informationen zum Registrieren eines Ressourcenanbieters finden Sie unter [Registrieren eines Abonnements bei einem Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_Register).

### <a name="powershell"></a>PowerShell
Im folgenden Beispiel wird veranschaulicht, wie Sie alle verfügbaren Ressourcenanbieter abrufen.

    Get-AzureRmResourceProvider -ListAvailable


Im nächsten Beispiel wird gezeigt, wie die Ressourcentypen für einen bestimmten Ressourcenanbieter abgerufen werden.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes

Die Ausgabe sieht in etwa wie folgt aus:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...

Geben Sie zum Registrieren eines Ressourcenanbieters den Namespace an:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Im folgenden Beispiel wird veranschaulicht, wie Sie alle verfügbaren Ressourcenanbieter abrufen.

    azure provider list

Die Ausgabe sieht in etwa wie folgt aus:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Mit dem folgenden Befehl können Sie die Informationen für einen bestimmten Ressourcenanbieter in einer Datei speichern:

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Geben Sie zum Registrieren eines Ressourcenanbieters den Namespace an:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Unterstützte Regionen
Bei der Bereitstellung von Ressourcen müssen Sie in der Regel eine Region für die Ressourcen angeben. Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen. Diese Einschränkungen können im Zusammenhang mit steuerlichen Problemen in Ihrem Land stehen, oder Sie sind das Ergebnis einer Richtlinie, die von Ihrem Abonnementadministrator implementiert wurde, sodass Sie nur bestimmte Regionen verwenden können. 

Eine vollständige Liste aller unterstützten Regionen für alle Azure-Dienste finden Sie unter [Dienste nach Region](https://azure.microsoft.com/regions/#services). Allerdings kann diese Liste Regionen enthalten, die Ihr Abonnement nicht unterstützt. Sie können über das Portal, die REST-API, PowerShell oder die Azure-CLI die Regionen für einen bestimmten Ressourcentyp festlegen, den Ihr Abonnement unterstützt.

### <a name="portal"></a>Portal
Sie können die unterstützten Regionen für einen Ressourcentyp über die folgenden Schritte anzeigen:

1. Wählen Sie **More services (Weitere Dienste)** > **Ressourcen-Explorer**.
   
    ![Ressourcen-Explorer](./media/resource-manager-supported-services/select-resource-explorer.png)
2. Öffnen Sie den Knoten **Anbieter**.
   
    ![Anbieter auswählen](./media/resource-manager-supported-services/select-providers.png)
3. Wählen Sie einen Ressourcenanbieter aus, und zeigen Sie die unterstützten Regionen und API-Regionen an.
   
    ![Anbieter anzeigen](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST-API
Verwenden Sie für die Ermittlung der verfügbaren Regionen für einen bestimmten Ressourcentyp im Abonnement den Vorgang [Auflisten aller Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_List) . 

### <a name="powershell"></a>PowerShell
Das folgende Beispiel zeigt, wie Sie die unterstützten Regionen für Websites abrufen.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Die Ausgabe sieht in etwa wie folgt aus:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Im folgenden Beispiel werden alle unterstützten Speicherorte für jeden Ressourcentyp zurückgegeben.

    azure location list

Sie können das Ortsergebnis auch mit einem JSON-Hilfsprogramm wie [jq](https://stedolan.github.io/jq/)filtern.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Ausgabe des Befehls:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Unterstützte API-Versionen
Beim Bereitstellen einer Vorlage müssen Sie eine API-Version angeben, die zum Erstellen der einzelnen Ressourcen verwendet werden soll. Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. Deshalb wirkt sich die Version der API, die Sie in Ihrer Vorlage angeben, darauf aus, welche Eigenschaften Sie in der Vorlage angeben können. Im Allgemeinen sollten Sie beim Erstellen von Vorlagen die neueste Version der API wählen. Für vorhandene Vorlagen können Sie entscheiden, ob Sie weiterhin eine frühere Version der API verwenden oder die Vorlage für die aktuelle Version aktualisieren möchten, um die neuen Features zu nutzen.

### <a name="portal"></a>Portal
Sie legen die unterstützten API-Versionen genau wie unterstützte Regionen fest (weiter oben erläutert).

### <a name="rest-api"></a>REST-API
Verwenden Sie den Vorgang zum [Auflisten aller Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_List) , um zu ermitteln, welche API-Versionen für Ressourcentypen verfügbar sind. 

### <a name="powershell"></a>PowerShell
Im folgenden Beispiel wird veranschaulicht, wie Sie die verfügbaren API-Versionen für einen bestimmten Ressourcentyp abrufen.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

Die Ausgabe sieht in etwa wie folgt aus:

    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie können die Informationen (einschließlich der verfügbaren API-Versionen) für einen Ressourcenanbieter mit dem folgenden Befehl in einer Datei speichern:

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Sie können die Datei öffnen und nach dem Element **apiVersions** suchen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).




<!--HONumber=Dec16_HO2-->


