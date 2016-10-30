<properties
    pageTitle="Microsoft Azure-Abonnements und Diensteinschränkungen, Kontingente und Einschränkungen"
    description="Stellt eine Liste allgemeiner Azure-Abonnements und Diensteinschränkungen, Kontingenten und Einschränkungen bereit. Dies umfasst Informationen zum Erhöhen von Einschränkungen und Höchstwerten."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>


# <a name="azure-subscription-and-service-limits,-quotas,-and-constraints"></a>Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen

## <a name="overview"></a>Übersicht

Dieses Dokument beschreibt einige der wichtigsten Einschränkungen in Microsoft Azure. Dieses Dokument behandelt zurzeit nicht alle Azure-Dienste. Mit der Zeit wird diese Liste der Einschränkungen erweitert, um größere Teile der Plattform abzudecken.

Weitere Informationen zu den Azure-Preisen finden Sie in der [Azure-Preisübersicht](https://azure.microsoft.com/pricing/) . Dort können Sie mithilfe des [Preisrechners](https://azure.microsoft.com/pricing/calculator/) oder durch Aufruf der Seite mit der Preisübersicht für einen Dienst (beispielsweise [Windows-VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)) Ihre Kosten bestimmen.

> [AZURE.NOTE] Falls Sie einen Grenzwert über den **Standardgrenzwert**anheben möchten, können Sie [eine gebührenfreie Onlinekundensupport-Anforderung öffnen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Die Limits können nicht über die Werte unter **Maximales Limit** in der folgenden Tabelle angehoben werden. Falls keine Spalte **Maximales Limit** existiert, bedeutet dies, dass für die entsprechende Ressource keine änderbaren Limits existieren.

## <a name="limits-and-the-azure-resource-manager"></a>Grenzwerte und der Azure-Ressourcen-Manager

Es ist jetzt möglich, mehrere Azure-Ressourcen in einer einzigen Azure-Ressourcengruppe zu kombinieren. Bei der Verwendung von Ressourcengruppen werden Grenzwerte, die bisher global waren, auf einer regionalen Ebene mit dem Azure-Ressourcen-Manager verwaltet. Weitere Informationen zu Azure-Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](resource-group-overview.md).

In den folgenden Grenzwerten wurde eine neue Tabelle hinzugefügt, um alle abweichenden Grenzwerte bei Verwendung des Azure-Ressourcen-Managers aufzuzeigen. Es gibt beispielsweise eine Tabelle für **Abonnementgrenzwerte** und eine Tabelle für **Abonnementgrenzwerte – Azure Resource Manager**. Wenn ein Grenzwert für beide Szenarien gilt, wird er nur in der ersten Tabelle angezeigt. Sofern nicht anders angegeben, gelten Grenzwerte global für alle Regionen.

> [AZURE.NOTE] Wichtig ist, dass der Zugriff Ihres Abonnements auf Kontingente für Ressourcen in Azure-Ressourcengruppen pro Region erfolgt und nicht pro Abonnement wie bei Dienstverwaltungskontingenten. Verwenden wir Kernspeicherkontingente als Beispiel. Wenn Sie eine Erhöhung des Kontingents mit Unterstützung für Kernspeicher anfordern müssen, müssen Sie entscheiden, wie viel Kernspeicher Sie in den einzelnen Regionen verwenden möchten, und anschließend eine spezifische Anforderung für Azure-Ressourcengruppen-Kernspeicherkontingente für die gewünschten Beträge und Regionen vornehmen. Wenn Sie für die Ausführung Ihrer Anwendung 30 Kerne in Westeuropa benötigen, sollten Sie daher 30 Kerne in Westeuropa anfordern. In anderen Regionen erfolgt jedoch keine Erhöhung des Kernspeicherkontingents. Das Kontingent von 30 Kernen gilt nur für Westeuropa.
<!-- -->
Daher sollten Sie gegebenenfalls überlegen, wie hoch Ihre Azure-Ressourcengruppenkontingente für Ihre Workload in jeder Region sein müssen, und diesen Betrag in jeder Region anfordern, in der Sie eine Bereitstellung in Betracht ziehen. Weitere Informationen zum Ermitteln Ihrer aktuellen Kontingente für bestimmte Regionen finden Sie unter [Problembehandlung bei der Bereitstellung](./resource-manager-common-deployment-errors.md) .


## <a name="service-specific-limits"></a>Dienstspezifische Grenzwerte

- [Active Directory](#active-directory-limits)
- [API Management](#api-management-limits)
- [App Service](#app-service-limits)
- [Application Insights](#application-insights-limits)
- [Automation](#automation-limits)
- [Azure Redis Cache](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Sicherung](#backup-limits)
- [Batch](#batch-limits)
- [BizTalk Services](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloud Services](#cloud-services-limits)
- [Data Factory](#data-factory-limits)
- [Data Lake Analytics](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Event Hubs](#event-hubs-limits)
- [IoT Hub](#iot-hub-limits)
- [Schlüsseltresor](#key-vault-limits)
- [Media Services](#media-services-limits)
- [Mobile Engagement](#mobile-engagement-limits)
- [Mobile Services](#mobile-services-limits)
- [Überwachung](#monitoring-limits)
- [Multi-Factor Authentication](#multi-factor-authentication)
- [Netzwerk](#networking-limits)
- [Notification Hubs-Dienst](#notification-hub-service-limits)
- [Operational Insights](#operational-insights-limits)
- [Ressourcengruppe](#resource-group-limits)
- [Scheduler](#scheduler-limits)
- [Suchen,](#search-limits)
- [Service Bus](#service-bus-limits)
- [Site Recovery](#site-recovery-limits)
- [SQL-Datenbank](#sql-database-limits)
- [Speicher](#storage-limits)
- [StorSimple-System](#storsimple-system-limits)
- [Stream Analytics](#stream-analytics-limits)
- [Abonnement](#subscription-limits)
- [Traffic Manager](#traffic-manager-limits)
- [Virtuelle Computer](#virtual-machines-limits)
- [Skalierungsgruppen für virtuelle Computer](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Grenzwerte für Abonnements
#### <a name="subscription-limits"></a>Grenzwerte für Abonnements
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonnementgrenzwerte – Azure Resource Manager

Die folgenden Grenzwerte gelten bei Verwendung des Azure-Ressourcen-Managers und der Azure-Ressourcengruppen. Grenzwerte, die durch den Azure-Ressourcen-Manager nicht geändert wurden, sind im Folgenden nicht aufgeführt. Diese Grenzwerte finden Sie in der vorherigen Tabelle.

Informationen zum Umgang mit Grenzwerten bei Resource Manager-Anforderungen finden Sie unter [Throttling Resource Manager requests](resource-manager-request-limits.md) (Drosselung von Resource Manager-Anforderungen).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Grenzwerte für Ressourcengruppen

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Grenzwerte für virtuelle Computer
#### <a name="virtual-machine-limits"></a>Grenzwerte für virtuelle Computer
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Grenzwerte für virtuelle Computer – Azure-Ressourcen-Manager

Die folgenden Grenzwerte gelten bei Verwendung des Azure-Ressourcen-Managers und der Azure-Ressourcengruppen. Grenzwerte, die durch den Azure-Ressourcen-Manager nicht geändert wurden, sind im Folgenden nicht aufgeführt. Diese Grenzwerte finden Sie in der vorherigen Tabelle.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Grenzwerte für VM-Skalierungsgruppen

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Grenzwerte für Netzwerke

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Grenzwerte für Netzwerke
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="traffic-manager-limits"></a>Traffic Manager-Grenzwerte

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-Grenzwerte

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Speichergrenzwerte

Weitere Informationen zu Grenzwerten für Speicherkonten finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Grenzwerte für den Speicherdienst

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Grenzwerte für Datenträger virtueller Computer

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Weitere Informationen finden Sie unter [Größen virtueller Computer](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Standardspeicherkonten**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Storage Premium-Konten**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Grenzwerte für Speicherressourcenanbieter

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Grenzwerte für Clouddienste

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>App Service-Grenzwerte
Die folgenden App Service-Grenzwerte umfassen Grenzwerte für Web-Apps, Mobile Apps, API-Apps und Logik-Apps.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler-Grenzwerte

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch-Grenzwerte

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>BizTalk Services-Grenzwerte
In der folgende Tabelle werden die Grenzwerte für Azure Biztalk Services aufgeführt.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB-Grenzwerte

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kontingente mit einem Sternchen (*) [können angepasst werden. Wenden Sie sich dafür an den Azure-Support](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Mobile Engagement-Grenzwerte

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Search-Grenzwerte

Die Tarife bestimmen die Kapazität und die Beschränkungen des Suchdiensts. Folgende Tarife sind verfügbar:

- *Free:* Mehrinstanzenfähiger Dienst, der mit anderen Azure-Abonnenten gemeinsam genutzt wird und für Bewertung und kleine Entwicklungsprojekte vorgesehen ist.
- *Basic* bietet dedizierte Computeressourcen für Produktionsworkloads mit geringerem Umfang, mit bis zu drei Replikaten für Abfrageworkloads mit hoher Verfügbarkeit.
- *Standard (S1, S2, S3, S3 High Density)* ist für größere Produktionsworkloads vorgesehen. Mehrere Ebenen sind im Standard-Tarif vorhanden, damit Sie eine Ressourcenkonfiguration für bestimmte Szenarien auswählen können.

**Grenzwerte pro Abonnement**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Grenzwerte pro Suchdienst**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Detailliertere Informationen zu anderen Grenzwerten, einschließlich Dokumentgröße, Abfragen pro Sekunde, Schlüssel, Anforderungen und Antworten, finden Sie unter [Grenzwerte für den Azure Search-Dienst](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services-Grenzwerte

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-Grenzwerte

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobile Services-Grenzwerte

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Überwachung von Grenzwerten

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Grenzwerte für den Notification Hubs-Dienst

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Event Hubs-Grenzwerte

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus-Grenzwerte

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT Hub-Grenzwerte

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Data Factory-Grenzwerte

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Grenzwerte für Data Lake Analytics
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics-Grenzwerte
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-Grenzwerte

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp-Grenzwerte

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple-Systemgrenzwerte

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Operational Insights-Grenzwerte

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Backup-Grenzwerte

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery-Grenzwerte

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights-Grenzwerte

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management-Grenzwerte

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis Cache-Grenzwerte

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Schlüsseltresor-Grenzwerte

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatisierungsgrenzwerte
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Grenzwerte für SQL-Datenbanken

Einschränkungen für SQL-Datenbanken finden Sie unter [Ressourceneinschränkungen für SQL-Datenbank](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Siehe auch

[Grundlegendes zu Azure-Einschränkungen und -Steigerungen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Größen virtueller Computer und Clouddienste für Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Größen für Cloud Services](cloud-services/cloud-services-sizes-specs.md)



<!--HONumber=Oct16_HO2-->


