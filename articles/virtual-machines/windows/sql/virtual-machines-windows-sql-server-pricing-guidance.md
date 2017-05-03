---
title: "Effektives Verwalten der Kosten für SQL Server auf virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Enthält bewährte Methoden zum Auswählen des richtigen Preismodells für virtuelle SQL Server-Computer."
services: virtual-machines-windows
documentationcenter: na
author: luisherring
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/18/2017
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 29a92f0c70bffedeb75c50b7fc3b687ee5ee227d
ms.lasthandoff: 04/20/2017


---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Preisinformationen für virtuelle Azure-Computer mit SQL Server

Dieses Thema bietet Preisinformationen für virtuelle SQL Server-Computer in Azure. Verschiedene Optionen wirken sich auf die Kosten aus, und es ist wichtig, das richtige Image auszuwählen, damit Kosten und Geschäftsanforderungen im Gleichgewicht sind.

## <a name="free-licensed-sql-server-editions"></a>SQL Server-Editionen mit kostenlosen Lizenzen

Wenn Sie Funktionen entwickeln oder testen oder eine Machbarkeitsstudie erstellen möchten, verwenden Sie die **SQL Server Developer Edition** mit kostenloser Lizenz. Diese Edition bietet den vollständigen Funktionsumfang der SQL Server Enterprise Edition, daher können Sie jede beliebige Anwendung damit erstellen. Die Edition darf allerdings nicht in der Produktion ausgeführt werden. Bei einem virtuellen SQL Server Developer-Computer werden nur die Kosten des virtuellen Computers berechnet, nicht die SQL Server-Lizenzierung.

Wenn Sie eine kleine Arbeitsauslastung in der Produktion ausführen möchten (<4 Kerne, <1 GB Arbeitsspeicher, <10 GB/Datenbank), verwenden Sie die **SQL Server Express Edition** mit kostenloser Lizenz. Bei einem virtuellen SQL Server Express-Computer werden nur die Kosten des virtuellen Computers berechnet, nicht die SQL-Lizenzierung.

Für solche Entwicklungs- oder Testarbeitsauslastungen oder Arbeitsauslastungen mit geringem Umfang können Sie auch Geld sparen, indem Sie eine kleinere VM-Größe auswählen, die für diese Arbeitsauslastungen ausreichend ist. Die Größe DS1v2 beispielsweise eignet sich für diese Art Arbeitsauslastungen.

Informationen zum Erstellen eines virtuellen Azure-Computers mit SQL Server 2016 mit einem dieser Images finden Sie unter den folgenden Links:

- [Virtueller Azure-Computer mit SQL Server 2016 Developer](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016-ARM)
- [Virtueller Azure-Computer mit SQL Server 2016 Express](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016-ARM)

## <a name="paid-sql-server-editions"></a>Kostenpflichtige SQL Server-Editionen

Für umfangreichere Arbeitsauslastungen in der Produktion verwenden Sie eine der folgenden SQL Server-Editionen:

| SQL Server-Edition | Workload |
|-----|-----|
| Web | Kleine Websites |
| Standard | Kleine bis mittelgroße Arbeitsauslastungen |
| Enterprise | Umfangreiche oder geschäftskritische Arbeitsauslastungen|

Es gibt zwei Möglichkeiten, für die Lizenzierung dieser SQL Server-Editionen zu zahlen: *Nutzungsbasierte Bezahlung* oder *Bring Your Own License (BYOL)*.

### <a name="pay-per-usage"></a>Nutzungsbasierte Bezahlung

Die **Bezahlung für SQL Server-Lizenzen je nach Nutzung** bedeutet, dass die minutenbasierten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz einschließen. Die Preise für die verschiedenen SQL-Server-Editionen (Web, Standard, Enterprise) finden Sie auf der [Preisseite für virtuelle Azure-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). Die Kosten sind für alle SQL Server-Versionen (2008 R2 bis 2016) gleich. Ebenso wie bei der SQL Server-Lizenzierung im Allgemeinen richten sich die Lizenzierungskosten pro Minute nach der Anzahl der VM-Kerne.

Die nutzungsbasierte Bezahlung der SQL Server-Lizenzierung empfiehlt sich in folgenden Fällen:

- Temporär oder regelmäßig auftretende Arbeitsauslastungen. Beispiele: eine App, die jedes Jahr einige Monate lang ein Ereignis unterstützen muss, oder Geschäftsanalysen, die jeden Montag ausgeführt werden.
- Arbeitsauslastungen mit unbekannter Lebensdauer oder unbekanntem Umfang. Beispiele: eine App, die einige Monate lang nicht benötigt wird, oder eine App, die je nach Bedarf mehr oder weniger Computeleistung benötigt.

Informationen zum Erstellen eines virtuellen Azure-Computers mit SQL Server 2016 mit einem dieser Images mit nutzungsbasierter Bezahlung finden Sie unter den folgenden Links:

- [Virtueller Azure-Computer mit SQL Server 2016 Web](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)
- [Virtueller Azure-Computer mit SQL Server 2016 Standard](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)
- [Virtueller Azure-Computer mit SQL Server 2016 Enterprise](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)

> [!IMPORTANT]
> Wenn Sie im Azure-Portal einen virtuellen SQL Server-Computer erstellen, werden bei den voraussichtlichen monatlichen Kosten auf dem Blatt **Größe auswählen** die SQL Server-Lizenzierungskosten nicht berücksichtigt. Hierbei handelt es sich ausschließlich um die Kosten für den virtuellen Computer.
>
> ![Blatt zum Auswählen der VM-Größe](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Bei der Express Edition und der Developer Edition von SQL Server mit kostenlosen Lizenzen sind das die voraussichtlichen Gesamtkosten. Für die Editionen Web, Standard und Enterprise finden Sie die zusätzlichen SQL-Lizenzierungskosten auf der [Preisseite für virtuelle Windows-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Wählen Sie auf der Preisseite die gewünschte Edition von SQL Server aus.

### <a name="bring-your-own-license-byol"></a>Bring Your Own License (BYOL)

Das Konzept der **Verwendung eigener SQL-Lizenzen über die „Lizenzmobilität“**, auch als **BYOL** bezeichnet, bedeutet, dass Sie eine vorhandene SQL Server-Volumenlizenz mit Software Assurance auf einem virtuellen Azure-Computer nutzen. Bei einem virtuellen SQL Server-Computer mit BYOL-Modell werden nur die Kosten für die Ausführung des Computers berechnet, nicht für die SQL Server-Lizenzierung. Dies setzt voraus, dass Sie bereits über ein Volumenlizenzprogramm Lizenzen und Software Assurance erworben haben.

Die Verwendung eigener SQL-Lizenzen über die „Lizenzmobilität“ empfiehlt sich in folgenden Fällen:

- Kontinuierliche Arbeitsauslastungen. Beispiel: eine App, die rund um die Uhr Geschäftsabläufe unterstützen muss.
- Arbeitsauslastungen mit bekannter Lebensdauer und bekanntem Umfang. Beispiel: eine App mit prognostizierten Anforderungen, die das ganze Jahr über benötigt wird.

Um BYOL mit einem virtuellen SQL Server-Computer zu verwenden, müssen Sie über eine Lizenz für SQL Server Standard oder Enterprise sowie über [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1) verfügen. Software Assurance ist in einigen Programmen zur [Volumenlizenzierung](https://www.microsoft.com/en-us/download/details.aspx?id=10585) erforderlich und in anderen Programmen optional.  Die Preisstufen, die in den verschiedenen Volumenlizenzprogrammen angeboten werden, unterscheiden sich je nach Art der Vereinbarung sowie nach Umfang und Nutzungsdauer von SQL Server. Generell bietet das BYOL-Modell für kontinuierliche Arbeitsauslastungen in der Produktion folgende Vorteile:

| BYOL-Vorteil | Beschreibung |
|-----|-----|
| **Kostenersparnis** | Die Nutzung eigener SQL Server-Lizenzen ist kostengünstiger als die nutzungsbasierte Bezahlung, wenn eine Arbeitsauslastung SQL Server Standard oder Enterprise *länger als 10 Monate* kontinuierlich ausführt. |
| **Langfristige Einsparungen** | Im Durchschnitt ist es in den ersten drei Jahren *pro Jahr um 30%* günstiger, eine SQL Server-Lizenz zu erwerben oder zu verlängern. Nach drei Jahren müssen Sie die Lizenz nicht mehr verlängern, sondern zahlen nur noch für Software Assurance. Ab diesem Punkt ist BYOL *200% günstiger*. |
| **Kostenloses passives sekundäres Replikat** | Ein weiterer Vorteil der Nutzung eigener Lizenzen ist die [kostenlose Lizenz für ein passives sekundäres Replikat](https://azure.microsoft.com/pricing/licensing-faq/) pro SQL Server-Instanz, um eine hohe Verfügbarkeit sicherzustellen. Damit halbieren sich die Lizenzierungskosten einer hochverfügbaren SQL Server-Bereitstellung (z.B. mit AlwaysOn-Verfügbarkeitsgruppen). Die Berechtigungen zum Ausführen des passiven sekundären Replikats werden über den Software Assurance-Vorteil für Failoverserver bereitgestellt. |

Um einen virtuellen Azure-Computer mit SQL Server 2016 mit einem dieser BYOL-Images zu erstellen, verwenden Sie die VMs mit dem Präfix „{BYOL}“:

- [Virtueller Azure-Computer mit SQL Server 2016 Enterprise](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [Virtueller Azure-Computer mit SQL Server 2016 Standard](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!NOTE]
> Informieren Sie uns innerhalb von 10 Tagen, wie viele SQL Server-Lizenzen Sie in Azure verwenden werden. Unter den Links zu den oben genannten Images finden Sie Anweisungen dazu.

## <a name="avoid-unecessary-costs"></a>Vermeiden unnötiger Kosten

Wenn Sie Arbeitsauslastungen verwenden, die nicht kontinuierlich ausgeführt werden, sollten Sie erwägen, den virtuellen Computer während der inaktiven Zeiträume herunterzufahren. Sie bezahlen nur für die tatsächliche Nutzung.

Wenn Sie SQL Server z.B. auf einem virtuellen Azure-Computer nur einmal ausprobieren möchten, liegt es sicher nicht in Ihrem Interesse, Gebühren zu verursachen, indem der Computer versehentlich wochenlang ausgeführt wird. Eine Lösung für dieses Problem ist die [Feature zum automatischen Herunterfahren](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Automatisches Herunterfahren von virtuellen SQL-Computern](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Das automatische Herunterfahren gehört zu einem größeren Satz ähnlicher Features, die von [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab) bereitgestellt wurden.

Bei anderen Workflows sollten Sie eine Skriptlösung wie z.B. [Azure Automation](https://azure.microsoft.com/services/automation/) in Betracht ziehen, um virtuelle Azure-Computer automatisch herunterzufahren und zu starten.

> [!IMPORTANT]
> Das Herunterfahren und Freigeben des virtuellen Computers ist die einzige Möglichkeit, Gebühren zu vermeiden. Wenn Sie einen virtuellen Computer einfach nur beenden oder in einen Energiesparmodus versetzen, fallen weiterhin Nutzungsgebühren an.

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zu den Preisen in Azure finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](../../../billing/billing-getting-started.md).

Die neuesten Preisinformationen für virtuelle Computer, einschließlich SQL Server, finden Sie auf der [Preisseite für virtuelle Azure-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Weitere Themen zu virtuellen SQL Server-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

