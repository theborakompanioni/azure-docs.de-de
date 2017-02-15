---
title: Zentrales Hochskalieren einer App in Azure | Microsoft Docs
description: "Erfahren Sie, wie Sie eine App in Azure App Service zentral hochskalieren, um Kapazität und Features hinzuzufügen."
services: app-service
documentationcenter: 
author: cephalin
manager: wpickett
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: b7ef8386e974486c2d327d0b042eeabdb9701b7e


---
# <a name="scale-up-an-app-in-azure"></a>Zentrales Hochskalieren einer App in Azure
In diesem Artikel wird die Vorgehensweise beim Skalieren Ihrer App in Azure App Service beschrieben. Es gibt zwei Workflows für die Skalierung: zentrales Hochskalieren und horizontales Hochskalieren. Dieser Artikel befasst sich mit dem Workflow zum zentralen Hochskalieren.

* [Zentrales Hochskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)Bei dieser Methode erhalten Sie mehr CPUs, mehr Arbeitsspeicher und mehr Speicherplatz sowie zusätzliche Features wie dedizierte virtuelle Computer, benutzerdefinierte Domänen und Zertifikate, Stagingslots, automatische Skalierung und mehr. Zum zentralen Hochskalieren muss der Tarif des App Service-Plans geändert werden, zu dem die App gehört.
* [Horizontales Hochskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling) Bei dieser Methode wird die Anzahl von VM-Instanzen erhöht, die Ihre App ausführen.
  Die Instanzenanzahl kann je nach Tarif auf bis zu 20 erhöht werden. [App Service-Umgebungen](../app-service/app-service-app-service-environments-readme.md) im Tarif **Premium** erhöhen die Anzahl der Instanzen bei horizontalem Hochskalieren weiter auf 50. Weitere Informationen zum horizontalen Hochskalieren finden Sie unter [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md) (Manuelles oder automatisches Skalieren der Instanzanzahl). Dort erfahren Sie, wie Sie die automatische Skalierung verwenden, d.h. das automatische Skalieren der Instanzenzahl basierend auf vordefinierten Regeln und Zeitplänen.

Diese Skalierungseinstellungen werden innerhalb von Sekunden angewendet und wirken sich auf alle Apps im [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)aus.
Dazu müssen Sie weder Ihren Code ändern noch Ihre Anwendung erneut bereitstellen.

Informationen zur Preisgestaltung und zu den Funktionen der einzelnen App Service-Pläne finden Sie unter [App Service Preise](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Bevor Sie einen App Service-Plan aus dem Tarif **Free** entfernen, müssen Sie zunächst die [Ausgabenlimits](https://azure.microsoft.com/pricing/spending-limits/) für Ihr Azure-Abonnement aufheben. Informationen zum Anzeigen oder Ändern von Optionen für Ihr Microsoft Azure App Service-Abonnement finden Sie unter [Microsoft Azure-Abonnements][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Zentrales Hochskalieren Ihres Tarifs
1. Öffnen Sie das [Azure-Portal][portal] in Ihrem Browser.
2. Klicken Sie auf dem Blatt Ihrer App auf **Alle Einstellungen** und anschließend auf **Zentral hochskalieren**.
   
    ![Navigieren Sie, um Ihre Azure-App zentral hochzuskalieren.][ChooseWHP]
3. Wählen Sie Ihren Tarif, und klicken Sie auf **Auswählen**.
   
    Auf der Registerkarte **Benachrichtigungen** wird in grüner Schrift der Text **ERFOLGREICH** angezeigt, nachdem der Vorgang abgeschlossen wurde.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Skalieren zugehöriger Ressourcen
Wenn Ihre App von anderen Diensten abhängt, beispielsweise von Azure SQL-Datenbank oder Azure Storage, können Sie diese Ressourcen basierend auf Ihren Anforderungen ebenfalls zentral hochskalieren. Diese Ressourcen werden nicht mit dem App Service-Plan skaliert, sondern müssen separat skaliert werden.

1. Klicken Sie in **Grundlegende Informationen** auf den Link **Ressourcengruppe**.
   
    ![Zentrales Hochskalieren der zugehörigen Ressourcen Ihrer Azure-App](./media/web-sites-scale/RGEssentialsLink.png)
2. Klicken Sie im Bereich **Zusammenfassung** des Blatts **Ressourcengruppe** auf eine Ressource, die Sie skalieren möchten. Der folgende Screenshot zeigt eine SQL-Datenbank-Ressource und eine Azure Storage-Ressource.
   
    ![Navigieren zum Blatt „Ressourcengruppe“, um Ihre Azure-App zentral hochzuskalieren](./media/web-sites-scale/ResourceGroup.png)
3. Klicken Sie für eine SQL-Datenbank-Ressource auf **Einstellungen** > **Tarif**, um den Tarif zu skalieren.
   
    ![Zentrales Hochskalieren des SQL-Datenbank-Back-Ends für Ihre Azure-App](./media/web-sites-scale/ScaleDatabase.png)
   
    Sie können auch die [Georeplikation](../sql-database/sql-database-geo-replication-overview.md) für Ihre SQL-Datenbankinstanz aktivieren.
   
    Klicken Sie für eine Azure Storage-Ressource auf **Einstellungen** > **Konfiguration**, um Ihre Speicheroptionen zentral hochzuskalieren.
   
    ![Zentrales Hochskalieren des Azure Storage-Kontos, das von Ihrer Azure-App verwendet wird](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a>Informationen zu Entwicklerfunktionen
Je nach Preisstufe stehen die folgenden entwicklungsbezogenen Funktionen zur Verfügung:

### <a name="bitness"></a>Bitness
* Die Preisstufen **Basic**, **Standard** und **Premium** unterstützen 64-Bit- und 32-Bit-Anwendungen.
* Die Preisstufen **Free** und **Shared** unterstützen nur 32-Bit-Anwendungen.

### <a name="debugger-support"></a>Debugger-Support
* Debugger-Support ist für die Modi **Free**, **Shared** und **Basic** bei einer Verbindung pro App Service-Plan verfügbar.
* Debugger-Support ist für die Modi **Standard** und **Premium** bei fünf gleichzeitigen Verbindungen pro App Service-Plan verfügbar.

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a>Informationen zu anderen Features
* Ausführliche Informationen zu allen weiteren Funktionen in den App Service-Plänen, einschließlich Preisgestaltung und allgemein interessanten Funktionen (auch für Entwickler) finden Sie in den [App Service – Preisdetails](https://azure.microsoft.com/pricing/details/web-sites/).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto registrieren, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte und keine Verpflichtungen erforderlich.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu den ersten Schritten mit Azure finden Sie unter [Kostenlose Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Informationen zu Preisen, Support und SLAs erhalten Sie unter den folgenden Links:
  
    [Preisdetails zur Datenübertragung](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Supportpläne für Microsoft Azure](https://azure.microsoft.com/support/plans/)
  
    [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/)
  
    [Preisdetails für SQL-Datenbanken](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Größen virtueller Computer und Clouddienste für Microsoft Azure][vmsizes]
  
    [App-Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service/)
  
    [App-Service-Preisdetails - SSL-Verbindungen](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* Informationen zu den Best Practices für Azure App Service, einschließlich dem Aufbau einer skalierbaren, robusten Architektur, finden Sie unter [Best Practices: Azure App Service-Web-Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* Videos zum Skalieren von App Service-Apps finden Sie in den folgenden Ressourcen:
  
  * [Wann sollte man Azure-Websites skalieren? - Mit Stefan Schackow](/documentation/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Automatisches Skalieren von Azure-Websites, CPU-abhängig oder geplant - Mit Stefan Schackow](/documentation/videos/auto-scaling-azure-web-sites/)
  * [Wie sollte man Azure-Websites skalieren? - Mit Stefan Schackow](/documentation/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png



<!--HONumber=Feb17_HO2-->


