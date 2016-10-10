<properties
   pageTitle="Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer | Microsoft Azure"
   description="Der Artikel enthält eine Übersicht über die Microsoft Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="krkhan"/>


#Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer

Bei Microsoft Azure wird sehr darauf geachtet, den Schutz Ihrer Daten und die Datenhoheit sicherzustellen. Außerdem können Sie für Ihre unter Azure gehosteten Daten eine Reihe von modernen Techniken zum Verschlüsseln, Steuern und Verwalten von Verschlüsselungsschlüsseln und Steuern und Überprüfen des Datenzugriffs nutzen. So können Azure-Kunden flexibel eine Lösung auswählen, die Ihre Anforderungen am besten erfüllt. In diesem Artikel stellen wir Ihnen die neue Technologie „Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer“ vor, die zum Schützen und Absichern Ihrer Daten dient, um Vorgaben in den Bereichen Unternehmenssicherheit und Compliance zu erfüllen. Der Artikel enthält eine ausführliche Anleitung zur Verwendung der Funktionen einer Azure-Datenträgerverschlüsselung, z. B. die unterstützten Szenarien und die Benutzeroberflächen.

**HINWEIS**: Einige Empfehlungen in diesem Artikel können unter Umständen zu einer erhöhten Daten-, Netzwerk- oder Serverressourcenauslastung führen, was mit zusätzlichen Lizenz- oder Abonnementkosten verbunden sein kann.

## Übersicht

Azure-Datenträgerverschlüsselung ist eine neue Funktion, mit der Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln können. Bei der Azure-Datenträgerverschlüsselung werden das Branchen-Standardfeature [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) von Windows und das Feature [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux genutzt, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in den [Azure-Schlüsseltresor](https://azure.microsoft.com/documentation/services/key-vault/) integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die geheimen Schlüssel unter Ihrem Schlüsseltresor-Abonnement steuern und verwalten können. Gleichzeitig können Sie sicherstellen, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand in Ihrem Azure-Speicher verschlüsselt sind.

Die Azure-Datenträgerverschlüsselung (Azure Disk Encryption) für Windows- und Linux-IaaS-VMs befindet sich in allen öffentlichen Azure-Regionen für Standard-VMs und VMs mit Storage Premium in der Phase **Allgemeine Verfügbarkeit**.

### Verschlüsselungsszenarien

Die Azure Disk Encryption-Lösung unterstützt die folgenden Kundenszenarien:

- Aktivieren der Verschlüsselung auf neuen virtuellen IaaS-Computern, die mit einer vorverschlüsselten VHD und Verschlüsselungsschlüsseln erstellt werden
- Aktivieren der Verschlüsselung auf neuen virtuellen IaaS-VM-Computern, die über Images aus dem Azure-Katalog erstellt werden
- Aktivieren der Verschlüsselung auf vorhandenen IaaS-VMs, die unter Azure ausgeführt werden
- Deaktivieren der Verschlüsselung auf Windows-IaaS-VMs
- Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs

Wenn diese Lösung in Microsoft Azure aktiviert ist, wird für virtuelle IaaS-Computer Folgendes unterstützt:

- Integration in Azure-Schlüsseltresor
- Standard-Tarif-VMs – [IaaS-VMs der Reihe A, D, DS, G, GS usw.](https://azure.microsoft.com/pricing/details/virtual-machines/)
- Aktivieren der Verschlüsselung auf virtuellen Windows- und Linux-IaaS-Computern
- Deaktivieren der Verschlüsselung auf Betriebssystem- und Datenlaufwerken für Windows-IaaS-VMs
- Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs
- Aktivieren der Verschlüsselung auf IaaS-VMs mit dem Windows-Clientbetriebssystem
- Aktivieren der Verschlüsselung auf Volumes mit Einbindungspfaden
- Aktivieren der Verschlüsselung auf Linux-VMs mit softwarebasiertem RAID-System
- Aktivieren der Verschlüsselung auf Windows-VMs mit Speicherplätzen
- Alle öffentlichen Azure-Regionen werden unterstützt.

Die Lösung unterstützt nicht die folgenden Szenarien, Features und Technologien:

- IaaS-VMs des Basic-Tarifs
- Deaktivieren der Verschlüsselung auf Betriebssystemlaufwerken für Linux-IaaS-VMs
- Virtuelle IaaS-Computer, die mit der herkömmlichen Methode für virtuelle Computer erstellt werden
- Integration in den lokalen Schlüsselverwaltungsdienst
- Windows Server 2016 Technical Preview wird in dieser Version nicht unterstützt.
- Azure Files (Azure-Dateifreigabe), Netzwerkdateisystem (NFS), Dynamische Volumes, Windows-VMs mit softwarebasierten RAID-Systemen


### Verschlüsselungsfunktionen

Beim Aktivieren und Bereitstellen der Azure-Datenträgerverschlüsselung für virtuelle Azure-IaaS-Computer werden je nach bereitgestellter Konfiguration die folgenden Funktionen aktiviert:

- Verschlüsselung des Betriebssystemvolumes zum Schützen des Startvolumes im Ruhezustand im Kundenspeicher
- Verschlüsselung von Datenvolumes zum Schützen der Datenvolumes im Ruhezustand im Kundenspeicher
- Deaktivieren der Verschlüsselung auf Betriebssystem- und Datenlaufwerken für Windows-IaaS-VMs
- Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs
- Schutz der Verschlüsselungsschlüssel und geheimen Schlüssel unter dem Azure-Schlüsseltresor-Abonnement des Kunden
- Meldung des Verschlüsselungsstatus des verschlüsselten virtuellen IaaS-Computers
- Entfernen der Konfigurationseinstellungen für die Datenträgerverschlüsselung vom virtuellen IaaS-Computer

Die Azure-Datenträgerverschlüsselung für virtuelle IaaS-Computer für Windows und Linux umfasst die Datenträgerverschlüsselungserweiterung für Windows, Datenträgerverschlüsselungserweiterung für Linux, Datenträgerverschlüsselungs-PowerShell-Cmdlets, Datenträgerverschlüsselungs-CLI-Cmdlets und Azure-Ressourcen-Manager-Vorlagen für die Datenträgerverschlüsselung. Die Lösung für die Azure-Datenträgerverschlüsselung wird auf virtuellen IaaS-Computern unter dem Windows- oder Linux-Betriebssystem unterstützt. Weitere Informationen zu den unterstützten Betriebssystemen finden Sie unten im Abschnitt „Voraussetzungen“.

**HINWEIS:** Für die Verschlüsselung von VM-Datenträgern per Azure Disk Encryption fallen keine zusätzlichen Gebühren an.

### Wertbeitrag

Mit der Verwaltungslösung für die Azure-Datenträgerverschlüsselung werden in der Cloud die folgenden geschäftlichen Anforderungen erfüllt:

-   Virtuelle IaaS-Computer werden im Ruhezustand geschützt, indem Verschlüsselungstechnologie nach Branchenstandard eingesetzt wird, um die Anforderungen an Unternehmenssicherheit und Compliance zu erfüllen.
-   Virtuelle IaaS-Computer werden mit vom Kunden gesteuerten Schlüsseln und Richtlinien gestartet, und die Nutzung kann im Schlüsseltresor überprüft werden.


### Verschlüsselungsworkflow

Im Allgemeinen sind folgende Schritte erforderlich, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer zu aktivieren:

1. Kunde wählt eines der obigen Verschlüsselungsszenarios.
2. Kunde nutzt die Aktivierung der Datenträgerverschlüsselung über die Azure-Datenträgerverschlüsselung per Resource Manager-Vorlage, PS-Cmdlets oder CLI-Befehl und gibt die Verschlüsselungskonfiguration an.

    - Für das vom Kunden verschlüsselte VHD-Szenario lädt der Kunde die verschlüsselte VHD in sein Speicherkonto und das Verschlüsselungsschlüsselmaterial in seinen Schlüsseltresor und stellt die Verschlüsselungskonfiguration bereit, um die Verschlüsselung für einen neuen virtuellen IaaS-Computer zu aktivieren.
    - Für die neuen virtuellen Computer, die über den Azure-Katalog erstellt werden, und vorhandene virtuelle Computer, die in Azure bereits ausgeführt werden, gibt der Kunde die Verschlüsselungskonfiguration an, um die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

3. Der Kunde gewährt Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus dem Schlüsseltresor auszulesen und so die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.
4. Der Kunde stellt eine Azure AD-Anwendungsidentität bereit, um das Verschlüsselungsschlüsselmaterial in den Schlüsseltresor zu schreiben und so die Verschlüsselung auf dem virtuellen IaaS-Computer für das obige zweite Szenario zu aktivieren.
5.  Über Azure wird das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor aktualisiert und der verschlüsselte virtuelle Computer für den Kunden bereitgestellt.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### Entschlüsselungsworkflow

Im Allgemeinen sind folgende Schritte erforderlich, um die Datenträgerverschlüsselung für virtuelle IaaS-Computer zu deaktivieren:

1. Um die Verschlüsselung auf einem laufenden virtuellen IaaS-Computer in Azure zu deaktivieren (Entschlüsselung), geben Kunden die Entschlüsselungskonfiguration über die Azure Disk Encryption-Resource Manager-Vorlage oder über PS-Cmdlets an.
2. Durch den Schritt zum Deaktivieren der Verschlüsselung wird die Verschlüsselung des Betriebssystemdatenträgers und/oder des Datenträgers für Daten in der laufenden Windows-IaaS-VM deaktiviert. Die Deaktivierung der Verschlüsselung des Betriebssystemdatenträgers für Linux wird nicht wie in der obigen Dokumentation beschrieben unterstützt. Der Deaktivierungsschritt ist nur für Datenlaufwerke auf Linux-VMs zulässig.
4. Azure aktualisiert das VM-Dienstmodell, und die IaaS-VM wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt.
5. Durch den Vorgang zum Deaktivieren der Verschlüsselung werden der Kundenschlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows oder Passphrase für Linux) nicht gelöscht.

## Voraussetzungen

Unten sind die Voraussetzungen für die Aktivierung der Azure-Datenträgerverschlüsselung auf virtuellen Azure-IaaS-Computern für die unterstützten Szenarien aufgeführt, die im Übersichtsabschnitt genannt werden.

- Benutzer müssen über ein gültiges aktives Azure-Abonnement verfügen, um in den unterstützten Regionen Ressourcen in Azure zu erstellen.
- Azure Disk Encryption wird für die folgenden Windows Server-SKUs unterstützt: Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2. Windows Server 2016 Technical Preview wird in dieser Version nicht unterstützt.
- Azure Disk Encryption wird für die folgenden Windows-Client-SKUs unterstützt: Windows 8-Client und Windows 10-Client.

**Hinweis**: Für Windows Server 2008 R2, MUSS .NET Framework 4.5 installiert werden, bevor die Verschlüsselung in Azure aktiviert wird. Sie können die Installation über Windows Update durchführen, indem Sie das optionale Update „Microsoft .NET Framework 4.5.2 für Windows Server 2008 R2 x64-basierte Systeme ([KB2901983](https://support.microsoft.com/kb/2901983))“ installieren.

- Azure Disk Encryption wird für die folgenden Linux-Server-SKUs unterstützt: Ubuntu, CentOS, SUSE und SUSE Linux Enterprise Server (SLES) sowie Red Hat Enterprise Linux.

**Hinweis:** Die Verschlüsselung von Linux-Betriebssystemdatenträgern wird derzeit für die folgenden Linux-Distributionen unterstützt: RHEL 7.2, CentOS 7.2, Ubuntu 16.04.

- Alle Ressourcen (z.B. Schlüsseltresor, Speicherkonto, VM usw.) müssen derselben Azure-Region und demselben Abonnement angehören.

**Hinweis:** Für die Azure-Datenträgerverschlüsselung ist es erforderlich, dass sich der Schlüsseltresor und die virtuellen Computer in derselben Azure-Region befinden. Sind sie in unterschiedlichen Regionen konfiguriert, kann die Azure-Datenträgerverschlüsselung nicht aktiviert werden.

- Informationen zum Einrichten und Konfigurieren von Azure Key Vault für die Verwendung von Azure Disk Encryption finden Sie in diesem Artikel unter *Voraussetzungen* im Abschnitt **Festlegen und Konfigurieren von Azure Key Vault für Azure Disk Encryption**.
- Informationen zum Einrichten und Konfigurieren der Azure AD-Anwendung in Azure Active Directory für die Verwendung von Azure Disk Encryption finden Sie in diesem Artikel unter *Voraussetzungen* im Abschnitt **Einrichten der Azure AD-Anwendung in Azure Active Directory**.
- Informationen zum Einrichten und Konfigurieren der Richtlinie für den Zugriff auf den Schlüsseltresor für die Azure AD-Anwendung finden Sie in diesem Artikel unter *Voraussetzungen* im Abschnitt **Festlegen der Richtlinie für den Zugriff auf den Schlüsseltresor für die Azure AD-Anwendung**.
- Informationen, wie eine vorverschlüsselte Windows-VHD vorbereitet wird, finden Sie im Abschnitt **Vorbereiten einer vorverschlüsselten Windows-VHD** im Anhang dieses Artikels.
- Informationen zur Vorbereitung einer vorverschlüsselten Linux-VHD finden Sie im Abschnitt **Vorbereiten einer vorverschlüsselten Linux-VHD** im Anhang dieses Artikels.
- Die Azure-Plattform benötigt Zugriff auf die Verschlüsselungsschlüssel oder geheimen Schlüssel im Azure-Schlüsseltresor des Kunden, um sie für den virtuellen Computer zur Verfügung zu stellen. Damit kann das Betriebssystemvolume des virtuellen Computers gestartet und entschlüsselt werden. Um der Azure-Plattform Berechtigungen für den Zugriff auf den Schlüsseltresor des Kunden zu gewähren, muss die **enabledForDiskEncryption**-Eigenschaft für diese Anforderung auf den Schlüsseltresor festgelegt werden. Weitere Informationen finden Sie im Abschnitt **Festlegen und Konfigurieren des Azure-Schlüsseltresors für die Azure-Datenträgerverschlüsselung** im Anhang dieses Artikels.
- Die URLs für den geheimen Schlüssel des Schlüsseltresors und den Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) müssen mit einer Versionsangabe versehen sein. Azure erzwingt diese Einschränkung der Versionsverwaltung. Hier sind Beispiele für gültige URLs für den geheimen Schlüssel und den Schlüsselverschlüsselungsschlüssel angegeben:
	- Beispiel für eine gültige URL des geheimen Schlüssels: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
	- Beispiel für gültigen KRK-Schlüsselverschlüsselungsschlüssel: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Die Azure-Datenträgerverschlüsselung unterstützt keine Portnummern, die als Teil von URLs des geheimen Schlüssels für den Schlüsseltresor und des Schlüsselverschlüsselungsschlüssels angegeben werden. Beispiele für unterstützte Schlüsseltresor-URL:
 	- Nicht akzeptierte Schlüsseltresor-URL: *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
	- Akzeptierte Schlüsseltresor-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Die virtuellen IaaS-Computer müssen die folgenden Anforderungen an die Netzwerkendpunktkonfiguration erfüllen, um die Azure-Datenträgerverschlüsselung zu erfüllen:
	- Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure Active Directory-Endpunkt herstellen können [Login.windows.net], um ein Token für die Verbindungsherstellung mit dem Azure-Schlüsseltresor zu erhalten.
	- Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure-Schlüsseltresor-Endpunkt herstellen können, um die Verschlüsselungsschlüssel in den Schlüsseltresor des Kunden schreiben zu können.
	- Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure-Speicherendpunkt herstellen können, an dem das Azure-Erweiterungsrepository gehostet wird, sowie mit dem Azure-Speicherkonto, unter dem die VHD-Dateien gehostet werden.

**Hinweis:** Wenn Ihre Sicherheitsrichtlinie den Zugriff von virtuellen Azure-Computern auf das Internet beschränkt, können Sie den obigen URI auflösen, zu dem Sie eine Verbindung benötigen, und eine spezielle Regel konfigurieren, um ausgehende Verbindungen mit den IP-Adressen zuzulassen.

- Verwenden Sie die neueste Version des Azure PowerShell-SDK, um Azure Disk Encryption zu konfigurieren. Laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter.

**Hinweis:** Azure Disk Encryption wird unter [Azure PowerShell SDK Version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016) nicht unterstützt. Wenn Sie im Zusammenhang mit der Verwendung von Azure PowerShell 1.1.0 einen Fehler erhalten, finden Sie weitere Informationen im Artikel [Azure Disk Encryption Error Related to Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx) (Azure Disk Encryption-Fehler im Zusammenhang mit Azure PowerShell 1.1.0).

- Sie müssen zuerst die richtige Azure-CLI-Version installieren, um Azure-CLI-Befehle ausführen und die Zuordnung zum Azure-Abonnement durchführen zu können:
	- Um Azure-CLI zu installieren und Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren von Azure-CLI](../xplat-cli-install.md).
	- Informationen zum Verwenden der Azure-CLI für Mac, Linux und Windows mit dem Azure-Ressourcen-Manager finden Sie [hier](azure-cli-arm-commands.md).
- Bei der Lösung für die Azure-Datenträgerverschlüsselung wird die externe BitLocker-Schlüsselschutzvorrichtung für virtuelle Windows-IaaS-Computer verwendet. Wenn Ihre virtuellen Computer der Domäne beigetreten sind, sollten Sie keine Gruppenrichtlinien nutzen, mit denen TPM-Schutzvorrichtungen durchgesetzt werden. Ausführliche Informationen zur Gruppenrichtlinie für „BitLocker ohne kompatibles TPM zulassen“ finden Sie in [diesem Artikel](https://technet.microsoft.com/library/ee706521).
- Das für Azure Disk Encryption erforderliche PowerShell-Skript zum Erstellen einer Azure AD-Anwendung, zum Erstellen eines neuen Schlüsseltresors oder zum Einrichten eines vorhandenen Schlüsseltresors und Aktivieren der Verschlüsselung finden Sie [hier](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).

#### Einrichten der Azure AD-Anwendung in Azure Active Directory

Wenn die Verschlüsselung auf einem ausgeführten virtuellen Computer in Azure aktiviert werden muss, werden die Verschlüsselungsschlüssel von der Azure-Datenträgerverschlüsselung generiert und in den Schlüsseltresor geschrieben. Zum Verwalten von Verschlüsselungsschlüsseln im Schlüsseltresor wird die Azure AD-Authentifizierung benötigt.

Zu diesem Zweck sollte eine Azure AD-Anwendung erstellt werden. Ausführliche Schritte zum Registrieren einer Anwendung finden Sie im Abschnitt „Get an Identity for the Application“ (Abrufen einer Identität für die Anwendung) in [diesem Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Der Beitrag enthält außerdem einige hilfreiche Beispiele zum Bereitstellen und Konfigurieren des Schlüsseltresors. Zu Authentifizierungszwecken kann entweder die auf einem geheimen Clientschlüssel basierende Authentifizierung oder die auf einem Clientzertifikat basierende Azure AD-Authentifizierung verwendet werden.

##### Auf geheimem Clientschlüssel basierende Authentifizierung für Azure AD

In den folgenden Abschnitten sind die erforderlichen Schritte zum Konfigurieren einer Authentifizierung für Azure AD angegeben, die auf einem geheimen Clientschlüssel basiert.

##### Erstellen einer neuen Azure AD-App mit Azure PowerShell

Verwenden Sie das unten angegebene PowerShell-Cmdlet, um eine neue Azure AD-App zu erstellen:

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

**Hinweis:** „$azureAdApplication.ApplicationId“ ist die Azure AD-Client-ID, und „$aadClientSecret“ ist der geheime Clientschlüssel. Verwenden Sie diese Angaben später zum Aktivieren der Azure-Datenträgerverschlüsselung. Sie sollten den geheimen Azure AD-Clientschlüssel entsprechend schützen.


##### Bereitstellen der Azure AD-Client-ID und des geheimen Schlüssels über das Portal „Klassisches Azure-Bereitstellungsmodell“

Sie können die Azure AD-Client-ID und den geheimen Schlüssel auch über das Portal „Klassisches Azure-Bereitstellungsmodell“ unter https://manage.windowsazure.com bereitstellen. Führen Sie hierzu die unten angegebenen Schritte aus:

1\. Klicken Sie wie in der Abbildung unten gezeigt auf die Registerkarte „Active Directory“:

![Azure-Datenträgerverschlüsselung](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2\. Klicken Sie auf „Anwendung hinzufügen“, und geben Sie den Anwendungsnamen wie unten gezeigt ein:

![Azure-Datenträgerverschlüsselung](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3\. Klicken Sie auf die Pfeilschaltfläche, und konfigurieren Sie die Eigenschaften der App wie unten gezeigt:

![Azure-Datenträgerverschlüsselung](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4\. Klicken Sie unten links auf das Häkchen, um den Vorgang abzuschließen. Die Konfigurationsseite der App wird angezeigt. Beachten Sie, dass sich die Azure AD-Client-ID unten auf der Seite befindet. Dies ist in der Abbildung unten dargestellt.

![Azure-Datenträgerverschlüsselung](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5\. Speichern Sie den geheimen Azure AD-Clientschlüssel, indem Sie auf die Schaltfläche „Speichern“ klicken. Klicken Sie auf die Schaltfläche „Speichern“, und sehen Sie sich den geheimen Schlüssel im Textfeld „Schlüssel“ an. Dies ist der geheime Azure AD-Clientschlüssel. Sie sollten den geheimen Azure AD-Clientschlüssel entsprechend schützen.

![Azure-Datenträgerverschlüsselung](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


**Hinweis:** Der obige Ablauf wird im Portal nicht unterstützt.

##### Verwenden einer vorhandenen App

Um die unten angegebenen Befehle auszuführen, benötigen Sie das Azure AD PowerShell-Modul, das Sie [hier](https://technet.microsoft.com/library/jj151815.aspx) erhalten.

**Hinweis:** Die unten angegebenen Befehle müssen über ein neues PowerShell-Fenster ausgeführt werden. Verwenden Sie NICHT Azure PowerShell oder den Azure-Ressourcen-Manager, um diese Befehle auszuführen. Der Grund für diese Empfehlung ist, dass sich diese Cmdlets im MSOnline-Modul oder in Azure AD PowerShell befinden.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### Zertifikatbasierte Authentifizierung für Azure AD

> [AZURE.NOTE] Die zertifikatbasierte AAD-Authentifizierung wird auf Linux-VMs derzeit nicht unterstützt.

In den folgenden Abschnitten sind die erforderlichen Schritte zum Konfigurieren einer zertifikatbasierten Authentifizierung für Azure AD angegeben.

##### Erstellen einer neuen Azure AD-App

Führen Sie die unten angegebenen PowerShell-Cmdlets aus, um eine neue Azure AD-App zu erstellen:

**Hinweis:** Ersetzen Sie die Zeichenfolge `yourpassword` durch Ihr sicheres Kennwort, und schützen Sie das Kennwort.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Laden Sie nach Abschluss dieses Schritts eine PFX-Datei in den Schlüsseltresor hoch, und aktivieren Sie die Zugriffsrichtlinie, die zum Bereitstellen dieses Zertifikats auf einem virtuellen Computer benötigt wird.

##### Verwenden einer vorhandenen Azure AD-App
Verwenden Sie die unten angegebenen PowerShell-Cmdlets, wenn Sie die zertifikatbasierte Authentifizierung für eine vorhandene App konfigurieren. Achten Sie darauf, dass Sie die Cmdlets in einem neuen PowerShell-Fenster ausführen.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Laden Sie nach Abschluss dieses Schritts eine PFX-Datei in den Schlüsseltresor hoch, und aktivieren Sie die Zugriffsrichtlinie, die zum Bereitstellen dieses Zertifikats auf einem virtuellen Computer benötigt wird.

##### Hochladen einer PFX-Datei in den Schlüsseltresor
Eine ausführliche Erklärung der Funktionsweise dieses Prozesses finden Sie in [diesem Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Für diese Aufgabe benötigen Sie nur die unten angegebenen PowerShell-Cmdlets. Stellen Sie sicher, dass Sie sie über die Azure PowerShell-Konsole ausführen:

**Hinweis:** Ersetzen Sie die Zeichenfolge `yourpassword` durch Ihr sicheres Kennwort, und schützen Sie das Kennwort.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### Bereitstellen eines Zertifikats im Schlüsseltresor auf einem vorhandenen virtuellen Computer
Führen Sie nach Abschluss des PFX-Uploads die unten angegebenen Schritte aus, um im Schlüsseltresor ein Zertifikat auf einem vorhandenen virtuellen Computer bereitzustellen:

    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName


#### Festlegen der Richtlinie für den Zugriff auf den Schlüsseltresor für die Azure AD-Anwendung

Ihre Azure AD-Anwendung benötigt Rechte zum Zugreifen auf die Schlüssel oder geheimen Schlüssel im Tresor. Verwenden Sie das [Set-AzureKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx)-Cmdlet zum Gewähren von Berechtigungen für die Anwendung, und verwenden Sie hierbei die Client-ID (die während der Registrierung der Anwendung generiert wurde) als –ServicePrincipalName-Parameterwert. Beispiele hierzu finden Sie in [diesem Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Hier ist auch ein Beispiel für die Durchführung dieser Aufgabe per PowerShell angegeben:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

**HINWEIS:** Bei Azure Disk Encryption müssen Sie die folgenden Zugriffsrichtlinien für Ihre AAD-Clientanwendung konfigurieren: Berechtigungen vom Typ „WrapKey“ und „Set“.

## Terminologie

Verwenden Sie die Terminologietabelle als Referenz, um sich mit einigen allgemeinen Begriffen vertraut zu machen, die von dieser Technologie genutzt werden:

| Terminologie | Definition |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure AD | Azure AD steht für [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Ein Azure AD-Konto ist eine Voraussetzung zum Authentifizieren, Speichern und Abrufen von geheimen Schlüsseln aus dem Schlüsseltresor. |
| Azure-Schlüsseltresor | Der Azure-Schlüsseltresor ist ein Dienst zum Verwalten kryptografischer Schlüssel, der auf Hardwaresicherheitsmodulen mit FIPS-Überprüfung basiert, damit Ihre kryptografischen Schlüssel und vertraulichen Daten sicher und geschützt sind. Weitere Informationen finden Sie in der Dokumentation zum [Schlüsseltresor](https://azure.microsoft.com/services/key-vault/). |
| ARM | Azure-Ressourcen-Manager |
| BitLocker | Bei [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) handelt es sich um eine branchenübliche Verschlüsselungstechnologie für Windows-Volumes, um die Datenträgerverschlüsselung auf virtuellen Windows-IaaS-Computern zu aktivieren. |
| BEK | BitLocker-Verschlüsselungsschlüssel (BitLocker Encryption Keys) werden verwendet, um das Startvolume des Betriebssystems und Datenvolumes zu verschlüsseln. Die BitLocker-Schlüssel sind im Azure-Schlüsseltresor als geheime Schlüssel geschützt. |
| Befehlszeilenschnittstelle (CLI) | [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) |
| DM-Crypt | [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) ist das Linux-basierte transparente Subsystem für die Datenträgerverschlüsselung, das zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Linux-IaaS-Computern verwendet wird. |
| KEK | Der Schlüsselverschlüsselungsschlüssel (Key Encryption Key) ist der asymmetrische Schlüssel (RSA 2048), der zum Schützen oder Umschließen (falls erforderlich) des geheimen Schlüssels verwendet wird. Sie können einen Schlüssel mit HSM-Schutz oder einen Schlüssel mit Softwareschutz bereitstellen. Weitere Informationen finden Sie in der Dokumentation zum [Azure-Schlüsseltresor](https://azure.microsoft.com/services/key-vault/). |
| PS-Cmdlets | [Azure PowerShell-Cmdlets](powershell-install-configure.md) |

### Festlegen und Konfigurieren des Azure-Schlüsseltresors für die Azure-Datenträgerverschlüsselung

Mit der Azure-Datenträgerverschlüsselung werden die Schlüssel und geheimen Schlüssel in Ihrem Azure-Schlüsseltresor geschützt. Führen Sie die Schritte unten in den einzelnen Abschnitten aus, um den Schlüsseltresor für die Azure-Datenträgerverschlüsselung einzurichten.

#### Erstellen eines neuen Schlüsseltresors
Verwenden Sie eine der unten angegebenen Optionen, um einen neuen Schlüsseltresor zu erstellen:

- Verwenden Sie die Resource Manager-Vorlage „101-Create-KeyVault“, die Sie [hier](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json) finden.
- Verwenden Sie die [Azure PowerShell-Cmdlets für den Schlüsseltresor](https://msdn.microsoft.com/library/dn868052.aspx).
- Verwenden Sie das Azure Resource Manager-Portal.

**Hinweis:** Fahren Sie mit dem nächsten Abschnitt fort, falls Sie bereits einen Schlüsseltresor für Ihr Abonnement eingerichtet haben.

![Azure-Schlüsseltresor](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### Bereitstellen eines Schlüsselverschlüsselungsschlüssels (optional)

Wenn Sie als zusätzliche Sicherheitsmaßnahme einen Schlüsselverschlüsselungsschlüssel als Wrapper für die BitLocker-Verschlüsselungsschlüssel verwenden möchten, sollten Sie Ihrem Schlüsseltresor einen Schlüsselverschlüsselungsschlüssel zur Verwendung während des Bereitstellungsprozesses hinzufügen. Verwenden Sie das [Add-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868048.aspx)-Cmdlet, um im Schlüsseltresor einen neuen Schlüsselverschlüsselungsschlüssel zu erstellen. Sie können den KEK auch aus Ihrem lokalen Hardwaresicherheitsmodul (HSM) für die Schlüsselverwaltung importieren. Weitere Informationen finden Sie in der [Dokumentation zum Schlüsseltresor](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Der KEK kann aus dem Azure Resource Manager-Portal und mit der Azure Key Vault-UX hinzugefügt werden.

![Azure-Schlüsseltresor](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### Festlegen von Schlüsseltresor-Berechtigungen zur Gewährung des Zugriffs auf Schlüssel und geheime Schlüssel für die Azure-Plattform

Die Azure-Plattform benötigt Zugriff auf die Verschlüsselungsschlüssel oder geheimen Schlüssel in Ihrem Azure-Schlüsseltresor, um sie für den virtuellen Computer zur Verfügung zu stellen, damit die Volumes gestartet und entschlüsselt werden können. Um der Azure-Plattform Berechtigungen für den Zugriff auf den Schlüsseltresor zu gewähren, muss die *enabledForDiskEncryption*-Eigenschaft für den Schlüsseltresor festgelegt werden. Sie können die enabledForDiskEncryption-Eigenschaft für den Schlüsseltresor mit dem PS-Cmdlet des Schlüsseltresors festlegen:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Sie können die *enabledForDiskEncryption*-Eigenschaft auch unter https://resources.azure.com festlegen. Sie müssen die *enabledForDiskEncryption*-Eigenschaft wie oben erwähnt für Ihren Schlüsseltresor festlegen. Andernfalls schlägt die Bereitstellung fehl.

Sie können Zugriffsrichtlinien für Ihre AAD-Anwendung über die Schlüsseltresor-UX einrichten:

![Azure-Schlüsseltresor](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure-Schlüsseltresor](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Stellen Sie sicher, dass der Schlüsseltresor für die Datenträgerverschlüsselung unter „Erweiterte Zugriffsrichtlinien“ aktiviert ist:

![Azure-Schlüsseltresor](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## Bereitstellungsszenarien und Benutzeroberflächen der Datenträgerverschlüsselung

Es gibt viele Szenarien, in denen Sie die Datenträgerverschlüsselung aktivieren können. Die Schritte können je nach Szenario variieren. In den folgenden Abschnitten werden diese Szenarien ausführlicher beschrieben.

### Aktivieren der Verschlüsselung auf neuen virtuellen IaaS-VM-Computern, die über den Azure-Katalog erstellt werden

Die Datenträgerverschlüsselung kann auf neuen virtuellen Windows-IaaS-Computern über den Azure-Katalog in Azure aktiviert werden, indem die [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) veröffentlichte Resource Manager-Vorlage genutzt wird. Klicken Sie in der Azure-Schnellstartvorlage auf die Schaltfläche „In Azure bereitstellen“, geben Sie die Verschlüsselungskonfiguration im Blatt „Parameter“ ein, und klicken Sie auf „OK“. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf die Schaltfläche „Erstellen“, um die Verschlüsselung auf einem neuen virtuellen IaaS-Computer zu aktivieren.

**Hinweis:** Mit dieser Vorlage wird ein neuer verschlüsselter virtueller Windows-Computer erstellt, indem das Windows Server 2012-Katalogimage verwendet wird.

Die Datenträgerverschlüsselung kann auf einer neuen IaaS-RedHat Linux 7.2-VM mit einem 200 GB RAID-0-Array aktiviert werden, indem [diese Resource Manager-Vorlage](https://aka.ms/fde-rhel) verwendet wird. Überprüfen Sie nach der Bereitstellung der Vorlage den VM-Verschlüsselungsstatus, indem Sie das `Get-AzureRmVmDiskEncryptionStatus`-Cmdlet wie unter „[Verschlüsseln des Betriebssystemdatenträgers auf einer laufenden Linux-VM](#encrypting-os-drive-on-a-running-linux-vm)“ beschrieben verwenden. Wenn der Computer den Status `VMRestartPending` zurückgibt, können Sie die VM neu starten.

Unten in der Tabelle sind die Parameterdetails der Resource Manager-Vorlage für neue virtuelle Computer aus dem Azure-Katalogszenario mit Azure AD-Client-ID aufgeführt:

| Parameter | Beschreibung|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| adminUserName | Administratorbenutzername für den virtuellen Computer. |
| adminPassword | Administratorkennwort für den virtuellen Computer. |
| newStorageAccountName | Name des Speicherkontos zum Speichern von Betriebssystem- und Daten-VHDs. |
| vmSize | Die Größe des virtuellen Computers. Derzeit werden nur die standardmäßigen A-, D- und G-Serien unterstützt. |
| virtualNetworkName | Der Name des VNET, zu dem die VM-Netzwerkschnittstellenkarte gehören sollte. |
| subnetName | Name des Subnetzes im VNET, zu dem die VM-Netzwerkschnittstellenkarte gehören sollte. |
| AADClientID | Client-ID der Azure AD-App, die über Berechtigungen zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor verfügt. |
| AADClientSecret | Geheimer Clientschlüssel der Azure AD-App, die über Berechtigungen zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor verfügt. |
| keyVaultURL | Die URL des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden sollte. Sie können sie über das PowerShell-Cmdlet abrufen: (Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI |
| keyEncryptionKeyURL | Die URL des Schlüsselverschlüsselungsschlüssels, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dies ist optional. |
| keyVaultResourceGroup | Ressourcengruppe des Schlüsseltresors. |
| vmName | Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird.


**Hinweis:** „KeyEncryptionKeyURL“ ist ein optionaler Parameter. Sie können Ihren eigenen Schlüsselverschlüsselungsschlüssel nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (geheimer Passphrase-Schlüssel) im Schlüsseltresor zu erhöhen.

### Aktivieren der Verschlüsselung auf neuen virtuellen IaaS-Computern, die mit einer vom Kunden verschlüsselten VHD und Verschlüsselungsschlüsseln erstellt werden

Bei diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie die Resource Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. In den folgenden Abschnitten werden die Resource Manager-Vorlage und die CLI-Befehle ausführlicher erläutert.

Befolgen Sie die Anleitung in einem dieser Abschnitte zum Vorbereiten von vorverschlüsselten Images, die in Azure verwendet werden können. Nach dem Erstellen des Image können Sie die Schritte im nächsten Abschnitt ausführen, um eine verschlüsselte Azure-VM zu erstellen.

- [Vorbereiten einer vorverschlüsselten Windows-VHD](#preparing-a-pre-encrypted-windows-vhd)
- [Vorbereiten einer vorverschlüsselten Linux-VHD](#preparing-a-pre-encrypted-linux-vhd)

#### Verwenden der Resource Manager-Vorlage

Die Datenträgerverschlüsselung kann auf einer vom Kunden verschlüsselten VHD aktiviert werden, indem die [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm) veröffentlichte Resource Manager-Vorlage genutzt wird. Klicken Sie in der Azure-Schnellstartvorlage auf die Schaltfläche „In Azure bereitstellen“, geben Sie die Verschlüsselungskonfiguration im Blatt „Parameter“ ein, und klicken Sie auf „OK“. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf die Schaltfläche „Erstellen“, um die Verschlüsselung auf einem neuen virtuellen IaaS-Computer zu aktivieren.

In der Tabelle unten sind die Parameterdetails der Resource Manager-Vorlage für ein Szenario mit einer vom Kunden verschlüsselten VHD beschrieben:

| Parameter | Beschreibung|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newStorageAccountName | Name des Speicherkontos zum Speichern des Betriebssystems auf einer verschlüsselten VHD. Dieses Speicherkonto sollte bereits in derselben Ressourcengruppe und an demselben Speicherort wie der virtuelle Computer erstellt worden sein. |
| osVhdUri | URI der Betriebssystem-VHD aus dem Speicherkonto |
| osType | Betriebssystem-Produkttyp (Windows/Linux) |
| virtualNetworkName | Der Name des VNET, zu dem die VM-Netzwerkschnittstellenkarte gehören sollte. Es sollte bereits in derselben Ressourcengruppe und an demselben Speicherort wie der virtuelle Computer erstellt worden sein. |
| subnetName | Name des Subnetzes im VNET, zu dem die VM-Netzwerkschnittstellenkarte gehören sollte. |
| vmSize | Die Größe des virtuellen Computers. Derzeit werden nur die standardmäßigen A-, D- und G-Serien unterstützt. |
| keyVaultResourceID | ResourceID zum Identifizieren der Schlüsseltresor-Ressource in ARM. Sie können sie mit dem PowerShell-Cmdlet abrufen: (Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId |
| keyVaultSecretUrl | ​URL des Schlüssels für die Datenträgerverschlüsselung im Schlüsseltresor |
| keyVaultKekUrl | Die URL des Schlüsselverschlüsselungsschlüssels, der zum Verschlüsseln des generierten Datenträgerverschlüsselungsschlüssels verwendet wird. |
| ​vmName | ​Name des virtuellen IaaS-Computers |


#### Verwenden von PowerShell-Cmdlets

Die Datenträgerverschlüsselung kann auf einer vom Kunden verschlüsselten VHD aktiviert werden, indem die [hier](https://msdn.microsoft.com/library/azure/mt603746.aspx) veröffentlichten PowerShell-Cmdlets genutzt werden.

#### Verwenden von CLI-Befehlen

Führen Sie die folgenden Schritte aus, um die Datenträgerverschlüsselung für dieses Szenario mit CLI-Befehlen zu aktivieren:

1. Legen Sie Zugriffsrichtlinien für den Schlüsseltresor fest:
	- Legen Sie das Flag „EnabledForDiskEncryption“ fest: `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
	- Legen Sie die Berechtigungen für die Azure AD-App fest, um geheime Schlüssel in KeyVault zu schreiben: `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys ["all"] --perms-to-secrets ["all"]`
2. Geben Sie Folgendes ein, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen Computer zu aktivieren: *azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId>*
3. Rufen Sie den Verschlüsselungsstatus ab: *azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json*
4. Verwenden Sie die unten angegebenen Parameter mit dem Befehl „azure vm create“, um die Verschlüsselung auf einem neuen virtuellen Computer über eine vom Kunden verschlüsselte VHD zu aktivieren:
	- disk-encryption-key-vault-id <disk-encryption-key-vault-id>
	- disk-encryption-key-url <disk-encryption-key-url>
	- key-encryption-key-vault-id <key-encryption-key-vault-id>
	- key-encryption-key-url <key-encryption-key-url>


### Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Windows-IaaS-Computer in Azure

Bei diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie die Resource Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. In den folgenden Abschnitten wird ausführlicher erläutert, wie Sie dies mit der Resource Manager-Vorlage und CLI-Befehlen aktivieren.

#### Verwenden der Resource Manager-Vorlage

Die Datenträgerverschlüsselung kann auf einem vorhandenen/aktiven Windows-IaaS-Computer in Azure aktiviert werden, indem die [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm) veröffentlichte Resource Manager-Vorlage genutzt wird. Klicken Sie in der Azure-Schnellstartvorlage auf die Schaltfläche „In Azure bereitstellen“, geben Sie die Verschlüsselungskonfiguration im Blatt „Parameter“ ein, und klicken Sie auf „OK“. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf die Schaltfläche „Erstellen“, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen IaaS-Computer zu aktivieren.

In der Tabelle unten sind die Parameterdetails der Resource Manager-Vorlage für ein Szenario mit vorhandenem/ausgeführtem virtuellem Computer per Azure AD-Client-ID angegeben:

| Parameter | Beschreibung|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ​AADClientID | ​Client-ID der Azure AD-App, die über Berechtigungen zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor verfügt. |
| AADClientSecret | ​Geheimer Clientschlüssel der Azure AD-App, die über Berechtigungen zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor verfügt. |
| keyVaultName | Der Name des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden sollte. Sie können ihn mithilfe dieses Cmdlets abrufen: (Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname |
| ​ keyEncryptionKeyURL | Die URL des Schlüsselverschlüsselungsschlüssels, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dies ist optional, wenn Sie in der Dropdownliste „UseExistingKek“ die Option `nokek` wählen. Wenn Sie in der Dropdownliste „UseExistingKek“ die Option `kek` wählen, müssen Sie den keyEncryptionKeyURL-Wert eingeben. |
| ​volumeType | ​Der Typ des Volumes, auf dem der Verschlüsselungsvorgang durchgeführt wird. Gültige Werte sind „OS“, „Data“, „All“. |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerverschlüsselung durchgeführt wird. |
| ​vmName | ​Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird.


**Hinweis:** „KeyEncryptionKeyURL“ ist ein optionaler Parameter. Sie können Ihren eigenen Schlüsselverschlüsselungsschlüssel nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (geheimer BitLocker-Verschlüsselungsschlüssel) im Schlüsseltresor zu erhöhen.

#### Verwenden von PowerShell-Cmdlets

Ausführliche Informationen dazu, wie Verschlüsselung über Azure-Datenträgerverschlüsselung mit PowerShell-Cmdlets aktiviert wird, finden Sie im Blogbeitrag **Explore Azure disk encryption with Azure PowerShell** in [Teil 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) und [Teil 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### Verwenden von CLI-Befehlen

Führen Sie die unten angegebenen Schritte aus, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen Windows-IaaS-Computer in Azure mit CLI-Befehlen zu aktivieren:

1. Legen Sie Zugriffsrichtlinien für den Schlüsseltresor fest:
	- Legen Sie das Flag „EnabledForDiskEncryption“ fest: „azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true“
	- Legen Sie Berechtigungen für die Azure AD-App zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor fest: „azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys ["all"] --perms-to-secrets ["all"]“
2. Geben Sie Folgendes ein, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen Computer zu aktivieren: *azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId>*
3. Rufen Sie den Verschlüsselungsstatus ab: *azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json*
4. Verwenden Sie die unten angegebenen Parameter mit dem Befehl „azure vm create“, um die Verschlüsselung auf einem neuen virtuellen Computer über eine vom Kunden verschlüsselte VHD zu aktivieren:
	- disk-encryption-key-vault-id <disk-encryption-key-vault-id>
	- disk-encryption-key-url <disk-encryption-key-url>
	- key-encryption-key-vault-id <key-encryption-key-vault-id>
	- key-encryption-key-url <key-encryption-key-url>


### Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Linux-IaaS-Computer in Azure

Die Datenträgerverschlüsselung kann auf einem vorhandenen/aktiven Linux-IaaS-Computer in Azure aktiviert werden, indem die [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm) veröffentlichte Resource Manager-Vorlage genutzt wird. Klicken Sie in der Azure-Schnellstartvorlage auf die Schaltfläche „In Azure bereitstellen“, geben Sie die Verschlüsselungskonfiguration im Blatt „Parameter“ ein, und klicken Sie auf „OK“. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf die Schaltfläche „Erstellen“, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen IaaS-Computer zu aktivieren.

In der Tabelle unten werden die Parameterdetails der Resource Manager-Vorlage für ein Szenario mit vorhandenem/ausgeführtem virtuellem Computer per Azure AD-Client-ID beschrieben:

| Parameter | Beschreibung|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ​AADClientID | ​Client-ID der Azure AD-App, die über Berechtigungen zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor verfügt. |
| AADClientSecret | ​Geheimer Clientschlüssel der Azure AD-App, die über Berechtigungen zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor verfügt. |
| keyVaultName | Der Name des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden sollte. Sie können ihn mithilfe dieses Cmdlets abrufen: (Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname |
| ​ keyEncryptionKeyURL | Die URL des Schlüsselverschlüsselungsschlüssels, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dies ist optional, wenn Sie in der Dropdownliste „UseExistingKek“ die Option „nokek“ wählen. Wenn Sie in der Dropdownliste „UseExistingKek“ die Option „kek“ wählen, müssen Sie den keyEncryptionKeyURL-Wert eingeben. |
| ​volumeType | ​Der Typ des Volumes, auf dem der Verschlüsselungsvorgang durchgeführt wird. Gültige unterstützte Werte sind „OS“/„All“ (für RHEL 7.2, CentOS 7.2 und Ubuntu 16.04) und „Data“ für alle anderen Distributionen. |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerverschlüsselung durchgeführt wird. |
| ​vmName | ​Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird.
| passPhrase | Geben Sie eine sichere Passphrase als Datenverschlüsselungsschlüssel ein. |                                                                                                                                                                                                                                                      

**Hinweis:** „KeyEncryptionKeyURL“ ist ein optionaler Parameter. Sie können Ihren eigenen Schlüsselverschlüsselungsschlüssel nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (geheimer Passphrase-Schlüssel) im Schlüsseltresor zu erhöhen.

#### CLI-Befehle

Die Datenträgerverschlüsselung kann auf vom Kunden verschlüsselten VHDs aktiviert werden, indem der von [hier](../xplat-cli-install.md) installierte CLI-Befehl verwendet wird. Führen Sie die unten angegebenen Schritte aus, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen Linux-IaaS-Computer in Azure mit CLI-Befehlen zu aktivieren:

1. Legen Sie Zugriffsrichtlinien für den Schlüsseltresor fest:
	- Legen Sie das Flag „EnabledForDiskEncryption“ fest: „azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true“
	- Legen Sie Berechtigungen für die Azure AD-App zum Schreiben von geheimen Schlüsseln in den Schlüsseltresor fest: „azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys ["all"] --perms-to-secrets ["all"]“
2. Geben Sie Folgendes ein, um die Verschlüsselung auf einem vorhandenen/ausgeführten virtuellen Computer zu aktivieren: *azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId>*
3. Rufen Sie den Verschlüsselungsstatus ab: „azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json“
4. Verwenden Sie die unten angegebenen Parameter mit dem Befehl „azure vm create“, um die Verschlüsselung auf einem neuen virtuellen Computer über eine vom Kunden verschlüsselte VHD zu aktivieren.
	- *disk-encryption-key-vault-id <disk-encryption-key-vault-id>*
	- *disk-encryption-key-url <disk-encryption-key-url>*
	- *key-encryption-key-vault-id <key-encryption-key-vault-id>*
	- *key-encryption-key-url <key-encryption-key-url>*

### Abrufen des Verschlüsselungsstatus eines verschlüsselten virtuellen IaaS-Computers

Sie können den Verschlüsselungsstatus über das Azure Resource Manager-Portal, mit [PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt622700.aspx) oder mithilfe von CLI-Befehlen abrufen. In den folgenden Abschnitten wird erläutert, wie Sie das Azure-Portal und CLI-Befehle zum Abrufen des Verschlüsselungsstatus verwenden.

#### Abrufen des Verschlüsselungsstatus einer verschlüsselten Windows-VM mit dem Azure Resource Manager-Portal

Sie können den Verschlüsselungsstatus des virtuellen IaaS-Computers über das Azure Resource Manager-Portal abrufen. Melden Sie sich unter https://portal.azure.com/ beim Azure-Portal an, und klicken Sie im linken Menü auf den Link zu den virtuellen Computern, um eine Zusammenfassung der virtuellen Computer in Ihrem Abonnement anzuzeigen. Sie können die Ansicht mit den virtuellen Computern filtern, indem Sie den Abonnementnamen in der Abonnement-Dropdownliste auswählen. Klicken Sie auf die Spalten, die sich am oberen Rand des Seitenmenüs für die virtuellen Computer befinden. Wählen Sie im Blatt „Spalte auswählen“ die Spalte „Datenträgerverschlüsselung“, und klicken Sie auf „Aktualisieren“. Sie sehen, dass in der Spalte „Datenträgerverschlüsselung“ für jeden virtuellen Computer der Verschlüsselungsstatus „Aktiviert“ oder „Nicht aktiviert“ angezeigt wird. Dies ist unten in der Abbildung dargestellt.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### Abrufen des Verschlüsselungsstatus eines verschlüsselten virtuellen IaaS-Computers (Windows/Linux) mit dem PS-Cmdlet für die Datenträgerverschlüsselung
Sie können den Verschlüsselungsstatus des virtuellen IaaS-Computers über das PS-Cmdlet „Get-AzureRmVMDiskEncryptionStatus“ für die Datenträgerverschlüsselung abrufen. Geben Sie zum Abrufen der Verschlüsselungseinstellungen für Ihren virtuellen Computer Ihre Azure PowerShell-Sitzung ein:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Die Ausgabe von „Get-AzureRmVMDiskEncryptionStatus“ kann auf Verschlüsselungsschlüssel-URLs untersucht werden.
    
    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMNam
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey
    
    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

Die Werte für die Einstellungen „OSVolumeEncrypted“ und „DataVolumesEncrypted“ sind auf „Encrypted“ festgelegt. Dies bedeutet, dass beide Volumes per Azure-Datenträgerverschlüsselung verschlüsselt sind. Ausführliche Informationen dazu, wie Verschlüsselung über Azure-Datenträgerverschlüsselung mit PowerShell-Cmdlets aktiviert wird, finden Sie im Blogbeitrag **Explore Azure disk encryption with Azure PowerShell** in [Teil 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) und [Teil 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

**HINWEIS**: Auf Linux-VMs benötigt das `Get-AzureRmVMDiskEncryptionStatus`-Cmdlet drei bis vier Minuten zum Melden des Verschlüsselungsstatus.

#### Abrufen des Verschlüsselungsstatus des virtuellen IaaS-Computers über den CLI-Befehl für die Datenträgerverschlüsselung

Sie können den Verschlüsselungsstatus des virtuellen IaaS-Computers über den CLI-Befehl *azure vm show-disk-encryption-status* für die Datenträgerverschlüsselung abrufen. Geben Sie Ihre Azure-CLI-Sitzung ein, um die Verschlüsselungseinstellungen für Ihren virtuellen Computer abzurufen:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### Deaktivieren der Verschlüsselung auf laufenden Windows-IaaS-VMs

Sie können die Verschlüsselung auf einem laufenden virtuellen Windows- oder Linux-IaaS-Computer über die Azure Disk Encryption-Resource Manager-Vorlage oder über PS-Cmdlets deaktivieren und die Entschlüsselungskonfiguration angeben.


##### Windows-VM

Durch den Schritt zum Deaktivieren der Verschlüsselung wird die Verschlüsselung des Betriebssystemdatenträgers und/oder des Datenträgers für Daten in der laufenden Windows-IaaS-VM deaktiviert. Sie können das Betriebssystemvolume nicht deaktivieren und das Datenvolume verschlüsselt lassen. Wenn der Verschlüsselungsschritt ausgeführt wird, aktualisiert das klassische Azure-Bereitstellungsmodell das VM-Dienstmodell, und die Windows-IaaS-VM wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt. Durch das Deaktivieren der Verschlüsselung werden der Kundenschlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows und Passphrase für Linux) nicht gelöscht.

##### Linux-VM

Durch den Schritt zum Deaktivieren der Verschlüsselung wird die Verschlüsselung des Datenträgers für Daten auf der laufenden Linux-IaaS-VM deaktiviert.

**HINWEIS**: Das Deaktivieren der Verschlüsselung auf dem Betriebssystemdatenträger ist auf Linux-VMs nicht zulässig.

##### Deaktivieren der Verschlüsselung einer vorhandenen/laufenden IaaS-VM in Azure per Resource Manager-Vorlage

Die Datenträgerverschlüsselung kann mithilfe der [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) veröffentlichten Resource Manager-Vorlage auf einer ausgeführten Windows-IaaS-VM deaktiviert werden. Klicken Sie in der Azure-Schnellstartvorlage auf die Schaltfläche „In Azure bereitstellen“, geben Sie die Entschlüsselungskonfiguration auf dem Blatt „Parameter“ ein, und klicken Sie auf „OK“. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf die Schaltfläche „Erstellen“, um die Verschlüsselung auf einem neuen virtuellen IaaS-Computer zu aktivieren.

Für die Linux-VM kann [diese](https://aka.ms/decrypt-linuxvm) Vorlage zum Deaktivieren der Verschlüsselung verwendet werden.

Details zu den Resource Manager-Vorlagenparametern für das Deaktivieren der Verschlüsselung auf einer laufenden IaaS-VM:

| ​vmName | ​Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird. |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ​volumeType | ​Der Typ des Volumes, auf dem der Entschlüsselungsvorgang durchgeführt wird. Gültige Werte sind „OS“, „Data“, „All“. **Hinweis:** Die Verschlüsselung auf einem ausgeführten Betriebssystem-/Startvolume einer Windows-IaaS-VM kann nicht deaktiviert werden, ohne die Verschlüsselung auf dem Volume „Data“ zu deaktivieren. **Hinweis**: Das Deaktivieren der Verschlüsselung auf dem Betriebssystemdatenträger ist auf Linux-VMs nicht zulässig. |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerentschlüsselung durchgeführt wird. |

##### Deaktivieren der Verschlüsselung auf einer vorhandenen/laufenden IaaS-VM in Azure mithilfe eines PS-Cmdlets

Das PS-Cmdlet [Disable-AzureRmVMDiskEncryption](https://msdn.microsoft.com/library/azure/mt715776.aspx) deaktiviert die Verschlüsselung auf einem virtuellen IaaS-Computer (Infrastructure-as-a-Service). Dieses Cmdlet unterstützt sowohl Windows- als auch Linux-Computer. Durch dieses Cmdlet wird eine Erweiterung auf dem virtuellen Computer installiert, um die Verschlüsselung zu deaktivieren. Wenn der Parameter „Name“ nicht angegeben ist, wird eine Erweiterung mit dem Standardnamen „AzureDiskEncryption für Windows-VMs“ erstellt.

Auf Linux-VMs wird die Erweiterung „AzureDiskEncryptionForLinux“ verwendet.

**Hinweis:** Durch dieses Cmdlet wird der virtuelle Computer neu gestartet.

## Anhang

### Verbinden mit Ihrem Abonnement

Lesen Sie in diesem Artikel den Abschnitt *Voraussetzungen* durch, bevor Sie fortfahren. Führen Sie nach dem Sicherstellen, dass alle Voraussetzungen erfüllt sind, die folgenden Schritte aus, um eine Verbindung mit Ihrem Abonnement herzustellen:

1\. Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:

    Login-AzureRmAccount

2\. Wenn Sie über mehrere Abonnements verfügen und zur Verwendung ein bestimmtes Abonnement verwenden möchten, geben Sie den folgenden Befehl ein, um die Abonnements für Ihr Konto anzuzeigen:

    Get-AzureRmSubscription

3\. Geben Sie Folgendes ein, um das zu verwendende Abonnement anzugeben:

    Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>

4\. Geben Sie Folgendes ein, um zu überprüfen, ob es sich um das richtige Abonnement handelt:

    Get-AzureRmSubscription

5\. Geben Sie Folgendes ein, um zu überprüfen, ob die Cmdlets für die Azure-Datenträgerverschlüsselung installiert sind:

    Get-command *diskencryption*

6\. Es sollte die unten angegebene Ausgabe erfolgen, um die PowerShell-Installation für die Azure-Datenträgerverschlüsselung zu bestätigen:

    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                               	      Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     

### Vorbereiten einer vorverschlüsselten Windows-VHD
Die folgenden Abschnitte sind erforderlich, um eine vorverschlüsselte Windows-VHD für die Bereitstellung als verschlüsselte VHD in Azure IaaS vorzubereiten. Die Schritte dienen zum Vorbereiten und Starten eines neuen virtuellen Windows-Computers (VHD) unter Hyper-V oder Azure.

#### Aktualisieren der Gruppenrichtlinie, um Schutz ohne TPM für das Betriebssystem zu ermöglichen
Sie müssen die BitLocker-Gruppenrichtlinieneinstellung mit dem Namen „BitLocker-Laufwerkverschlüsselung“ konfigurieren, die sich unter „ Lokale Computerrichtlinie\\Computerkonfiguration\\Administrative Vorlagen\\Windows-Komponenten“ befindet. Ändern Sie diese Einstellung wie folgt: *Betriebssystemlaufwerke – Zusätzliche Authentifizierung beim Start anfordern – BitLocker ohne kompatibles TPM zulassen*. Dies ist unten in der Abbildung dargestellt:

![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### Installieren der Komponenten der BitLocker-Funktion
Verwenden Sie für Windows Server 2012 und höher den folgenden Befehl:

    dism /online /Enable-Feature /all /FeatureName:Bitlocker /quiet /norestart

Verwenden Sie für Windows Server 2008 R2 den folgenden Befehl:

    ServerManagerCmd -install BitLockers

#### Vorbereiten des Betriebssystemvolumes für BitLocker mit `bdehdcfg`

Führen Sie den unten angegebenen Befehl aus, um die Betriebssystempartition zu kombinieren, und bereiten Sie den Computer für BitLocker vor.

    bdehdcfg -target c: shrink -quiet

#### Verwenden von BitLocker zum Schützen des Betriebssystemvolumes
Verwenden Sie den Befehl [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx), um die Verschlüsselung auf dem Startvolume mit einer externen Schlüsselschutzvorrichtung zu aktivieren, und legen Sie den externen Schlüssel (BEK-Datei) auf dem externen Laufwerk oder Volume ab. Die Verschlüsselung wird auf dem System-/Startvolume nach dem nächsten Neustart aktiviert.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

**Hinweis:** Der virtuelle Computer muss mit einer separaten Daten-/Ressourcen-VHD zum Abrufen des externen Schlüssels mit BitLocker vorbereitet werden.

#### Verschlüsseln des Betriebssystemdatenträgers auf einer laufenden Linux-VM

Die Verschlüsselung des Betriebssystemdatenträgers auf einer laufenden Linux-VM wird für die folgenden Distributionen unterstützt:

- RHEL 7.2
- CentOS 7.2
- Ubuntu 16.04

Voraussetzungen für die Verschlüsselung des Betriebssystemdatenträgers:

- Die VM muss über ein Image aus dem Azure-Katalog im Azure Resource Manager-Portal erstellt werden.
- Azure-VM mit mindestens 4 GB RAM (7 GB empfohlen).
- (Für RHEL und CentOS) SELinux muss auf der VM [deaktiviert](https://access.redhat.com/documentation/de-DE/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) werden. Die VM muss nach dem Deaktivieren von SELinux mindestens einmal neu gestartet werden.


##### Schritte

1\. Erstellen Sie eine VM mit einer der oben angegebenen Distributionen.

Für CentOS 7.2 wird die Verschlüsselung des Betriebssystemdatenträgers über ein spezielles Image unterstützt. Geben Sie zum Verwenden dieses Image beim Erstellen der VM „7.2n“ als SKU an:

    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"

2\. Konfigurieren Sie die VM gemäß Ihren Anforderungen. Wenn Sie alle Datenträger (für Betriebssystem und Daten) verschlüsseln möchten, müssen die Datenträger für Daten angegeben und über „/etc/fstab“ bereitgestellt werden.

> [AZURE.NOTE] Verwenden Sie „UUID=...“, um Datenlaufwerke nicht über den Blockgerätnamen, z.B. /dev/sdb1“, sondern unter „/etc/fstab“ anzugeben. Während der Verschlüsselung ändert sich die Reihenfolge der Laufwerke auf dem virtuellen Computer. Wenn für Ihren virtuellen Computer eine bestimmte Reihenfolge der Blockgeräte erforderlich ist, funktioniert die Bereitstellung nach der Verschlüsselung nicht mehr.

3\. Führen Sie die Abmeldung für die SSH-Sitzungen durch.

4\. Geben Sie zum Verschlüsseln des Betriebssystems „volumeType“ als „All“ oder „OS“ an, wenn Sie die [Verschlüsselung aktivieren](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

> [AZURE.NOTE] Alle Prozesse des Benutzerbereichs, die nicht als `systemd`-Dienste ausgeführt werden, müssen per `SIGKILL` beendet werden. Der virtuelle Computer muss neu gestartet werden. Planen Sie für den virtuellen Computer eine Ausfallzeit ein, wenn Sie die Verschlüsselung des Betriebssystemdatenträgers auf einem laufenden virtuellen Computer aktivieren.

5\. Überprüfen Sie den Fortschritt der Verschlüsselung in regelmäßigen Abständen, indem Sie die Anleitung im [nächsten Abschnitt](#monitoring-os-encryption-progress) befolgen.

6\. Nachdem für „Get-AzureRmVmDiskEncryptionStatus“ der Status „VMRestartPending“ angezeigt wird, können Sie die VM neu starten, indem Sie sich entweder bei der VM anmelden oder die Anmeldung per Portal/PowerShell/CLI durchführen.

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, please reboot the VM

Es ist ratsam, die [Startdiagnose](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) der VM *vor* dem Neustart zu speichern.

#### Überwachen des Fortschritts der Betriebssystemverschlüsselung

Es gibt drei Möglichkeiten, den Fortschritt der Betriebssystemverschlüsselung zu überwachen.

1\. Verwenden Sie das Get-AzureRmVmDiskEncryptionStatus-Cmdlet, und untersuchen Sie das Feld „ProgressMessage“:
 
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started

Nachdem die VM den Status „OS disk encryption started“ erreicht hat, dauert der Vorgang auf einer VM mit Premium-Speicher ca. 40 bis 50 Minuten.

Aufgrund von [Issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent werden `OsVolumeEncrypted` und `DataVolumesEncrypted` in einigen Distributionen als `Unknown` angezeigt. Bei WALinuxAgent Version 2.1.5 und höher wird dies automatisch behoben. Falls Sie `Unknown` in der Ausgabe sehen, können Sie den Status der Datenverschlüsselung mit der Azure-Ressourcenanzeige überprüfen.

Wechseln Sie zur Azure-Ressourcenanzeige ([Azure Resource Viewer](https://resources.azure.com/)), und erweitern Sie diese Hierarchie im Auswahlbereich auf der linken Seite:

~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

Scrollen Sie unter „InstanceView“ nach unten, um den Verschlüsselungsstatus Ihrer Laufwerke anzuzeigen.

![VM-Instanzanzeige](./media/azure-security-disk-encryption/vm-instanceview.png)

2\. Sehen Sie sich die [Startdiagnose](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) an. Meldungen der ADE-Erweiterung verfügen über das Präfix `[AzureDiskEncryption]`.

3\. Melden Sie sich per SSH an der VM an, und rufen Sie das Erweiterungsprotokoll ab.

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

Es ist nicht ratsam, die Anmeldung an der VM durchzuführen, während der Betriebssystem-Verschlüsselungsvorgang läuft. Daher sollten die Protokolle nur kopiert werden, wenn die beiden anderen Methoden nicht erfolgreich waren.

#### Vorbereiten einer vorverschlüsselten Linux-VHD

##### Ubuntu 16

###### Konfigurieren der Verschlüsselung während der Installation einer Distribution

1\. Wählen Sie beim Partitionieren von Datenträgern die Option „Configure encrypted volumes“ (Verschlüsselte Volumes konfigurieren).

![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2\. Erstellen Sie ein separates Startlaufwerk, das nicht verschlüsselt sein darf. Verschlüsseln Sie Ihr Stammlaufwerk.

![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3\. Geben Sie eine Passphrase an. Dies ist die Passphrase, die Sie in KeyVault hochladen.

![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4\. Schließen Sie die Partitionierung ab.

![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5\. Beim Starten der VM werden Sie nach einer Passphrase gefragt. Verwenden Sie die Passphrase, die Sie in Schritt 3 angegeben haben.

![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6\. Bereiten Sie die VM für das Hochladen in Azure mit [dieser Anleitung](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/) vor. Führen Sie den letzten Schritt (das Aufheben der VM-Bereitstellung) noch nicht aus.

###### Konfigurieren der Verschlüsselung für Azure

1\. Erstellen Sie unter „/usr/local/sbin/azure\_crypt\_key.sh“ eine Datei mit dem Inhalt, der unten im Skript enthalten ist. Achten Sie auf KeyFileName, da dies der von Azure vergebene Passphrase-Name ist.

    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do
        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi


2\. Ändern Sie die Verschlüsselungskonfiguration in */etc/crypttab*. Diese sollte wie folgt aussehen:

    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh

3\. Wenn Sie die Datei *azure\_crypt\_key.sh* in Windows bearbeitet und nach Linux kopiert haben, sollten Sie nicht vergessen, *dos2unix /usr/local/sbin/azure\_crypt\_key.sh* auszuführen.

4\. Fügen Sie dem Skript Berechtigungen für die Ausführung hinzu:

    chmod +x /usr/local/sbin/azure_crypt_key.sh

5\. Bearbeiten Sie */etc/initramfs-tools/modules*, indem Sie Zeilen anfügen:

    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1

6\. Führen Sie `update-initramfs -u -k all` zum Aktualisieren von „initramfs“ aus, damit `keyscript` wirksam wird. 7. Nun können Sie die Bereitstellung der VM aufheben.

![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8\. Fahren Sie mit dem nächsten Schritt fort, und [laden Sie Ihre VHD](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure hoch.

##### openSUSE 13.2

###### Konfigurieren der Verschlüsselung während der Installation einer Distribution

1\. Wählen Sie beim Partitionieren von Datenträgern die Option „Encrypt Volume Group“ (Volumegruppe verschlüsseln). Geben Sie eine Passphrase an. Dies ist die Passphrase, die Sie in KeyVault hochladen.

![openSUSE 13.2-Setup](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2\. Starten Sie die VM, indem Sie Ihre Passphrase verwenden.

![openSUSE 13.2-Setup](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3\. Bereiten Sie die VM für das Hochladen in Azure mit [dieser Anleitung](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131) vor. Führen Sie den letzten Schritt (das Aufheben der VM-Bereitstellung) noch nicht aus.

###### Konfigurieren der Verschlüsselung für Azure

1\. Bearbeiten Sie die Datei „/etc/dracut.conf“, und fügen Sie die folgende Zeile hinzu:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2\. Kommentieren Sie diese Zeilen am Ende der Datei „/usr/lib/dracut/modules.d/90crypt/module-setup.sh“ aus:

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3\. Fügen Sie die folgende Zeile am Anfang der Datei „/usr/lib/dracut/modules.d/90crypt/parse-crypt.sh“ an,

    DRACUT_SYSTEMD=0

und ändern Sie alle Vorkommen von

    if [ -z "$DRACUT_SYSTEMD" ]; then

in

    if [ 1 ]; then

4\. Bearbeiten Sie die Datei „/usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh“, und fügen Sie sie nach „# Open LUKS device“ an.

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5\. Führen Sie „/usr/sbin/dracut -f -v“ aus, um initrd zu aktualisieren.

6\. Nun können Sie die Bereitstellung der VM aufheben und [Ihre VHD in Azure hochladen](#upload-encrypted-vhd-to-an-azure-storage-account).

##### CentOS 7:

###### Konfigurieren der Verschlüsselung während der Installation einer Distribution

1\. Wählen Sie beim Partitionieren von Datenträgern die Option „Encrypt my data“ (Daten verschlüsseln).

![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2\. Stellen Sie sicher, dass für die Stammpartition die Option „Encrypt“ (Verschlüsseln) ausgewählt ist.

![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3\. Geben Sie eine Passphrase an. Dies ist die Passphrase, die Sie in KeyVault hochladen.

![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4\. Beim Starten der VM werden Sie nach einer Passphrase gefragt. Verwenden Sie die Passphrase, die Sie in Schritt 3 angegeben haben.

![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5\. Bereiten Sie die VM für das Hochladen in Azure mit [dieser Anleitung](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70) vor. Führen Sie den letzten Schritt (das Aufheben der VM-Bereitstellung) noch nicht aus.

6\. Nun können Sie die Bereitstellung der VM aufheben und [Ihre VHD in Azure hochladen](#upload-encrypted-vhd-to-an-azure-storage-account).

###### Konfigurieren der Verschlüsselung für Azure

1\. Bearbeiten Sie die Datei „/etc/dracut.conf“, und fügen Sie die folgende Zeile hinzu:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2\. Kommentieren Sie diese Zeilen am Ende der Datei „/usr/lib/dracut/modules.d/90crypt/module-setup.sh“ aus:

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3\. Fügen Sie die folgende Zeile am Anfang der Datei „/usr/lib/dracut/modules.d/90crypt/parse-crypt.sh“ an,

    DRACUT_SYSTEMD=0

und ändern Sie alle Vorkommen von

    if [ -z "$DRACUT_SYSTEMD" ]; then

in

    if [ 1 ]; then

4\. Bearbeiten Sie die Datei „/usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh“, und fügen Sie sie nach „# Open LUKS device“ an.

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5\. Führen Sie „/usr/sbin/dracut -f -v“ aus, um initrd zu aktualisieren.

![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### Hochladen einer verschlüsselten VHD in ein Azure-Speicherkonto
Nachdem die BitLocker-Verschlüsselung über DM-Crypt-Verschlüsselung aktiviert wurde, muss die lokale verschlüsselte VHD in Ihr Speicherkonto hochgeladen werden.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### Hochladen des geheimen Schlüssels der Datenträgerverschlüsselung für den vorverschlüsselten virtuellen Computer in den Schlüsseltresor
Der zuvor abgerufene geheime Schlüssel der Datenträgerverschlüsselung muss als geheimer Schlüssel in den Schlüsseltresor hochgeladen werden. Der Schlüsseltresor muss über aktivierte Berechtigungen für Ihren AAD-Client und für die Datenträgerverschlüsselung verfügen.


    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $KeyVault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### Geheimer Schlüssel der Datenträgerverschlüsselung ohne Verschlüsselung per KEK
Verwenden Sie [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) , um den geheimen Schlüssel im Schlüsseltresor bereitzustellen. Im Falle eines virtuellen Windows-Computers wird die BEK-Datei als base64-Zeichenfolge codiert und dann mit dem Set-AzureKeyVaultSecret-Cmdlet in den Schlüsseltresor hochgeladen. Für Linux wird die Passphrase als base64-Zeichenfolge codiert und dann in den Schlüsseltresor hochgeladen. Stellen Sie außerdem sicher, dass die folgenden Tags festgelegt sind, während der geheime Schlüssel im Schlüsseltresor erstellt wird.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

`$secretUrl` wird im nächsten Schritt zum [Anfügen des Betriebssystemdatenträgers ohne KEK](#without-using-a-kek) verwendet.

#### Geheimer Schlüssel der Datenträgerverschlüsselung mit Verschlüsselung per KEK

Der geheime Schlüssel kann optional mit einem Schlüsselverschlüsselungsschlüssel verschlüsselt werden, bevor er in den Schlüsseltresor hochgeladen wird. Verwenden Sie die Wrapper-[API](https://msdn.microsoft.com/library/azure/dn878066.aspx), um den geheimen Schlüssel zuerst mit dem Schlüsselverschlüsselungsschlüssel zu verschlüsseln. Die Ausgabe dieses Wrapper-Vorgangs ist eine base64-codierte Zeichenfolge, die dann als geheimer Schlüssel mit dem [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx)-Cmdlet hochgeladen wird.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

`$KeyEncryptionKey` und `$secretUrl` werden im nächsten Schritt zum [Anfügen des Betriebssystemdatenträgers mit KEK](#using-a-kek) verwendet.

### Angeben der URL des geheimen Schlüssels beim Anfügen des Betriebssystemdatenträgers

#### Ohne KEK

Beim Anfügen des Betriebssystemdatenträgers muss `$secretUrl` übergeben werden. Die URL wurde im Abschnitt [„Geheimer Schlüssel der Datenträgerverschlüsselung ohne Verschlüsselung per KEK“](#disk-encryption-secret-not-encrypted-with-a-kek) generiert.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### Mit KEK

Beim Anfügen des Betriebssystemdatenträgers müssen `$KeyEncryptionKey` und `$secretUrl` übergeben werden. Die URL wurde im Abschnitt [„Geheimer Schlüssel der Datenträgerverschlüsselung mit Verschlüsselung per KEK“](#disk-encryption-secret-encrypted-with-a-kek) generiert.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## Diese Anleitung herunterladen
Sie können diese Anleitung aus dem [TechNet-Katalog](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) herunterladen.


## Weitere Informationen
[Explore Azure Disk Encryption with Azure PowerShell](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)

[Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)

<!---HONumber=AcomDC_0928_2016-->