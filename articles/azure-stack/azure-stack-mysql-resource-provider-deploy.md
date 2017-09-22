---
title: Verwenden von MySQL-Datenbanken als PaaS in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Ressourcenanbieter bereitstellen und MySQL-Datenbanken als Dienst auf Azure Stack bereitstellen.
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: JeffGo
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 4e9da524ef9dfa2d5b7150bc6a888536a1435dfd
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Verwenden von MySQL-Datenbanken in Microsoft Azure Stack


Sie können einen MySQL-Ressourcenanbieter in Azure Stack bereitstellen. Nach der Bereitstellung des Ressourcenanbieters können Sie MySQL-Server und Datenbanken über Azure Resource Manager-Bereitstellungsvorlagen erstellen und MySQL-Datenbanken als Dienst bereitstellen. MySQL-Datenbanken, die häufig auf Websites verwendet werden, unterstützen zahlreiche Website-Plattformen. Beispielsweise können Sie nach der Bereitstellung des Ressourcenanbieters WordPress-Websites über die Azure-Web-Apps-Plattform als Dienst (PaaS)-Add-On für Azure Stack erstellen.

Zum Bereitstellen des MySQL-Anbieters in einem System ohne Internetzugriff können Sie die Datei [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) in eine lokale Freigabe kopieren und den Namen dieser Freigabe angeben, wenn Sie dazu aufgefordert werden (siehe unten). Sie müssen auch die PowerShell-Module für Azure und Azure Stack installieren.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architektur des MySQL Server-Ressourcenanbieteradapters

Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des MySQL-Ressourcenanbieteradapters**, ein virtueller Windows-Computer, der die Anbieterdienste ausführt.
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht.
- **Servern, die MySQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden. 

In diesem Release wird keine MySQL-Instanz mehr erstellt. Sie müssen diese erstellen und/oder Zugriff auf externe SQL-Instanzen bereitstellen. Im [Azure Stack-Schnellstartkatalog](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) finden Sie eine Beispielvorlage, mit der ein MySQL-Server für Sie erstellt wird. Sie können auch einen MySQL-Server aus dem Marketplace herunterladen und bereitstellen.

## <a name="deploy-the-resource-provider"></a>Bereitstellung des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter - Eval“ über die Marketplace-Verwaltung herunter. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden.

2. [Laden Sie die Binärdatei des MySQL-Ressourcenanbieters herunter](https://aka.ms/azurestackmysqlrp), und extrahieren Sie sie auf dem Development Kit-Host.

3. Melden Sie sich am Development Kit-Host an, und extrahieren Sie die MySQL RP-Installationsdatei in ein temporäres Verzeichnis.

4. Das Azure Stack-Stammzertifikat wird abgerufen, und im Rahmen dieses Prozesses wird ein selbstsigniertes Zertifikat erstellt. 

    __Optional:__ Wenn Sie eigene Zertifikate bereitstellen müssen, bereiten Sie diese vor und kopieren sie in ein lokales Verzeichnis, wenn Sie die Zertifikate (die an das Installationsskript übergeben werden) anpassen möchten. Sie benötigen Folgendes:

    a. Ein Platzhalterzertifikat für *.dbadapter.\<Region\>.\<external fqdn\>. Diesem Zertifikat muss so vertraut werden, als würde es z.B. von einer Zertifizierungsstelle ausgestellt werden (d.h. die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind). (Ein einzelnes Sitezertifikat kann mit dem expliziten Namen des virtuellen Computers verwendet werden, den Sie während der Installation bereitstellen.)

    b. Das von Azure Resource Manager für Ihre Azure Stack-Instanz verwendete Stammzertifikat. Wenn es nicht gefunden wird, wird das Stammzertifikat abgerufen.

5. Öffnen Sie eine **neue** PowerShell-Konsole mit erhöhten Rechten, und wechseln Sie zum Verzeichnis, in dem Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen, die bereits in Ihrem System geladen sind, zu vermeiden.

6. Wenn Sie andere Versionen der AzureRm- oder AzureStack-PowerShell-Module als 1.2.9 oder 1.2.10 installiert haben, werden Sie aufgefordert, diese zu entfernen, ansonsten wird die Installation nicht fortgesetzt. Dies schließt Versionen ab 1.3 ein.

7. Führen Sie „DeployMySqlProvider.ps1“ aus.

Mit diesem Skript werden folgende Schritte ausgeführt:

* Laden Sie bei Bedarf eine kompatible Version von Azure PowerShell herunter.
* Laden Sie die Binärdatei des MySQL-Connectors herunter (diese kann offline bereitgestellt werden).
* Laden Sie das Zertifikat und alle anderen Artefakte in ein Azure Stack-Speicherkonto hoch.
* Veröffentlichen Sie Katalogpakete, damit Sie MySQL-Datenbanken über den Katalog bereitstellen können.
* Stellen Sie einen virtuellen Computer (VM) bereit, der Ihren Ressourcenanbieter hostet.
* Registrieren Sie einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
* Registrieren Sie Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz.

Geben Sie mindestens die erforderlichen Parameter in der Befehlszeile an. Wenn Sie die Ausführung ohne Parameter vornehmen, werden Sie zur Eingabe aufgefordert. 

Hier ist ein Beispiel, das Sie über die PowerShell-Eingabeaufforderung ausführen können (ändern Sie jedoch die Kontoinformationen und Portalendpunkte entsprechend):


```
# Install the AzureRM.Bootstrapper module
Install-Module -Name AzureRm.BootStrapper -Force

# Installs and imports the API Version Profile required by Azure Stack into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.10 -Force

# Download the Azure Stack Tools from GitHub and set the environment
cd c:\
Invoke-Webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip
Expand-Archive master.zip -DestinationPath . -Force

# This endpoint may be different for your installation
Import-Module C:\AzureStack-Tools-master\Connect\AzureStack.Connect.psm1
Add-AzureRmEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external" 

# For AAD, use the following
$tenantID = Get-AzsDirectoryTenantID -AADTenantName "<your directory name>" -EnvironmentName AzureStackAdmin

# For ADFS, replace the previous line with
# $tenantID = Get-AzsDirectoryTenantID -ADFS -EnvironmentName AzureStackAdmin

$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ("admin@mydomain.onmicrosoft.com", $AdminPass)

# change this as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files 
# and adjust the endpoints
<extracted file directory>\DeployMySQLProvider.ps1 -DirectoryTenantID $tenantID -AzCredential $AdminCreds -VMLocalCredential $vmLocalAdminCreds -ResourceGroupName "MySqlRG" -VmName "MySQLRP" -ArmEndpoint "https://adminmanagement.local.azurestack.external" -TenantArmEndpoint "https://management.local.azurestack.external" -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath
 ```

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1-Parameter

Sie können diese Parameter in der Befehlszeile angeben. Wenn sie nicht festgelegt werden oder eine Parameterüberprüfung fehlschlägt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | Beschreibung | Kommentar oder Standardwert |
| --- | --- | --- |
| **DirectoryTenantID** | Die Azure- oder ADFS-Verzeichnis-ID (GUID) | _erforderlich_ |
| **ArmEndpoint** | Der Azure Resource Manager-Endpunkt des Azure Stack-Administrators | _erforderlich_ |
| **TenantArmEndpoint** | Der Azure Resource Manager-Endpunkt des Azure Stack-Mandanten | _erforderlich_ |
| **AzCredential** | Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto (verwenden Sie dasselbe Konto wie für die Bereitstellung von Azure Stack) | _erforderlich_ |
| **VMLocalCredential** | Das lokale Administratorkonto des virtuellen Computers mit dem MySQL-Ressourcenanbieter | _erforderlich_ |
| **ResourceGroupName** | Ressourcengruppe für die von diesem Skript erstellten Elemente |  _erforderlich_ |
| **VmName** | Name des virtuellen Computers mit dem Ressourcenanbieter |  _erforderlich_ |
| **AcceptLicense** | Überspringt die Aufforderung zum Akzeptieren der GPL-Lizenz (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |
| **DependencyFilesLocalPath** | Pfad zu einer lokalen Freigabe, die [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi) enthält. Wenn Sie Zertifikatdateien bereitstellen, müssen diese ebenfalls in diesem Verzeichnis abgelegt werden. | _optional_ |
| **DefaultSSLCertificatePassword** | Kennwort für das PFX-Zertifikat | _erforderlich_ |
| **MaxRetryCount** | Jeder Vorgang wird wiederholt, wenn ein Fehler auftritt. | 2 |
| **RetryDuration** | Timeout zwischen Wiederholungen in Sekunden | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter | Nein |
| **DebugMode** | Verhindert die automatische Bereinigung bei einem Fehler | Nein |


Je nach Systemleistung und Downloadgeschwindigkeiten kann die Installation zwischen 20 Minuten und mehreren Stunden dauern. Wenn das MySQLAdapter-Blatt nicht verfügbar ist, aktualisieren Sie das Administratorportal.

> [!NOTE]
> Wenn die Installation länger als 90 Minuten dauert, schlägt sie u.U. fehl, und auf dem Bildschirm sowie in der Protokolldatei wird eine Fehlermeldung angezeigt. Der Bereitstellungsvorgang wird ab dem fehlgeschlagenen Schritt wiederholt. Systeme, die den empfohlenen Arbeitsspeicher- und Core-Spezifikationen nicht entsprechen, können den MySQL-Ressourcenanbieter möglicherweise nicht bereitstellen.


## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Bereitstellen von Kapazität durch Verbindung mit einem MySQL-Hostserver

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Klicken Sie auf **Ressourcenanbieter** &gt; **MySQLAdapter** &gt; **Hostserver** &gt; **+Hinzufügen**.

    Auf dem Blatt **MySQL-Hostserver** können Sie den MySQL-Server-Ressourcenanbieter mit tatsächlichen MySQL-Serverinstanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

    ![Hostserver](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Füllen Sie das Formular mit den Verbindungsdetails für Ihre MySQL Server-Instanz aus. Geben Sie den vollqualifizierten Domänennamen (FQDN) oder eine gültige IPv4-Adresse an, nicht den Kurznamen des virtuellen Computers. Diese Installation stellt keine standardmäßige MySQL-Instanz mehr bereit. Die angegebene Größe hilft dem Ressourcenanbieter bei der Verwaltung der Datenbankkapazität. Diese sollte in etwa der physischen Kapazität des Datenbankservers entsprechen.

    > [!NOTE]
    > Sofern die MySQL-Instanz für den Azure Resource Manager-Mandanten und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die MySQL-Instanz __muss__ ausschließlich dem Ressourcenanbieter zugeordnet sein.

4. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um die Differenzierung von Dienstangeboten zu ermöglichen. Beispielsweise können Sie über eine Enterprise-Instanz verfügen, die Datenbankkapazität und automatische Sicherung bereitstellt, Hochleistungsserver für einzelne Abteilungen reservieren usw. Der SKU-Name muss die Eigenschaften beschreiben, damit Mandanten ihre Datenbanken entsprechend platzieren können, und alle Hostserver in einer SKU müssen denselben Funktionsumfang aufweisen.

    ![Erstellen einer MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU erstellt wurde.


## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Erstellen Ihrer ersten MySQL-Datenbank zum Testen der Bereitstellung


1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Klicken Sie auf die Schaltfläche **+Neu** &gt; **Daten und Speicher** &gt; **MySQL-Datenbank (Vorschauversion)**.

3. Füllen Sie das Formular mit den Datenbankdetails aus.

    ![Erstellen einer MySQL-Testdatenbank](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Wählen Sie eine SKU aus.

    ![SKU auswählen](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Erstellen Sie eine Einstellung für die Anmeldung. Als Einstellung für die Anmeldung kann eine vorhandene wiederverwendet oder eine neue erstellt werden. Sie enthält den Benutzernamen und das Kennwort für die Datenbank.

    ![Neue Datenbankanmeldung erstellen](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Die Verbindungszeichenfolge enthält den echten Namen des Datenbankservers. Kopieren Sie ihn aus dem Portal.

    ![Abrufen der Verbindungszeichenfolge für die MySQL-Datenbank](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Benutzernamen dürfen bei MySQL 5.7 höchstens 32 Zeichen und bei früheren Editionen höchstens 16 Zeichen aufweisen. Dies ist eine Einschränkung der MySQL-Implementierungen.


## <a name="add-capacity"></a>Kapazität hinzufügen

Fügen Sie Kapazität hinzu, indem Sie im Azure Stack-Portal weitere MySQL-Server hinzufügen. Wenn Sie eine andere Instanz von MySQL verwenden möchten, klicken Sie auf **Ressourcenanbieter** &gt; **MySQLAdapter** &gt; **MySQL-Hostserver** &gt; **+Hinzufügen**.


## <a name="making-mysql-databases-available-to-tenants"></a>Verfügbarmachen von MySQL-Datenbanken für Mandanten
Erstellen Sie Pläne und Angebote, um MySQL-Datenbanken für Mandanten zur Verfügung stellen. Fügen Sie den Dienst „Microsoft.MySqlAdapter“ hinzu, fügen Sie ein Kontingent hinzu usw.

![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="removing-the-mysql-adapter-resource-provider"></a>Entfernen des MySQL-Adapter-Ressourcenanbieters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des Ressourcenanbieters heruntergeladen haben.

2. Alle Mandantendatenbanken müssen aus dem Ressourcenanbieter gelöscht werden (die Daten werden dabei nicht gelöscht). Diese Aktion muss von den Mandanten selbst ausgeführt werden.

3. Mandanten müssen die Namespace-Registrierung aufheben.

4. Der Administrator muss die Hostserver aus dem MySQL-Adapter löschen.

5. Der Administrator muss alle Pläne löschen, die auf den MySQL-Adapter verweisen.

6. Der Administrator muss alle Kontingente löschen, die dem MySQL-Adapter zugewiesen sind.

7. Führen Sie das Bereitstellungsskript mit dem Parameter „-Uninstall“, Azure Resource Manager-Endpunkten, DirectoryTenantID und Anmeldeinformationen für das Dienstadministratorkonto erneut aus.




## <a name="next-steps"></a>Nächste Schritte


Probieren Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md) und den [App Services-Ressourcenanbieter](azure-stack-app-service-overview.md) aus.

