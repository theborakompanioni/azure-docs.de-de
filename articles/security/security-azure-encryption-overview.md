---
title: "Übersicht über die Azure-Verschlüsselung | Microsoft-Dokumentation"
description: "Lernen Sie die verschiedenen Verschlüsselungsoptionen in Azure kennen."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 752568747ab96bc0a9c7fc5f24ff28c3bce4e09f
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="azure-encryption-overview"></a>Übersicht über die Azure-Verschlüsselung

Dieser Artikel bietet eine Übersicht über die Verwendung der Verschlüsselung in Microsoft Azure. Er behandelt die wichtigsten Bereiche der Verschlüsselung, einschließlich Verschlüsselung ruhender Daten, Verschlüsselung in Aktion und Schlüsselverwaltung mit Key Vault. Jeder Abschnitt enthält Links zu ausführlicheren Informationen.

## <a name="encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten

Ruhende Daten umfassen Informationen, die in einem beliebigen digitalen Format im dauerhaften Speicher auf physischen Medien gespeichert sind. Dazu gehören Dateien auf Magnet- oder optischen Datenträgern, archivierte Daten und Datensicherungen. Microsoft Azure bietet eine Reihe von Datenspeicherlösungen für verschiedene Anforderungen, darunter Datei-, Daten-, Blob- und Tabellenspeicher. Microsoft bietet zudem die Verschlüsselung zum Schutz von [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md), [Cosmos DB](../cosmos-db/introduction.md) und Azure Data Lake.

Die Verschlüsselung ruhender Daten ist für Dienste in allen Azure Software-as-a-Service- (SaaS), Platform-as-a-Service- (PaaS) und Infrastructure-as-a-Service-Cloudmodellen (IaaS) verfügbar. In diesem Dokument werden Ressourcen zusammenfassend beschrieben und bereitgestellt, mit denen Sie die Verschlüsselungsoptionen von Azure nutzen können.

Eine ausführliche Erörterung zur Verschlüsselung ruhender Daten in Azure finden Sie im Dokument [Azure-Datenverschlüsselung ruhender Daten](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Azure-Verschlüsselungsmodelle

Azure unterstützt verschiedene Verschlüsselungsmodelle, darunter die serverseitige Verschlüsselung unter Verwendung dienstverwalteter Schlüssel, unter Verwendung kundenverwalteter Schlüssel in Azure Key Vault oder unter Verwendung kundenverwalteter Schlüssel auf vom Kunden gesteuerter Hardware. Mit der clientseitigen Verschlüsselung können Sie Schlüssel lokal oder an anderen sicheren Speicherorten verwalten und speichern.

### <a name="client-side-encryption"></a>Clientseitige Verschlüsselung

Die clientseitige Verschlüsselung erfolgt außerhalb von Azure. Die clientseitige Verschlüsselung betrifft folgende Daten:

- Daten, die über eine im Rechenzentrum des Kunden ausgeführte Anwendung oder über eine Dienstanwendung verschlüsselt werden
- Daten, die beim Empfang in Azure bereits verschlüsselt sind

Mit der clientseitigen Verschlüsselung hat der Clouddienstanbieter keinen Zugriff auf die Verschlüsselungsschlüssel und kann diese Daten nicht entschlüsseln. Sie behalten die vollständige Kontrolle über die Schlüssel.

### <a name="server-side-encryption"></a>Serverseitige Verschlüsselung

Die drei Modelle für die serverseitige Verschlüsselung bieten unterschiedliche Merkmale der Schlüsselverwaltung, die je nach Ihren Anforderungen gewählt werden können.

- **Dienstverwaltete Schlüssel** bieten eine Kombination aus Kontrolle und Benutzerfreundlichkeit mit geringem Mehraufwand

- **Kundenverwaltete Schlüssel** ermöglichen Ihnen die Kontrolle über die Schlüssel, einschließlich der Möglichkeit, die BYOK-Funktion (Bring your Own Key) zu verwenden oder neue Schlüssel zu generieren.

- Mit **dienstverwalteten Schlüsseln auf vom Kunden gesteuerter Hardware** können Sie Schlüssel in Ihrem proprietären Repository verwalten, das sich außerhalb des Einflussbereichs von Microsoft befindet. Dies wird als „Host Your Own Key“ (HYOK) bezeichnet. Die Konfiguration ist jedoch komplex, und die meisten Azure-Dienste unterstützen dieses Modell nicht.

### <a name="azure-disk-encryption"></a>Azure-Datenträgerverschlüsselung

Virtuelle Windows- und Linux-Computer können mithilfe von [Azure Disk Encryption](azure-security-disk-encryption.md) geschützt werden. Hierbei werden die [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx)-Technologie und Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) verwendet, um Betriebssystemdatenträger sowie Datenträger mit vollständiger Volumeverschlüsselung zu schützen.

Verschlüsselungsschlüssel und Geheimnisse werden in Ihrem [Azure Key Vault](../key-vault/key-vault-whatis.md)-Abonnement geschützt. Verschlüsselte virtuelle Computer, die mit der KEK-Konfiguration verschlüsselt sind, können Sie mithilfe des Azure Backup-Diensts sichern und wiederherstellen.

### <a name="azure-storage-service-encryption"></a>Azure Storage Service Encryption

Ruhende Daten in Azure Storage (Blob Storage und File Storage) können in serverseitigen und clientseitigen Szenarien verschlüsselt werden.

[Azure Storage Service Encryption](../storage/storage-service-encryption.md) (SSE) kann Daten automatisch verschlüsseln, bevor sie gespeichert werden, und entschlüsselt sie automatisch wenn Sie sie abrufen, sodass der Vorgang für Benutzer vollkommen transparent ist. Storage Service Encryption verwendet die [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) mit 256 Bit, d.h. eine der stärksten verfügbaren Blockverschlüsselungen, und verarbeitet die Verschlüsselung, Entschlüsselung und Schlüsselverwaltung auf transparente Weise.

### <a name="client-side-encryption-of-azure-blobs"></a>Clientseitige Verschlüsselung von Azure-Blobs

Die clientseitige Verschlüsselung von Azure-Blobs kann auf unterschiedliche Weisen durchgeführt werden.

Sie können das NuGet-Paket „Azure Storage-Clientbibliothek für .NET“ verwenden, um Daten in Ihren Clientanwendungen vor dem Hochladen in Azure Storage zu verschlüsseln.

Weitere Informationen zum Verwenden und Herunterladen des NuGet-Pakets „Azure Storage-Clientbibliothek für .NET“ finden Sie im Dokument [Windows Azure Storage 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage).

Bei Verwendung der clientseitigen Verschlüsselung mit Azure Key Vault werden Ihre Daten mit einem einmaligen symmetrischen Inhaltsverschlüsselungsschlüssel (CEK, Content Encryption Key) verschlüsselt, der vom Azure Storage Client SDK generiert wird. Der CEK wird mit einem Schlüsselverschlüsselungsschlüssel (KEK, Key Encryption Key) verschlüsselt, bei dem es sich entweder um ein symmetrisches oder ein asymmetrisches Schlüsselpaar handeln kann. Sie können ihn lokal verwalten oder in Azure Key Vault speichern. Die verschlüsselten Daten werden dann in den Azure Storage-Dienst hochgeladen.

Weitere Informationen zur clientseitigen Verschlüsselung mit Azure Key Vault und den ersten Schritten mit entsprechenden Anweisungen finden Sie im Dokument [Tutorial: Verschlüsseln und Entschlüsseln von Blobs in Microsoft Azure Storage per Azure Key Vault](../storage/storage-encrypt-decrypt-blobs-key-vault.md).

Schließlich können Sie auch die Azure Storage-Clientbibliothek für Java verwenden, um die clientseitige Verschlüsselung vor dem Hochladen von Daten in Azure Storage und die Entschlüsselung der Daten beim Herunterladen auf den Client durchzuführen. Um die Schlüsselverwaltung für Speicherkonten zu ermöglichen, unterstützt diese Bibliothek zudem die Integration in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Verschlüsselung ruhender Daten mit Azure SQL-Datenbank

[Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md) ist ein relationaler Datenbankdienst in Microsoft Azure für allgemeine Zwecke, der Strukturen wie relationale Daten, JSON, räumliche Daten und XML unterstützt. Azure SQL unterstützt die serverseitige Verschlüsselung über das TDE-Feature verschlüsseln (Transparent Data Encryption) sowie die clientseitige Verschlüsselung über die Always Encrypted-Funktion.

#### <a name="transparent-data-encryption"></a>Transparent Data Encryption

[Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) wird verwendet, um [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016)-, [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md)- und [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)-Datendateien in Echtzeit mit einem Datenbankverschlüsselungsschlüssel (DEK, Database Encryption Key) zu verschlüsseln, der für die Verfügbarkeit während der Wiederherstellung im Datenbank-Startdatensatz gespeichert ist.

TDE schützt die Daten- und Protokolldateien über die Verschlüsselungsalgorithmen AES und 3DES. Die Verschlüsselung der Datenbankdatei erfolgt auf Seitenebene. Die Seiten in einer verschlüsselten Datenbank werden verschlüsselt, bevor sie auf den Datenträger geschrieben werden, und entschlüsselt, bevor sie in den Arbeitsspeicher eingelesen werden. TDE ist jetzt bei neu erstellten Azure SQL-Datenbanken standardmäßig aktiviert.

#### <a name="always-encrypted"></a>Always Encrypted

Mit dem [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)-Feature in Azure SQL können Sie Daten in Clientanwendungen vor dem Speichern in Azure SQL-Datenbank verschlüsseln sowie die Delegierung der lokalen Datenbankverwaltung an Drittanbieter aktivieren und die Trennung zwischen den Benutzern, die die Daten besitzen und anzeigen können, und den Benutzern aufrechterhalten, die die Daten verwalten, jedoch keinen Zugriff darauf haben sollen.

#### <a name="cellcolumn-level-encryption"></a>Verschlüsselung auf Zellen-/Spaltenebene

Mit Azure SQL-Datenbank können Sie mithilfe von Transact-SQL eine symmetrische Verschlüsselung auf eine Datenspalte anwenden. Dies wird als [Verschlüsselung auf Zellenebene oder Verschlüsselung auf Spaltenebene](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (CLE) bezeichnet, da Sie bestimmte Datenspalten oder sogar bestimmte Datenzellen mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsseln können. Diese Verschlüsselungsfunktion ist differenzierter als die TDE, bei der Daten in Seiten verschlüsselt werden.

CLE verfügt über integrierte Funktionen, mit denen Sie Daten über symmetrische oder asymmetrische Schlüssel, mit dem öffentlichen Schlüssel eines Zertifikats oder mit einer Passphrase über 3DES verschlüsseln können.

### <a name="cosmos-db-database-encryption"></a>Cosmos DB-Datenbankverschlüsselung

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) ist eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Die in Cosmos DB in nichtflüchtigem Speicher (Solid State Drives) gespeicherten Benutzerdaten werden standardmäßig verschlüsselt. Es sind keine Steuerelemente zum Aktivieren oder Deaktivieren vorhanden. Die Verschlüsselung ruhender Daten wird über eine Reihe von Sicherheitstechnologien implementiert, einschließlich der Speichersysteme für sichere Schlüssel, verschlüsselter Netzwerke und Kryptografie-APIs. Die Verschlüsselungsschlüssel werden von Microsoft verwaltet und entsprechend internen Microsoft-Richtlinien rotiert.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Verschlüsselung ruhender Daten in Azure Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) ist ein unternehmensweites Repository für alle Arten von Daten, die vor jeglicher formalen Definition von Anforderungen oder Schemas an einer zentralen Stelle gesammelt werden. Azure Data Lake Store unterstützt die standardmäßig aktivierte transparente Verschlüsselung von ruhenden Daten, die während der Erstellung Ihres Kontos eingerichtet wird. Standardmäßig verwaltet Data Lake Store die Schlüssel, Sie haben jedoch die Möglichkeit, diese selbst zu verwalten.

Drei Arten von Schlüsseln werden beim Verschlüsseln und Entschlüsseln von Daten verwendet: Masterverschlüsselungsschlüssel (MEK, Master Encryption Key), Datenverschlüsselungsschlüssel (DEK, Data Encryption Key) und Blockverschlüsselungsschlüssel (BEK, Block Encryption Key). Der MEK wird zum Verschlüsseln des DEK verwendet, der auf persistenten Medien gespeichert wird. Der BEK wird vom DEK und dem Datenblock abgeleitet. Wenn Sie Ihre eigenen Schlüssel verwalten, können Sie den MEK rotieren.

## <a name="encryption-of-data-in-transit"></a>Verschlüsselung von Daten während der Übertragung

Azure bietet viele Verfahren zum Schutz von Daten beim Übertragen zwischen verschiedenen Speicherorten.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL-Verschlüsselung in Azure

Microsoft verwendet das [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)-Protokoll (TLS) zum Schutz von Daten bei der Übertragung zwischen den Clouddiensten und Kunden. Die Microsoft-Rechenzentren verhandeln eine TLS-Verbindung mit Clientsystemen, die eine Verbindung mit Azure-Diensten herstellen. TLS bietet strenge Authentifizierung, Datenschutz von Nachrichten und Integrität (ermöglicht die Erkennung von Manipulation, Abfangen und Fälschung von Nachrichten), Interoperabilität, Algorithmusflexibilität sowie einfache Bereitstellung und Verwendung.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) schützt Verbindungen zwischen den Clientsystemen von Kunden und den Clouddiensten von Microsoft durch eindeutige Schlüssel. Die Verbindungen verwenden zudem RSA-basierte Verschlüsselungsschlüssellängen von 2.048 Bit. Diese Kombination erschwert das Abfangen und den Zugriff auf die Daten während der Übertragung.

### <a name="azure-storage-transactions"></a>Transaktionen in Azure Storage

Wenn Sie mit Azure Storage über das Azure-Portal interagieren, erfolgen alle Transaktionen über HTTPS. Für die Interaktion mit Azure Storage können Sie auch die Storage-REST-API über HTTPS verwenden. Sie können die Verwendung von HTTPS beim Aufruf von REST-APIs für den Zugriff auf Objekte in Speicherkonten erzwingen, indem Sie die Option „Sichere Übertragung erforderlich“ aktivieren.

Mit Shared Access Signatures ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), die zum Delegieren des Zugriffs auf Azure Storage-Objekte verwendet werden können, können Sie festlegen, dass bei Verwendung von Shared Access Signatures nur das HTTPS-Protokoll verwendet werden darf. So wird sichergestellt, dass jeder, der Links mit SAS-Token sendet, das richtige Protokoll verwendet.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption), das für den Zugriff auf Azure-Dateifreigaben verwendet wird, unterstützt die Verschlüsselung und ist in Windows Server 2012 R2, Windows 8, Windows 8.1 und Windows 10 verfügbar, sodass regionsübergreifender Zugriff und sogar Zugriff auf dem Desktop möglich ist.

Die Daten werden mit der clientseitigen Verschlüsselung verschlüsselt, bevor sie an Azure Storage gesendet werden, sodass sie während der Übertragung im Netzwerk verschlüsselt sind.

### <a name="smb-encryption-over-azure-virtual-networks"></a>SMB-Verschlüsselung in virtuellen Azure-Netzwerken 

Mit [SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) auf virtuellen Azure-Computern unter Windows Server 2012 und höher haben Sie die Möglichkeit der sicheren Datenübertragung durch Verschlüsselung der Daten während der Übertragung in virtuellen Azure-Netzwerken, um die Daten vor Manipulationen und Lauschangriffen zu schützen. Administratoren können die SMB-Verschlüsselung für den gesamten Server oder nur für bestimmte Freigaben aktivieren.

Wenn die SMB-Verschlüsselung für eine Freigabe oder einen Server aktiviert ist, können standardmäßig nur SMB 3-Clients auf die verschlüsselten Freigaben zugreifen.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Verschlüsselung von Daten während der Übertragung in virtuellen Azure-Computern

Daten, die auf, von und zwischen virtuellen Azure-Computern unter Windows übertragen werden, werden je nach Art der Verbindung auf unterschiedliche Weise verschlüsselt.

### <a name="rdp-sessions"></a>RDP-Sitzungen

Sie können über das [Remotedesktopprotokoll](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) über einen Windows-Clientcomputer oder einen Macintosh-Computer mit installiertem RDP-Client eine Verbindung mit einem virtuellen Azure-Computer herstellen und sich bei diesem anmelden. Daten während der Übertragung über das Netzwerk in RDP-Sitzungen können durch TLS geschützt werden.

Sie können auch über Remotedesktop eine Verbindung mit einem virtuellen Linux-Computer in Azure herstellen.

### <a name="secure-access-to-linux-vms-with-ssh"></a>Sicherer Zugriff auf virtuelle Linux-Computer mit SSH

Sie können [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) zum Herstellen von Verbindungen mit in Azure ausgeführten virtuellen Linux-Computern zur Remoteverwaltung verwenden. SSH ist ein Protokoll für verschlüsselte Verbindungen, das die sichere Anmeldung über ungesicherte Verbindungen ermöglicht. Es ist das Standardverbindungsprotokoll für in Azure gehostete virtuelle Linux-Computer. Durch Verwendung von SSH-Schlüsseln für die Authentifizierung sind keine Kennwörter für die Anmeldung erforderlich. SSH verwendet ein Schlüsselpaar aus einem öffentlichen und privaten Schlüssel (asymmetrische Verschlüsselung) für die Authentifizierung.

## <a name="azure-vpn-encryption"></a>Azure-VPN-Verschlüsselung

Sie können die Verbindung mit Azure über ein virtuelles privates Netzwerk herstellen, das einen sicheren Tunnel zum Datenschutz der über das Netzwerk gesendeten Daten erstellt.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

Mit [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) kann verschlüsselter Datenverkehr zwischen Ihrem virtuellen Netzwerk und Ihrem lokalen Standort über eine öffentliche Verbindung oder zwischen virtuellen Netzwerken gesendet werden.

Die Site-to-Site-VPN-Verbindung verwendet [IPsec](https://en.wikipedia.org/wiki/IPsec) für die Verschlüsselung beim Datentransport. Azure-VPN-Gateways verwenden einen Satz von Standardvorschlägen. Sie können Azure-VPN-Gateways so konfigurieren, dass anstelle der Azure-Standardrichtliniensätze eine benutzerdefinierte IPsec/IKE-Richtlinie mit bestimmten Kryptografiealgorithmen und Schlüssellängen verwendet wird.

### <a name="point-to-site-vpn"></a>Point-to-Site-VPN-Verbindung

Über Point-to-Site-VPN-Verbindungen können einzelne Clientcomputer auf ein virtuelles Azure-Netzwerk zugreifen. Das [Secure Socket Tunneling-Protokoll](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) wird verwendet, um den VPN-Tunnel zu erstellen, und kann Firewalls durchlaufen (der Tunnel wird als HTTPS-Verbindung angezeigt). Sie können für Point-to-Site-Verbindungen Ihre eigene interne PKI-Stammzertifizierungsstelle verwenden.

Sie können eine Point-to-Site-VPN-Verbindung mit einem virtuellen Netzwerk über das Azure-Portal mit Zertifikatauthentifizierung oder über PowerShell konfigurieren.

Weitere Informationen zu Point-to-Site-VPN-Verbindungen mit Azure-VNETs finden Sie in den folgenden Artikeln: [Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der Zertifikatauthentifizierung: Azure-Portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) und

[Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der Zertifikatauthentifizierung: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Standort-zu-Standort-VPN-Verbindung 

Eine Site-to-Site-VPN-Gateway-Verbindung wird verwendet, um Ihr lokales Netzwerk über einen IPsec/IKE-VPN-Tunnel (IKEv1 oder IKEv2) mit einem virtuellen Azure-Netzwerk zu verbinden. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist.

Sie können eine Site-to-Site-VPN-Verbindung mit einem virtuellen Netzwerk über das Azure-Portal, PowerShell oder die Azure-Befehlszeilenschnittstelle (CLI) konfigurieren.

Weitere Informationen finden Sie in den folgenden Artikeln:

[Erstellen einer Standort-zu-Standort-Verbindung im Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Einrichten einer Standort-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung per CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Verschlüsselung von Daten während der Übertragung in Azure Data Lake

Auch übertragene Daten (Daten in Bewegung) werden in Data Lake Store immer verschlüsselt. Zusätzlich zur Verschlüsselung von Daten vor dem Speichern auf persistenten Medien werden Daten auch während der Übertragung immer über HTTPS geschützt. Für die Data Lake Store-REST-Schnittstellen wird ausschließlich das HTTPS-Protokoll unterstützt.

Weitere Informationen zur Verschlüsselung von Daten während der Übertragung in Azure Data Lake finden Sie im Dokument [Datenverschlüsselung in Azure Data Lake Store](../data-lake-store/data-lake-store-encryption.md).

## <a name="key-management-with-azure-key-vault"></a>Schlüsselverwaltung mit Azure Key Vault

Ohne angemessenen Schutz und die richtige Verwaltung der Schlüssel ist Verschlüsselung unbrauchbar. Azure Key Vault ist die von Microsoft empfohlene Lösung zur Verwaltung und Steuerung des Zugriffs auf die in Clouddiensten verwendeten Verschlüsselungsschlüssel. Berechtigungen für Zugriffsschlüssel können Diensten oder Benutzern über Azure Active Directory-Konten zugewiesen werden.

Mit Azure Key Vault gehören für Organisationen das Konfigurieren, Patchen und Verwalten von HSMs (Hardware Security Modules) und wichtiger Verwaltungssoftware der Vergangenheit an. Mit Azure Key Vault behalten Sie die Kontrolle: Microsoft erhält niemals Einblick in Ihre Schlüssel, und Anwendungen haben keinen direkten Zugriff auf die Schlüssel. Sie können zudem Schlüssel in HSMs importieren oder generieren.

## <a name="next-steps"></a>Nächste Schritte

- [Azure-Sicherheitsübersicht](security-get-started-overview.md)
- [Die Netzwerksicherheit in Azure in der Übersicht](security-network-overview.md)
- [Übersicht über die Sicherheit der Azure-Datenbank](azure-database-security-overview.md)
- [Virtuelle Azure-Computer – Sicherheitsübersicht](security-virtual-machines-overview.md)
- [Datenverschlüsselung ruhender Daten](azure-security-encryption-atrest.md)
- [Bewährte Methoden für Datensicherheit und Verschlüsselung](azure-security-data-encryption-best-practices.md)

