<properties 
	pageTitle="Konfigurieren einer Verbindung eines Azure Search-Indexers mit SQL Server auf einer Azure-VM | Microsoft Azure | Indexer" 
	description="Aktivieren Sie verschlüsselte Verbindungen, und konfigurieren Sie die Firewall für Verbindungen mit SQL Server auf einem virtuellen Azure-Computer (VM) über einen Azure Search-Indexer." 
	services="search" 
	documentationCenter="" 
	authors="jack4it" 
	manager="pablocas" 
	editor=""/>

<tags 
	ms.service="search" 
	ms.devlang="rest-api" 
	ms.workload="search" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.date="09/06/2016" 
	ms.author="jackma"/>

# Konfigurieren einer Verbindung eines Azure Search-Indexers mit SQL Server auf einer Azure-VM

Wie in [Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions) erwähnt, wird das Erstellen von Indexern für **SQL Server auf Azure-VMs** (oder kurz **SQL Azure-VMs**) von Azure Search unterstützt, doch müssen zunächst zwei sicherheitsbezogene Voraussetzungen erfüllt werden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Aktivieren von verschlüsselten Verbindungen

Azure Search erfordert eine sichere Verbindung zum Lesen von Daten aus der Datenbank. Dies bedeutet, dass Sie verschlüsselte Verbindungen auf Ihrer SQL-Azure-VM aktivieren müssen, indem Sie ein SSL-Zertifikat konfigurieren.

Die Schritte zum Aktivieren von verschlüsselten Verbindungen mit SQL Server werden in [Aktivieren von verschlüsselten Verbindungen zum Datenbankmodul (SQL Server-Konfigurations-Manager)](https://msdn.microsoft.com/library/ms191192.aspx) dokumentiert, aber für öffentliche Internetverbindungen, z.B. eine Verbindung von Azure Search mit einer SQL-Azure-VM, müssen einige zusätzliche Anforderungen erfüllt werden.

### Angeben eines FQDN im SSL-Zertifikat

Der Antragstellername des SSL-Zertifikats muss dem vollqualifizierten Domänennamen (oder **FQDN**) der SQL-Azure-VM entsprechen. Es ist der gleiche FQDN, den Sie beim Erstellen einer Datenquelle in Ihrem Suchdienst in der Datenbank-Verbindungszeichenfolge angeben. Ein FQDN wird für **Resource Manager**-VMs als `<your-VM-name>.<region>.cloudapp.azure.com` formatiert. Wenn Sie noch **klassische** VMs einsetzen, wird er als `<your-cloud-service-name.cloudapp.net>` formatiert. Sie finden den FQDN Ihrer SQL-Azure-VM als DNS-Name/-Bezeichnung im [Azure-Portal](https://portal.azure.com/).

### Verwenden von REGEDIT zum Konfigurieren des SSL-Zertifikats

Der SQL Server-Konfigurations-Manager kann das FQDN-SSL-Zertifikat in der Dropdownliste **Zertifikat** nicht anzeigen, wie in der Dokumentation beschrieben. Konfigurieren Sie zur Problemumgehung das SSL-Zertifikat, indem Sie diesen Registrierungsschlüssel bearbeiten: **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server[MSSQL13.MSSQLSERVER]\\MSSQLServer\\SuperSocketNetLib\\Certificate**. Der Teil *[MSSQL13. MSSQLSERVER]* hängt vom Namen Ihrer SQL Server-Version und -Instanz ab. Dieser Schlüssel muss mit dem **Fingerabdruck** des SSL-Zertifikats aktualisiert werden, das Sie auf der SQL-Azure-VM installiert haben.

### Gewähren von Berechtigungen für das Dienstkonto

Stellen Sie sicher, dass dem SQL Server-Dienstkonto die entsprechende Berechtigung für den privaten Schlüssel des SSL-Zertifikats gewährt wird. Wenn Sie diesen Schritt übersehen, startet SQL Server nicht.

## Konfigurieren der Firewall, um Verbindungen von Azure Search zuzulassen

Es ist nicht ungewöhnlich, wenn Sie die Firewall und den entsprechenden Azure-Endpunkt oder die Zugriffssteuerungsliste (Access Control List, ACL) konfigurieren, um Ihre Azure-VM für andere Parteien zugänglich zu machen. Wahrscheinlich haben Sie diese Konfiguration bereits durchgeführt, damit Ihre eigene Anwendungslogik die Verbindung mit Ihrer SQL-Azure-VM herstellen kann. Für eine Azure Search-Verbindung mit Ihrer SQL-Azure-VM ist es nicht anders. Wenn Sie dies noch nicht durchgeführt haben, beachten Sie bitte diese bewährten Sicherheitspraktiken.

Wenn Sie **Resource Manager**-VMs verwenden, finden Sie weitere Informationen unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Resource Manager)](../virtual-machines/virtual-machines-windows-sql-connect.md). Wenn Sie noch **klassische** VMs einsetzen, beachten Sie die Informationen unter [Herstellen einer Verbindung mit einem virtuellen SQL Server-Computer in Azure (klassische Bereitstellung)](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

### Einschränken des Zugriffs auf die Suchdienst-IP-Adresse

Bei jedem Bereitstellungsmodell sollten Sie beim Konfigurieren von Verbindungen von Azure Search unbedingt den Zugriff auf die IP-Adresse Ihres Suchdiensts in der ACL einschränken, anstatt Ihre SQL-Azure-VMs für alle Verbindungsanfragen weit zu öffnen. Sie können die IP-Adresse auf einfache Weise herausfinden, indem Sie den vollqualifizierten Domänennamen (z.B. `<your-search-service-name>.search.windows.net`) Ihres Suchdiensts pingen.

### Konfigurieren eines IP-Adressbereichs

Beachten Sie, dass die IP-Adresse sich während routinemäßiger Dienstneustarts ändern kann, wenn Ihr Suchdienst nur eine Sucheinheit hat (d.h. ein Replikat und eine Partition). Um Verbindungsfehler zu vermeiden, sollten Sie den IP-Adressbereich der Azure-Region angeben, in dem Ihr Suchdienst bereitgestellt wird. Die Liste mit den IP-Adressbereichen, aus denen Azure-Ressourcen öffentliche IP-Adressen zugewiesen werden, finden Sie unter [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653).

### Einbeziehen der IP-Adressen des Azure Search-Portals

Wenn Sie zum Erstellen eines Indexers das Azure-Portal verwenden, benötigt die Logik des Azure Search-Portals außerdem auch während der Erstellungszeit Zugriff auf Ihre SQL-Azure-VM. Die IP-Adressen des Azure Search-Portals ermitteln Sie durch Pingen von `stamp1.search.ext.azure.com` und `stamp2.search.ext.azure.com`.

## Nächste Schritte

Da die obigen Konfigurationsanforderungen nun erfüllt sind, können Sie jetzt eine SQL Server-Instanz auf Ihrer Azure-VM als Datenquelle für einen Azure Search Indexer angeben. Weitere Informationen finden Sie unter [Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

<!---HONumber=AcomDC_0907_2016-->