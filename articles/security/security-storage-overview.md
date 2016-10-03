<properties
   pageTitle="Azure Storage-Sicherheitsübersicht | Microsoft Azure"
   description=" Azure Storage ist eine Cloudspeicherlösung für moderne Anwendungen, die eine Kombination aus Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit benötigen, um die Anforderungen ihrer Kunden zu erfüllen. Dieser Artikel bietet eine Übersicht über die wichtigsten Sicherheitsfunktionen von Azure, die mit Azure Storage verwendet werden können. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# Übersicht über die Sicherheit von Azure Storage

Azure Storage ist eine Cloudspeicherlösung für moderne Anwendungen, die eine Kombination aus Zuverlässigkeit, Verfügbarkeit und Skalierbarkeit bietet, um die Anforderungen Ihrer Kunden zu erfüllen. Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen:

- Das Speicherkonto kann mit rollenbasierter Zugriffssteuerung und Azure Active Directory geschützt werden.
- Daten können während der Übertragung zwischen einer Anwendung und Azure mit clientseitiger Verschlüsselung, HTTPS oder SMB 3.0 geschützt werden.
- Es kann festgelegt werden, dass Daten automatisch mit Storage Service Encryption verschlüsselt werden, wenn sie in Azure Storage geschrieben werden.
- Betriebssystemdatenträger und sonstige Datenträger, die von virtuellen Computern verwendet werden, können mit Azure Disk Encryption verschlüsselt werden.
- Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit Shared Access Signatures erteilt werden.
- Die Authentifizierungsmethode, die ein Benutzer verwendet, wenn er auf den Speicher zugreift, kann mithilfe der Speicheranalyse nachvollzogen werden.

Eine ausführlichere Betrachtung der Sicherheit in Azure Storage finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage/storage-security-guide.md). Dieser Leitfaden liefert ausführliche Informationen zu den Sicherheitsfeatures von Azure Storage. Hierzu zählen etwa Speicherkontoschlüssel, die Verschlüsselung von Daten während der Übertragung und im Ruhezustand sowie die Speicheranalyse.

Dieser Artikel bietet eine Übersicht über die Sicherheitsfunktionen von Azure, die mit Azure Storage verwendet werden können. Es sind jeweils Links zu Artikeln mit weiteren Informationen zu jedem Feature angegeben.

Hier sind die wichtigsten Features aufgeführt, die in diesem Artikel behandelt werden:

- Rollenbasierte Access Control
- Delegierter Zugriff auf Speicherobjekte
- Verschlüsselung während der Übertragung
- Verschlüsselung ruhender Daten/Storage Service Encryption
- Azure-Datenträgerverschlüsselung
- Azure-Schlüsseltresor

## Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Sie können Ihr Speicherkonto mit rollenbasierter Zugriffssteuerung (RBAC) sichern. Das Einschränken des Zugriffs auf der Grundlage der Sicherheitsprinzipien [Need to know](https://en.wikipedia.org/wiki/Need_to_know) und [Least Privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ist für Organisationen unerlässlich, die Sicherheitsrichtlinien für den Datenzugriff erzwingen möchten. Diese Zugriffsrechte werden gewährt, indem Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zugewiesen wird. Sie können [integrierte RBAC-Rollen](../active-directory/role-based-access-built-in-roles.md) (etwa „Speicherkontomitwirkender“) verwenden, um Benutzern Berechtigungen zuzuweisen.

Weitere Informationen:

- [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## Delegierter Zugriff auf Speicherobjekte

Shared Access Signatures (SAS) bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Eine SAS bietet die Möglichkeit, einem Client für einen bestimmten Zeitraum spezielle eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto zu erteilen. Dazu müssen Sie nicht Ihre Kontozugriffsschlüssel freigeben. Die SAS ist ein URI, dessen Abfrageparameter alle erforderlichen Informationen für den authentifizierten Zugriff auf eine Speicherressource enthalten. Für den Zugriff auf Speicherressourcen mit der SAS braucht der Client diese nur an den entsprechenden Konstruktor bzw. die entsprechende Methode zu übergeben.

Weitere Informationen:

- [Grundlagen zum SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Erstellen und Verwenden einer SAS mit Blob Storage](../storage/storage-dotnet-shared-access-signature-part-2.md)

## Verschlüsselung während der Übertragung
Verschlüsselung während der Übertragung ist ein Mechanismus zum Schutz der Daten bei der Übertragung über Netzwerke hinweg. Mit Azure Storage können Sie die Daten sichern mit:

- [Verschlüsselung auf Transportebene](../storage/storage-security-guide.md#encryption-in-transit) (etwa HTTPS), wenn Sie Daten in oder aus Azure Storage übertragen.
- [Wire-Verschlüsselung](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares) (etwa SMB 3.0-Verschlüsselung für Azure-Dateifreigaben).
- [Clientseitiger Verschlüsselung](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), um die Daten zu verschlüsseln, bevor sie in den Speicher übertragen werden, und nach der Übertragung aus dem Speicher zu entschlüsseln.

Weitere Informationen zur clientseitigen Verschlüsselung finden Sie hier:

- [Clientseitige Verschlüsselung für Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud security controls series: Encrypting Data in Transit (Cloudsicherheit steuert Serie: Verschlüsseln von Daten bei der Übertragung)](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## Verschlüsselung ruhender Daten

Für viele Organisationen ist die Verschlüsselung von [ruhenden Daten](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt in Richtung Datenschutz, Compliance und Datenhoheit. Drei Azure-Features ermöglichen die Verschlüsselung „ruhender“ Daten:

- Mit [Storage Service Encryption](../storage/storage-security-guide.md#encryption-at-rest) können Sie anfordern, dass der Speicherdienst die Daten beim Schreiben in Azure Storage automatisch verschlüsselt.
- Die [clientseitige Verschlüsselung](../storage/storage-security-guide.md#client-side-encryption) ermöglicht ebenfalls eine Verschlüsselung ruhender Daten.
- Mit [Azure Disk Encryption](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) können Sie die Betriebssystemdatenträger und andere Datenträger verschlüsseln, die von einem virtuellen IaaS-Computer verwendet werden.

Weitere Informationen zu Storage Service Encryption finden Sie hier:

- [Azure Storage Service Encryption](https://azure.microsoft.com/services/storage/) ist für [Azure-Blobspeicher](https://azure.microsoft.com/services/storage/blobs/) verfügbar. Ausführliche Informationen zu anderen Arten von Azure-Speicher finden Sie unter [Datei](https://azure.microsoft.com/services/storage/files/), [Datenträger (Storage Premium)](https://azure.microsoft.com/services/storage/premium-storage/), [Tabelle](https://azure.microsoft.com/services/storage/tables/) und [Warteschlange](https://azure.microsoft.com/services/storage/queues/).
- [Azure Storage Service Encryption für ruhende Daten](../storage/storage-service-encryption.md)

## Azure-Datenträgerverschlüsselung

Mithilfe von Azure Disk Encryption für virtuelle Computer können Sie die Sicherheits- und Complianceanforderungen Ihrer Organisation erfüllen, indem Sie die Datenträger Ihrer virtuellen Computer (Startdatenträger und allgemeine Datenträger) mit Schlüsseln und Richtlinien verschlüsseln, die Sie über den [Azure-Schlüsseltresor](https://azure.microsoft.com/services/key-vault/) steuern.

Datenträgerverschlüsselung für VMs funktioniert für Linux- und Windows-Betriebssysteme. Es wird auch Key Vault verwendet, um Ihnen das Schützen, Verwalten und Überwachen der Nutzung Ihrer Datenträgerverschlüsselungs-Schlüssel zu erleichtern. Alle Daten auf den Datenträgern Ihrer virtuellen Computer werden im Ruhezustand mithilfe von Verschlüsselungstechnologien des Industriestandards in Ihren Azure Storage-Konten verschlüsselt. Die Disk Encryption-Lösung für Windows basiert auf der [Microsoft BitLocker-Laufwerkverschlüsselung](https://technet.microsoft.com/library/cc732774.aspx), die Linux-Lösung auf [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Weitere Informationen:

- [Azure Disk Encryption for Windows and Linux IaaS Virtual Machines (Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer)](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## Azure-Schlüsseltresor

Azure Disk Encryption verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), um Sie bei der Steuerung und Verwaltung von Datenträger-Verschlüsselungsschlüsseln und geheimen Schlüsseln in Ihrem Schlüsseltresor-Abonnement zu unterstützen. Dabei wird gleichzeitig sichergestellt, dass alle ruhenden Daten auf den Laufwerken der virtuellen Computer in Ihrer Azure Storage-Instanz verschlüsselt sind. Es empfiehlt sich, die Verwendung von Schlüsseln und Richtlinien mithilfe des Azure-Schlüsseltresors zu überwachen.

Weitere Informationen:

- [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-whatis.md)
- [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md)

<!---HONumber=AcomDC_0921_2016-->