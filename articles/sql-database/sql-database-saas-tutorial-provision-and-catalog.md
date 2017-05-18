---
title: Bereitstellen und Katalogisieren neuer Mandanten (SaaS-Beispielanwendung unter Verwendung von Azure SQL-Datenbank) | Microsoft-Dokumentation
description: Bereitstellen und Katalogisieren neuer Mandanten
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 4eeada941f8615fa04624bc725efcb44f05d56c7
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Bereitstellen neuer Mandanten und Registrieren der Mandanten im Katalog

In diesem Tutorial stellen Sie neue Mandanten in der Wingtip Tickets Platform (WTP)-SaaS-Anwendung bereit. Sie erstellen Mandanten und Mandantendatenbanken und registrieren diese Mandanten im Katalog. Der *Katalog* ist eine Datenbank, in der die Zuordnung zwischen den SaaS-Anwendungen, vielen Mandanten und den zugehörigen Daten verwaltet wird. Verwenden Sie diese Skripts, um die verwendeten Bereitstellungs- und Katalogmuster zu erkunden und zu erfahren, wie die Registrierung neuer Mandanten im Katalog implementiert wird. Der Katalog spielt eine wichtige Rolle, da er die Anwendungsanforderungen an die richtigen Datenbanken weiterleitet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Bereitstellen eines einzelnen neuen Mandanten
> * Bereitstellen eines Batches von zusätzlichen Mandanten
> * Kennenlernen der Details zur Bereitstellung neuer Mandanten und zu deren Registrierung im Katalog.


Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die WTP-App wurde bereitgestellt. Unter [Bereitstellen und Kennenlernen der SaaS-Anwendung WTP](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell ist installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Einführung in die SaaS-Katalogmuster

In einer in Datenbanken gesicherten, mehrinstanzenfähigen SaaS-Anwendung ist es wichtig zu wissen, wo Informationen über jeden einzelnen Mandanten gespeichert sind. Im SaaS-Katalogmuster dient eine Katalogdatenbank zum Speichern der Zuordnungen zwischen Mandanten und den Speicherorten ihrer Daten. Die WTP-App verwendet eine Datenbankarchitektur mit einem einzelnen Mandanten, aber das grundlegende Muster zum Speichern der Zuordnung zwischen Mandant und Datenbank in einem Katalog gilt, ganz gleich, ob eine mehrinstanzenfähige Datenbank oder eine Datenbank mit nur einem Mandanten verwendet wird.

Jedem Mandanten ist ein Schlüssel zugewiesen, der seine Daten im Katalog identifiziert. In der WTP-Anwendung wird der Schlüssel aus einem Hash des Mandantennamens gebildet. Anhand dieses Musters kann der Mandantennamensteil der Anwendungs-URL verwendet werden, um den Schlüssel zu erstellen und die Verbindung eines bestimmten Mandanten abzurufen. Andere ID-Schemas könnten ohne Auswirkungen auf das gesamte Muster verwendet werden.

Der Katalog in der App WTP wird mithilfe der Shardverwaltungstechnologie in der [Clientbibliothek für elastische Datenbanken (Elastic Database Client Library, EDCL)](sql-database-elastic-database-client-library.md) implementiert. EDCL dient zum Erstellen und Verwalten eines in einer Datenbank gesicherten _Katalogs_, der eine _Shardzuordnung_ verwaltet. Der Katalog enthält die Zuordnung zwischen Schlüsseln (Mandanten) und ihren Datenbanken (Shards).

> [!IMPORTANT]
> Sie können auf die Zuordnungsdaten in der Katalogdatenbank zugreifen, *allerdings sollten Sie sie nicht bearbeiten*. Bearbeiten Sie Zuordnungsdaten nur über EDCL-APIs. Ein direktes Bearbeiten der Zuordnungsdaten birgt das Risiko, den Katalog zu beschädigen, und wird daher nicht unterstützt.

Die Wingtip-SaaS-App stellt neue Mandanten durch Kopieren einer *goldenen* Datenbank bereit.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip Tickets-Skripts und der Quellcode der Anwendung stehen im [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS)-Github-Repository zur Verfügung. Skriptdateien befinden sich im Ordner [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) (Lernmodule). Laden Sie den Ordner **Learning Modules** auf den lokalen Computer herunter, wobei Sie dessen Ordnerstruktur beibehalten.

## <a name="provision-a-new-tenant"></a>Bereitstellen eines neuen Mandanten

Wenn Sie bereits im ersten WTP-Tutorial einen Mandanten erstellt haben, können Sie direkt mit dem nächsten Abschnitt fortfahren: [Bereitstellen ein Batches von Mandanten](#provision-a-batch-of-tenants).

Führen Sie das Skript *Demo-ProvisionAndCatalog* aus, um schnell einen Mandanten zu erstellen und im Katalog zu registrieren:

1. Öffnen Sie „**Demo-ProvisionAndCatalog.ps1**“ in der PowerShell-ISE, und legen Sie die folgenden Werte fest:
   * **$TenantName** = Name des neuen Veranstaltungsorts (z.B., *Bushwillow Blues*). 
   * **$VenueType** = einer der vordefinierten Veranstaltungsorttypen: „blues“, „classicalmusic“, „dance“, „jazz“, „judo“, „motorracing“, „multipurpose“, „opera“, „rockmusic“, „soccer“.
   * **$DemoScenario** = 1, behalten Sie hierfür die Einstellung _1_ für **Bereitstellen eines einzelnen Mandanten** bei.

1. Drücken Sie **F5**, um das Skript auszuführen.

Nach Abschluss des Skripts wird der neue Mandant bereitgestellt und seine *Veranstaltungen*-App im Browser geöffnet:

![Neuer Mandant](./media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Bereitstellen eines Batches von Mandanten

In dieser Übung wird einen Batch von zusätzlichen Mandanten bereitgestellt. Es wird empfohlen, diese Übung auszuführen, bevor Sie weitere WTP-Tutorials absolvieren.

1. Öffnen Sie „...\\Learning Modules\\Utilities\\*Demo-ProvisionAndCatalog.ps1*“ in der *PowerShell-ISE*, und legen Sie die folgenden Werte fest:
   * **$DemoScenario** = **3**, festgelegt auf **3** für **Bereitstellen eines Batches von Mandanten**.
1. Drücken Sie **F5**, um das Skript auszuführen.

Das Skript stellt einen Batch von zusätzlichen Mandanten bereit. Es verwendet eine [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-template-walkthrough.md), die den Batch steuert und anschließend die Bereitstellung der einzelnen Datenbanken an eine verknüpfte Vorlage delegiert. Indem Sie Vorlagen auf diese Weise verwenden, können Sie mit Azure Resource Manager den Bereitstellungsprozess für das Skript vermitteln. Vorlagen stellen Datenbanken, wann immer möglich, parallel bereit, und verarbeiten nötigenfalls Wiederholungen, um den Gesamtprozess zu optimieren. Das Skript ist idempotent, das bedeutet, es wird bei einer Unterbrechung erneut ausgeführt.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Überprüfen, ob der Mandantenbatch erfolgreich bereitgestellt wurde

* Öffnen Sie den Server *tenants1* im [Azure-Portal](https://portal.azure.com), und klicken Sie auf **SQL-Datenbanken**:

   ![Datenbankliste](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)


## <a name="provision-and-catalog-detailed-walkthrough"></a>Bereitstellen und Katalogisieren: detaillierte exemplarische Vorgehensweise

Um besser zu verstehen, wie die Wingtip-Anwendung die Bereitstellung neuer Mandanten implementiert, führen Sie das Skript „*Demo-ProvisionAndCatalog*“ erneut aus, und stellen Sie noch einen weiteren Mandanten bereit. Fügen Sie dieses Mal einen Haltepunkt ein, und durchlaufen Sie den Workflow:

1. Öffnen Sie „...\\Learning Modules\Utilities\_Demo-ProvisionAndCatalog.ps1_“, und legen die folgenden Einstellungen auf die Werte des neuen Mandanten, die im aktuellen Katalog nicht vorhanden sind:
   * **$TenantName** = festgelegt auf einen neuen Namen (z.B. *Hackberry Hitters*).
   * **$VenueType** = verwenden Sie einen der vordefinierten Veranstaltungsorttypen (z.B. *Judo*).
   * **$DemoScenario** = 1, festgelegt auf **1** für **Bereitstellen eines einzelnen Mandanten**.

1. Fügen Sie einen Haltepunkt hinzu, indem Sie den Cursor an eine beliebige Stelle in der folgenden Zeile bewegen: *New-Tenant `*, und drücken Sie **F9**.

   ![Haltepunkt](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Drücken Sie **F5** , um das Skript auszuführen. Wenn der Haltepunkt erreicht wird, drücken Sie **F11** für eine schrittweise Ausführung. Verfolgen Sie die Ausführung des Skripts mit **F10** und **F11** nach, um die aufgerufenen Funktionen zu überspringen oder schrittweise auszuführen. [Tipps zum Arbeiten mit und Debuggen von PowerShell-Skripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

### <a name="examine-the-provision-and-catalog-implementation-in-detail-by-stepping-through-the-script"></a>Überprüfen der Bereitstellung und Katalogimplementierung im Detail durch schrittweises Durchlaufen des Skripts

Das Skript führt die Bereitstellung und Katalogisierung neuer Mandanten mit den die folgenden Schritten aus:

1. **Importieren Sie das Modul „SubscriptionManagement.psm1“**, das Funktionen enthält, um sich bei Azure anzumelden und das Azure-Abonnement auszuwählen, mit dem Sie arbeiten.
1. **Importieren Sie das Modul „CatalogAndDatabaseManagement.psm1“**, das eine Abstraktion auf Katalog- und Mandantenebene über die [Shardverwaltungsfunktionen](sql-database-elastic-scale-shard-map-management.md) bereitstellt. Dies ist ein wichtiges Modul, das einen Großteil des Katalogmusters kapselt. Es lohnt sich, dieses Modul genauer zu betrachten.
1. **Rufen Sie Konfigurationsinformationen ab**. Führen Sie _Get-Configuration_ schrittweise aus (mit **F11**), und schauen Sie sich an, wie die App-Konfiguration angegeben ist. Hier sind Ressourcennamen und andere App-spezifische Werte definiert. Ändern Sie hier aber keine Werte, es sei denn, sie kennen sich gut mit Skripts aus.
1. **Rufen Sie das Katalogobjekt ab**. Führen Sie *Get-Catalog* schrittweise aus, und schauen Sie sich an, wie der Katalog mit den aus **AzureShardManagement.psm1** importierten Shardverwaltungsfunktionen initialisiert wird. Der Katalog besteht aus den folgenden Objekten:
   * „$catalogServerFullyQualifiedName“ wird aus dem Standardstamm plus Ihrem Benutzernamen gebildet: _catalog-\<Benutzer\>.database.windows.net_.
   * „$catalogDatabaseName“ wird aus der Konfigurationsdatei abgerufen: *tenantcatalog*.
   * Das Objekt „$shardMapManager“ wird aus der Katalogdatenbank initialisiert.
   * Das Objekt „$shardMap“ wird aus der Shardzuordnung *tenantcatalog* in der Katalogdatenbank initialisiert.
   Ein Katalogobjekt wird gebildet und aus zurückgegeben und im Skript der höheren Ebene verwendet.
1. **Berechnen Sie den neuen Mandantenschlüssel**. Eine Hashfunktion wird verwendet, um den Mandantenschlüssel aus dem Mandantennamen zu erstellen.
1. **Überprüfen Sie, ob der Mandantenschlüssel bereits vorhanden ist**. Der Katalog wird überprüft, um sicherzustellen, dass der Schlüssel verfügbar ist.
1. **Die Mandantendatenbank wird mit „New-TenantDatabase“ bereitgestellt.** Drücken Sie **F11** und schauen Sie sich an, wie die Datenbank mithilfe einer Resource Manager-Vorlage bereitgestellt wird.
    
Der Datenbankname wird aus dem Mandantennamen gebildet, um zu verdeutlichen, welcher Shard zu welchem Mandanten gehört. (Andere Strategien für die Datenbankbenennung können problemlos verwendet werden.)

Eine Resource Manager-Vorlage wird verwendet, um **durch Kopieren einer *goldenen* Datenbank (baseTenantDB) eine Mandantendatenbank auf dem Katalogserver zu erstellen**.  Ein alternativer Ansatz könnte sein, eine leere Datenbank zu erstellen, und diese anschließend durch Importieren einer BACPAC-Datei zu initialisieren.

Die Resource Manager-Vorlage befindet sich im Ordner „…\\Learning Modules\\Common\\“: *tenantdatabasecopytemplate.json*

Nachdem die Mandantendatenbank erstellt wurde, wird sie anschließend mit dem Namen des Veranstaltungsorts (Mandanten) und dem Veranstaltungsorttyp initialisiert. Hier können Sie auch weitere Initialisierungen vornehmen.

Die Mandantendatenbank wird im Katalog mit *Add-TenantDatabaseToCatalog* unter Verwendung des Mandantenschlüssels registriert. Drücken Sie **F11**, um sich die Schritte im Detail anzuschauen:

* Die Katalogdatenbank wird der Shardzuordnung (Liste der bekannten Datenbanken) hinzugefügt.
* Die Zuordnung, die den Schlüsselwert mit dem Shard verknüpft, wird erstellt.
* Zusätzliche Metadaten (Name des Veranstaltungsorts) über den Mandanten werden hinzugefügt.

Nach Abschluss der Bereitstellung kehrt die Ausführung zum ursprünglichen *Demo-ProvisionAndCatalog*-Skript zurück, und die Seite mit den **Ereignissen** für den neuen Mandanten wird im Browser geöffnet:

   ![events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant2.png)


## <a name="other-provisioning-patterns"></a>Weitere Bereitstellungsmuster

Weitere Bereitstellungsmuster, die in diesem Tutorial nicht behandelt werden:

**Vorabbereitstellung von Datenbanken.** Dieses Muster nutzt den Fakt, dass Datenbanken in einem Pool für elastische Datenbanken keine zusätzliche Kosten verursachen (die Abrechnung erfolgt für den Pool für elastische Datenbanken, nicht für die Datenbanken) und dass Datenbanken im Leerlauf keine Ressourcen verbrauchen. Indem Sie Datenbanken vorab in einem Pool bereitstellen, und diese später bei Bedarf zuordnen, kann die Integration von Mandanten (Onboarding) erheblich beschleunigt werden. Die Anzahl der vorab bereitgestellten Datenbanken kann bedarfsorientiert angepasst werden, um einen für die erwartete Bereitstellungsrate geeigneten Puffer bereitzuhalten.

**Automatische Bereitstellung.** Bei diesem Muster wird ein dedizierter Bereitstellungsdienst verwendet, um Server, Pools und Datenbanken entsprechend dem automatisch bereitzustellen – einschließlich vorab bereitgestellter Datenbanken in Pools für elastische Datenbanken, falls erforderlich. Wenn Datenbanken außer Betrieb genommen und gelöscht werden, können Lücken in den Pools für elastische Datenbanken durch den Bereitstellungsdienst bedarfsorientiert gefüllt werden. Ein solcher Dienst kann einfach oder komplex sein, die Bereitstellung also beispielsweise über mehrere geografische Regionen verarbeiten, und könnte automatisch Georeplikation einrichten, wenn diese Strategie zur Notfallwiederherstellung verwendet wird. Mit dem Muster der automatischen Bereitstellung würde eine Clientanwendung oder ein Skript eine Bereitstellungsanforderung an eine Warteschlange senden, damit diese vom Bereitstellungsdienst verarbeitet wird, und würde dann vom Dienst den Abschluss des Vorgangs abfragen. Wurden Datenbanken vorab bereitgestellt, würden die Bereitstellungsanforderungen schnell verarbeitet, indem der Dienst die Bereitstellung einer Ersatzdatenbank im Hintergrund verwaltet.


## <a name="stopping-wingtip-saas-application-related-billing"></a>Beenden der Wingtip SaaS-anwendungsbezogenen Abrechnung

Wenn Sie kein weiteres Tutorial absolvieren möchten, empfiehlt es sich, alle Ressourcen zu löschen, um deren Berechnung zu beenden. Löschen Sie die Ressourcengruppe, in der die WTP-Anwendung bereitgestellt wurde, und alle zugehörigen Ressourcen werden gelöscht.

* Browsen Sie im Portal zur Ressourcengruppe der Anwendung, und löschen Sie sie, um jegliche künftige Abrechnung für diese WTP-Bereitstellung zu vermeiden.

## <a name="tips"></a>Tipps

* EDCL enthält außerdem wichtige Features, die Clientanwendungen ermöglichen, eine Verbindung zum Katalog herzustellen und ihn zu bearbeiten. Sie können EDCL auch verwenden, um eine ADO.NET-Verbindung für einen bestimmten Schlüsselwert abzurufen, wodurch die Anwendung die Verbindung mit der richtigen Datenbank herstellen kann. Der Client speichert diese Verbindungsinformationen zwischen, um den Datenverkehr zur Katalogdatenbank zu minimieren und die Anwendung zu beschleunigen.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen eines einzelnen neuen Mandanten
> * Bereitstellen eines Batches von zusätzlichen Mandanten
> * Kennenlernen der Details zur Bereitstellung neuer Mandanten und zu deren Registrierung im Katalog.

[Tutorial zur Leistungsüberwachung](sql-database-saas-tutorial-performance-monitoring.md)

## <a name="additional-resources"></a>Weitere Ressourcen

* [Weitere Tutorials, die auf der ersten Wingtip Tickets Platform-Anwendungsbereitstellung (WTP) aufbauen](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Clientbibliothek für elastische Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [So wird's gemacht: Debuggen von Skripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

