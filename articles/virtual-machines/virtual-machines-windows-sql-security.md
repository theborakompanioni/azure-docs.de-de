<properties
	pageTitle="Sicherheitsüberlegungen für SQL Server in Azure | Microsoft Azure"
	description="Dieses Thema bezieht sich auf Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, und bietet einen allgemeinen Leitfaden zum Sichern von SQL Server auf einem virtuellen Computer in Azure."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="03/23/2016"
	ms.author="jroth" />

# Sicherheitsüberlegungen für SQL Server in Azure Virtual Machines

Dieses Thema enthält allgemeine Sicherheitsrichtlinien, die dazu beitragen sollen, den Zugriff auf SQL Server-Instanzen auf einem virtuellen Azure-Computer zu schützen. Um einen besseren Schutz für die SQL Server-Datenbankinstanzen in Azure zu gewährleisten, wird jedoch empfohlen, dass Sie zusätzlich zu den bewährten Methoden für Azure auch die herkömmlichen lokalen Sicherheitsmaßnahmen umsetzen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.


Weitere Informationen über die Sicherheitsmethoden für SQL Server finden Sie in [Bewährte Methoden für SQL Server 2008 R2 – Betriebs- und Verwaltungsaufgaben](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx) (in englischer Sprache).

Azure erfüllt mehrere Branchenvorgaben und Standards, mit denen Sie eine kompatible Lösung mit SQL Server auf einem virtuellen Computer erstellen können. Weitere Informationen zur Einhaltung von Vorschriften durch Azure finden Sie unter [Azure Trust Center](https://azure.microsoft.com/support/trust-center/).

Im Folgenden finden Sie eine Liste mit Sicherheitsempfehlungen für das Konfigurieren und das Herstellen von Verbindungen mit SQL Server-Instanz auf einem virtuellen Azure-Computer berücksichtigt werden sollten.

## Überlegungen für das Verwalten von Konten:

- Erstellen Sie ein eindeutiges lokales Administratorkonto, das nicht den Namen **Administrator** hat.

- Verwenden Sie komplexe, sichere Kennwörter für alle Konten. Weitere Informationen zum Erstellen eines sicheren Kennworts finden Sie im Artikel [Tipps zum Erstellen eines sicheren Kennworts](http://windows.microsoft.com/de-DE/windows-vista/Tips-for-creating-a-strong-password).

- Standardmäßig wird für Azure während des Einrichtens von virtuellen Computern mit SQL Server die Windows-Authentifizierung ausgewählt. Aus diesem Grund wird beim Einrichten die **SA**-Anmeldung deaktiviert und ein Kennwort zugewiesen. Es wird empfohlen, die **SA**-Anmeldung nicht zu verwenden oder zu aktivieren. Die folgenden alternativen Strategien können für eine SQL-Anmeldung verwendet werden:
	- Erstellen Sie ein SQL-Konto, das über eine Mitgliedschaft in „sysadmin“ verfügt.
	- Wenn Sie eine **SA**-Anmeldung verwenden müssen, aktivieren Sie die Anmeldung, benennen Sie sie um, und weisen Sie ihr ein neues Kennwort zu.
	- Beide zuvor erwähnten Optionen erfordern eine Änderung des Authentifizierungsmodus in den **SQL Server- und Windows-Authentifizierungsmodus**. Weitere Informationen finden Sie unter [Ändern des Serverauthentifizierungsmodus](https://msdn.microsoft.com/library/ms188670.aspx).

## Überlegungen zum Sichern von Verbindungen mit virtuellen Azure-Computern:

- Ziehen Sie die Verwendung von [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) zur Verwaltung der virtuellen Computer anstelle von öffentlichen RDP-Ports in Betracht.

- Verwenden Sie eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) (NSG), um Netzwerkdatenverkehr an Ihren virtuellen Computer zu erlauben oder zu verweigern. Wenn Sie eine NSG verwenden möchten und bereits eine Endpunkt-ACL eingerichtet ist, entfernen Sie zuerst die Endpunkt-ACL. Informationen zur Vorgehensweise finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Falls Sie Endpunkte verwenden, entfernen Sie alle nicht verwendeten Endpunkte auf dem virtuellen Computer. Weitere Informationen zur Verwendung von ACLs für Endpunkte finden Sie unter [Verwalten der ACL für einen Endpunkt](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint)

- Aktivieren Sie eine verschlüsselte Verbindungsoption für eine Instanz des SQL Server-Datenbankmoduls in Azure Virtual Machines. Konfigurieren Sie die SQL Server-Instanz mit einem signierten Zertifikat. Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zum Datenbankmodul](https://msdn.microsoft.com/library/ms191192.aspx) und [Syntax für Verbindungszeichenfolgen](https://msdn.microsoft.com/library/ms254500.aspx).

- Wenn auf die virtuellen Computer nur von einem bestimmten Netzwerk aus zugegriffen werden soll, verwenden Sie Windows-Firewall, um den Zugriff auf bestimmte IP-Adressen oder Netzwerksubnetze zu beschränken.

## Nächste Schritte

Weitere Informationen zu bewährten Methoden in Bezug auf Leistung finden Sie unter [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md).

Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie in der [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0413_2016-->