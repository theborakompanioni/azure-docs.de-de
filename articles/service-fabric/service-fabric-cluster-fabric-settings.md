
<properties
   pageTitle="Anpassen von Service Fabric-Clustereinstellungen und der Fabric-Upgraderichtlinie | Microsoft Azure"
   description="Dieser Artikel beschreibt die Fabric-Einstellungen und Fabric-Upgraderichtlinien, die Sie anpassen können."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="chackdan"/>

# Anpassen von Service Fabric-Clustereinstellungen und der Fabric-Upgraderichtlinie

In diesem Dokument wird erläutert, wie Sie die verschiedenen Fabric-Einstellungen und die Fabric-Upgraderichtlinie für Ihren Service Fabric-Cluster anpassen. Die Anpassungen können im Portal oder mithilfe einer Resource Manager-Vorlage erfolgen.

## Fabric-Einstellungen, die Sie anpassen können


Es folgen die Fabric-Einstellungen, die Sie anpassen können:

### Name des Abschnitts: Security

|**Parameter**|**Zulässige Werte**|**Anleitung oder Kurzbeschreibung**|
|-----------------------|--------------------------|--------------------------|
|ClusterProtectionLevel|„None“ oder „EncryptAndSign“| „None“ (Standard) für unsichere Cluster, „EncryptAndSign“ für sichere Cluster. |

### Name des Abschnitts: Hosting

|**Parameter**|**Zulässige Werte**|**Anleitung oder Kurzbeschreibung**|
|-----------------------|--------------------------|--------------------------|
|ServiceTypeRegistrationTimeout|Zeit in Sekunden, Standardwert 300| Die maximale Zeit, die der Diensttyp bei der Fabric registriert sein darf|
|ServiceTypeDisableFailureThreshold|Ganze Zahl, Standardeinstellung 1| Dies ist der Schwellenwert für die Anzahl der Fehler. Bei Überschreitung wird FailoverManager (FM) benachrichtigt. FM deaktiviert den Diensttyp auf diesem Knoten und versucht, die Workload auf einem anderen Knoten zu platzieren.|
|ActivationRetryBackoffInterval|Zeit in Sekunden, Standardwert 5|Backoffintervall bei jedem Aktivierungsfehler. Bei jedem fortlaufenden Aktivierungsfehler wiederholt das System die Aktivierung bis zum Wert von „MaxActivationFailureCount“. Das Wiederholungsintervall bei jedem Versuch ist das Produkt der Anzahl fortlaufender Aktivierungsfehler und des Backoffintervalls für die Aktivierung.|
|ActivationMaxRetryInterval|Zeit in Sekunden, Standardwert 300| Bei jedem fortlaufenden Aktivierungsfehler wiederholt das System die Aktivierung bis zum Wert von „ActivationMaxFailureCount“. „ActivationMaxRetryInterval“ gibt die Wartezeit nach jedem Aktivierungsfehler vor der Wiederholung an |
|ActivationMaxFailureCount|Ganze Zahl, Standardeinstellung 10| Die maximale Anzahl der Wiederholungsversuche nach misslungener Aktivierung, ehe die Versuche eingestellt werden |

### Name des Abschnitts: FailoverManager

|**Parameter**|**Zulässige Werte**|**Anleitung oder Kurzbeschreibung**|
|-----------------------|--------------------------|--------------------------|
|PeriodicLoadPersistInterval|Zeit in Sekunden, Standardwert 10| Hiermit wird bestimmt, wie oft der FM eine Überprüfung auf neue Auslastungsberichte durchführt|

### Name des Abschnitts: Federation

|**Parameter**|**Zulässige Werte**|**Anleitung oder Kurzbeschreibung**|
|-----------------------|--------------------------|--------------------------|
|LeaseDuration|Zeit in Sekunden, Standardwert 30|Dauer einer Lease zwischen einem Knoten und seinen Nachbarn.|
|LeaseDurationAcrossFaultDomain|Zeit in Sekunden, Standardwert 30|Dauer einer fehlerdomänenübergreifenden Lease zwischen einem Knoten und seinen Nachbarn.|

### Name des Abschnitts: ClusterManager

|**Parameter**|**Zulässige Werte**|**Anleitung oder Kurzbeschreibung**|
|-----------------------|--------------------------|--------------------------|
|UpgradeStatusPollInterval|Zeit in Sekunden, Standardwert 60|Die Häufigkeit des Abrufs des Upgradestatus der Anwendung. Dieser Wert bestimmt die Aktualisierungsrate für Aufrufe von „GetApplicationUpgradeProgress“|
|UpgradeHealthCheckInterval|Zeit in Sekunden, Standardwert 60|Die Häufigkeit der Integritätsstatusprüfungen während eines überwachten Anwendungsupgrades|
|FabricUpgradeStatusPollInterval|Zeit in Sekunden, Standardwert 60|Die Häufigkeit des Abrufs des Upgradestatus der Fabric. Dieser Wert bestimmt die Aktualisierungsrate für Aufrufe von „GetFabricUpgradeProgress“ |
|FabricUpgradeHealthCheckInterval|Zeit in Sekunden, Standardwert 60|Die Häufigkeit der Integritätsstatusprüfungen während eines überwachten Fabric-Upgrades|



## Nächste Schritte

Lesen Sie die folgenden Artikel, um weitere Informationen zur Clusterverwaltung zu enthalten:

[Hinzufügen, Rollover und Entfernen von Zertifikaten in einem Azure-Cluster ](service-fabric-cluster-security-update-certs-azure.md)

<!---HONumber=AcomDC_0921_2016-->