---
title: "Azure Government – verfügbare Dienste | Microsoft Docs"
description: "Bietet eine Übersicht über die verfügbaren Dienste in Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: smichelotti
manager: liki
ms.assetid: a453a23c-bc0f-4203-9075-0f579dea7e23
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/13/2017
ms.author: stemi
translationtype: Human Translation
ms.sourcegitcommit: ffe3991f68bbd255ff6f4ffedbd6638f32897d1a
ms.openlocfilehash: 914870f59b488716bb283162078535849f732018
ms.lasthandoff: 02/21/2017


---
# <a name="available-services-on-azure-government"></a>Verfügbare Dienste in Azure Government
Die in Azure Government verfügbaren Dienste werden fortlaufend ergänzt.  Diese Dienste werden mit demselben Code bereitgestellt, der in Azure Public verwendet wird.  In diesem Abschnitt sind die Dienste aufgeführt, die derzeit in Azure Government verfügbar sind, einschließlich zweier wichtiger Arten von Informationen:

* **Variationen:** Abweichungen aufgrund von Features, die noch nicht bereitgestellt wurden, oder bei Eigenschaften (z.B. URLs), die in der Government-Umgebung anders sind.  
* **Überlegungen:** Government-spezifische Implementierungsdetails, mit denen sichergestellt werden soll, dass Daten stets im Compliancerahmen bleiben.

Alles Weitere, was Sie über diese Dienste wissen müssen, finden Sie in der allgemeinen Dokumentation.

Die aktuelle Liste der Dienste finden Sie unter [Produkte nach Region](https://azure.microsoft.com/regions/services/). 

Die in den folgenden Tabellen als Resource Manager-fähig angegebenen Dienste verfügen über Ressourcenanbieter und können mithilfe von PowerShell verwaltet werden. Ausführliche Informationen zu Resource Manager-Anbietern, API-Versionen und Schemas finden Sie [hier](../azure-resource-manager/resource-manager-supported-services.md). Die als im Portal verfügbar angegebenen Dienste können im [Azure Government-Portal](https://portal.azure.us/) verwaltet werden. 


## <a name="computedocumentation-government-computemd"></a>[Compute](documentation-government-compute.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| [Virtuelle Computer](documentation-government-compute.md#virtual-machines) | Ja | Ja |
| Batch | Ja | Ja |
| Cloud Services | Ja | Ja |
| Service Fabric | Ja | Ja |
| VM-Skalierungsgruppen | Ja | Ja |


## <a name="networkingdocumentation-government-networkingmd"></a>[Netzwerk](documentation-government-networking.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| [ExpressRoute](documentation-government-networking.md#expressroute-private-connectivity) | Ja | Ja |
| Virtuelles Netzwerk | Ja | Ja |
| [Load Balancer](documentation-government-networking.md#support-for-load-balancer) | Ja | Ja |
| [Traffic Manager](documentation-government-networking.md#support-for-traffic-manger) | Ja | Ja |
| [VPN Gateway](documentation-government-networking.md#support-for-vpn-gateway) | Ja | Ja |
| Application Gateway | Ja | Ja |
| ExpressRoute | Ja | Ja |



## <a name="storagedocumentation-government-services-storagemd"></a>[Speicher](documentation-government-services-storage.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| [Storage – Blobs](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [Storage – Tabellen](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [Storage – Warteschlangen](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [Storage – Dateien](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [Storage – Datenträger](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [StorSimple](documentation-government-services-storage.md) | Ja | Ja |
| [Sicherung](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [Site Recovery](documentation-government-services-storage.md#azure-storage) | Ja | Ja |
| [Import/Export](documentation-government-services-storage.md#azure-storage) | Ja | Nein |



## <a name="web--mobiledocumentation-government-services-webandmobilemd"></a>[Web und mobil](documentation-government-services-webandmobile.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| [App Service – Web-Apps](documentation-government-services-webandmobile.md#app-services) | Ja | Ja |
| [App Service – API-Apps](documentation-government-services-webandmobile.md#app-services) | Ja | Ja |
| [App Service – Mobile Apps](documentation-government-services-webandmobile.md#app-services) | Ja | Ja |
| Media Services | Ja | Ja |


## <a name="databasesdocumentation-government-services-databasemd"></a>[Datenbanken](documentation-government-services-database.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| [SQL-Datenbank](documentation-government-services-database.md#sql-database) | Ja | Ja |
| SQL Data Warehouse | Ja | Ja |
| SQL Server Stretch-Datenbank | Ja | Ja |
| [Redis Cache](documentation-government-services-database.md#azure-redis-cache) | Ja | Ja |




## <a name="internet-of-things-iot"></a>Internet der Dinge (IoT, Internet of Things)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| Event Hubs | Ja | Ja |
| Notification Hubs | Nein | Nein (Wechseln zum [Legacyportal](https://manage.windowsazure.us/)) |


## <a name="enterprise-integration"></a>Enterprise Integration

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| Service Bus | Ja | Ja |
| [StorSimple](documentation-government-services-storage.md) | Ja | Ja |
| SQL Server Stretch-Datenbank | Ja | Ja |



## <a name="security--identitydocumentation-government-services-securityandidentitymd"></a>[Sicherheit und Identität](documentation-government-services-securityandidentity.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| Azure Active Directory | Ja | Ja |
| [Key Vault](documentation-government-services-securityandidentity.md#key-vault) | Ja | Nein (in Kürze verfügbar) |
| Multi-Factor Authentication | Ja | Ja |
| Intune | Ja | Nein |


## <a name="intelligence--analytics"></a>Daten und Analyse

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| Power BI | Ja | Nein |
| HDInsight | Ja | Ja |



## <a name="monitoring--managementdocumentation-government-services-monitoringandmanagementmd"></a>[Überwachung und Verwaltung](documentation-government-services-monitoringandmanagement.md)

| Dienst | Ressourcen-Manager aktiviert | Portal |
| --- | --- | --- |
| [Automation](documentation-government-services-monitoringandmanagement.md#automation) | Ja | Ja |
| [Sicherung](documentation-government-services-backup.md) | Ja | Ja |
| [Log Analytics](documentation-government-services-monitoringandmanagement.md#log-analytics) | Ja | Ja |
| [Site Recovery](documentation-government-services-monitoringandmanagement.md#site-recovery) | Ja | Ja |
| Scheduler | Ja | Nein |
| Überwachung und Diagnose | Ja | Ja |




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den [Microsoft Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/) abonnieren.


