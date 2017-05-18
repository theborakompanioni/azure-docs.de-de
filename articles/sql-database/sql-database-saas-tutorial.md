---
title: Bereitstellen und Kennenlernen der Wingtip Tickets Platform-Anwendung (WTP-SaaS-Beispielanwendung, die Azure SQL-Datenbank verwendet) | Microsoft-Dokumentation
description: Bereitstellen und Kennenlernen einer SaaS-Beispielanwendung, die Azure SQL-Datenbank verwendet
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
ms.openlocfilehash: ff4dc19bb6e24ea9ceeed9721cfb3a85b4d10965
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-azure-sql-database"></a>Bereitstellen und Kennenlernen einer mehrinstanzenfähigen SaaS-Anwendung, die Azure SQL-Datenbank verwendet

In diesem Tutorial stellen Sie die Wingtip Tickets Platform-SaaS-Anwendung (WTP) bereit und machen sich mit dieser vertraut. Die Anwendung verwendet ein SaaS-Anwendungsmuster mit einer Datenbank pro Mandant, um mit mehreren Mandanten zu kommunizieren. Mit der Anwendung sollen die Funktionen von Azure SQL-Datenbank vorgestellt werden, die SaaS-Szenarien ermöglichen, einschließlich von SaaS-Entwurfs- und -Verwaltungsmustern.

Innerhalb von fünf Minuten, nachdem Sie unten auf die Schaltfläche *Bereitstellung in Azure* geklickt haben, verfügen Sie über eine funktionstüchtige mehrinstanzenfähige SaaS-Anwendung in der Cloud, die SQL-Datenbank verwendet. Die Anwendung wird mit drei Beispielmandanten mit jeweils einer eigenen Datenbank bereitgestellt. Diese werden alle in einem Pool für elastische SQL-Datenbanken bereitgestellt. Die App wird in Ihrem Azure-Abonnement bereitgestellt, wodurch Sie uneingeschränkten Zugriff erhalten und die einzelnen Anwendungskomponenten untersuchen und mit ihnen arbeiten können.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]

> * Vorgehensweise beim Bereitstellen der WTP-Anwendung
> * Die Server, Pools und Datenbanken, aus denen sich die App zusammensetzt
> * Mandanten werden den zugehörigen Daten mithilfe des *Katalogs* zugeordnet
> * Vorgehensweise beim Bereitstellen neuer Mandanten
> * Anzeigen der Poolnutzung zum Überwachen der Mandantenaktivität
> * Löschen von Beispielressourcen, um die zugehörige Abrechnung einzustellen

Zum Kennenlernen diverser SaaS-Entwurfs- und -Verwaltungsmuster ist eine [Reihe entsprechender Tutorials](sql-database-wtp-overview.md) verfügbar, die auf dieser Bereitstellung aufbauen. Machen Sie sich beim Durcharbeiten der Tutorials mit den bereitgestellten Skripts vertraut, und erfahren Sie, wie die verschiedenen SaaS-Muster implementiert werden. Arbeiten Sie die Skripts in jedem Tutorial schrittweise durch, um ein besseres Verständnis davon zu erlangen, wie Sie die vielen SQL-Datenbankfunktionen implementieren, die das Entwickeln von SaaS-Anwendungen erleichtern.

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Azure PowerShell ist installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-wtp-saas-application"></a>Bereitstellen der Wingtip Tickets Platform-SaaS-Anwendung (WTP)

Stellen Sie die Wingtip Tickets Platform in weniger als fünf Minuten bereit:

1. Klicken Sie hier, um die Bereitstellung zu starten:

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Geben Sie die erforderlichen Parameterwerte für die Bereitstellung ein:

    > [!IMPORTANT]
    > Die Sicherheit einiger Authentifizierungs- und Serverfirewalls wurde zu Vorführungszwecken absichtlich aufgehoben. **Erstellen Sie eine neue Ressourcengruppe**, und verwenden Sie keine vorhandenen Ressourcengruppen, Server oder Pools. Verwenden Sie in keinem Fall diese Anwendung oder von dieser erstellte Ressourcen in einer Produktionsumgebung. Wenn Sie sich umfassend mit der Anwendung vertraut gemacht haben, löschen Sie diese Ressourcengruppe, um die zugehörige Abrechnung einzustellen.

    * **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie **Name** und **Speicherort** an.
    * **Benutzer**: Für einige Ressourcen sind Namen erforderlich, die für sämtliche Azure-Abonnements eindeutig sind. Um eine solche Eindeutigkeit sicherzustellen, geben Sie einen Wert zur Unterscheidung der von Ihnen erstellten Ressourcen von den erstellten Ressourcen anderer Benutzer an, die die Wingtip-Anwendung bereitstellen. Es empfiehlt sich, einen kurzen **Benutzernamen** anzugeben, z.B. Ihre Initialen (wie *bg1*), und diesen dann im Namen der Ressourcengruppe zu verwenden (z.B. *wingtip-bg1*). Der Parameter **Benutzer** darf nur Buchstaben, Ziffern und Bindestriche enthalten. Beim ersten und letzten Zeichen muss es sich um einen Buchstaben oder eine Ziffer handeln (es wird empfohlen, ausschließlich Kleinbuchstaben zu verwenden).

     ![Vorlage](./media/sql-database-saas-tutorial/template.png)

1. **Bereitstellen der Anwendung**.

    * Klicken Sie auf die entsprechende Option, um den Geschäftsbedingungen zuzustimmen.
    * Wählen Sie die Option **An Dashboard anheften** aus.
    * Klicken Sie auf **Kaufen**.

1. Überwachen Sie den Bereitstellungsstatus, indem Sie auf **Benachrichtigungen** klicken (das Glockensymbol rechts neben dem Suchfeld). Das Bereitstellen der WTP-App dauert ca. vier Minuten.

   ![Bereitstellung erfolgreich](media/sql-database-saas-tutorial/succeeded.png)

## <a name="explore-the-application"></a>Erkunden der Anwendung

In der App werden Einrichtungen wie Konzerthallen, Jazzclubs, Sportstadien usw. vorgestellt, in denen Veranstaltungen stattfinden. Die Veranstalter registrieren sich als Kunden (oder Mandanten) der Wingtip Tickets Platform (WTP) und verfügen damit über eine einfache Möglichkeit, Veranstaltungen anzukündigen und Tickets zu verkaufen. Jeder Veranstalter erhält eine personalisierte Web-App zum Verwalten und Auflisten seiner Veranstaltungen und zum Verkaufen von Tickets. Diese ist unabhängig und abgegrenzt von denen anderer Mandanten. Für jeden Mandanten wird im Hintergrund eine SQL-Datenbank in einem Pool für elastische SQL-Datenbanken bereitgestellt.

In einem zentralen **Ereignis-Hub** wird eine Liste der Mandanten-URLs aufgeführt, die spezifisch für Ihre Bereitstellung ist. Alle Mandanten-URLs enthalten Ihren speziellen Wert für *Benutzer* und weisen das folgende Format auf: http://events.wtp.&lt;BENUTZER&gt;.trafficmanager.net/*fabrikamjazzclub*. 

1. Öffnen Sie den _Ereignis-Hub_: http://events.wtp.&lt;BENUTZER&gt;.trafficmanager.net (durch Ihren Benutzernamen ersetzen):

    ![Ereignis-Hub](media/sql-database-saas-tutorial/events-hub.png)

1. Klicken Sie im **Ereignis-Hub** auf *Fabrikam Jazz Club*.

   ![Ereignisse](./media/sql-database-saas-tutorial/fabrikam.png)

1. Klicken Sie auf **Tickets**, und prüfen Sie den Ticketverkauf für ein Ereignis.

Die WTP-Anwendung verwendet [*Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview), um die Verteilung des eingehenden Datenverkehrs zu steuern. Für die Ereignisseiten, die mandantenspezifisch sind, müssen Mandantennamen in die URLs eingeschlossen sein. Die Ereignis-App analysiert den Mandantennamen in der URL und erstellt auf seiner Grundlage einen Schlüssel für den Zugriff auf einen Katalog mithilfe der [*Shard-Zuordnungsverwaltung*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management). Im Katalog wird der Schlüssel dem Datenbankspeicherort für den Mandanten zugeordnet. Der **Ereignis-Hub** ruft mit erweiterten Metadaten im Katalog den Namen des Mandanten ab, der der jeweiligen Datenbank zugeordnet ist.

In einer Produktionsumgebung würde typischerweise ein CNAME-DNS-Eintrag zum [*Zuordnen einer Unternehmensinternetdomäne*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) zum Traffic Manager-Profil erstellt werden.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen der Wingtip-Anwendungsskripts

Die Wingtip Tickets-Skripts und der Quellcode der Anwendung stehen im [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS)-GitHub-Repository zur Verfügung. Skriptdateien befinden sich im Ordner [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Laden Sie den Ordner **Learning Modules** auf den lokalen Computer herunter, wobei Sie dessen Ordnerstruktur beibehalten.

## <a name="provision-a-new-tenant"></a>Bereitstellen eines neuen Mandanten

Bei der ersten Bereitstellung werden drei Beispielmandanten erstellt. In diesem Tutorial müssen jedoch weitere Mandanten erstellt werden. In diesem Schritt erstellen Sie schnell einen neuen Mandanten. Später sollten Sie sich im [Tutorial zum Bereitstellen und Katalogisieren](sql-database-saas-tutorial-provision-and-catalog.md) eingehender mit den Einzelheiten beim Bereitstellen neuer Mandanten vertraut machen. Dort werden Sie feststellen, wie einfach es ist, eine Registrierungskomponente in der Anwendung zu implementieren, sodass Mandanten beim Registrieren von Kunden automatisch bereitgestellt werden.

### <a name="initialize-the-user-config-file-for-your-deployment"></a>Initialisieren der Benutzerkonfigurationsdatei für die Bereitstellung

Da dies das erste Skript ist, das Sie nach der erstmaligen Bereitstellung der WTP-Anwendung ausführen, müssen Sie die Benutzerkonfigurationsdatei aktualisieren. Aktualisieren Sie die Variablen mit den spezifischen Werten aus Ihrer Bereitstellung.

   1. Öffnen Sie ...\\Learning Modules\\*UserConfig.psm1* in der *PowerShell ISE*
   1. Ändern Sie _$userConfig.ResourceGroupName_ in die _Ressourcengruppe_, die Sie beim Bereitstellen der App festgelegt haben.
   1. Ändern Sie _$userConfig.Name_ in den _Benutzernamen_, den Sie beim Bereitstellen der App festgelegt haben.

### <a name="provision-the-new-tenant"></a>Bereitstellen des neuen Mandanten

1. Öffnen Sie ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1* in der *PowerShell ISE*.
1. Drücken Sie **F5**, um das Skript auszuführen (übernehmen Sie zunächst die Standardwerte des Skripts).

Der neue Mandant wird im Katalog registriert. Seine Datenbank wird erstellt und einem Pool für elastische SQL-Datenbanken hinzugefügt. Nach erfolgreicher Bereitstellung wird die Ticketverkauf-Website des neuen Mandanten in Ihrem Browser angezeigt:

![Neuer Mandant](./media/sql-database-saas-tutorial/red-maple-racing.png)

Aktualisieren Sie den Ereignis-Hub, und vergewissern Sie sich, dass der neue Mandant in der Liste aufgeführt wird.

## <a name="start-generating-load-on-the-tenant-databases"></a>Generieren von Last in den Mandantendatenbanken

Nun verfügen wir über mehrere Mandantendatenbanken, die wir gleich einmal verwenden wollen. Ein PowerShell-Skript wird bereitgestellt, mit dem eine Workload für alle Mandantendatenbanken simuliert wird. Die Last wird standardmäßig 60 Minuten lang ausgeführt. Bei Wingtip Tickets handelt es sich um eine SaaS-App, und die reale Last auf eine SaaS-App ist normalerweise sporadisch und unvorhersagbar. Um dies zu simulieren, erzeugt der Last-Generator eine randomisierte Last, die auf alle Mandanten verteilt wird. Es dauert einige Minuten, bis das Muster ersichtlich wird. Lassen Sie daher den Last-Generator 5 bis 10 Minuten laufen, bevor Sie die Last in den folgenden Abschnitten beobachten.

1. Öffnen Sie ...\\Learning Modules\\Utilities\\*Demo-LoadGenerator.psm1* in der **PowerShell ISE**.
1. Drücken Sie **F5**, um das Skript auszuführen und den Last-Generator zu starten (übernehmen Sie zunächst die Standard-Parameterwerte).

> [!IMPORTANT]
> Der Last-Generator wird in Form einer Reihe von Aufträgen in Ihrer lokalen PowerShell-Sitzung ausgeführt. Sie dürfen also die PowerShell-Sitzung nicht schließen! Wenn Sie die Registerkarte, auf der der Last-Generator gestartet wurde, oder PowerShell schließen oder Ihren Computer in den Ruhezustand versetzen, wird die Ausführung der Aufträge beendet.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Untersuchen der Server, Pools und Mandantendatenbanken

Da Sie nun die Anwendung kennengelernt, einen neuen Mandanten bereitgestellt und eine Last für sämtliche Mandanten ausgeführt haben, betrachten Sie einige der bereitgestellten Ressourcen.

1. Öffnen Sie im [Azure-Portal](http://portal.azure.com) den Server **catalog-&lt;BENUTZER&gt;**. Der Katalogserver enthält zwei Datenbanken. Dies sind **tenantcatalog** und **basetenantdb** (eine leere *goldene* Datenbank, die kopiert wurde, um neue Mandanten zu erstellen).

   ![Datenbanken](./media/sql-database-saas-tutorial/databases.png)

1. Öffnen Sie den **tenants1-&lt;BENUTZER&gt;**-Server, auf dem sich die Mandantendatenbanken befinden. Beachten Sie, dass es sich bei jeder Mandantendatenbank um eine _elastische standardmäßige_ Datenbank in einem 50 eDTU-Standardpool handelt. Beachten Sie weiter, dass eine Datenbank _Red Maple Racing_ vorhanden ist, der zuvor von Ihnen bereitgestellte Mandant.

   ![server](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-the-pool"></a>Überwachen des Pools

Wenn der Last-Generator mehrere Minuten ausgeführt wurde, sollte eine ausreichende Datenmenge verfügbar sein, um einige der in Pools und Datenbanken integrierten Überwachungsfunktionen zu betrachten.

1. Navigieren Sie zum Server *tenants1-&lt;BENUTZER&gt;*, und klicken Sie auf **Pool1**, um die Ressourcennutzung für den Pool anzuzeigen:

   ![Überwachen des Pools](./media/sql-database-saas-tutorial/monitor-pool.png)

Diese beide Abbildungen veranschaulichen vorzüglich, wie gut sich Pools für elastische Datenbanken und SQL-Datenbank für Workloads von SaaS-Anwendungen eignen. Vier Datenbanken mit Bursts auf bis zu 40 eDTUs werden in einem 50 eDTU-Pool problemlos unterstützt. Wenn jede als eigenständige Datenbank bereitgestellt werden würde, muss jede eine S2-Datenbank (50 DTU) darstellen, um die Bursts zu unterstützen. Die Kosten für vier eigenständige S2-Datenbanken würde sich jedoch auf beinahe das Dreifache der Kosten für den Pool belaufen. Und der Pool bietet noch großzügigen Spielraum für viele weitere Datenbanken. In der Praxis betreiben Kunden derzeit bis zu 500 Datenbanken in 200 eDTU-Pools. Weitere Informationen finden Sie im [Tutorial zur Leistungsüberwachung](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="deleting-the-resources-created-with-this-tutorial"></a>Löschen der mit diesem Tutorial erstellten Ressourcen

Sobald Sie mit der WTP-App vertraut sind, navigieren Sie im Portal zur Ressourcengruppe der Anwendung, und löschen Sie sie, um jegliche künftige Abrechnung für diese Bereitstellung zu vermeiden. Wenn Sie die begleitenden Tutorials genutzt haben, werden alle von diesen erstellten Ressourcen ebenfalls zusammen mit der Anwendung gelöscht.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Vorgehensweise beim Bereitstellen der WTP-Anwendung
> * Die Server, Pools und Datenbanken, aus denen sich die App zusammensetzt
> * Mandanten werden den zugehörigen Daten mithilfe des *Katalogs* zugeordnet
> * Vorgehensweise beim Bereitstellen neuer Mandanten
> * Anzeigen der Poolnutzung zum Überwachen der Mandantenaktivität
> * Löschen von Beispielressourcen, um die zugehörige Abrechnung einzustellen

Arbeiten Sie nun das [Tutorial zum Bereitstellen und Katalogisieren von Mandanten](sql-database-saas-tutorial-provision-and-catalog.md) durch.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Weitere Tutorials, die auf der ersten Wingtip Tickets Platform-Anwendungsbereitstellung (WTP) aufbauen](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* Weitere Informationen zu Pools für elastische Datenbanken finden Sie unter [*Was ist ein Pool für elastische Azure SQL-Datenbanken*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* Weitere Informationen zu elastischen Aufträgen finden Sie unter [*Verwalten horizontal hochskalierter Clouddatenbanken*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
* Weitere Informationen zu mehrinstanzenfähigen SaaS-Anwendungen finden Sie unter [*Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)

