<properties
	pageTitle="Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern (Ressourcen-Manager)"
	description="Erfahren Sie, wie Sie die Konfiguration der SQL Server-Verschlüsselung zur Verwendung mit dem Azure-Schlüsseltresor automatisieren. In diesem Thema wird beschrieben, wie Sie die Azure-Schlüsseltresor-Integration mit virtuellen SQL Server-Computern verwenden, die mit dem Ressourcen-Manager erstellt wurden."
	services="virtual-machines-windows"
	documentationCenter=""
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
	ms.date="07/14/2016"
	ms.author="jroth"/>

# Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-ps-sql-keyvault.md)
- [Klassisch](virtual-machines-windows-classic-ps-sql-keyvault.md)

## Übersicht
Es gibt mehrere SQL Server-Verschlüsselungsfunktionen, z. B. [Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [Column Level Encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx) und [Sicherungsverschlüsselung](https://msdn.microsoft.com/library/dn449489.aspx). Bei diesen Arten der Verschlüsselung müssen Sie die kryptografischen Schlüssel verwalten und speichern, die Sie für die Verschlüsselung verwenden. Der Azure-Schlüsseltresor-Dienst (Azure Key Vault, AKV) ist dafür ausgelegt, die Sicherheit und Verwaltung dieser Schlüssel an einem sicheren und hoch verfügbaren Speicherort zu verbessern. Mit dem [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) kann SQL Server diese Schlüssel aus dem Azure-Schlüsseltresor verwenden.

Wenn Sie SQL Server mit lokalen Computern ausführen, können Sie die [Schritte zum Zugreifen auf den Azure-Schlüsseltresor von Ihrem lokalen SQL Server-Computer ausführen](https://msdn.microsoft.com/library/dn198405.aspx). Sie können für SQL Server auf virtuellen Azure-Computern aber Zeit sparen, indem Sie die Funktion für die *Azure-Schlüsseltresor-Integration* verwenden.

Wenn diese Funktion aktiviert ist, wird der SQL Server-Connector automatisch aktiviert und der Anbieter für erweiterbare Schlüsselverwaltung für den Zugriff auf den Azure-Schlüsseltresor konfiguriert. Außerdem werden die Anmeldeinformationen erstellt, um den Zugriff auf Ihren Tresor zu ermöglichen. Wenn Sie sich die Schritte in der oben erwähnten Dokumentation für die Ausführung auf lokalen Computern angesehen haben, haben Sie erfahren, dass die Schritte 2 und 3 mit dieser Funktion automatisiert werden. Der einzige Schritt, den Sie noch manuell ausführen müssen, ist die Erstellung des Schlüsseltresors und der Schlüssel. Ab diesem Punkt ist das gesamte Setup des virtuellen SQL-Computers automatisiert. Nachdem die Funktion dieses Setup abgeschlossen hat, können Sie die T-SQL-Anweisungen ausführen, um mit der Verschlüsselung Ihrer Datenbanken oder Backups zu beginnen, wie Sie dies normalerweise auch tun.

[AZURE.INCLUDE [Vorbereiten der Azure-Schlüsseltresor-Integration](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## Aktivieren und Konfigurieren der AKV-Integration
Sie können die AKV-Integration während der Bereitstellung aktivieren oder für vorhandene virtuelle Computer konfigurieren.

### Neue virtuelle Computer
Wenn Sie mithilfe von Resource Manager einen neuen virtuellen SQL Server-Computer bereitstellen, bietet das Azure-Portal einen Schritt zum Aktivieren der Azure Key Vault-Integration.

![SQL – Azure-Schlüsseltresor-Integration](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Eine ausführliche exemplarische Vorgehensweise zur Bereitstellung finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

### Vorhandene virtuelle Computer
Wählen Sie für vorhandene virtuelle Computer mit SQL Server Ihren virtuellen Computer mit SQL Server aus. Wählen Sie dann auf dem Blatt **Einstellungen** den Abschnitt **SQL Server-Konfiguration** aus.

![SQL-AKV-Integration für vorhandene virtuelle Computer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Klicken Sie auf dem Blatt **SQL Server-Konfiguration** im Abschnitt für die automatisierte Schlüsseltresorintegration auf die Schaltfläche **Bearbeiten**.

![Konfigurieren der SQL-AKV-Integration für vorhandene virtuelle Computer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Klicken Sie abschließend unten auf dem Blatt **SQL Server-Konfiguration** auf die Schaltfläche **OK**, um die Änderungen zu speichern.

>[AZURE.NOTE] Sie können die AKV-Integration auch mithilfe einer Vorlage konfigurieren. Weitere Informationen finden Sie unter [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update) (Azure-Schnellstartvorlage für die Azure Key Vault-Integration).

[AZURE.INCLUDE [Nächste Schritte der Azure-Schlüsseltresor-Integration](../../includes/virtual-machines-sql-server-akv-next-steps.md)]

<!---HONumber=AcomDC_0720_2016-->