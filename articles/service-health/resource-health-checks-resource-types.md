---
title: "Durch Azure Resource Health unterstützte Ressourcentypen | Microsoft-Dokumentation"
description: "Durch Azure Resource Health unterstützte Ressourcentypen"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/19/2017
ms.author: BernardoAMunoz
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: ed9c955096a5427e8184bb6c542ad85ff4c302be
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---

# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Ressourcentypen und Integritätsprüfungen in Azure Resource Health
Es folgt eine vollständige Auflistung nach Ressourcentypen aller von Resource Health ausgeführten Überprüfungen.

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Sind alle Cacheknoten funktionstüchtig?</li><li>Kann der Cache vom Rechenzentrum aus erreicht werden?</li><li>Hat der Cache die maximale Anzahl von Verbindungen erreicht?</li><li> Hat der Cache seinen verfügbaren Arbeitsspeicher ausgschöpft? </li><li>Tritt im Cache eine große Anzahl von Seitenfehlern auf?</li><li>Ist der Cache stark ausgelastet?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Ausgeführte Überprüfungen|
|---|
|<ul> <li>Wurde einer der Endpunkte angehalten, entfernt oder falsch konfiguriert?</li><li>Können CDN-Konfigurationsvorgänge auf das zusätzliche Portal zugreifen?</li><li>Treten bei den CDN-Endpunkten laufend Übermittlungsprobleme auf?</li><li>Können Benutzer die Konfiguration ihrer CDN-Ressourcen ändern?</li><li>Werden Konfigurationsänderungen mit der erwarteten Rate weitergegeben?</li><li>Können Benutzer die CDN-Konfiguration mithilfe von Azure-Portal, PowerShell oder API verwalten?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Ist der Hostserver funktionstüchtig?</li><li>Ist das Starten des Hostbetriebssystems abgeschlossen?</li><li>Ist der VM-Container bereitgestellt und hochgefahren?</li><li>Besteht Netzwerkkonnektivität zwischen Host und Speicherkonto?</li><li>Ist das Starten des Gastbetriebssystems abgeschlossen?</li><li>Gibt es eine laufende geplante Wartung?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Kann das Konto vom Rechenzentrum aus erreicht werden?</li><li>Ist der Cognitive Services-Ressourcenanbieter verfügbar?</li><li>Sind die Cognitive Services in der entsprechenden Region verfügbar?</li><li>Können Lesevorgänge in dem Speicherkonto durchgeführt werden, auf dem die Ressourcenmetadaten gespeichert sind?</li><li>Wurde das API-Aufrufkontingent erreicht?</li><li>Wurde die API-Aufruflesebeschränkung erreicht?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.compute/virtualmachines
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Hostet der Server diese VM funktionstüchtig?</li><li>Ist das Starten des Hostbetriebssystems abgeschlossen?</li><li>Ist der VM-Container bereitgestellt und hochgefahren?</li><li>Besteht Netzwerkkonnektivität zwischen Host und Speicherkonto?</li><li>Ist das Starten des Gastbetriebssystems abgeschlossen?</li><li>Gibt es eine laufende geplante Wartung?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Können Benutzer in der Region Aufträge an Data Lake Analytics senden?</li><li>Werden einfache Aufträge in der Region ausgeführt und erfolgreich abgeschlossen?</li><li>Können Benutzer Katalogelemente in der Region auflisten?</li>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Können Benutzer Daten zu Data Lake Store in der Region hochladen?</li><li>Können Benutzer Daten von Data Lake Store in der Region herunterladen?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Wurden Datenbank- oder Sammlungsanforderungen nicht ausgeführt, weil der DocumentDB-Dienst nicht verfügbar war?</li><li>Wurden Dokumentanforderungen nicht ausgeführt, weil der DocumentDB-Dienst nicht verfügbar war?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.network/connections
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Ist der VPN-Tunnel verbunden?</li><li>Treten Konfigurationskonflikte in der Verbindung auf?</li><li>Sind die vorinstallierten Schlüssel ordnungsgemäß konfiguriert?</li><li>Ist das lokale VPN-Gerät erreichbar?</li><li>Treten Konflikte in der IPSec-/IKE-Sicherheitsrichtlinie auf?</li><li>Ist die S2S-VPN-Verbindung ordnungsgemäß bereitgestellt oder in einem fehlerhaften Zustand?</li><li>Ist die VNET-zu-VNET-Verbindung ordnungsgemäß bereitgestellt oder in einem fehlerhaften Zustand?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Ist das VPN-Gateway vom Internet aus erreichbar?</li><li>Ist das VPN-Gateway im Standbymodus?</li><li>Wird der VPN-Dienst auf dem Gateway ausgeführt?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Ausgeführte Überprüfungen|
|---|
|<ul><li> Können Laufzeitvorgänge wie Registrierung, Installation oder Senden im Namespace ausgeführt werden?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Ist das Host-BS funktionstüchtig?</li><li>Ist die workspaceCollection von außerhalb des Rechenzentrums erreichbar?</li><li>Ist der Power BI-Ressourcenanbieter verfügbar?</li><li>Ist der Power BI-Dienst in der entsprechenden Region verfügbar?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Können Diagnosevorgänge auf dem Cluster ausgeführt werden?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Ausgeführte Überprüfungen|
|---|
|<ul><li> Wurden Anmeldungen bei der Datenbank durchgeführt?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Sind alle Hosts funktionstüchtig, auf denen der Auftrag ausgeführt wird?</li><li>Konnte der Auftrag nicht gestartet werden?</li><li>Gibt es laufende Laufzeitupgrades?</li><li>Ist der Auftrag in einem erwarteten Zustand (z.B. vom Kunden ausgeführt oder beendet)?</li><li>Sind bei dem Auftrag Arbeitsspeichermangel-Ausnahmefehler aufgetreten?</li><li>Gibt es laufende geplante Computeupdates?</li><li>Ist der Ausführungs-Manager (Steuerungsplan) verfügbar?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Ist der Hostserver funktionstüchtig?</li><li>Werden die Internetinformationsdienste ausgeführt?</li><li>Wird der Load Balancer ausgeführt?</li><li>Kann der Webdiensttarif vom Rechenzentrum aus erreicht werden?</li><li>Ist das Speicherkonto verfügbar, das den Inhalt der Websites für die Serverfarm hostet?</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/sites
|Ausgeführte Überprüfungen|
|---|
|<ul><li>Ist der Hostserver funktionstüchtig?</li><li>Wir der Internet Information Server ausgeführt?</li><li>Wird der Load Balancer ausgeführt?</li><li>Kann die Web-App vom Rechenzentrum aus erreicht werden?</li><li>Ist das Speicherkonto verfügbar, das den Inhalt der Website hostet?</li></ul>|

# <a name="next-steps"></a>Nächste Schritte
-  Weitere Informationen finden Sie unter [Einführung in Azure Service Health](service-health-overview.md) und [Einführung in Azure Resource Health](resource-health-overview.md). 
-  [Azure Resource Health – FAQ](resource-health-faq.md)
- Richten Sie Warnungen ein, damit Sie über Integritätsprobleme benachrichtigt werden. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 
