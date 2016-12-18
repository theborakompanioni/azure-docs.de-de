---
title: "Dienstverwaltung für Azure Search im Azure-Portal"
description: Es wird beschrieben, wie Sie Azure Search, einen gehosteten Cloudsuchdienst in Microsoft Azure, mit dem Azure-Portal verwalten.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/17/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 71cfd2ea327cad22cdb1085558658934804c15f1


---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Dienstverwaltung für Azure Search im Azure-Portal
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [REST-API](search-get-started-management-api.md)
> 
> 

Azure Search ist ein vollständig verwalteter cloudbasierter Suchdienst zum Erstellen einer umfassenden Suchoberfläche für benutzerdefinierte Apps. In diesem Artikel werden die Aufgaben der *Dienstverwaltung* beschrieben, die Sie im [Azure-Portal](https://portal.azure.com) für einen zuvor bereitgestellten Suchdienst durchführen können. *Service administration* ist nicht sehr umfangreich und auf die folgenden Aufgaben beschränkt:

* Verwalten und Schützen des Zugriffs auf die *API-Schlüssel* , die für den Lese- oder Schreibzugriff auf den Dienst verwendet werden
* Anpassen der Dienstkapazität, indem Sie die Zuordnung von Partitionen und Replikaten ändern
* Überwachen der Ressourcennutzung relativ zu den Grenzwerten Ihrer Dienstebene

**Nicht behandelte Bereiche** 

*Content Management* (oder Indexverwaltung) bezieht sich auf Vorgänge wie z.B. das Analysieren von Suchdatenverkehr, mit dem Ziel, Informationen zum Abfragevolumen und verwendeten Suchbegriffen zu erhalten und zu ermitteln, wie erfolgreich die Suchergebnisse Kunden zu bestimmten Dokumenten in Ihrem Index leiten. Eine weiter gehende Beschreibung des Content Management würde den Rahmen dieses Artikels sprengen. Informationen dazu, wie Sie Einblicke in die internen Abläufe auf Indexebene erhalten, finden Sie unter [„Datenverkehrsanalyse durchsuchen“ für Azure Search](search-traffic-analytics.md).

*Abfrageleistung* wird in diesem Artikel ebenfalls nicht eingegangen. Weitere Informationen finden Sie unter [Überwachen der Nutzung und Abfragemetriken](search-monitor-usage.md) und [Leistung und Optimierung](search-performance-optimization.md).

Bei einem Ausfalls wird ein Failover von Azure Search in andere Cluster und Rechenzentren ausgeführt, aber es gibt keine integrierte Lösungen für manuelle Sicherungs- und Wiederherstellungsvorgänge, wenn der Index oder der Dienst vorsätzlich oder unbeabsichtigt gelöscht wird. Für Kunden, die Objekte und Daten per Pushvorgang an ihren Dienst übermitteln, stellt der Quellcode für die Erstellung und Auffüllung eines Index praktisch die Wiederherstellungsoption dar, wenn ein Index versehentlich gelöscht wird. 

Azure Search bietet keine dienstübergreifende Georeplikation von Indizes. Wenn die Lösung global genutzt wird, könnten Sie Redundanz über einen zusätzlichen Dienst in einem anderen regionalen Rechenzentrum hinzufügen, damit alle Anwendungskomponenten an einem Ort gehostet werden. Weitere Informationen finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md).

<a id="admin-rights"></a>

## <a name="administrator-rights-in-azure-search"></a>Administratorrechte in Azure Search
Die Bereitstellung oder Außerbetriebnahme des eigentlichen Diensts kann von einem Administrator oder Co-Administrator des Azure-Abonnements durchgeführt werden.

Innerhalb eines Diensts verfügen alle Benutzer mit Zugriff auf die Dienst-URL und mit einem Administrator-API-Schlüssel über Lese-/Schreibzugriff auf den Dienst und können entsprechend Serverobjekte, z.B. API-Schlüssel, Indizes, Indexer, Datenquellen, Zeitpläne und Rollenzuweisungen hinzufügen, löschen oder ändern, die über [per RBAC-definierte Rollen](#rbac) implementiert wurden.

Alle Benutzerinteraktionen mit Azure Search fallen unter einen dieser beiden Modi: Lese-/Schreibzugriff auf den Dienst (Administratorrechte) oder Lesezugriff auf den Dienst (Abfragerechte). Weitere Informationen finden Sie unter [Verwalten der API-Schlüssel](#manage-keys).

<a id="sys-info"></a>

## <a name="logging-in-azure-search-and-system-information"></a>Protokollierung in Azure Search und Systeminformationen
In Azure Search werden keine Protokolldateien für einen einzelnen Dienst über das Portal oder programmgesteuerte Schnittstellen verfügbar gemacht. Im Basic-Tarif und höher überwacht Microsoft alle Azure Search-Dienste auf eine Verfügbarkeit von 99,9% gemäß der Vereinbarung zum Servicelevel (SLA). Wenn der Dienst langsam ist oder der Anforderungsdurchsatz unter die SLA-Schwellenwerte fällt, überprüfen Supportteams die verfügbaren Protokolldateien und kümmern sich um die Lösung des Problems.

Allgemeine Informationen zu Ihrem Dienst erhalten Sie wie folgt:

* Im Portal über das Dashboard des Diensts in Form von Benachrichtigungen, Eigenschaften und Statusmeldungen.
* Über [PowerShell](search-manage-powershell.md) oder die [Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn832684.aspx), um [Diensteigenschaften abzurufen](https://msdn.microsoft.com/library/azure/dn832694.aspx) oder den Status der Indexressourcennutzung zu ermitteln.
* Über [Datenverkehrsanalyse durchsuchen](search-traffic-analytics.md), wie bereits erwähnt.

<a id="manage-keys"></a>

## <a name="manage-the-api-keys"></a>Verwalten der API-Schlüssel
Für alle Anforderungen an einen Suchdienst wird ein API-Schlüssel benötigt, der speziell für Ihren Dienst generiert wurde. Dieser API-Schlüssel ist der einzige Authentifizierungsmechanismus für den Zugriff auf Ihren Dienstendpunkt. 

Ein API-Schlüssel ist eine Zeichenfolge, die aus zufällig generierten Zahlen und Buchstaben besteht. Er wird ausschließlich von Ihrem Dienst generiert. Mit [RBAC-Berechtigungen](#rbac) können Sie die Schlüssel löschen oder lesen, aber das Überschreiben eines generierten Schlüssels mit einer benutzerdefinierten Zeichenfolge ist nicht möglich (wenn Sie routinemäßig Kennwörter verwenden, können Sie einen API-Schlüssel nicht durch ein benutzerdefiniertes Kennwort ersetzen). 

Zwei Arten von Schlüsseln werden für den Zugriff auf Ihren Suchdienst verwendet:

* Admin (gültig für alle Lese-/Schreibvorgänge für den Dienst)
* Abfrage (gültig für Lesevorgänge, z.B. Abfragen eines Index)

Ein Admin-API-Schlüssel wird erstellt, wenn der Dienst bereitgestellt wird. Es gibt zwei Administratorschlüssel, die zur besseren Unterscheidung als *primärer* und *sekundärer* Schlüssel bezeichnet werden. In Wirklichkeit sind sie aber austauschbar. Jeder Dienst verfügt über zwei Admin-Schlüssel, sodass Sie für einen Schlüssel ein Rollover durchführen können, ohne den Zugriff auf den Dienst zu verlieren. Sie können beide Admin-Schlüssel austauschen, können allerdings keine weiteren Schlüssel hinzufügen. Pro Dienst können maximal zwei Admin-Schlüssel vorhanden sein.

Abfrage-Schlüssel sind für Clientanwendungen bestimmt, bei denen Search direkt aufgerufen wird. Sie können bis zu 50 Abfrage-Schlüssel generieren. Im Anwendungscode geben Sie die Such-URL und einen Abfrage-API-Schlüssel ein, um den Lesezugriff auf den Dienst zuzulassen. Im Anwendungscode wird auch der Index angegeben, der von der Anwendung verwendet wird. Zusammen definieren der Endpunkt, ein API-Schlüssel für den Lesezugriff und ein Zielindex den Bereich und die Zugriffsebene der Verbindung von Ihrer Clientanwendung.

Öffnen Sie das Dienst-Dashboard, um API-Schlüssel abzurufen oder zu generieren. Klicken Sie auf **SCHLÜSSEL**, um die Schlüsselverwaltungsseite zu öffnen. Sie finden die Befehle zum Generieren bzw. erneuten Generieren von Schlüsseln am oberen Seitenrand. Standardmäßig werden nur Admin-Schlüssel erstellt. Abfrage-API-Schlüssel müssen manuell erstellt werden.

 ![][9]

<a id="rbac"></a>

## <a name="set-rbac-roles-on-administrative-access-for-azure-search"></a>Festlegen von RBAC-Rollen für den administrativen Zugriff für Azure Search
In Azure wird ein [globales Modell für die rollenbasierte Autorisierung](../active-directory/role-based-access-control-configure.md) für alle Dienste bereitgestellt, die über das Portal oder mit Resource Manager-APIs verwaltet werden. Die Rollen „Besitzer“, „Mitwirkender“ und „Leser“ bestimmen die Ebene der Dienstverwaltung für Active Directory-Benutzer, -Gruppen und -Dienstprinzipale, die einer Rolle jeweils zugewiesen sind. 

Für Azure Search werden die folgenden administrativen Aufgaben mit RBAC-Berechtigungen gesteuert:

| Rolle | Task |
| --- | --- |
| Besitzer |Erstellen oder löschen Sie den Dienst bzw. jedes Objekt im Dienst, einschließlich API-Schlüsseln, Indizes, Indexern sowie Datenquellen und Zeitplänen für Indexer.<p>Zeigen Sie den Dienststatus an, einschließlich Anzahl und Speichergröße.<p>Hinzufügen oder Löschen von Rollenmitgliedschaften (Nur ein Besitzer kann die Rollenmitgliedschaften verwalten.)<p>Abonnementadministratoren und Dienstbesitzer sind automatisch Mitglieder der Besitzerrolle. |
| Mitwirkender |Die gleiche Ebene wie „Besitzer“, mit Ausnahme der RBAC-Rollenverwaltung. So kann z.B. ein Mitwirkender den `api-key` anzeigen und neu generieren, aber nicht die Rollenmitgliedschaften ändern. |
| Leser |Dienststatus und Abfrageschlüssel anzeigen. Mitglieder dieser Rolle können die Dienstkonfiguration nicht ändern und auch keine Admin-Schlüssel anzeigen. |

Beachten Sie, dass die Rollen keine Zugriffsrechte für den Dienstendpunkt erteilen. Suchdienstoperationen, wie z. B. die Indexverwaltung, Auffüllung des Indexes und Abfragen von Suchdaten werden durch die API-Schlüssel und nicht durch Rollen gesteuert. Weitere Informationen finden Sie unter „Autorisierung für Verwaltungsvorgänge im Vergleich zu Datenvorgängen“ im Artikel [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>

## <a name="secure-the-api-keys"></a>Verwalten der API-Schlüssel
Die Sicherheit der Schlüssel wird erreicht, indem der Zugriff über das Portal oder Resource Manager-Oberflächen (PowerShell oder Befehlszeilenschnittstelle) eingeschränkt wird. Wie bereits erwähnt, können Abonnementadministratoren alle API-Schlüssel anzeigen und neu generieren. Informieren Sie sich über Rollenzuweisungen, damit Sie wissen, wer Zugriff auf die Admin-Schlüssel hat.

1. Klicken Sie im Dashboard des Diensts auf das Zugriffssymbol, um das Blatt „Benutzer“ zu öffnen.
   ![][7]
2. Sehen Sie sich auf dem Blatt „Benutzer“ die vorhandenen Rollenzuweisungen an. Wie Sie bereits vermuten, haben Abonnementadministratoren über die Rolle „Besitzer“ bereits Vollzugriff auf den Dienst.
3. Fahren Sie fort, indem Sie auf **Abonnementadministratoren** klicken. Erweitern Sie dann die Liste mit den Rollenzuweisungen, um zu ermitteln, wer über Co-Administratorrechte für Ihren Suchdienst verfügt.

Eine weitere Möglichkeit zum Anzeigen der Zugriffsberechtigungen besteht darin, auf dem Blatt „Benutzer“ auf **Rollen** zu klicken. Die verfügbaren Rollen und die Anzahl von Benutzern oder Gruppen, die einer Rolle jeweils zugewiesen sind, werden angezeigt.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Überwachen der Ressourcenauslastung
Die Ressourcenüberwachung im Dashboard ist auf die Informationen im Dienst-Dashboard und einige weitere Metriken beschränkt, die Sie durch Abfragen an den Dienst erhalten. Im Bereich Nutzung im Dienst-Dashboard können Sie direkt ablesen, ob die Partitionsressourcen für Ihre Anwendung angemessen sind.

Mit der Suchdienst-API können Sie Gesamtzahlen für Dokumente und Indizes abrufen. Für diese Werte gelten feste Obergrenzen basierend auf dem Tarif. Weitere Informationen finden Sie unter [Einschränkungen für Suchdienste](search-limits-quotas-capacity.md). 

* [Abrufen von Indexstatistiken](http://msdn.microsoft.com/library/dn798942.aspx)
* [Dokumentenanzahl](http://msdn.microsoft.com/library/dn798924.aspx)

> [!NOTE]
> Durch Caching-Eigenheiten können die Obergrenzen vorübergehend überschritten werden. Wenn Sie z. B. den gemeinsam genutzten Dienst verwenden, kann es passieren, dass eine Dokumentenanzahl oberhalb der Grenze von 10.000 Dokumenten angezeigt wird. Dies ist vorübergehend und wird bei der nächsten Einhaltungsprüfung für die Obergrenze entdeckt. 
> 
> 

<a id="scale"></a>

## <a name="scale-up-or-down"></a>Herauf- oder Herunterskalieren
Jeder Suchdienst enthält zunächst ein Minimum von einem Replikat und einer Partition. Wenn Sie sich mit den Tarifen [Basic oder Standard](search-limits-quotas-capacity.md)für fest zugeordnete Ressourcen angemeldet haben, können Sie im Dienstdashboard auf die Kachel **STAFFELUNG** klicken, um die Anzahl von Partitionen und Replikaten für Ihren Dienst anzupassen.

Wenn Sie zusätzliche Kapazität über eine dieser Ressourcen hinzufügen, wird diese vom Dienst automatisch verwendet. Sie müssen nichts weiter tun. Allerdings kann eine kleine Verzögerung auftreten, bevor die Auswirkungen der neuen Ressource spürbar sind. Die Bereitstellung zusätzlicher Ressourcen kann 15 Minuten oder mehr in Anspruch nehmen.

 ![][10]

### <a name="add-replicas"></a>Hinzufügen von Replikaten
Fügen Sie Replikate hinzu, um die Anzahl der Abfragen pro Sekunde (Queries per Second QPS) zu steigern oder hohe Verfügbarkeit einzurichten. Jedes Replikat enthält eine Kopie eines Index. Jedes zusätzliche Replikat führt also zu einem zusätzlichen Index, der zur Behandlung von Abfragen verwendet werden kann. Für eine hohe Verfügbarkeit sind mindestens drei Replikate erforderlich (Details unter [Kapazitätsplanung](search-capacity-planning.md)).

Suchdienste mit mehr Replikaten können Abfragen per Lastenausgleich auf eine größere Anzahl von Indizes verteilen. Für ein gegebenes Abfragevolumen ist ein höherer Durchsatz an Abfragen möglich, wenn mehr Kopien des Index zur Beantwortung bereitstehen. Wenn Sie eine hohe Latenz in Ihren Abfragen bemerken, können Sie eine positive Auswirkung auf die Leistung erwarten, sobald die zusätzlichen Replikate online sind.

Auch wenn der Abfragedurchsatz beim Hinzufügen von Replikaten ansteigt, wird dieser mit zusätzlichen Replikaten nicht exakt verdoppelt, verdreifacht usw. Alle Suchanwendungen unterliegen externen Faktoren, die die Abfrageleistung beeinträchtigen können. Komplexe Abfragen und Netzwerklatenz sind zwei Faktoren, die die Antwortzeiten von Abfragen beeinflussen.

### <a name="add-partitions"></a>Hinzufügen von Partitionen
Bei den meisten Dienstanwendungen sind anstelle von Partitionen eher mehr Replikate erforderlich. In Fällen, in denen Sie eine zusätzliche Anzahl von Dokumenten benötigen, können Sie Partitionen hinzufügen, falls Sie für den Standarddienst registriert sind. In der Preisstufe Basic stehen keine zusätzlichen Partitionen zur Verfügung.

In der Preisstufe Standard werden Partitionen jeweils in Teilern von 12 hinzugefügt (1, 2, 3, 4, 6 oder 12). Dies ist ein Artefakt für das Sharding (Horizontales Partitionieren). Indizes werden in 12 Shards erstellt, die entweder in 1 Partition gespeichert oder in gleichen Teilen auf 2, 3, 4, 6 oder 12 Partitionen (1 Shard pro Partition) verteilt werden können.

### <a name="remove-replicas"></a>Entfernen von Replikaten
Nach Zeiten mit hohem Abfragevolumen kann es ratsam sein, die Replikate zu reduzieren, wenn sich die Last wieder normalisiert hat (z.B. nach Abschluss des Weihnachtsgeschäfts).

Schieben Sie hierzu den Schieberegler für Replikate auf eine niedrigere Einstellung. Keine weiteren Schritte sind erforderlich. Beim Senken der Replikatanzahl werden virtuelle Computer im Rechenzentrum freigegeben. Ihre Abfrage- und Datenerfassungsoperationen werden nun auf weniger VMs ausgeführt als zuvor. Die Untergrenze liegt bei einem Replikat.

### <a name="remove-partitions"></a>Entfernen von Partitionen
Im Gegensatz zum Entfernen von Replikaten, bei dem Sie keine weiteren Schritte ausführen müssen, erfordert die Senkung des verbrauchten Speicherplatzes unter Umständen einigen Arbeitsaufwand. Wenn Sie in der Lösung beispielsweise drei Partitionen verwenden, wird bei der Reduzierung auf ein oder zwei Partitionen ein Fehler generiert, falls weniger Speicherplatz als nötig vorhanden ist. Wie Sie sicherlich schon vermuten, können Sie entweder Indizes oder Dokumente innerhalb eines Index löschen, um Speicherplatz freizugeben, oder die aktuelle Konfiguration beibehalten.

Es ist nicht möglich, herauszufinden, welche Index-Shards auf welcher Partition gespeichert sind. Jede Partition bietet ca. 25 GB an Speicherplatz an. Sie müssen Ihren Speicherverbrauch also auf einen Wert senken, der von Ihren konfigurierten Partitionen unterstützt werden kann. Wenn Sie auf eine Partition zurückschalten, müssen alle 12 Shards in diese Partition passen.

Für die zukünftige Planung können Sie den Speicherverbrauch prüfen (siehe [Abrufen der Index-Statistiken](http://msdn.microsoft.com/library/dn798942.aspx)), um herauszufinden, wie viel Speicher Sie tatsächlich nutzen. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment-video"></a>Bewährte Methoden zur Skalierung und Bereitstellung (Video)
In diesem 30-minütigen Video werden die bewährten Methoden für erweiterte Bereitstellungsszenarien vorgestellt, z.B. geografisch verteilte Workloads. Unter [Leistung und Optimierung von Azure Search](search-performance-optimization.md) finden Sie auch Hilfeseiten zu denselben Punkten.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den verschiedenen Vorgängen der Dienstverwaltung vertraut gemacht haben, können Sie sich über die unterschiedlichen Ansätze für die Dienstverwaltung informieren:

* [PowerShell](search-manage-powershell.md)
* [Verwaltungs-REST-API](search-get-started-management-api.md)

Falls noch nicht geschehen, können Sie sich auch den [Artikel zur Leistung und Optimierung](search-performance-optimization.md)durchlesen und sich optional das im vorherigen Abschnitt angegebene Video ansehen, um eingehendere Informationen und Veranschaulichungen der empfohlenen Vorgehensweisen zu erhalten.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png






<!--HONumber=Nov16_HO3-->


