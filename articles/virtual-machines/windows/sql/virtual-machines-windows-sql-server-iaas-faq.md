---
title: "SQL Server auf virtuellen Azure-Computern – FAQ | Microsoft Docs"
description: "Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Ausführung von SQL Server auf Azure-VMs."
services: virtual-machines-windows
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/07/2017
ms.author: v-shysun
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: a7d1878931ccf45e8e3192d5d03a4292c5291366


---
# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server auf virtuellen Azure-Computern – FAQ
Dieses Thema bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
1. **Wie erstelle ich einen virtuellen Azure-Computer mit SQL Server?**
   
    Die einfachste Lösung ist die Erstellung eines virtuellen Computers, der SQL Server enthält. Ein Tutorial für die Anmeldung bei Azure und das Erstellen einer SQL-VM im Portal finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Sie können ein virtuelles Computerimage auswählen, das eine SQL Server-Lizenzierung mit minutenbasierter Bezahlung verwendet, oder Sie verwenden ein Image, das die Nutzung Ihrer eigenen SQL Server-Lizenz zulässt. Sie können SQL Server auch manuell auf einem virtuellen Computer installieren und dafür eine lokale Lizenz wiederverwenden. Wenn Sie Ihre eigene Lizenz verwenden, benötigen Sie die [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/).
2. **Was ist der Unterschied zwischen SQL-VMs und dem SQL-Datenbank-Dienst?**
   
    Im Prinzip unterscheidet sich die Ausführung von SQL Server auf virtuellen Azure-Computern nicht von der Ausführung in einem Remoterechenzentrum. Im Gegensatz dazu bietet [SQL-Datenbank](../../../sql-database/sql-database-technical-overview.md) Database-as-a-Service. Mit SQL-Datenbank haben Sie keinen Zugriff auf die Computer, die Ihre Datenbanken hosten. Einen vollständigen Vergleich finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).
3. **Wie kann ich meine lokale SQL Server-Datenbank zur Cloud migrieren?**
   
    Erstellen Sie zuerst einen virtuellen Azure-Computer mit einer SQL Server-Instanz. Migrieren Sie dann Ihre lokalen Datenbanken zu dieser Instanz. Datenmigrationsstrategien finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).
4. **Kann ich die installierten Funktionen ändern oder eine zweite Instanz von SQL Server auf dem gleichen virtuellen Computer installieren?**
   
    Ja. Die SQL Server-Installationsmedien befinden sich in einem Ordner auf dem Laufwerk **C** . Führen Sie **Setup.exe** von diesem Speicherort aus, um neue SQL Server-Instanzen hinzuzufügen oder andere installierte Features von SQL Server auf dem Computer zu ändern.
5. **Wie aktualisiere ich auf eine neue Version/Edition von SQL Server auf einer Azure-VM?**
   
    Derzeit ist kein direktes Upgrade für SQL Server auf einer Azure-VM verfügbar. Erstellen Sie einen neuen virtuellen Azure-Computer mit der gewünschten Version/Edition von SQL Server, und migrieren Sie dann Ihre Datenbanken mit dem standardmäßigen [Datenmigrationsverfahren](virtual-machines-windows-migrate-sql.md) zu dem neuen Server.
6. **Wie kann ich meine lizenzierte Kopie von SQL Server auf einer Azure-VM installieren?**
   
    Es gibt hierbei zwei Möglichkeiten. Sie können eines der [virtuellen Computerimages bereitstellen, das Lizenzen unterstützt](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Eine weitere Möglichkeit ist das Kopieren der SQL Server-Installationsmedien auf einen virtuellen Windows Server-Computer und das anschließende Installieren von SQL Server auf dem virtuellen Computer. Aus Gründen der Lizenzierung benötigen Sie die [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/).
7. **Kann ich einen virtuellen Computer so ändern, dass meine eigene SQL Server-Lizenz verwendet wird, wenn er mithilfe eines der Katalogimages mit nutzungsbasierter Bezahlung erstellt wurde?**

    Nr. Der Wechsel von Lizenzierung mit minutenbasierter Bezahlung zur Nutzung der eigenen Lizenz ist nicht möglich. Erstellen Sie mithilfe eines der [BYOL-Images](virtual-machines-windows-sql-server-iaas-overview.md#BYOL) einen neuen virtuellen Azure-Computer, und migrieren Sie dann Ihre Datenbanken mit dem standardmäßigen [Datenmigrationsverfahren](virtual-machines-windows-migrate-sql.md) zum neuen Server.

7. **Muss ich Lizenzgebühren für SQL Server auf einem virtuellen Azure-Computer bezahlen, wenn dieser nur für Standby/Failover verwendet wird?**
   
    Sie müssen keine Lizenzgebühren für einen SQL Server bezahlen, wenn dieser als passives sekundäres Replikat in einer Bereitstellung mit hoher Verfügbarkeit fungiert, sofern Sie über Software Assurance verfügen und Lizenzmobilität gemäß der Beschreibung in [FAQ zur Lizenzierung von virtuellen Computer](http://azure.microsoft.com/pricing/licensing-faq/) verwenden.
    
8. **Wie werden Updates und Servicepacks auf eine SQL Server-VM angewendet?**
   
    Virtuelle Computer bieten Ihnen die Kontrolle über die Hostcomputer, einschließlich des Zeitpunkts, zu dem Updates angewendet werden sollen, und der Art und Weise. Für das Betriebssystem können Sie manuell Windows-Updates anwenden oder einen Planungsdienst namens [Automatisiertes Patchen](virtual-machines-windows-sql-automated-patching.md) aktivieren. Automatisiertes Patchen installiert alle wichtigen Updates, einschließlich SQL Server-Updates in dieser Kategorie. Andere optionale Updates für SQL Server müssen manuell installiert werden.
9. **Ist es möglich, Konfigurationen einzurichten, die nicht im Katalog der virtuellen Computer gezeigt werden (z.B. Windows 2008 R2 + SQL Server 2012) ?**
   
    Nein. Für Katalogimages von virtuellen Computern, die SQL Server enthalten, müssen Sie eines der bereitgestellten Images auswählen.
10. **Wie installiere ich SQL Data-Tools auf meiner Azure-VM?**
    
     Laden Sie die SQL Data-Tools von [Microsoft SQL Server Data Tools – Business Intelligence für Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313) herunter, und installieren Sie sie.

## <a name="resources"></a>Ressourcen
Eine Übersicht über SQL Server auf virtuellen Azure-Computern bietet das Video [Azure VM is the best platform for SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)(Azure VM ist die beste Plattform für SQL Server 2016). Eine gute Einführung erhalten Sie auch im Thema [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

Zu weiteren Ressourcen zählen:

* [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrieren einer Datenbank zu SQL Server auf einer Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md)
* [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md)
* [Anwendungsmuster und Entwicklungsstrategien für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)




<!--HONumber=Jan17_HO2-->


