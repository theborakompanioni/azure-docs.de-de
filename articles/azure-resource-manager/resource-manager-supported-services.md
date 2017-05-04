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
ms.date: 03/20/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: bf4e400e30cc93ca8fa0cc727ada0f1b224b05cb
ms.lasthandoff: 04/20/2017


---
# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Anbieter, Regionen, API-Versionen und Schemas für den Ressourcen-Manager
Dieses Thema enthält eine Liste der Ressourcenanbieter, die Azure Resource Manager unterstützen.

Beim Bereitstellen der Ressourcen müssen Sie auch wissen, welche Bereiche diese Ressourcen unterstützen und welche API-Versionen für die Ressourcen verfügbar sind. Der Abschnitt [Unterstützte Regionen](#supported-regions) zeigt, wie Sie herausfinden, in welchen Regionen Ihr Abonnement und Ihre Ressourcen verwendet werden können. Im Abschnitt [Unterstützte API-Versionen](#supported-api-versions) wird gezeigt, wie Sie ermitteln, welche API-Versionen Sie verwenden können.

Informationen dazu, welche Dienste im Azure-Portal und im klassischen Portal unterstützt werden, finden Sie im [Verfügbarkeitsdiagramm für die Azure-Portale](https://azure.microsoft.com/features/azure-portal/availability/). Informationen dazu, welche Dienste das Verschieben von Ressourcen ermöglichen, finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

Die folgenden Tabellen zeigen, welche Microsoft-Dienste Bereitstellung und Verwaltung über Resource Manager unterstützen und welche nicht. Darüber hinaus gibt es zahlreiche Ressourcendrittanbieter, die Resource Manager unterstützen. Im Abschnitt [Ressourcenanbieter und -typen](#resource-providers-and-types) wird erläutert, wie Sie alle Ressourcenanbieter anzeigen.

## <a name="compute"></a>Compute
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| Batch |Ja |[Batch REST (in englischer Sprache)](/rest/api/batchservice) |[Batch-Ressourcen](/azure/templates/microsoft.batch/batchaccounts) |
| Containerregistrierung |Ja |[Containerregistrierung – REST](/rest/api/containerregistry) |[Containerregistrierungsressourcen](/azure/templates/microsoft.containerregistry/registries) |
| Container Service |Ja |[Container Service REST](/rest/api/compute/containerservices) |[Container Service-Ressourcen](/azure/templates/microsoft.containerservice/containerservices) |
| Dynamics Lifecycle Services |Ja | | |
| Skalierungsgruppen |Ja |[Scale Set REST](/rest/api/virtualmachinescalesets/) |[Skalierungsgruppenressourcen](/azure/templates/microsoft.compute/virtualmachinescalesets) |
| Service Fabric |Ja |[Service Fabric REST](/rest/api/servicefabric) | [Service Fabric-Ressourcen](/azure/templates/microsoft.servicefabric/clusters) |
| Virtual Machines |Ja |[VM REST](/rest/api/compute/virtualmachines) |[VM-Ressourcen](/azure/templates/microsoft.compute/virtualmachines) |
| Virtuelle Computer (Klassisch) |Eingeschränkt |- |- |
| Remote App |Nein |- |- |
| Cloud Services (klassisch) |Eingeschränkt (siehe unten) |- |- |

"Virtuelle Computer (klassisch)" bezieht sich auf Ressourcen, die über das klassische Bereitstellungsmodell statt über das Ressourcen-Manager-Bereitstellungsmodell bereitgestellt wurden. Im Allgemeinen unterstützen diese Ressourcen keine Ressourcen-Manager-Vorgänge, es wurden jedoch einige Vorgänge aktiviert. Weitere Informationen zu diesen Bereitstellungsmodellen finden Sie unter [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](resource-manager-deployment-model.md). 

Clouddienste (klassisch) können mit anderen klassischen Ressourcen verwendet werden. Klassische Ressourcen nutzen allerdings nicht alle Resource Manager-Funktionen und sind keine gute Option für zukünftige Lösungen. Ändern Sie stattdessen Ihre Infrastruktur, um Ressourcen aus den Namespaces Microsoft.Compute, Microsoft.Storage und Microsoft.Network zu verwenden.

## <a name="networking"></a>Netzwerk
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| Application Gateway |Ja |[Application Gateway REST](https://msdn.microsoft.com/library/azure/mt684939.aspx) | [Application Gateway-Ressourcen](/azure/templates/microsoft.network/applicationgateways) |
| DNS |Ja |[DNS REST](/rest/api/dns) |[DNS-Ressourcen](/azure/templates/microsoft.network/dnszones) |
| ExpressRoute |Ja |[ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx) | [ExpressRoute-Ressourcen](/azure/templates/microsoft.network/expressroutecircuits) |
| Lastenausgleichsmodul |Ja |[Load Balancer REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) |[Load Balancer-Ressourcen](/azure/templates/microsoft.network/loadbalancers) |
| Traffic Manager |Ja |[Traffic Manager REST](https://msdn.microsoft.com/library/azure/mt163667.aspx) |[Traffic Manager-Ressourcen](/azure/templates/microsoft.network/trafficmanagerprofiles) |
| Virtuelle Netzwerke |Ja |[Virtual Network REST](https://msdn.microsoft.com/library/azure/mt163650.aspx) |[Virtual Network-Ressourcen](/azure/templates/microsoft.network/virtualnetworks) |
| Netzwerkgateway |Ja |[Netzwerkgateway – REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) | [Verbindungsressourcen](/azure/templates/microsoft.network/connections) <br /> [Ressourcen für Gateways des lokalen Netzwerks](/azure/templates/microsoft.network/localnetworkgateways) <br /> [Ressourcen für Gateways des virtuellen Netzwerks](/azure/templates/microsoft.network/virtualnetworkgateways) |

## <a name="storage"></a>Speicher
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- | --- |
| Import/Export | Ja | [Import Export REST](/rest/api/storageimportexport/) | [Import/Export-Ressourcen](/azure/templates/microsoft.importexport/jobs) |
| Speicher |Ja |[Storage REST](/rest/api/storagerp) |[Storage-Ressourcen](/azure/templates/microsoft.storage/storageaccounts) |
| StorSimple |Ja | | |

## <a name="databases"></a>Datenbanken
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- | --- |
| DocumentDB |Ja |[DocumentDB REST](/rest/api/documentdbresourceprovider) |[DocumentDB-Ressourcen](/azure/templates/microsoft.documentdb/databaseaccounts) |
| Redis Cache |Ja | [Redis Cache REST](/rest/api/redis) |[Redis-Ressourcen](/azure/templates/microsoft.cache/redis) |
| SQL-Datenbank |Ja |[SQL-Datenbank REST](/rest/api/sql) |[SQL-Datenbank-Ressourcen](/azure/templates/microsoft.sql/servers) |
| SQL Data Warehouse |Ja | | |

## <a name="web--mobile"></a>Web- und mobile Anwendungen
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| API-Apps |Ja | [App Service REST](/rest/api/appservice) |[Webressourcen](/azure/templates/microsoft.web/sites) |
| API Management |Ja |[API Management REST](/rest/api/apimanagement) |[API Management-Ressourcen](/azure/templates/microsoft.apimanagement/service) |
| Zertifikatregistrierung | Ja | [Zertifikatregistrierung – REST](/rest/api/appservice/appservicecertificateorders) | [Zertifikatregistrierungsressourcen](/azure/templates/microsoft.certificateregistration/certificateorders)  |
| Content Moderator |Ja | | | |
| Domänenregistrierung | Ja | [Domänenregistrierung](/rest/api/appservice/domains) | [Domänenregistrierungsressourcen](/azure/templates/microsoft.domainregistration/domains)  |
| Funktionen-App |Ja | [Funktionen-App-REST-API](/rest/api/appservice) | [Webressourcen](/azure/templates/microsoft.web/sites) |
| Logik-Apps |Ja |[Logic Apps-REST-API](/rest/api/logic) |[Logic Apps-Ressourcen](/azure/templates/microsoft.logic/workflows) |
| Mobile Apps |Ja | [App Service REST](/rest/api/appservice) | [Webressourcen](/azure/templates/microsoft.web/sites) |
| Mobile Engagements |Ja |[Mobile Engagement REST](https://msdn.microsoft.com/library/azure/mt683754.aspx) | |
| Suche |Ja |[Azure-Suchdienst-REST-API](/rest/api/searchservice) | [Search-Ressourcen](/azure/templates/microsoft.search/searchservices) |
| Web-Apps |Ja | [Web-Apps-REST-API](/rest/api/appservice/webapps) | [Webressourcen](/azure/templates/microsoft.web/sites) |

## <a name="intelligence--analytics"></a>Daten und Analyse
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat | 
| --- | --- | --- | --- |
| Analysis Services | Ja | [Analysis Services-REST-API](/rest/api/analysisservices) | [Analysis Services-Ressourcen](/azure/templates/microsoft.analysisservices/servers) |
| Cognitive Services |Ja | [Cognitive Services REST](/rest/api/cognitiveservices) |[Cognitive Services-Ressourcen](/azure/templates/microsoft.cognitiveservices/accounts) |
| Datenkatalog |Ja |[Data Catalog REST](/rest/api/datacatalog) |[Data Catalog-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |
| Data Factory |Ja |[Data Factory REST](/rest/api/datafactory) | |
| Data Lake Analytics |Ja | [Data Lake REST](/rest/api/datalakeanalytics) |[Data Lake Analytics-Ressourcen](/azure/templates/microsoft.datalakeanalytics/accounts) |
| Data Lake-Speicher |Ja |[Data Lake Store REST](/rest/api/datalakestore) |[Data Lake Store-Ressourcen](/azure/templates/microsoft.datalakestore/accounts) |
| HDInsights |Ja |[HDInsights REST](/rest/api/hdinsight) | |
| Machine Learning |Ja |[Machine Learning REST](/rest/api/machinelearning) |[Machine Learning-Ressourcen](/azure/templates/microsoft.machinelearning/commitmentplans) |
| Stream Analytics |Ja |[Stream Analytics REST](/rest/api/streamanalytics) | |
| Power BI |Ja |[Power BI Embedded REST](/rest/api/powerbiembedded) |[Power BI-Ressourcen](/azure/templates/microsoft.powerbi/workspacecollections) |


## <a name="internet-of-things"></a>Internet der Dinge
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| Event Hub |Ja |[Event Hub REST](/rest/api/eventhub) |[Event Hub-Ressourcen](/azure/templates/microsoft.eventhub/namespaces) |
| IoTHubs |Ja |[IoT Hub REST](/rest/api/iothub) |[IoT Hub-Ressourcen](/azure/templates/microsoft.devices/iothubs) |
| Notification Hubs |Ja |[Notification Hub REST](/rest/api/notificationhubs) |[Notification Hubs-Ressourcen](/azure/templates/microsoft.notificationhubs/namespaces) |

## <a name="media--cdn"></a>Medien und CDN
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| CDN |Ja |[CDN REST](/rest/api/cdn) |[CDN-Ressourcen](/azure/templates/microsoft.cdn/profiles) |
| Mediendienst |Ja |[Media Services REST](/rest/api/media) |[Medienressourcen](/azure/templates/microsoft.media/mediaservices) |

## <a name="enterprise-integration"></a>Unternehmensintegration
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| BizTalk Services |Ja | |[BizTalk-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |
| Relay | Ja |  | [Relay-Ressourcen](/azure/templates/microsoft.relay/namespaces) |
| Service Bus |Ja |[Service Bus REST](/rest/api/servicebus) |[Service Bus-Ressourcen](/azure/templates/microsoft.servicebus/namespaces) |

## <a name="identity--access-management"></a>Identitäts- und Zugriffsverwaltung
Azure Active Directory arbeitet mit dem Ressourcen-Manager zusammen, um die rollenbasierte Access Control für Ihr Abonnement zu aktivieren. Weitere Informationen zur rollenbasierten Zugriffssteuerung und Azure Active Directory finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Entwicklerdienste
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| Überwachen |Ja |[Monitor-REST-API](/rest/api/monitor) |[Insights-Ressourcen](/azure/templates/microsoft.insights/alertrules) |
| Bing Maps |Ja | | |
| DevTest Labs |Ja | [DevTest Labs-REST](/rest/api/dtl) |[DevTest Labs-Ressourcen](/azure/templates/microsoft.devtestlab/labs) |
| Visual Studio-Konto |Ja | |[Visual Studio-Schema](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |

## <a name="management-and-security"></a>Verwaltung und Sicherheit
| Dienst | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- |
| Ratgeber | Ja | [Advisor REST](/rest/api/advisor/) | - |
| Automation |Ja |[Automation REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) |[Artikel zu Automation-Ressourcen](/azure/templates/microsoft.automation/automationaccounts) |
| Abrechnung | Ja | [Billing REST](/rest/api/billing/) | - |
| Schlüsseltresor |Ja |[Referenz zur Schlüsseltresor-REST-API](/rest/api/keyvault) |[Key Vault-Ressourcen](/azure/templates/microsoft.keyvault/vaults) |
| Operational Insights |Ja | | |
| Wiederherstellungsdienst |Ja |[Recovery Services-REST-API](/rest/api/recoveryservices) |[Recovery Services-Ressourcen](/azure/templates/microsoft.recoveryservices/vaults) |
| Scheduler |Ja |[Scheduler REST](/rest/api/scheduler) |[Scheduler-Ressourcen](/azure/templates/microsoft.scheduler/jobcollections) |
| Sicherheit |Ja |[Security REST](https://msdn.microsoft.com/library/azure/mt704034.aspx) | |
| Server Management | Ja | [Server Management REST](/rest/api/servermanagement/) | [Server Management-Ressourcen](/azure/templates/microsoft.servermanagement/gateways) |

## <a name="resource-manager"></a>Ressourcen-Manager
| Funktion | Ressourcen-Manager aktiviert | REST-API | Vorlagenformat |
| --- | --- | --- | --- | --- |
| Autorisierung |Ja |[Autorisierungs-REST-API](/rest/api/authorization) |[Autorisierungsressourcen](/azure/templates/microsoft.authorization/locks) |
| Ressourcen |Ja |[Ressourcen-REST-API](/rest/api/resources) |[Bereitstellungsressourcen](/azure/templates/microsoft.resources/deployments) |

## <a name="resource-providers-and-types"></a>Ressourcenanbieter und -typen
Beim Bereitstellen von Ressourcen müssen Sie häufig Informationen zu den Ressourcenanbietern und -typen abrufen. Sie können diese Informationen über die REST-API, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle abrufen.

Damit Sie mit einem Ressourcenanbieter arbeiten können, muss dieser Ressourcenanbieter in Ihrem Konto registriert werden. Viele Ressourcenanbieter werden standardmäßig automatisch registriert, dennoch müssen Sie unter Umständen einige Ressourcenanbieter manuell registrieren. In den Beispielen in diesem Abschnitt wird gezeigt, wie der Registrierungsstatus eines Ressourcenanbieters abgerufen und der Ressourcenanbieter registriert wird.

### <a name="portal"></a>Portal
Sie können eine Liste der unterstützten Ressourcenanbieter anzeigen, indem Sie auf dem Abonnementblatt **Ressourcenanbieter** auswählen. Zum Registrieren des Abonnements bei einem Ressourcenanbieter wählen Sie den Link **Registrieren**.
   
![Liste der Ressourcenanbieter](./media/resource-manager-supported-services/view-resource-providers.png)

### <a name="rest-api"></a>REST-API
Verwenden Sie zum Abrufen aller verfügbaren Ressourcenanbieter, einschließlich Typen, Speicherorten, API-Versionen und Registrierungsstatus, den Vorgang zum [Auflisten aller Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_List). Informationen zum Registrieren eines Ressourcenanbieters finden Sie unter [Registrieren eines Abonnements bei einem Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_Register).

### <a name="powershell"></a>PowerShell
Im folgenden Beispiel wird veranschaulicht, wie Sie alle verfügbaren Ressourcenanbieter abrufen.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```


Im nächsten Beispiel wird gezeigt, wie die Ressourcentypen für einen bestimmten Ressourcenanbieter abgerufen werden.

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
```

Geben Sie zum Registrieren eines Ressourcenanbieters den Namespace an:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Im folgenden Beispiel wird veranschaulicht, wie Sie alle verfügbaren Ressourcenanbieter abrufen.

```azurecli
az provider list
```

Mit dem folgenden Befehl können Sie die Information für einen bestimmten Ressourcenanbieter anzeigen:

```azurecli
az provider show --namespace Microsoft.Web
```

Geben Sie zum Registrieren eines Ressourcenanbieters den Namespace an:

```azurecli
az provider register --namespace Microsoft.ServiceBus
```

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

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Das folgende Beispiel zeigt, wie Sie die unterstützten Speicherorte für Websites abrufen.

```azurecli
az provider show --namespace Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```


## <a name="supported-api-versions"></a>Unterstützte API-Versionen
Beim Bereitstellen einer Vorlage müssen Sie eine API-Version angeben, die zum Erstellen der einzelnen Ressourcen verwendet werden soll. Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. Deshalb wirkt sich die Version der API, die Sie in Ihrer Vorlage angeben, darauf aus, welche Eigenschaften Sie in der Vorlage angeben können. Im Allgemeinen sollten Sie beim Erstellen von Vorlagen die neueste Version der API wählen. Für vorhandene Vorlagen können Sie entscheiden, ob Sie weiterhin eine frühere Version der API verwenden oder die Vorlage für die aktuelle Version aktualisieren möchten, um die neuen Features zu nutzen.

### <a name="portal"></a>Portal
Sie legen die unterstützten API-Versionen genau wie unterstützte Regionen fest (weiter oben erläutert).

### <a name="rest-api"></a>REST-API
Verwenden Sie den Vorgang zum [Auflisten aller Ressourcenanbieter](https://docs.microsoft.com/rest/api/resources/providers#Providers_List) , um zu ermitteln, welche API-Versionen für Ressourcentypen verfügbar sind. 

### <a name="powershell"></a>PowerShell
Im folgenden Beispiel wird veranschaulicht, wie Sie die verfügbaren API-Versionen für einen bestimmten Ressourcentyp abrufen.

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

Die Ausgabe sieht in etwa wie folgt aus:

```powershell
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
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie erhalten die verfügbaren API-Versionen für einen Ressourcenanbieter mit dem folgenden Befehl:

```azurecli
az provider show --namespace Microsoft.Web --query "resourceTypes[?resourceType=='sites'].apiVersions"
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).


