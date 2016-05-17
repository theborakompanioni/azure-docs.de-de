<properties
	pageTitle="SQL Server auf virtuellen Azure-Computern – FAQ | Microsoft Azure"
	description="Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Ausführung von SQL Server auf Azure-VMs."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="v-shysun"
	manager="msmets"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="04/27/2016"
	ms.author="v-shysun"/>

# SQL Server auf virtuellen Azure-Computern – FAQ

Dieses Thema bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/).

## Häufig gestellte Fragen

1. **Wie erstelle ich einen virtuellen Azure-Computer mit SQL Server?**

	Es gibt hierbei zwei Möglichkeiten. Die einfachste Lösung ist die Erstellung eines virtuellen Computers, der SQL Server enthält. Ein Tutorial für die Anmeldung bei Azure und das Erstellen einer SQL-VM aus dem Portal heraus finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Sie haben auch die Möglichkeit der manuellen Installation von SQL Server auf einem virtuellen Computer und Wiederverwenden einer lokalen Lizenz mit [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Was ist der Unterschied zwischen SQL-VMs und dem SQL-Datenbank-Dienst?**

	Im Prinzip unterscheidet sich die Ausführung von SQL Server auf virtuellen Azure-Computern nicht von der Ausführung in einem Remoterechenzentrum. Im Gegensatz dazu bietet [SQL-Datenbank](../sql-database/sql-database-technical-overview.md) Database-as-a-Service. Mit SQL-Datenbank haben Sie keinen Zugriff auf die Computer, die Ihre Datenbanken hosten. Einen vollständigen Vergleich finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../sql-database/media/data-management-azure-sql-database-and-sql-server-iaas.md).

1. **Wie kann ich meine lokale SQL Server-Datenbank zur Cloud migrieren?**

	Erstellen Sie zuerst einen virtuellen Azure-Computer mit einer SQL Server-Instanz. Migrieren Sie dann Ihre lokalen Datenbanken zu dieser Instanz. Datenmigrationsstrategien finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

2. **Kann ich die installierten Funktionen ändern oder eine zweite Instanz von SQL Server auf dem gleichen virtuellen Computer installieren?**

	Ja. Die SQL Server-Installationsmedien befinden sich in einem Ordner auf dem **C**-Laufwerk. Führen Sie **Setup.exe** von diesem Speicherort aus, um neue SQL Server-Instanzen hinzuzufügen oder andere installierte Features von SQL Server auf dem Computer zu ändern.

3. **Wie aktualisiere ich auf eine neue Version/Edition von SQL Server auf einer Azure-VM?**

	Derzeit ist kein direktes Upgrade für SQL Server auf einer Azure-VM verfügbar. Erstellen Sie einen neuen virtuellen Azure-Computer mit der gewünschten Version/Edition von SQL Server, und migrieren Sie dann Ihre Datenbanken mit dem standardmäßigen [Datenmigrationsverfahren](virtual-machines-windows-migrate-sql.md) zu dem neuen Server.

4. **Wie kann ich meine lizenzierte Kopie von SQL Server auf einer Azure-VM installieren?**

	Kopieren Sie die SQL Server-Installationsmedien auf die Windows Server-VM, und installieren Sie SQL Server auf dem virtuellen Computer. Aus Gründen der Lizenzierung benötigen Sie die [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/).

5. **Muss ich die SQL-Kosten für einen virtuellen Computer bezahlen, wenn ich ihn nur für Standby/Failover verwende?**

	Wenn Sie die SQL-VM über den Katalog erstellen, benötigen Sie eine separate Lizenz für die Standby-SQL-VM, und die Preise sind identisch. Wenn Sie SQL Server manuell auf einem virtuellen Computer mit [Lizenzmobilität](https://azure.microsoft.com/pricing/license-mobility/) installieren, haben Sie die Möglichkeit, eine kostenlose passive SQL-Instanz für das Failover zu bekommen. Überprüfen Sie die Einschränkungen und Anforderungen.

6. **Wie werden Updates und Servicepacks auf eine SQL Server-VM angewendet?**

	Virtuelle Computer bieten Ihnen die Kontrolle über die Hostcomputer, einschließlich des Zeitpunkts, zu dem Updates angewendet werden sollen, und der Art und Weise. Für das Betriebssystem können Sie manuell Windows-Updates anwenden oder einen Planungsdienst namens [Automatisiertes Patchen](virtual-machines-windows-classic-sql-automated-patching.md) aktivieren. Automatisiertes Patchen installiert alle wichtigen Updates, einschließlich SQL Server-Updates in dieser Kategorie. Andere optionale Updates für SQL Server müssen manuell installiert werden.

7. **Ist es möglich, Konfigurationen einzurichten, die nicht im Katalog der virtuellen Computer gezeigt werden (z.B. Windows 2008 R2 + SQL Server 2012) ?**

	Nein. Für Katalogimages von virtuellen Computern, die SQL Server enthalten, müssen Sie eines der bereitgestellten Images auswählen.

9. **Wie installiere ich SQL Data-Tools auf meiner Azure-VM?**

	Laden Sie die SQL Data-Tools von [Microsoft SQL Server Data Tools – Business Intelligence für Visual Studio 2013](https://www.microsoft.com/de-DE/download/details.aspx?id=42313) herunter, und installieren Sie sie.

## Ressourcen

Eine Übersicht über SQL Server auf virtuellen Azure-Computern bietet das Video [Azure VM is the best platform for SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016) (Azure-VM ist die beste Plattform für SQL Server 2016). Sie erhalten auch eine gute Einführung im Thema [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

Zu weiteren Ressourcen zählen:

- [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Migrieren einer Datenbank zu SQL Server auf einer Azure-VM](virtual-machines-windows-migrate-sql.md)
- [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md)
- [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md)
- [Anwendungsmuster und Entwicklungsstrategien für SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

<!---HONumber=AcomDC_0504_2016-->