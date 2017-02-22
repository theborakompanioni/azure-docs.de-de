---
title: "Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer | Microsoft-Dokumentation"
description: "Dieser Artikel bietet eine Übersicht über Microsoft Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: kakhan
translationtype: Human Translation
ms.sourcegitcommit: d86c39ca96633fc84311a4843deee997df9305a6
ms.openlocfilehash: 692ee9272224369b8d5f892160bc92d5a30c346c


---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer
Microsoft Azure setzt alles daran, den Schutz Ihrer Daten sowie die Datenhoheit sicherzustellen. Für Ihre unter Azure gehosteten Daten können Sie eine Reihe von modernen Technologien zum Verschlüsseln, Steuern und Verwalten von Verschlüsselungsschlüsseln sowie zum Steuern und Überwachen des Datenzugriffs nutzen. Dank dieser Steuerelemente können Sie flexibel die Lösung auswählen, die Ihre Geschäftsanforderungen am besten erfüllt.

In diesem Artikel stellen wir Ihnen die neue Azure Disk Encryption-Technologie für virtuelle Windows- und Linux-IaaS-Computer vor, die Sie beim Schutz und bei der Absicherung Ihrer Daten unterstützt. Azure Disk Encryption wurde konzipiert, um Ihre Vorgaben in den Bereichen Unternehmenssicherheit und Compliance zu erfüllen. Der Artikel enthält eine ausführliche Anleitung zur Verwendung der Azure Disk Encryption-Features, einschließlich der unterstützten Szenarien und Benutzeroberflächen.

> [!NOTE]
> Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung, was zusätzliche Lizenz- oder Abonnementkosten nach sich ziehen kann.

## <a name="overview"></a>Übersicht
Azure Disk Encryption ist eine neue Funktion, mit der Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln können. Diese Funktion verwendet das Branchen-Standardfeature [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) von Windows und das Feature [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, damit Sie die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement steuern und verwalten können. Diese Lösung stellt außerdem sicher, dass alle ruhenden Daten auf den Datenträgern der virtuellen Computer in Azure Storage verschlüsselt sind.

Azure Disk Encryption für Windows- und Linux-IaaS-VMs ist jetzt in allen öffentlichen Azure-Regionen für VMs mit Storage Standard- und Storage Premium-Konten allgemein verfügbar.

### <a name="encryption-scenarios"></a>Verschlüsselungsszenarien
Die Azure Disk Encryption-Lösung unterstützt die folgenden Kundenszenarien:

* Aktivieren der Verschlüsselung auf neuen virtuellen IaaS-Computern, die mit einer vorverschlüsselten virtuellen Festplatte (Virtual Hard Disk, VHD) und Verschlüsselungsschlüsseln erstellt werden
* Aktivieren der Verschlüsselung auf neuen IaaS-VMs, die über Azure Marketplace-Images erstellt wurden
* Aktivieren der Verschlüsselung auf vorhandenen IaaS-VMs, die unter Azure ausgeführt werden
* Deaktivieren der Verschlüsselung auf Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs

Die Lösung unterstützt die folgenden Szenarien für virtuelle IaaS-Computer, wenn sie in Microsoft Azure aktiviert sind:

* Integration in Azure Key Vault
* VMs im Standard-Tarif: [IaaS-VMs der Serien A, D, DS, G, GS usw.](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Aktivieren der Verschlüsselung auf virtuellen Windows- und Linux-IaaS-Computern
* Deaktivieren der Verschlüsselung auf Betriebssystem- und Datenlaufwerken für Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung auf Datenlaufwerken für Linux-IaaS-VMs
* Aktivieren der Verschlüsselung auf IaaS-VMs mit dem Windows-Clientbetriebssystem
* Aktivieren der Verschlüsselung auf Volumes mit Bereitstellungspfaden
* Aktivieren der Verschlüsselung auf Linux-VMs, die mithilfe von mdadm mit Datenträgerstriping (RAID) konfiguriert sind
* Aktivieren der Verschlüsselung auf Linux-VMs mit LVM für Datenträger
* Aktivieren der Verschlüsselung auf virtuellen Windows-Computern, die mithilfe von Speicherplätzen konfiguriert sind
* Alle öffentlichen Azure-Regionen werden unterstützt.

Die Lösung unterstützt nicht die folgenden Szenarien, Features und Technologien:

* IaaS-VMs im Basic-Tarif
* Deaktivieren der Verschlüsselung auf Betriebssystemlaufwerken für Linux-IaaS-VMs
* IaaS-VMs, die mithilfe der klassischen Methode zum Erstellen von virtuellen Computern erstellt werden
* Integration in den lokalen Schlüsselverwaltungsdienst
* Azure Files (freigegebenes Dateisystem), Netzwerkdateisystem (NFS), dynamische Volumes und Windows-VMs, die mit softwarebasierten RAID-Systemen konfiguriert sind

### <a name="encryption-features"></a>Verschlüsselungsfunktionen
Beim Aktivieren und Bereitstellen von Azure Disk Encryption für virtuelle Azure-IaaS-Computer werden je nach bereitgestellter Konfiguration die folgenden Funktionen aktiviert:

* Verschlüsselung des Betriebssystemvolumes zum Schützen des Startvolumes im Ruhezustand in Ihrem Speicher
* Verschlüsselung des Datenvolumes zum Schützen des Datenvolumes im Ruhezustand in Ihrem Speicher
* Deaktivieren der Verschlüsselung auf den Betriebssystem- und Datenlaufwerken für Windows-IaaS-VMs
* Deaktivieren der Verschlüsselung auf den Datenlaufwerken für Linux-IaaS-VMs
* Schutz der Verschlüsselungsschlüssel und Geheimnisse in Ihrem Key Vault-Abonnement
* Meldung des Verschlüsselungsstatus des verschlüsselten virtuellen IaaS-Computers
* Entfernen der Konfigurationseinstellungen für die Datenträgerverschlüsselung vom virtuellen IaaS-Computer
* Sichern und Wiederherstellen verschlüsselter virtueller Computer mit dem Azure Backup-Dienst

> [!NOTE]
> Das Sichern und Wiederherstellen verschlüsselter virtueller Computer wird nur für virtuelle Computer unterstützt, die mit der KEK-Konfiguration (Key Encryption Key, Schlüsselverschlüsselungsschlüssel) verschlüsselt sind. Für virtuelle Computer ohne KEK-Verschlüsselung wird es nicht unterstützt. KEK ist ein optionaler Parameter zum Aktivieren der Verschlüsselung des virtuellen Computers.

Azure Disk Encryption für IaaS-VMs für die Windows- und Linux-Lösung beinhaltet:

* Die Datenträgerverschlüsselungs-Erweiterung für Windows.
* Die Datenträgerverschlüsselungs-Erweiterung für Linux.
* Die PowerShell-Cmdlets zur Datenträgerverschlüsselung.
* Die Azure-Befehlszeilenschnittstellen-Cmdlets (Command Line Interface, CLI) zur Datenträgerverschlüsselung.
* Die Azure Resource Manager-Vorlagen zur Datenträgerverschlüsselung.

Die Azure Disk Encryption-Lösung wird auf virtuellen IaaS-Computern unterstützt, die unter dem Windows- oder Linux-Betriebssystem ausgeführt werden. Weitere Informationen zu den unterstützten Betriebssystemen finden Sie im Abschnitt „Voraussetzungen“.

> [!NOTE]
> Für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption fallen keine zusätzlichen Gebühren an.

### <a name="value-proposition"></a>Wertversprechen
Mit der Azure Disk Encryption-Verwaltungslösung können Sie die folgenden geschäftlichen Anforderungen erfüllen:

* Virtuelle IaaS-Computer werden im Ruhezustand geschützt, weil Sie Verschlüsselungstechnologie nach Branchenstandard einsetzen können, um die Anforderungen an Unternehmenssicherheit und Compliance zu erfüllen.
* Virtuelle IaaS-Computer werden mit vom Kunden gesteuerten Schlüsseln und Richtlinien gestartet, und Sie können ihre Nutzung in Ihrem Schlüsseltresor überwachen.

### <a name="encryption-workflow"></a>Verschlüsselungsworkflow
Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer zu aktivieren:

1. Wählen Sie aus den vorangehenden Szenarien für die Verschlüsselung ein Verschlüsselungsszenario aus.
2. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder einen CLI-Befehl, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einer neuen IaaS-VM an.
   * Geben Sie die Verschlüsselungskonfiguration für die über den Marketplace neu erstellten virtuellen Computer und für die vorhandenen, bereits in Azure ausgeführten virtuellen Computer an, um die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

3. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf dem virtuellen IaaS-Computer zu aktivieren.

4. Geben Sie die Identität der Azure Active Directory-Anwendung (Azure AD) an, um das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor zu schreiben. So wird die Verschlüsselung auf der IaaS-VM für die in Schritt 2 erwähnten Szenarien aktiviert.

5. Azure aktiviert das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor und richtet Ihren verschlüsselten virtuellen Computer ein.

 ![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Entschlüsselungsworkflow
Um die Datenträgerverschlüsselung für virtuelle IaaS-Computer zu deaktivieren, führen Sie die folgenden allgemeinen Schritte aus:

1. Aktivieren Sie die Verschlüsselung (Entschlüsselung) auf einem ausgeführten virtuellen IaaS-Computer in Azure über die Azure Disk Encryption-Resource Manager-Vorlage oder mithilfe von PowerShell-Cmdlets, und geben Sie die Entschlüsselungskonfiguration an.

 Dieser Schritt deaktiviert die Verschlüsselung des Betriebssystemdatenträgers und/oder Datenträgers für Daten in der ausgeführten Windows-IaaS-VM. Wie jedoch im vorherigen Abschnitt erwähnt, wird die Deaktivierung der Verschlüsselung des Betriebssystemdatenträgers für Linux nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf Linux-VMs zulässig.
2. Azure aktualisiert das VM-Dienstmodell, und die IaaS-VM wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt.

 Durch den Vorgang zum Deaktivieren der Verschlüsselung werden Ihr Schlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme oder Passphrase für Linux) nicht gelöscht.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie Azure Disk Encryption auf virtuellen Azure-IaaS-Computern für die unterstützten Szenarien aktivieren, die im Abschnitt „Übersicht“ erörtert wurden, beachten Sie die folgenden Voraussetzungen:

* Sie müssen über ein gültiges aktives Azure-Abonnement verfügen, um in den unterstützten Regionen Ressourcen in Azure zu erstellen.
* Azure Disk Encryption wird für die folgenden Windows Server-Versionen unterstützt: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016.
* Azure Disk Encryption wird für die folgenden Windows-Clientversionen unterstützt: Windows 8-Client und Windows 10-Client.

> [!NOTE]
> Für Windows Server 2008 R2 müssen Sie .NET Framework 4.5 installieren, bevor Sie die Verschlüsselung in Azure aktivieren. Sie können die Installation über Windows Update durchführen, indem Sie das optionale Update Microsoft .NET Framework 4.5.2 für Windows Server 2008 R2 x64-basierte Systeme ([KB2901983](https://support.microsoft.com/kb/2901983)) installieren.
>
> Azure Disk Encryption wird für die folgenden Linux-Server-Versionen unterstützt: Ubuntu, CentOS, SUSE und SUSE Linux Enterprise Server (SLES) sowie Red Hat Enterprise Linux.

> [!NOTE]
> Die Verschlüsselung von Linux-Betriebssystemdatenträgern wird derzeit für die folgenden Linux-Distributionen unterstützt: RHEL 7.2, CentOS 7.2n und Ubuntu 16.04.
>
> Alle Ressourcen (z.B. Ihr Schlüsseltresor, Ihr Speicherkonto, Ihre VM) müssen derselben Azure-Region und demselben Abonnement angehören.

> [!NOTE]
> Azure Disk Encryption setzt voraus, dass sich Ihr Schlüsseltresor und die virtuellen Computer in derselben Azure-Region befinden. Bei einer Einrichtung in unterschiedlichen Regionen kann Azure Disk Encryption nicht aktiviert werden.

* Informationen zum Einrichten und Konfigurieren Ihres Schlüsseltresors für Azure Disk Encryption finden Sie im Anhang unter „Einrichten und Konfigurieren Ihres Schlüsseltresors für Azure Disk Encryption“.
* Informationen zum Einrichten und Konfigurieren der Azure AD-Anwendung in Azure Active Directory für Azure Disk Encryption finden Sie im Abschnitt „Voraussetzungen“ unter „Einrichten der Azure AD-Anwendung in Azure Active Directory“.
* Informationen zum Einrichten und Konfigurieren der Richtlinie für den Zugriff auf den Schlüsseltresor für die Azure AD-Anwendung finden Sie im Abschnitt „Voraussetzungen“ unter „Einrichten der Richtlinie für den Zugriff auf den Schlüsseltresor für die Azure AD-Anwendung“.
* Informationen zum Vorbereiten einer vorverschlüsselten Windows-VHD finden Sie im Anhang unter „Vorbereiten einer vorverschlüsselten Windows-VHD“.
* Informationen zum Vorbereiten einer vorverschlüsselten Linux-VHD finden Sie im Anhang unter „Vorbereiten einer vorverschlüsselten Linux-VHD“.
* Die Azure-Plattform benötigt Zugriff auf die Verschlüsselungsschlüssel oder Geheimnisse in Ihrem Schlüsseltresor, um sie für den virtuellen Computer zur Verfügung zu stellen, wenn er startet und das Betriebssystemvolume entschlüsselt. Um Berechtigungen für die Azure-Plattform zu erteilen, legen Sie die **EnabledForDiskEncryption**-Eigenschaft im Schlüsseltresor fest. Weitere Informationen finden Sie im Anhang unter „Einrichten und Konfigurieren Ihres Schlüsseltresors für Azure Disk Encryption“.
* Ihre URLs für das Geheimnis des Schlüsseltresors und den Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) müssen mit einer Versionsangabe versehen sein. Azure erzwingt diese Einschränkung der Versionsverwaltung. Gültige URLs für Geheimnisse und KEKs finden Sie in den folgenden Beispielen:

  * Beispiel einer gültigen URL für einen geheimen Schlüssel:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Beispiel einer gültigen KEK-URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption unterstützt nicht die Angabe von Portnummern als Teil von URLs für Schlüsseltresorgeheimnisse und KEK-URLs. Beispiele für nicht unterstützte und unterstützte Schlüsseltresor-URLs finden Sie hier:

  * Nicht akzeptierte Schlüsseltresor-URL:  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Akzeptierte Schlüsseltresor-URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Die virtuellen IaaS-Computer müssen die folgenden Anforderungen an die Netzwerkendpunktkonfiguration erfüllen, um Azure Disk Encryption zu aktivieren:

  * Um ein Token für die Verbindungsherstellung mit Ihrem Schlüsseltresor zu erhalten, muss der virtuelle IaaS-Computer eine Verbindung mit dem Azure Active Directory-Endpunkt \[Login.windows.net\] herstellen können.
  * Um die Verschlüsselungsschlüssel in Ihren Schlüsseltresor schreiben zu können, muss der virtuelle IaaS-Computer eine Verbindung mit dem Schlüsseltresorendpunkt herstellen können.
  * Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure-Speicherendpunkt herstellen können, an dem das Azure-Erweiterungsrepository gehostet wird, sowie mit einem Azure Storage-Konto, das die VHD-Dateien hostet.

  > [!NOTE]
  > Wenn Ihre Sicherheitsrichtlinie den Zugriff von virtuellen Azure-Computern auf das Internet beschränkt, können Sie den obigen URI auflösen und eine spezielle Regel konfigurieren, um ausgehende Verbindungen mit den IP-Adressen zuzulassen.
  >
  > Um Azure Disk Encryption zu konfigurieren, laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter.

 > [!NOTE]
  > Azure Disk Encryption wird unter [Azure PowerShell SDK Version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016) nicht unterstützt. Wenn Sie im Zusammenhang mit der Verwendung von Azure PowerShell 1.1.0 eine Fehlermeldung erhalten, nutzen Sie die Informationen unter [Azure Disk Encryption Error Related to Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx) (Azure Disk Encryption-Fehler im Zusammenhang mit Azure PowerShell 1.1.0).

* Sie müssen zuerst die richtige Azure-CLI-Version installieren, um Azure-CLI-Befehle ausführen und die Zuordnung zum Azure-Abonnement durchführen zu können:
  * Um die Azure-CLI zu installieren und Ihrem Azure-Abonnement zuzuordnen, lesen Sie im Abschnitt zum [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md).
  * Informationen zur Verwendung der Azure-CLI für Mac, Linux und Windows mit Azure Resource Manager finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](../virtual-machines/azure-cli-arm-commands.md).
* Die Azure Disk Encryption-Lösung verwendet für virtuelle Windows-IaaS-Computer die externe BitLocker-Schlüsselschutzvorrichtung. Wenn Ihre virtuellen Computer der Domäne beigetreten sind, sollten Sie keine Gruppenrichtlinien nutzen, mit denen TPM-Schutzvorrichtungen durchgesetzt werden. Informationen zur Gruppenrichtlinie „BitLocker ohne kompatibles TPM zulassen“ finden Sie unter [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521) (Referenz zur BitLocker-Gruppenrichtlinie).
* Informationen zum Erstellen einer Azure AD-Anwendung, zum Erstellen eines neuen Schlüsseltresors oder zum Einrichten eines vorhandenen Schlüsseltresors und Aktivieren der Verschlüsselung finden Sie auf der Seite zum für [Azure Disk Encryption erforderlichen PowerShell-Skript](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* Um die Datenträgerverschlüsselung mithilfe der Azure-CLI zu konfigurieren, benötigen Sie [dieses Bash-Skript](https://github.com/ejarvi/ade-cli-getting-started).
* Wenn Sie mit Azure Disk Encryption verschlüsselte virtuelle Computer mit dem Azure Backup-Dienst sichern und wiederherstellen möchten, verschlüsseln Sie Ihre VMs mithilfe der Azure Disk Encryption-Schlüsselkonfiguration. Der Azure Backup-Dienst unterstützt nur virtuelle Computer, die mit der KEK-Konfiguration verschlüsselt sind. Der Dienst unterstützt nur mit KEK verschlüsselte VMs. Nachfolgend finden Sie Verschlüsselungsbereitstellungsszenarien zur Aktivierung der Verschlüsselung virtueller Computer mit der KEK-Option.

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a>Einrichten der Azure AD-Anwendung in Azure Active Directory
Wenn Sie die Verschlüsselung auf einem ausgeführten virtuellen Computer in Azure aktivieren müssen, werden die Verschlüsselungsschlüssel von Azure Disk Encryption generiert und in Ihren Schlüsseltresor geschrieben. Das Verwalten von Verschlüsselungsschlüsseln im Schlüsseltresor erfordert eine Azure AD-Authentifizierung.

Erstellen Sie zu diesem Zweck eine Azure AD-Anwendung. Ausführliche Schritte zum Registrieren einer Anwendung finden Sie im Abschnitt „Get an Identity for the Application“ (Abrufen einer Identität für die Anwendung) im Blogbeitrag [ Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) (Azure Key Vault – Schritt für Schritt). Der Beitrag enthält außerdem einige hilfreiche Beispiele zum Einrichten und Konfigurieren Ihres Schlüsseltresors. Zu Authentifizierungszwecken können Sie entweder die auf einem Clientgeheimnis basierende Authentifizierung oder die auf einem Clientzertifikat basierende Azure AD-Authentifizierung verwenden.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Auf Clientgeheimnis basierende Authentifizierung für Azure AD
In den folgenden Abschnitten sind die erforderlichen Schritte zum Konfigurieren einer Authentifizierung für Azure AD angegeben, die auf einem Clientgeheimnis basiert.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Erstellen einer Azure AD-Anwendung mit Azure PowerShell
Verwenden Sie das folgende PowerShell-Cmdlet, um eine Azure AD-Anwendung zu erstellen:

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> „$azureAdApplication.ApplicationId“ ist die Azure AD-Client-ID, und „$aadClientSecret“ ist das Clientgeheimnis. Sie verwenden diese Angaben später zum Aktivieren von Azure Disk Encryption. Sorgen Sie für eine sichere Aufbewahrung des Azure AD-Clientgeheimnisses.

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-classic-portal"></a>Einrichten der Azure AD-Client-ID und des Geheimnisses über das klassische Portal
Sie können die Azure AD-Client-ID und das zugehörige Geheimnis auch im [klassischen Azure-Portal]( https://manage.windowsazure.com) einrichten. Führen Sie hierzu die folgenden Schritte aus:

1. Klicken Sie auf die Registerkarte **Active Directory**.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klicken Sie auf **Anwendung hinzufügen**, und geben Sie den Anwendungsnamen ein.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klicken Sie auf die Pfeilschaltfläche, und konfigurieren Sie die Anwendungseigenschaften.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klicken Sie unten links auf das Häkchen, um den Vorgang abzuschließen. Die Seite für die Anwendungskonfiguration wird geöffnet, auf der die Azure AD-Client-ID unten links angezeigt wird.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. Speichern Sie das Azure AD-Clientgeheimnis, indem Sie auf die Schaltfläche **Speichern** klicken. Das Azure AD-Clientgeheimnis wird im Textfeld für die Schlüssel angezeigt. Sorgen Sie für eine sichere Aufbewahrung.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > Der obige Ablauf wird im klassischen Azure-Portal nicht unterstützt.

##### <a name="use-an-existing-application"></a>Verwenden einer vorhandenen Anwendung
Um die folgenden Befehle auszuführen, benötigen Sie das [Azure AD PowerShell-Modul](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> Die nachstehenden Befehle müssen über ein neues PowerShell-Fenster ausgeführt werden. Verwenden Sie nicht Azure PowerShell oder den Azure Resource Manager, um die Befehle auszuführen. Dieser Ansatz wird empfohlen, weil sich diese Cmdlets im MSOnline-Modul oder in Azure AD PowerShell befinden.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Zertifikatbasierte Authentifizierung für Azure AD
> [!NOTE]
> Die zertifikatbasierte Azure AD-Authentifizierung wird auf Linux-VMs aktuell nicht unterstützt.

In den folgenden Abschnitten wird gezeigt, wie Sie die zertifikatbasierten Authentifizierung für Azure AD konfigurieren.

##### <a name="create-an-azure-ad-application"></a>Erstellen einer Azure AD-Anwendung
Führen Sie das folgende PowerShell-Cmdlet aus, um eine Azure AD-Anwendung zu erstellen:

> [!NOTE]
> Ersetzen Sie die Zeichenfolge `yourpassword` durch Ihr sicheres Kennwort, und schützen Sie das Kennwort.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Laden Sie nach Abschluss dieses Schritts eine PFX-Datei in den Schlüsseltresor hoch, und aktivieren Sie die Zugriffsrichtlinie, die zum Bereitstellen dieses Zertifikats auf einem virtuellen Computer benötigt wird.

##### <a name="use-an-existing-azure-ad-application"></a>Verwenden einer vorhandenen Azure AD-Anwendung
Verwenden Sie die hier gezeigten PowerShell-Cmdlets, wenn Sie die zertifikatbasierte Authentifizierung für eine vorhandene Anwendung konfigurieren. Achten Sie darauf, dass Sie die Cmdlets in einem neuen PowerShell-Fenster ausführen.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Laden Sie nach Abschluss dieses Schritts eine PFX-Datei in den Schlüsseltresor hoch, und aktivieren Sie die Zugriffsrichtlinie, die zum Bereitstellen dieses Zertifikats auf einem virtuellen Computer benötigt wird.

##### <a name="upload-a-pfx-file-to-your-key-vault"></a>Hochladen einer PFX-Datei in Ihren Schlüsseltresor
Eine ausführliche Erläuterung zu diesem Vorgang finden Sie im [offiziellen Azure Key Vault-Teamblog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Für diese Aufgabe benötigen Sie nur die folgenden PowerShell-Cmdlets. Stellen Sie sicher, dass Sie sie über die Azure PowerShell-Konsole ausführen.

> [!NOTE]
> Ersetzen Sie die Zeichenfolge `yourpassword` durch Ihr sicheres Kennwort, und schützen Sie das Kennwort.

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

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a>Bereitstellen eines Zertifikats im Schlüsseltresor auf einem vorhandenen virtuellen Computer
Nach dem Hochladen der PFX-Datei stellen Sie über die folgenden Befehle ein Zertifikat im Schlüsseltresor einer vorhandenen VM bereit:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a>Einrichten der Richtlinie für den Zugriff auf den Schlüsseltresor für die Azure AD-Anwendung
Ihre Azure AD-Anwendung benötigt Rechte zum Zugreifen auf die Schlüssel oder geheimen Schlüssel im Tresor. Verwenden Sie das Cmdlet [`Set-AzureKeyVaultAccessPolicy`](https://msdn.microsoft.com/library/azure/dn903607.aspx) zum Gewähren von Berechtigungen für die Anwendung, und verwenden Sie hierbei die Client-ID (die während der Registrierung der Anwendung generiert wurde) als _–ServicePrincipalName_-Parameterwert. Weitere Informationen finden Sie im Blogbeitrag [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) (Azure Key Vault – Schritt für Schritt). Hier sehen Sie ein Beispiel für die Durchführung dieser Aufgabe über PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk Encryption erfordert das Konfigurieren der folgenden Zugriffsrichtlinien für Ihre Azure AD-Clientanwendung: Berechtigungen _WrapKey_ und _Set_.

## <a name="terminology"></a>Terminologie
Verwenden Sie die folgende Terminologietabelle, um sich mit einigen allgemeinen Begriffen im Rahmen dieser Technologie vertraut zu machen:

| Begriff | Definition |
| --- | --- |
| Azure AD | Azure AD steht für [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Ein Azure AD-Konto ist eine Voraussetzung zum Authentifizieren, Speichern und Abrufen von Geheimnissen aus einem Schlüsseltresor. |
| Azure Key Vault | Azure Key Vault ist ein Dienst zum Verwalten kryptografischer Schlüssel, der auf Hardwaresicherheitsmodulen mit FIPS-Überprüfung (Federal Information Processing Standards) basiert, damit Ihre kryptografischen Schlüssel und Geheimnisse sicher und geschützt sind. Weitere Informationen finden Sie in der [Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| ARM | Azure Resource Manager |
| BitLocker |Bei [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) handelt es sich um eine branchenübliche Verschlüsselungstechnologie für Windows-Volumes, um die Datenträgerverschlüsselung auf virtuellen Windows-IaaS-Computern zu aktivieren. |
| BEK | BitLocker-Verschlüsselungsschlüssel (BitLocker Encryption Keys) werden verwendet, um das Startvolume des Betriebssystems und Datenvolumes zu verschlüsseln. Die BitLocker-Schlüssel sind in einem Schlüsseltresor als Geheimnisse geschützt. |
| CLI | Siehe [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) ist das Linux-basierte transparente Subsystem für die Datenträgerverschlüsselung, das zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Linux-IaaS-Computern verwendet wird. |
| KEK | Der Schlüsselverschlüsselungsschlüssel (Key Encryption Key) ist der asymmetrische Schlüssel (RSA 2048), der zum Schützen oder Umschließen des Geheimnisses verwendet wird. Sie können einen mit Hardwaresicherheitsmodulen geschützten Schlüssel oder einen Schlüssel mit Softwareschutz bereitstellen. Weitere Informationen finden Sie in der Dokumentation zu [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| PS-Cmdlets | Siehe [Azure PowerShell-Cmdlets](/powershell/azureps-cmdlets-docs). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Einrichten und Konfigurieren Ihres Schlüsseltresors für Azure Disk Encryption
Mit Azure Disk Encryption werden die Schlüssel und Geheimnisse in Ihrem Azure-Schlüsseltresor geschützt. Führen Sie die Schritte in jedem der Abschnitte unten aus, um den Schlüsseltresor für Azure Disk Encryption einzurichten.

#### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Verwenden Sie eine der folgenden Optionen, um einen Schlüsseltresor zu erstellen:

* [Resource Manager-Vorlage „101-Create-KeyVault“](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json)
* [Azure PowerShell-Cmdlets für den Schlüsseltresor](https://msdn.microsoft.com/library/dn868052.aspx)
* Azure Resource Manager

> [!NOTE]
> Fahren Sie mit dem nächsten Abschnitt fort, falls Sie bereits einen Schlüsseltresor für Ihr Abonnement eingerichtet haben.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Einrichten eines Schlüsselverschlüsselungsschlüssels (optional)
Wenn Sie als zusätzliche Sicherheitsmaßnahme einen KEK für die BitLocker-Verschlüsselungsschlüssel verwenden möchten, sollten Sie Ihrem Schlüsseltresor einen KEK hinzufügen. Verwenden Sie das Cmdlet [`Add-AzureKeyVaultKey`](https://msdn.microsoft.com/library/dn868048.aspx), um einen KEK im Schlüsseltresor zu erstellen. Sie können den KEK auch aus Ihrem lokalen Hardwaresicherheitsmodul (HSM) für die Schlüsselverwaltung importieren. Weitere Informationen finden Sie in der [Dokumentation zu Key Vault](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Der KEK kann aus dem Azure Resource Manager oder über die Key Vault-Benutzeroberfläche hinzugefügt werden.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Festlegen von Schlüsseltresorberechtigungen
Die Azure-Plattform benötigt Zugriff auf die Verschlüsselungsschlüssel oder geheimen Schlüssel in Ihrem Schlüsseltresor, um sie für den virtuellen Computer zur Verfügung zu stellen, damit die Volumes gestartet und entschlüsselt werden können. Um der Azure-Plattform Berechtigungen zu gewähren, muss mit dem Key Vault-PowerShell-Cmdlet die **EnabledForDiskEncryption**-Eigenschaft für Ihren Schlüsseltresor festgelegt werden:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Sie können die **EnabledForDiskEncryption**-Eigenschaft auch über den [Azure Resource Explorer](https://resources.azure.com) festlegen.

Wie oben erwähnt, müssen Sie die **EnabledForDiskEncryption**-Eigenschaft für Ihren Schlüsseltresor festlegen. Andernfalls funktioniert die Bereitstellung nicht.

Sie können die Zugriffsrichtlinien für Ihre Azure AD-Anwendung über die Benutzeroberfläche des Schlüsseltresors einrichten, wie hier gezeigt:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Stellen Sie sicher, dass der Schlüsseltresor für die Datenträgerverschlüsselung unter **Erweiterte Zugriffsrichtlinien** aktiviert ist:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Bereitstellungsszenarien und Benutzeroberflächen für die Datenträgerverschlüsselung
Es gibt viele Szenarien für die Aktivierung der Datenträgerverschlüsselung. Die Schritte können je nach Szenario variieren. In den folgenden Abschnitten werden diese Szenarien ausführlicher beschrieben.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a>Aktivieren der Verschlüsselung auf neuen, über den Marketplace erstellten IaaS-VMs
Die Datenträgerverschlüsselung kann auf neuen virtuellen IaaS-Windows-VMs aus dem Azure Marketplace aktiviert werden, indem die [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) genutzt wird.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **In Azure bereitstellen**, geben Sie die Verschlüsselungskonfiguration auf dem Blatt **Parameter** ein, und klicken Sie auf **OK**.

2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Erstellen**, um die Verschlüsselung auf einem neuen virtuellen IaaS-Computer zu aktivieren.

> [!NOTE]
> Mit dieser Vorlage wird ein neuer verschlüsselter virtueller Windows-Computer erstellt, der das Windows Server 2012-Katalogimage verwendet.

Sie können die Datenträgerverschlüsselung auf einer neuen IaaS-RedHat Linux 7.2-VM mit einem RAID-0-Array mit 200 GB aktivieren, indem Sie [diese Resource Manager-Vorlage](https://aka.ms/fde-rhel) verwenden. Überprüfen Sie nach der Bereitstellung der Vorlage den VM-Verschlüsselungsstatus, indem Sie das `Get-AzureRmVmDiskEncryptionStatus`-Cmdlet wie unter [Verschlüsseln des Betriebssystemdatenträgers auf einer laufenden Linux-VM](#encrypting-os-drive-on-a-running-linux-vm) beschrieben verwenden. Wenn der Computer den Status _VMRestartPending_ zurückgibt, starten Sie die VM neu.

In der folgenden Tabelle sind die Parameter der Resource Manager-Vorlage für das Szenario neuer virtuelle Computer aus dem Marketplace mit Azure AD-Client-ID aufgeführt:

| Parameter | Beschreibung |
| --- | --- |
| adminUserName | Administratorbenutzername für den virtuellen Computer. |
| adminPassword | Administratorkennwort für den virtuellen Computer. |
| newStorageAccountName | Name des Speicherkontos zum Speichern von Betriebssystem- und Daten-VHDs. |
| vmSize | Die Größe des virtuellen Computers. Derzeit werden nur die standardmäßigen A-, D- und G-Serien unterstützt. |
| virtualNetworkName | Der Name des VNET, dem die VM-NIC angehören soll. |
| subnetName | Name des Subnetzes im VNET, dem die VM-NIC angehören soll. |
| AADClientID | Client-ID der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| AADClientSecret | Clientgeheimnis der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| keyVaultURL | Die URL des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden soll. Diese kann mit dem Cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI` abgerufen werden. |
| keyEncryptionKeyURL | Die URL des KEK, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird (optional). |
| keyVaultResourceGroup | Die Ressourcengruppe des Schlüsseltresors. |
| vmName | Der Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird. |

> [!NOTE]
> _KeyEncryptionKeyURL_ ist ein optionaler Parameter. Sie können Ihren eigenen KEK nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (Passphrasegeheimnis) im Schlüsseltresor zu erhöhen.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Aktivieren der Verschlüsselung auf neuen virtuellen IaaS-Computern, die mit einer vom Kunden verschlüsselten VHD und Verschlüsselungsschlüsseln erstellt werden
In diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie die Resource Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. In den folgenden Abschnitten werden die Resource Manager-Vorlage und die CLI-Befehle ausführlicher erläutert.

Befolgen Sie die Anleitung in einem dieser Abschnitte zum Vorbereiten von vorverschlüsselten Images, die in Azure verwendet werden können. Nach dem Erstellen des Images können Sie die Schritte im nächsten Abschnitt ausführen, um eine verschlüsselte Azure-VM zu erstellen.

* [Vorbereiten einer vorverschlüsselten Windows-VHD](#preparing-a-pre-encrypted-windows-vhd)
* [Vorbereiten einer vorverschlüsselten Linux-VHD](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a>Erstellen der Resource Manager-Vorlage
Sie können die Datenträgerverschlüsselung auf einer verschlüsselten VHD aktivieren, indem die [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm) nutzen.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **In Azure bereitstellen**, geben Sie die Verschlüsselungskonfiguration auf dem Blatt **Parameter** ein, und klicken Sie auf **OK**.

2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Erstellen**, um die Verschlüsselung auf dem neuen virtuellen IaaS-Computer zu aktivieren.

In der folgenden Tabelle werden die Parameter der Resource Manager-Vorlage für Ihre verschlüsselte VHD beschrieben:

| Parameter | Beschreibung |
| --- | --- |
| newStorageAccountName | Name des Speicherkontos zum Speichern des Betriebssystems auf einer verschlüsselten VHD. Dieses Speicherkonto sollte bereits in derselben Ressourcengruppe und an demselben Standort wie der virtuelle Computer erstellt worden sein. |
| osVhdUri | URI der Betriebssystem-VHD aus dem Speicherkonto. |
| osType | Betriebssystem-Produkttyp (Windows/Linux). |
| virtualNetworkName | Der Name des VNET, dem die VM-NIC angehören soll. Der Name sollte bereits in derselben Ressourcengruppe und an demselben Standort wie der virtuelle Computer erstellt worden sein. |
| subnetName | Name des Subnetzes im VNET, dem die VM-NIC angehören soll. |
| vmSize | Die Größe des virtuellen Computers. Derzeit werden nur die standardmäßigen A-, D- und G-Serien unterstützt. |
| keyVaultResourceID | Die ResourceID zum Identifizieren der Key Vault-Ressource im Azure Resource Manager. Diese kann mit dem PowerShell-Cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId` abgerufen werden. |
| keyVaultSecretUrl | Die URL des Datenträgerverschlüsselungsschlüssels, der im Schlüsseltresor eingerichtet ist. |
| keyVaultKekUrl | Die URL des KEK, der zum Verschlüsseln des generierten Datenträgerverschlüsselungsschlüssels verwendet wird. |
| vmName | Name des virtuellen IaaS-Computers. |

#### <a name="using-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Sie können die Datenträgerverschlüsselung auf einer verschlüsselten VHD aktivieren, indem das PowerShell-Cmdlet [`Set-AzureRmVMOSDisk`](https://msdn.microsoft.com/library/azure/mt603746.aspx) verwenden.  

#### <a name="using-cli-commands"></a>Verwenden von CLI-Befehlen
Führen Sie die folgenden Schritte aus, um die Datenträgerverschlüsselung für dieses Szenario mit CLI-Befehlen zu aktivieren:

1. Legen Sie Zugriffsrichtlinien in Ihrem Schlüsseltresor fest:

   * Legen Sie das Flag **EnabledForDiskEncryption** fest:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Legen Sie Berechtigungen für die Azure AD-Anwendung fest, um Geheimnisse in Ihren Schlüsseltresor zu schreiben:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Geben Sie zum Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Computer Folgendes ein:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Rufen Sie den Verschlüsselungsstatus ab:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Verwenden Sie die unten angegebenen Parameter mit dem Befehl `azure vm create`, um die Verschlüsselung auf einem neuen virtuellen Computer über Ihre verschlüsselte VHD zu aktivieren:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Windows-IaaS-Computer in Azure
In diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie die Resource Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. In den folgenden Abschnitten wird die Verschlüsselung über die Resource Manager-Vorlage und die CLI-Befehle ausführlicher erläutert.

#### <a name="using-the-resource-manager-template"></a>Erstellen der Resource Manager-Vorlage
Sie können die Datenträgerverschlüsselung auf vorhandenen oder ausgeführten virtuellen IaaS-Windows-VMs in Azure aktivieren, indem Sie die [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm) verwenden.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **In Azure bereitstellen**, geben Sie die Verschlüsselungskonfiguration auf dem Blatt **Parameter** ein, und klicken Sie auf **OK**.

2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Erstellen**, um die Verschlüsselung auf der vorhandenen oder ausgeführten virtuellen IaaS-VM zu aktivieren.

In der folgenden Tabelle werden die Parameter der Resource Manager-Vorlage für vorhandene oder ausgeführte VMs aufgeführt, die eine Azure AD-Client-ID verwenden:

| Parameter | Beschreibung |
| --- | --- |
| AADClientID | Client-ID der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| AADClientSecret | Clientgeheimnis der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| keyVaultName | Der Name des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden soll. Dieser kann mit dem Cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname` abgerufen werden. |
|  keyEncryptionKeyURL | Die URL des KEK, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dieser Parameter ist optional, wenn Sie in der Dropdownliste „UseExistingKek“ die Option **nokek** auswählen. Wenn Sie in der Dropdownliste „UseExistingKek“ die Option **kek** auswählen, müssen Sie den Wert _keyEncryptionKeyURL_ eingeben. |
| volumeType | Der Typ des Volumes, auf dem der Verschlüsselungsvorgang durchgeführt wird. Gültige Werte sind _OS_, _Data_ und _All_. |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerverschlüsselung durchgeführt wird. |
| vmName | Der Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird. |

> [!NOTE]
> _KeyEncryptionKeyURL_ ist ein optionaler Parameter. Sie können Ihren eigenen KEK nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (geheimer BitLocker-Verschlüsselungsschlüssel) im Schlüsseltresor zu erhöhen.

#### <a name="using-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Informationen dazu, wie die Verschlüsselung mit Azure Disk Encryption über PowerShell-Cmdlets aktiviert wird, finden Sie in den Blogbeiträgen [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1) und [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2).

#### <a name="using-cli-commands"></a>Verwenden von CLI-Befehlen
Führen Sie die folgenden Schritte aus, um die Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Windows-Computer in Azure mit CLI-Befehlen zu aktivieren:

1. Legen Sie Zugriffsrichtlinien im Schlüsseltresor fest:
   * Legen Sie das Flag **EnabledForDiskEncryption** fest:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Legen Sie Berechtigungen für die Azure AD-Anwendung fest, um Geheimnisse in Ihren Schlüsseltresor zu schreiben:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. Geben Sie zum Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Computer Folgendes ein:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. Rufen Sie den Verschlüsselungsstatus ab:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. Verwenden Sie die unten angegebenen Parameter mit dem Befehl `azure vm create`, um die Verschlüsselung auf einem neuen virtuellen Computer über Ihre verschlüsselte VHD zu aktivieren:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Linux-Computer in Azure
Sie können die Datenträgerverschlüsselung auf einer vorhandenen oder ausgeführten virtuellen IaaS-Linux-VM in Azure aktivieren, indem Sie die [Resource Manager-Vorlage](https://github.com/Azure/azure-quick-start-templates/tree/master/201-encrypt-running-linux-vm) verwenden.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **In Azure bereitstellen**, geben Sie die Verschlüsselungskonfiguration auf dem Blatt **Parameter** ein, und klicken Sie dann auf **OK**.

2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Erstellen**, um die Verschlüsselung auf der vorhandenen oder ausgeführten virtuellen IaaS-VM zu aktivieren.

In der folgenden Tabelle werden Parameter der Resource Manager-Vorlage für vorhandene oder ausgeführte VMs aufgeführt, die eine Azure AD-Client-ID verwenden:

| Parameter | Beschreibung |
| --- | --- |
| AADClientID | Client-ID der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| AADClientSecret | Clientgeheimnis der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| keyVaultName | Der Name des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden soll. Dieser kann mit dem Cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname` abgerufen werden. |
|  keyEncryptionKeyURL | Die URL des KEK, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dieser Parameter ist optional, wenn Sie in der Dropdownliste „UseExistingKek“ die Option **nokek** auswählen. Wenn Sie in der Dropdownliste „UseExistingKek“ die Option **kek** auswählen, müssen Sie den Wert _keyEncryptionKeyURL_ eingeben. |
| volumeType | Der Typ des Volumes, auf dem der Verschlüsselungsvorgang durchgeführt wird. Gültige unterstützte Werte sind _OS_ oder _All_ (für RHEL 7.2, CentOS 7.2 und Ubuntu 16.04) und _Data_ (für alle anderen Distributionen). |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerverschlüsselung durchgeführt wird. |
| vmName | Der Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird. |
| passPhrase | Geben Sie eine sichere Passphrase als Datenverschlüsselungsschlüssel ein. |

> [!NOTE]
> _KeyEncryptionKeyURL_ ist ein optionaler Parameter. Sie können Ihren eigenen KEK nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (Passphrasegeheimnis) im Schlüsseltresor zu erhöhen.

#### <a name="cli-commands"></a>CLI-Befehle
Sie können die Datenträgerverschlüsselung auf Ihrer verschlüsselten VHD aktivieren, indem Sie den [CLI-Befehl](../xplat-cli-install.md) installieren und verwenden. Führen Sie die folgenden Schritte aus, um die Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Linux-Computer in Azure mit CLI-Befehlen zu aktivieren:

1. Legen Sie Zugriffsrichtlinien im Schlüsseltresor fest:

 * Legen Sie das Flag **EnabledForDiskEncryption** fest:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Legen Sie Berechtigungen für die Azure AD-Anwendung fest, um Geheimnisse in Ihren Schlüsseltresor zu schreiben:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Geben Sie zum Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Computer Folgendes ein:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Rufen Sie den Verschlüsselungsstatus ab:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Verwenden Sie die unten angegebenen Parameter mit dem Befehl `azure vm create`, um die Verschlüsselung auf einem neuen virtuellen Computer über Ihre verschlüsselte VHD zu aktivieren:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a>Abrufen des Verschlüsselungsstatus eines verschlüsselten virtuellen IaaS-Computers
Sie können den Verschlüsselungsstatus über den Azure Resource Manager, über [PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt622700.aspx) oder mithilfe von CLI-Befehlen abrufen. In den folgenden Abschnitten wird erläutert, wie Sie das klassische Azure-Portal und CLI-Befehle zum Abrufen des Verschlüsselungsstatus verwenden.

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Abrufen des Verschlüsselungsstatus einer verschlüsselten Windows-VM mit dem Azure Resource Manager
Sie können den Verschlüsselungsstatus des virtuellen IaaS-Computers folgendermaßen über den Azure Resource Manager abrufen:

1. Melden Sie sich beim [klassischen Azure-Portal](https://portal.azure.com/) an, und klicken Sie im linken Bereich auf **Virtuelle Computer**, um eine Zusammenfassung der virtuellen Computer in Ihrem Abonnement anzuzeigen. Sie können die Ansicht mit den virtuellen Computern filtern, indem Sie den Abonnementnamen in der Dropdownliste **Abonnement** auswählen.

2. Klicken Sie oben auf dem Blatt **Virtuelle Computer** auf **Spalten**.

3. Wählen Sie auf dem Blatt **Spalte auswählen** die Spalte **Datenträgerverschlüsselung** aus, und klicken Sie auf **Aktualisieren**. In der Spalte „Datenträgerverschlüsselung“ sollte für jeden virtuellen Computer der Verschlüsselungsstatus _Aktiviert_ oder _Nicht aktiviert_ angezeigt werden. Dies wird in der folgenden Abbildung dargestellt:

 ![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a>Abrufen des Verschlüsselungsstatus eines verschlüsselten virtuellen IaaS-Computers (Windows/Linux) mit dem PowerShell-Cmdlet für die Datenträgerverschlüsselung
Sie können den Verschlüsselungsstatus des virtuellen IaaS-Computers über das PowerShell-Cmdlet `Get-AzureRmVMDiskEncryptionStatus` für die Datenträgerverschlüsselung abrufen. Geben Sie zum Abrufen der Verschlüsselungseinstellungen für Ihre VM Folgendes ein:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Sie können die Ausgabe von _Get-AzureRmVMDiskEncryptionStatus_ kann auf Verschlüsselungsschlüssel-URLs untersuchen.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
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

Die Werte für die Einstellungen „OSVolumeEncrypted“ und „DataVolumesEncrypted“ sind auf _Encrypted_ festgelegt. Dies bedeutet, dass beide Volumes mithilfe der Azure Disk Encryption verschlüsselt wurden. Informationen dazu, wie die Verschlüsselung mit Azure Disk Encryption über PowerShell-Cmdlets aktiviert wird, finden Sie in den Blogbeiträgen [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1) und [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2).

> [!NOTE]
> Auf Linux-VMs benötigt das Cmdlet `Get-AzureRmVMDiskEncryptionStatus` drei bis vier Minuten zum Melden des Verschlüsselungsstatus.

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a>Abrufen des Verschlüsselungsstatus des virtuellen IaaS-Computers über den CLI-Befehl für die Datenträgerverschlüsselung
Sie können den Verschlüsselungsstatus des virtuellen IaaS-Computers über den CLI-Befehl `azure vm show-disk-encryption-status` für die Datenträgerverschlüsselung abrufen. Geben Sie Ihre Azure-CLI-Sitzung ein, um die Verschlüsselungseinstellungen für Ihren virtuellen Computer abzurufen:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Deaktivieren der Verschlüsselung auf ausgeführten Windows-IaaS-VMs
Sie können die Verschlüsselung auf einem ausgeführten virtuellen Windows- oder Linux-IaaS-Computer über die Azure Disk Encryption-Resource Manager-Vorlage oder über PowerShell-Cmdlets deaktivieren und die Entschlüsselungskonfiguration angeben.

##### <a name="windows-vm"></a>Windows-VM
Durch den Schritt zum Deaktivieren der Verschlüsselung wird die Verschlüsselung des Betriebssystemdatenträgers und/oder des Datenträgers für Daten in der ausgeführten Windows-IaaS-VM deaktiviert. Sie können das Betriebssystemvolume nicht deaktivieren und das Datenvolume verschlüsselt lassen. Wenn der Verschlüsselungsschritt ausgeführt wird, aktualisiert das klassische Azure-Bereitstellungsmodell das VM-Dienstmodell, und die Windows-IaaS-VM wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt. Durch das Entschlüsseln werden Ihr Schlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme oder Passphrase für Linux) nicht gelöscht.

##### <a name="linux-vm"></a>Linux-VM
Durch den Schritt zum Deaktivieren der Verschlüsselung wird die Verschlüsselung des Datenträgers für Daten auf der ausgeführten Linux-IaaS-VM deaktiviert.

> [!NOTE]
> Das Deaktivieren der Verschlüsselung auf dem Betriebssystemdatenträger ist auf virtuellen Linux-Computern nicht zulässig.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Deaktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Computer
Die Datenträgerverschlüsselung kann mithilfe der [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) auf einer ausgeführten Windows-IaaS-VM deaktiviert werden.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **In Azure bereitstellen**, geben Sie die Entschlüsselungskonfiguration auf dem Blatt **Parameter** ein, und klicken Sie auf **OK**.

2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Erstellen**, um die Verschlüsselung auf einem neuen virtuellen IaaS-Computer zu aktivieren.

Für die Linux-VM kann die [Vorlage zum Deaktivieren der Verschlüsselung auf einer ausgeführten Linux-VM](https://aka.ms/decrypt-linuxvm) verwendet werden.

In der folgenden Tabelle werden die Resource Manager-Vorlagenparameter für das Deaktivieren der Verschlüsselung auf einer ausgeführten IaaS-VM aufgeführt:

| Parameter | Beschreibung |
| --- | --- |
| vmName | Der Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird.
| volumeType | Der Typ des Volumes, auf dem der Entschlüsselungsvorgang durchgeführt wird. Gültige Werte sind _OS_, _Data_ und _All_. Die Verschlüsselung auf einem ausgeführten Betriebssystem-/Startvolume einer Windows-IaaS-VM kann nicht deaktiviert werden, ohne die Verschlüsselung auf dem Volume _Data_ zu deaktivieren. Beachten Sie außerdem, dass das Deaktivieren der Verschlüsselung auf dem Betriebssystemdatenträger ist auf virtuellen Linux-Computern unzulässig ist. |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerentschlüsselung durchgeführt wird. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Deaktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Computer
Informationen zum Deaktivieren der Verschlüsselung auf einer vorhandenen oder ausgeführten IaaS-VM mithilfe des PowerShell-Cmdlets finden Sie unter [`Disable-AzureRmVMDiskEncryption`](https://msdn.microsoft.com/library/azure/mt715776.aspx). Dieses Cmdlet unterstützt sowohl Windows- als auch Linux-Computer. Das Cmdlet installiert eine Erweiterung auf dem virtuellen Computer, um die Verschlüsselung zu deaktivieren. Wenn der Parameter _Name_ nicht angegeben ist, wird eine Erweiterung mit dem Standardnamen _AzureDiskEncryption für Windows-VMs_ erstellt.

Auf Linux-VMs wird die Erweiterung „AzureDiskEncryptionForLinux“ verwendet.

> [!NOTE]
> Durch dieses Cmdlet wird der virtuelle Computer neu gestartet.

## <a name="appendix"></a>Anhang
### <a name="connect-to-your-subscription"></a>Verbinden mit Ihrem Abonnement
Lesen Sie in diesem Artikel den Abschnitt „Voraussetzungen“, bevor Sie fortfahren. Wenn Sie sichergestellt haben, dass alle Voraussetzungen erfüllt sind, führen Sie die folgenden Schritte aus, um eine Verbindung mit Ihrem Abonnement herzustellen:

1. Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:

    `Login-AzureRmAccount`

2. Wenn Sie über mehrere Abonnements verfügen und ein bestimmtes Abonnement verwenden möchten, geben Sie den folgenden Befehl ein, um die Abonnements für Ihr Konto anzuzeigen:

    `Get-AzureRmSubscription`

3. Geben Sie Folgendes ein, um das zu verwendende Abonnement anzugeben:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. Geben Sie Folgendes ein, um zu überprüfen, ob es sich um das richtige Abonnement handelt:

    `Get-AzureRmSubscription`

5. Geben Sie Folgendes ein, um zu überprüfen, ob die Cmdlets für Azure Disk Encryption installiert sind:

    `Get-command *diskencryption*`

6. Durch die folgende Ausgabe wird die PowerShell-Installation für Azure Disk Encryption bestätigt:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Vorbereiten einer vorverschlüsselten Windows-VHD
Die folgenden Abschnitte sind erforderlich, um eine vorverschlüsselte Windows-VHD für die Bereitstellung als verschlüsselte VHD in Azure IaaS vorzubereiten. Verwenden Sie die Informationen, um einen neuen virtuellen Windows-Computers (VHD) in Azure Site Recovery oder Azure vorzubereiten und zu starten.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Aktualisieren der Gruppenrichtlinie, um Schutz ohne TPM für das Betriebssystem zu ermöglichen
Sie müssen die BitLocker-Gruppenrichtlinieneinstellung mit dem Namen **BitLocker-Laufwerkverschlüsselung** konfigurieren, die sich unter **Lokale Computerrichtlinie** > **Computerkonfiguration** > **Administrative Vorlagen** > **Windows-Komponenten** befindet. Ändern Sie diese Einstellung wie folgt: **Betriebssystemlaufwerke** > **Zusätzliche Authentifizierung beim Start anfordern** > **BitLocker ohne kompatibles TPM zulassen**. Dies wird in der folgenden Abbildung dargestellt:

![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>Installieren der Komponenten der BitLocker-Funktion
Verwenden Sie für Windows Server 2012 und höher den folgenden Befehl:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Verwenden Sie für Windows Server 2008 R2 den folgenden Befehl:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Vorbereiten des Betriebssystemvolumes für BitLocker mit `bdehdcfg`
Führen Sie den folgenden Befehl aus, um die Betriebssystempartition zu komprimieren und den Computer für BitLocker vorzubereiten:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a>Schützen des Betriebssystemvolumes mit BitLocker
Verwenden Sie den Befehl [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx), um die Verschlüsselung auf dem Startvolume mit einer externen Schlüsselschutzvorrichtung zu aktivieren. Platzieren Sie außerdem den externen Schlüssel (BEK-Datei) auf dem externen Laufwerk oder Volume. Die Verschlüsselung wird nach dem nächsten Neustart auf dem System-/Startvolume aktiviert.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> Bereiten Sie den virtuellen Computer mit einer separaten Daten-/Ressourcen-VHD zum Abrufen des externen Schlüssels mit BitLocker vor.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Verschlüsseln eines Betriebssystemdatenträgers auf einer ausgeführten Linux-VM
Die Verschlüsselung des Betriebssystemdatenträgers auf einer ausgeführten Linux-VM wird für die folgenden Distributionen unterstützt:

* RHEL 7.2
* CentOS 7.2
* Ubuntu 16.04

##### <a name="prerequisites-for-os-disk-encryption"></a>Voraussetzungen für die Verschlüsselung des Betriebssystemdatenträgers

* Die VM muss über ein Marketplace-Image im Azure Resource Manager erstellt werden.
* Azure-VM mit mindestens 4 GB RAM (7 GB empfohlen).
* (Für RHEL und CentOS) Deaktivieren Sie SELinux. Informationen zum Deaktivieren von SELinux finden Sie unter „4.4.2. Disabling SELinux“ (Deaktivieren von SELinux) im [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) (SELinux-Handbuch für Benutzer und Administratoren) in der VM.
* Die VM muss nach dem Deaktivieren von SELinux mindestens einmal neu gestartet werden.

##### <a name="steps"></a>Schritte
1. Erstellen Sie eine VM mit einer der oben angegebenen Distributionen.

 Für CentOS 7.2 wird die Verschlüsselung des Betriebssystemdatenträgers über ein spezielles Image unterstützt. Geben Sie zum Verwenden dieses Images beim Erstellen der VM „7.2n“ als SKU an:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Konfigurieren Sie die VM gemäß Ihren Anforderungen. Wenn Sie alle Datenträger (für Betriebssystem und Daten) verschlüsseln möchten, müssen die Datenträger für Daten angegeben und über „/etc/fstab“ bereitgestellt werden.

 > [!NOTE]
 > Verwenden Sie „UUID=...“, um Datenlaufwerke nicht über den Blockgerätnamen, z.B. „/dev/sdb1“, sondern unter „/etc/fstab“ anzugeben. Während der Verschlüsselung ändert sich die Reihenfolge der Laufwerke auf dem virtuellen Computer. Wenn für Ihren virtuellen Computer eine bestimmte Reihenfolge der Blockgeräte erforderlich ist, funktioniert die Bereitstellung nach der Verschlüsselung nicht mehr.

3. Melden Sie sich bei den SSH-Sitzungen ab.

4. Geben Sie zum Verschlüsseln des Betriebssystems „volumeType“ als **All** oder **OS** an, wenn Sie die [Verschlüsselung aktivieren](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Alle Prozesse des Benutzerbereichs, die nicht als `systemd`-Dienste ausgeführt werden, müssen per `SIGKILL` beendet werden. Starten Sie die VM neu. Planen Sie für den virtuellen Computer eine Downtime ein, wenn Sie die Verschlüsselung des Betriebssystemdatenträgers auf einem ausgeführten virtuellen Computer aktivieren.

5. Überprüfen Sie den Fortschritt der Verschlüsselung in regelmäßigen Abständen, indem Sie die Anleitung im [nächsten Abschnitt](#monitoring-os-encryption-progress) befolgen.

6. Wenn für „Get-AzureRmVmDiskEncryptionStatus“ der Status „VMRestartPending“ angezeigt wird, können Sie die VM neu starten, indem Sie sich entweder bei der VM anmelden oder die Anmeldung per Portal/PowerShell/CLI durchführen.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Es wird empfohlen, vor dem Neustart die [Startdiagnose](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) zu speichern.

#### <a name="monitoring-os-encryption-progress"></a>Überwachen des Fortschritts der Betriebssystemverschlüsselung
Es gibt drei Möglichkeiten, den Fortschritt der Betriebssystemverschlüsselung zu überwachen:

* Verwenden Sie das `Get-AzureRmVmDiskEncryptionStatus`-Cmdlet, und untersuchen Sie das Feld „ProgressMessage“:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Wenn die VM den Status „OS disk encryption started“ erreicht hat, dauert der Vorgang auf einer VM mit Premium-Speicher ca. 40 bis 50 Minuten.

 Aufgrund von [Issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent werden `OsVolumeEncrypted` und `DataVolumesEncrypted` in einigen Distributionen als `Unknown` angezeigt. Bei WALinuxAgent Version 2.1.5 und höher wird dieses Problem automatisch behoben. Falls Sie `Unknown` in der Ausgabe sehen, können Sie den Status der Datenverschlüsselung über den Azure Resource Explorer überprüfen.

 Wechseln Sie zum [Azure Resource Explorer](https://resources.azure.com/), und erweitern Sie diese Hierarchie im Auswahlbereich auf der linken Seite:

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

* Sehen Sie sich die [Startdiagnose](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) an. Meldungen der ADE-Erweiterung weisen das Präfix `[AzureDiskEncryption]` auf.

* Melden Sie sich per SSH bei der VM an, und rufen Sie das Erweiterungsprotokoll ab:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Es wird empfohlen, sich nicht bei der VM anzumelden, während die Betriebssystemverschlüsselung durchgeführt wird. Kopieren Sie die Protokolle nur dann, wenn die zwei weiteren Methoden nicht zum Erfolg geführt haben.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Vorbereiten einer vorverschlüsselten Linux-VHD
##### <a name="ubuntu-16"></a>Ubuntu 16
Konfigurieren Sie die Verschlüsselung während der Installation einer Distribution, indem Sie diese Schritte ausführen:

1. Wählen Sie beim Partitionieren von Datenträgern die Option **Configure encrypted volumes** (Verschlüsselte Volumes konfigurieren).

 ![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Erstellen Sie ein separates Startlaufwerk, das nicht verschlüsselt sein darf. Verschlüsseln Sie Ihr Stammlaufwerk.

 ![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Geben Sie eine Passphrase an. Dies ist die Passphrase, die Sie in den Schlüsseltresor hochladen.

 ![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Schließen Sie die Partitionierung ab.

 ![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Beim Starten der VM werden Sie nach einer Passphrase gefragt. Verwenden Sie die Passphrase, die Sie in Schritt 3 angegeben haben.

 ![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Bereiten Sie die VM für das Hochladen in Azure anhand [dieser Anleitung](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/) vor. Führen Sie den letzten Schritt (das Aufheben der VM-Bereitstellung) noch nicht aus.

Konfigurieren Sie die Verschlüsselung für Azure, indem Sie die folgenden Schritte ausführen:

1. Erstellen Sie unter „/usr/local/sbin/azure_crypt_key.sh“ eine Datei mit dem Inhalt des folgenden Skripts. Achten Sie auf KeyFileName, da dies der von Azure verwendete Passphrasename ist.

    ```
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
```

2. Ändern Sie die Verschlüsselungskonfiguration in */etc/crypttab*. Diese sollte wie folgt aussehen:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Wenn Sie *azure_crypt_key.sh* in Windows bearbeitet und nach Linux kopiert haben, führen Sie `dos2unix /usr/local/sbin/azure_crypt_key.sh` aus.

4. Fügen Sie dem Skript Berechtigungen für die Ausführung hinzu:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Bearbeiten Sie */etc/initramfs-tools/modules*, indem Sie Zeilen anfügen:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Führen Sie `update-initramfs -u -k all` zum Aktualisieren von „initramfs“ aus, damit `keyscript` wirksam wird.

7. Nun können Sie die Bereitstellung des virtuellen Computers aufheben.

 ![Ubuntu 16.04-Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Fahren Sie mit dem nächsten Schritt fort, und [laden Sie Ihre VHD in Azure hoch](#upload-encrypted-vhd-to-an-azure-storage-account).

##### <a name="opensuse-132"></a>openSUSE 13.2
Konfigurieren Sie die Verschlüsselung während der Installation einer Distribution, indem Sie diese Schritte ausführen:
1. Wählen Sie beim Partitionieren der Datenträger die Option **Encrypt Volume Group** (Volumegruppe verschlüsseln) aus, und geben Sie dann ein Kennwort ein. Dies ist das Kennwort, die Sie in Ihren Schlüsseltresor hochladen.

 ![openSUSE 13.2-Setup](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Starten Sie die VM, indem Sie Ihr Kennwort verwenden.

 ![openSUSE 13.2-Setup](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Bereiten Sie die VM für das Hochladen in Azure mithilfe der Anweisungen im Abschnitt zum [Vorbereiten einer SLES- oder openSUSE-VM für Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131) vor. Führen Sie den letzten Schritt (das Aufheben der VM-Bereitstellung) noch nicht aus.

Konfigurieren Sie die Verschlüsselung für Azure, indem Sie die folgenden Schritte ausführen:
1. Bearbeiten Sie die Datei „/etc/dracut.conf“, und fügen Sie die folgende Zeile hinzu:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Kommentieren Sie diese Zeilen am Ende der Datei „/usr/lib/dracut/modules.d/90crypt/module-setup.sh“ aus:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. Fügen Sie die folgende Zeile am Anfang der Datei „/usr/lib/dracut/modules.d/90crypt/parse-crypt.sh“ an:
 ```
    DRACUT_SYSTEMD=0
 ```
Und ändern Sie alle Vorkommen von:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
in:
```
    if [ 1 ]; then
```
4. Bearbeiten Sie die Datei „/usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh“, und fügen Sie sie an „# Open LUKS device“ an:

    ```
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
    ```
5. Führen Sie `/usr/sbin/dracut -f -v` aus, um initrd zu aktualisieren.

6. Nun können Sie die Bereitstellung der VM aufheben und [Ihre VHD in Azure hochladen](#upload-encrypted-vhd-to-an-azure-storage-account).

##### <a name="centos-7"></a>CentOS 7:
Konfigurieren Sie die Verschlüsselung während der Installation einer Distribution, indem Sie diese Schritte ausführen:
1. Wählen Sie beim Partitionieren von Datenträgern die Option **Encrypt my data** (Daten verschlüsseln).

 ![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Stellen Sie sicher, dass für die Stammpartition die Option **Encrypt** (Verschlüsseln) ausgewählt ist.

 ![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Geben Sie eine Passphrase an. Dies ist die Passphrase, die Sie in Ihren Schlüsseltresor hochladen.

 ![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Beim Starten der VM werden Sie nach einer Passphrase gefragt. Verwenden Sie die Passphrase, die Sie in Schritt 3 angegeben haben.

 ![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Bereiten Sie die VM für das Hochladen in Azure mithilfe der Anweisungen zu CentOS 7.0+ im Abschnitt zum [Vorbereiten einer CentOS-basierten VM für Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70) vor. Führen Sie den letzten Schritt (das Aufheben der VM-Bereitstellung) noch nicht aus.

6. Nun können Sie die Bereitstellung der VM aufheben und [Ihre VHD in Azure hochladen](#upload-encrypted-vhd-to-an-azure-storage-account).

Konfigurieren Sie die Verschlüsselung für Azure, indem Sie die folgenden Schritte ausführen:

1. Bearbeiten Sie die Datei „/etc/dracut.conf“, und fügen Sie die folgende Zeile hinzu:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Kommentieren Sie diese Zeilen am Ende der Datei „/usr/lib/dracut/modules.d/90crypt/module-setup.sh“ aus:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. Fügen Sie die folgende Zeile am Anfang der Datei „/usr/lib/dracut/modules.d/90crypt/parse-crypt.sh“ an:
```
    DRACUT_SYSTEMD=0
```
Und ändern Sie alle Vorkommen von:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
in:
```
    if [ 1 ]; then
```
4. Bearbeiten Sie die Datei „/usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh“, und fügen Sie sie nach „# Open LUKS device“ an:
    ```
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
    ```    
5. Führen Sie „/usr/sbin/dracut -f -v“ aus, um initrd zu aktualisieren.

![CentOS 7-Setup](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Hochladen einer verschlüsselten VHD in ein Azure-Speicherkonto
Wenn die BitLocker-Verschlüsselung über DM-Crypt-Verschlüsselung aktiviert wurde, muss die lokale verschlüsselte VHD in Ihr Speicherkonto hochgeladen werden.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Hochladen des Geheimnisses der Datenträgerverschlüsselung für den vorverschlüsselten virtuellen Computer in Ihren Schlüsseltresor
Das zuvor abgerufene Geheimnis der Datenträgerverschlüsselung muss als Geheimnis in den Schlüsseltresor hochgeladen werden. Der Schlüsseltresor muss für die Datenverschlüsselung aktiviert sein und über Berechtigungen für Ihren Azure AD-Client verfügen.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Geheimnis der Datenträgerverschlüsselung ohne Verschlüsselung per KEK
Verwenden Sie [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx), um das Geheimnis im Schlüsseltresor einzurichten. Im Falle eines virtuellen Windows-Computers wird die BEK-Datei als Base64-Zeichenfolge codiert und dann mit dem `Set-AzureKeyVaultSecret`-Cmdlet in den Schlüsseltresor hochgeladen. Für Linux wird die Passphrase als Base64-Zeichenfolge codiert und dann in den Schlüsseltresor hochgeladen. Stellen Sie außerdem sicher, dass die folgenden Tags festgelegt sind, während das Geheimnis im Schlüsseltresor erstellt wird.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Verwenden Sie im nächsten Schritt `$secretUrl`, um [den Betriebssystemdatenträger ohne KEK anzufügen](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Geheimnis der Datenträgerverschlüsselung mit Verschlüsselung per KEK
Das Geheimnis kann optional mit einem KEK verschlüsselt werden, bevor er in den Schlüsseltresor hochgeladen wird. Verwenden Sie die Wrapper-[API](https://msdn.microsoft.com/library/azure/dn878066.aspx), um das Geheimnis zuerst mit dem KEK zu verschlüsseln. Die Ausgabe dieses Wrapper-Vorgangs ist eine Base64-codierte URL-Zeichenfolge, die dann als Geheimnis mit dem [`Set-AzureKeyVaultSecret`](https://msdn.microsoft.com/library/dn868050.aspx)-Cmdlet hochgeladen wird.

    # This is the passphrase that was provided for encryption during the distribution installation
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

Verwenden Sie im nächsten Schritt `$KeyEncryptionKey` und `$secretUrl`, um den [ Betriebssystemdatenträger mit KEK](#using-a-kek) anzufügen.

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Angeben einer Geheimnis-URL beim Anfügen eines Betriebssystemdatenträgers
#### <a name="without-using-a-kek"></a>Ohne KEK
Beim Anfügen des Betriebssystemdatenträgers muss `$secretUrl` übergeben werden. Die URL wurde im Abschnitt „Geheimnis der Datenträgerverschlüsselung ohne Verschlüsselung per KEK“ generiert.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Mit KEK
Beim Anfügen des Betriebssystemdatenträgers müssen `$KeyEncryptionKey` und `$secretUrl` übergeben werden. Die URL wurde im Abschnitt „Geheimnis der Datenträgerverschlüsselung ohne Verschlüsselung per KEK“ generiert.

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

## <a name="download-this-guide"></a>Dieses Handbuch herunterladen
Sie können diese Anleitung aus dem [TechNet-Katalog](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) herunterladen.

## <a name="for-more-information"></a>Weitere Informationen
[Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1)  
[Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2)



<!--HONumber=Feb17_HO1-->


