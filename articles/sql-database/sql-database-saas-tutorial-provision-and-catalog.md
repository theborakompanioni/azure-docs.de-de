---
title: "Bereitstellen neuer Mandanten in einer mehrinstanzenfähigen App, die die Azure SQL-Datenbank verwendet | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie neue Mandanten in der Wingtip-SaaS-App bereitstellen und katalogisieren können."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: sstein
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 8fa4c4f95386a92c8c818eef1a5b4de5a086fe07
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Bereitstellen neuer Mandanten und Registrieren der Mandanten im Katalog

In diesem Tutorial erfahren Sie, wie Sie SaaS-Muster bereitstellen und katalogisieren und wie diese Muster in der Wingtip-SaaS-Anwendung implementiert werden. Sie erstellen und initialisieren neue Mandantendatenbanken und registrieren sie im Mandantenkatalog der Anwendung. Der Katalog ist eine Datenbank, in der die Zuordnung zwischen den zahlreichen Mandanten von SaaS-Anwendungen und den dazugehörigen Daten verwaltet wird. Der Katalog spielt eine wichtige Rolle, da er die Anwendungsanforderungen an die richtigen Datenbanken weiterleitet.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Bereitstellen eines einzelnen neuen Mandanten einschließlich schrittweiser Betrachtung der Implementierung
> * Bereitstellen eines Batches von zusätzlichen Mandanten


Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Wingtip-SaaS-App wird bereitgestellt. Unter [Bereitstellen und Kennenlernen einer mehrinstanzenfähigen SaaS-Anwendung, die Azure SQL-Datenbank verwendet](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Einführung in die SaaS-Katalogmuster

Für eine mehrinstanzenfähige SaaS-Anwendung mit Datenbankunterstützung ist es wichtig zu wissen, wo die Informationen über jeden einzelnen Mandanten gespeichert sind. Im SaaS-Katalogmuster wird eine Katalogdatenbank zum Speichern der Zuordnungen zwischen den einzelnen Mandanten und deren Datenspeicherorten verwendet. Die Wingtip-SaaS-App verwendet einen einzelnen Mandanten pro Datenbankarchitektur, aber das grundlegende Muster zum Speichern der Zuordnung zwischen Mandant und Datenbank in einem Katalog gilt unabhängig davon, ob eine mehrinstanzenfähige Datenbank oder eine Datenbank mit nur einem Mandanten verwendet wird.

Jedem Mandanten wird ein Schlüssel zugewiesen, der ihn im Katalog identifiziert und dem Speicherort der entsprechenden Datenbank zugeordnet ist. In der Wingtip-SaaS-App wird der Schlüssel aus einem Hash des Mandantennamens gebildet. Dadurch kann der Mandantennamensteil der Anwendungs-URL zur Erstellung des Schlüssels herangezogen werden. Es können auch andere Mandantenschlüsselschemas verwendet werden.  

Dank des Katalogs kann der Name oder Speicherort der Datenbank ohne größere Auswirkungen auf die Anwendung geändert werden.  In einem Datenbankmodell mit mehreren Mandanten kann dadurch auch ein Mandant zwischen Datenbanken „verschoben“ werden.  Darüber hinaus kann der Katalog angeben, ob ein Mandant oder eine Datenbank offline ist (beispielsweise zur Wartung). Weitere Informationen hierzu finden Sie im Tutorial zum [Wiederherstellen eines einzelnen Mandanten](sql-database-saas-tutorial-restore-single-tenant.md).

Der Katalog ist im Prinzip eine Verwaltungsdatenbank für eine SaaS-Anwendung und kann zusätzliche Mandanten- oder Datenbankmetadaten (etwa die Ebene oder Edition einer Datenbank, die Schemaversion, den Serviceplan oder SLAs für Mandanten) und andere hilfreiche Informationen für die Anwendungsverwaltung, den Kundensupport oder DevOps-Prozesse speichern.  

Abgesehen von der SaaS-Anwendung kann der Katalog auch die Verwendung von Datenbanktools ermöglichen.  Im Wingtip-SaaS-Beispiel wird der Katalog verwendet, um mandantenübergreifende Abfragen zu ermöglichen (siehe [Ausführen von Ad-hoc-Analyseabfragen über alle Wingtip-SaaS-Mandanten hinweg](sql-database-saas-tutorial-adhoc-analytics.md)). Die datenbankübergreifende Auftragsverwaltung wird in den Tutorials [Verwalten des Schemas für mehrere Mandanten in der Wingtip-SaaS-Anwendung](sql-database-saas-tutorial-schema-management.md) und [Extrahieren von Daten aus Mandantendatenbanken in eine Analysedatenbank für die Offlineanalyse](sql-database-saas-tutorial-tenant-analytics.md) behandelt. 

In der Wingtip-SaaS-App wird der Katalog mithilfe der Shardverwaltungsfeatures der [Clientbibliothek für elastische Datenbanken (Elastic Database Client Library, EDCL)](sql-database-elastic-database-client-library.md) implementiert. Die EDCL ermöglicht einer Anwendung die Erstellung, Verwaltung und Verwendung einer datenbankgestützten Shardzuordnung. Eine Shardzuordnung enthält eine Liste mit Shards (Datenbanken) und die Zuordnung zwischen Schlüsseln (Mandanten) und Datenbanken.  EDCL-Funktionen können in Anwendungen oder PowerShell-Skripts im Rahmen der Mandantenbereitstellung verwendet werden, um Einträge in der Shardzuordnung zu erstellen, und in Anwendungen, um effizient eine Verbindung mit der richtigen Datenbank herzustellen. EDCL speichert Verbindungsinformationen zwischen, um den Datenverkehr für die Katalogdatenbank zu minimieren und die Anwendung zu beschleunigen.  

> [!IMPORTANT]
> Sie können auf die Zuordnungsdaten in der Katalogdatenbank zugreifen, *allerdings sollten Sie sie nicht bearbeiten*. Bearbeiten Sie Zuordnungsdaten nur über EDCL-APIs. Ein direktes Bearbeiten der Zuordnungsdaten birgt das Risiko, den Katalog zu beschädigen, und wird daher nicht unterstützt.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Einführung in das SaaS-Bereitstellungsmuster

Beim Onboarding eines neuen Mandanten in einer SaaS-Anwendung, die ein Datenbankmodell mit einem einzelnen Mandanten verwendet, muss eine neue Mandantendatenbank bereitgestellt werden.  Sie muss am korrekten Ort und auf der korrekten Dienstebene erstellt, mit dem korrekten Schema und den korrekten Referenzdaten initialisiert und anschließend im Katalog unter dem korrekten Mandantenschlüssel registriert werden.  

Für die Datenbankbereitstellung können verschiedene Ansätze verwendet werden. So können Sie beispielsweise SQL-Skripts ausführen, eine BACPAC-Datei bereitstellen oder eine Gold-Version einer Vorlagendatenbank kopieren.  

Der verwendete Bereitstellungsansatz muss Teil Ihrer allgemeinen Schemaverwaltungsstrategie sein. Dabei muss gewährleistet sein, dass neue Datenbanken mit dem neuesten Schema bereitgestellt werden.  Dies wird im Tutorial [Verwalten des Schemas für mehrere Mandanten in der Wingtip-SaaS-Anwendung](sql-database-saas-tutorial-schema-management.md) behandelt.  

Die Wingtip-SaaS-App stellt neue Mandanten bereit, indem sie eine Gold-Version einer auf dem Katalogserver bereitgestellten Datenbank namens „basetenantdb“ kopiert.  Die Bereitstellung kann im Rahmen der Registrierung in die Anwendung integriert und/oder offline mithilfe von Skripts unterstützt werden. In diesem Tutorial wird die Bereitstellung mithilfe von PowerShell durchgeführt. Die Bereitstellungsskripts kopieren die Datenbank „basetenantdb“, um eine neue Mandantendatenbank in einem Pool für elastische Datenbanken zu erstellen. Anschließend wird die Datenbank mit mandantenspezifischen Informationen initialisiert und in der Katalogshardzuordnung registriert.  In der Beispiel-App werden Datenbanken auf der Grundlage des Mandantennamens benannt, dies ist jedoch kein entscheidender Aspekt des Musters. Durch die Verwendung des Katalogs kann der Datenbank ein beliebiger Name zugewiesen werden.+ 


## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip-SaaS-Skripts und der Quellcode der Anwendung stehen im GitHub-Repository [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) zur Verfügung. [Schritte zum Herunterladen der Wingtip-SaaS-Skripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts)


## <a name="provision-and-catalog-detailed-walkthrough"></a>Bereitstellen und Katalogisieren: detaillierte exemplarische Vorgehensweise

Um nachzuvollziehen, wie die Wingtip-Anwendung die Bereitstellung neuer Mandanten implementiert, fügen Sie einen Haltepunkt hinzu, und durchlaufen Sie während der Bereitstellung eines Mandanten den Workflow:

1. Öffnen Sie „...\\Learning Modules\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_“, und legen Sie die folgenden Parameter fest:
   * **$TenantName** = Name des neuen Veranstaltungsorts (z.B., *Bushwillow Blues*).
   * **$VenueType** = einer der vordefinierten Veranstaltungsorttypen: *blues*, „classicalmusic“, „dance“, „jazz“, „judo“, „motorracing“, „multipurpose“, „opera“, „rockmusic“, „soccer“.
   * **$DemoScenario** = **1**, festgelegt auf **1** für das *Bereitstellen eines einzelnen Mandanten*.

1. Fügen Sie einen Haltepunkt hinzu, indem Sie den Cursor an eine beliebige Stelle in Zeile 48 bewegen (der Zeile mit *New-Tenant `*) und **F9** drücken.

   ![Haltepunkt](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Betätigen Sie **F5**, um das Skript auszuführen.

1. Nachdem die Skriptausführung am Haltepunkt beendet wurde, drücken Sie **F11**, um den Code schrittweise auszuführen.

   ![Haltepunkt](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



Verfolgen Sie die Ausführung des Skripts mit den Optionen (**F10** und **F11**) im Menü **Debug** nach, um die aufgerufenen Funktionen zu überspringen oder einzeln auszuführen. Weitere Informationen zum Debuggen von PowerShell-Skripts finden Sie unter [Tipps zum Arbeiten mit und Debuggen von PowerShell-Skripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Die folgenden Schritte müssen nicht genau befolgt werden, sondern stellen eine Erläuterung des Workflows dar, den Sie beim Debuggen des Skripts schrittweise durchlaufen:

1. **Importieren Sie das Modul „SubscriptionManagement.psm1“**, das Funktionen enthält, um sich bei Azure anzumelden und das Azure-Abonnement auszuwählen, mit dem Sie arbeiten.
1. **Importieren Sie das Modul „CatalogAndDatabaseManagement.psm1“**, das eine Abstraktion auf Katalog- und Mandantenebene über die [Shardverwaltungsfunktionen](sql-database-elastic-scale-shard-map-management.md) bereitstellt. Dies ist ein wichtiges Modul, das einen Großteil des Katalogmusters kapselt. Es lohnt sich, dieses Modul genauer zu betrachten.
1. **Rufen Sie Konfigurationsinformationen ab**. Wechseln Sie zu „Get-Configuration“ (mit F11), und sehen Sie sich an, wie die App-Konfiguration angegeben ist. Hier sind Ressourcennamen und andere App-spezifische Werte definiert. Ändern Sie hier aber keine Werte, es sei denn, sie kennen sich gut mit Skripts aus.
1. **Rufen Sie das Katalogobjekt ab**. Wechseln Sie zu „Get-Catalog“. Hierdurch wird ein im übergeordneten Skript verwendetes Katalogobjekt erstellt und zurückgegeben.  Diese Funktion verwendet aus **AzureShardManagement.psm1** importierte Shardverwaltungsfunktionen. Das Katalogobjekt besteht aus Folgendem:
   * „$catalogServerFullyQualifiedName“ wird aus dem Standardstamm plus Ihrem Benutzernamen gebildet: _catalog-\<Benutzer\>.database.windows.net_.
   * „$catalogDatabaseName“ wird aus der Konfigurationsdatei abgerufen: *tenantcatalog*.
   * Das Objekt „$shardMapManager“ wird aus der Katalogdatenbank initialisiert.
   * Das Objekt „$shardMap“ wird aus der Shardzuordnung *tenantcatalog* in der Katalogdatenbank initialisiert.
   Ein Katalogobjekt wird gebildet und aus zurückgegeben und im Skript der höheren Ebene verwendet.
1. **Berechnen Sie den neuen Mandantenschlüssel**. Eine Hashfunktion wird verwendet, um den Mandantenschlüssel aus dem Mandantennamen zu erstellen.
1. **Überprüfen Sie, ob der Mandantenschlüssel bereits vorhanden ist**. Der Katalog wird überprüft, um sicherzustellen, dass der Schlüssel verfügbar ist.
1. **Die Mandantendatenbank wird mit „New-TenantDatabase“ bereitgestellt.** Drücken Sie**** F11, und sehen Sie sich an, wie die Datenbank mithilfe einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-template-walkthrough.md) bereitgestellt wird.

Der Datenbankname wird aus dem Mandantennamen gebildet, um zu verdeutlichen, welcher Shard zu welchem Mandanten gehört. (Sie können problemlos auch andere Strategien für die Datenbankbenennung verwenden.)+ Eine Resource Manager-Vorlage wird verwendet, um durch Kopieren einer Gold-Version einer Datenbank (baseTenantDB) auf dem Katalogserver eine Mandantendatenbank zu erstellen. Alternativ können Sie eine leere Datenbank erstellen und sie anschließend durch Importieren einer BACPAC-Datei initialisieren oder ein Initialisierungsskript von einem bekannten Speicherort ausführen.  

Die Resource Manager-Vorlage befindet sich im Ordner „……\Learning Modules\Common\“: *tenantdatabasecopytemplate.json*

Nach Erstellung der Mandantendatenbank wird sie **mit dem Namen des Veranstaltungsorts (Mandantenname) und dem Veranstaltungsorttyp initialisiert**. Hier können Sie auch weitere Initialisierungen vornehmen.

Die **Mandantendatenbank wird im Katalog registriert**. Hierzu wird *Add-TenantDatabaseToCatalog* mit dem Mandantenschlüssel verwendet. Drücken Sie **F11**, um sich die Schritte im Detail anzuschauen:

* Die Katalogdatenbank wird der Shardzuordnung (Liste der bekannten Datenbanken) hinzugefügt.
* Die Zuordnung, die den Schlüsselwert mit dem Shard verknüpft, wird erstellt.
* Zusätzliche Metadaten (Name des Veranstaltungsorts) für den Mandanten werden der Mandantentabelle im Katalog hinzugefügt.  Die Mandantentabelle ist nicht Teil des Schemas „ShardManagement“ und wird nicht durch die EDCL installiert.  Diese Tabelle veranschaulicht, wie die Katalogdatenbank erweitert werden kann, um zusätzliche anwendungsspezifische Daten zu unterstützen.   


Nach Abschluss der Bereitstellung kehrt die Ausführung zum ursprünglichen Skript *Demo-ProvisionAndCatalog* zurück, das die Seite **Ereignisse** für den neuen Mandanten im Browser öffnet:

   ![events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Bereitstellen eines Batches von Mandanten

In dieser Übung wird ein Batch mit 17 Mandanten bereitgestellt. Es empfiehlt sich, diesen Mandantenbatch bereitzustellen, bevor Sie mit anderen Wingtip-SaaS-Tutorials beginnen, damit Ihnen mehrere Datenbanken zur Verfügung stehen.

1. Öffnen Sie „...\\Learning Modules\\ProvisionAndCatalog\\*Demo-ProvisionAndCatalog.ps1*“ in der *PowerShell-ISE*, und ändern Sie den *$DemoScenario*-Parameter in „3“:
   * **$DemoScenario** = **3**, geändert in **3** für das *Bereitstellen eines Batches von Mandanten*.
1. Drücken Sie **F5**, um das Skript auszuführen.

Das Skript stellt einen Batch von zusätzlichen Mandanten bereit. Es verwendet eine [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-template-walkthrough.md), die den Batch steuert und anschließend die Bereitstellung der einzelnen Datenbanken an eine verknüpfte Vorlage delegiert. Indem Sie Vorlagen auf diese Weise verwenden, können Sie mit Azure Resource Manager den Bereitstellungsprozess für das Skript vermitteln. Vorlagen stellen Datenbanken, wann immer möglich, parallel bereit, und verarbeiten nötigenfalls Wiederholungen, um den Gesamtprozess zu optimieren. Das Skript ist idempotent. Wenn also ein Fehler auftritt oder das Skript aus irgendeinem Grund angehalten wird, führen Sie es erneut aus.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Überprüfen, ob der Mandantenbatch erfolgreich bereitgestellt wurde

* Öffnen Sie den Server *tenants1*, indem Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Liste der Server wechseln. Klicken Sie auf **SQL-Datenbanken**, und vergewissern Sie sich, dass der Batch von 17 zusätzlichen Datenbanken jetzt in der Liste enthalten ist:

   ![Datenbankliste](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Weitere Bereitstellungsmuster

Weitere Bereitstellungsmuster, die in diesem Tutorial nicht behandelt werden:

**Vorabbereitstellung von Datenbanken.** Die Vorabbereitstellung profitiert von der Tatsache, dass Datenbanken in einem Pool für elastische Datenbanken keine zusätzlichen Kosten hervorrufen. Die Abrechnung erfolgt für den Pool für elastische Datenbanken und nicht für die Datenbanken, und Datenbanken im Leerlauf verbrauchen keine Ressourcen. Indem Sie Datenbanken vorab in einem Pool bereitstellen und später bei Bedarf zuordnen, können Sie das Onboarding von Mandanten erheblich beschleunigen. Die Anzahl der vorab bereitgestellten Datenbanken kann bedarfsorientiert angepasst werden, um einen für die erwartete Bereitstellungsrate geeigneten Puffer bereitzuhalten.

**Automatische Bereitstellung.** Bei der Vorabbereitstellung wird ein dedizierter Bereitstellungsdienst verwendet, um Server, Pools und Datenbanken bei Bedarf automatisch bereitzustellen – einschließlich vorab bereitgestellter Datenbanken in Pools für elastische Datenbanken, falls erforderlich. Wenn Datenbanken außer Betrieb genommen und gelöscht werden, können Lücken in den Pools für elastische Datenbanken durch den Bereitstellungsdienst bedarfsorientiert gefüllt werden. Ein solcher Dienst kann einfach oder komplex sein. Die Bereitstellung kann also beispielsweise über mehrere geografische Regionen erfolgen, und die Georeplikation kann automatisch eingerichtet werden, wenn diese Strategie zur Notfallwiederherstellung verwendet wird. Mit dem Muster der automatischen Bereitstellung würde eine Clientanwendung oder ein Skript eine Bereitstellungsanforderung an eine Warteschlange senden, damit diese vom Bereitstellungsdienst verarbeitet wird, und würde dann vom Dienst den Abschluss des Vorgangs abfragen. Wurden Datenbanken vorab bereitgestellt, würden die Bereitstellungsanforderungen schnell verarbeitet, indem der Dienst die Bereitstellung einer Ersatzdatenbank im Hintergrund verwaltet.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen eines einzelnen neuen Mandanten
> * Bereitstellen eines Batches von zusätzlichen Mandanten
> * Kennenlernen der Details zur Bereitstellung von Mandanten und zu deren Registrierung im Katalog

Sehen Sie sich das [Tutorial zur Leistungsüberwachung](sql-database-saas-tutorial-performance-monitoring.md) an.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Zusätzliche [Tutorials, die auf der Wingtip-SaaS-Anwendung aufbauen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Clientbibliothek für elastische Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [So wird's gemacht: Debuggen von Skripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

