<properties
   pageTitle="Azure Storage-Sicherheitsübersicht | Microsoft Azure"
   description="Azure Storage ist eine Cloudspeicherlösung für moderne Anwendungen, die eine Kombination aus Zuverlässigkeit, Verfügbarkeit und Skalierbarkeit bietet, um die Anforderungen Ihrer Kunden zu erfüllen. Dieser Artikel bietet eine Übersicht über die wichtigsten Sicherheitsfunktionen von Azure, die mit Azure Storage verwendet werden können."
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/26/2016"
   ms.author="terrylan"/>

# Azure Storage-Sicherheitsübersicht

Azure Storage ist eine Cloudspeicherlösung für moderne Anwendungen, die eine Kombination aus Zuverlässigkeit, Verfügbarkeit und Skalierbarkeit bietet, um die Anforderungen Ihrer Kunden zu erfüllen. Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen:

- Das Speicherkonto kann mit rollenbasierter Zugriffssteuerung und Azure Active Directory geschützt werden.
- Daten können während der Übertragung zwischen einer Anwendung und Azure mit clientseitiger Verschlüsselung, HTTPS oder SMB 3.0 geschützt werden.
- Es kann festgelegt werden, dass Daten automatisch mit Storage Service Encryption verschlüsselt werden, wenn sie in Azure Storage geschrieben werden.
- Betriebssystemdatenträger und sonstige Datenträger, die von virtuellen Computern verwendet werden, können mit Azure Disk Encryption verschlüsselt werden.
- Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit Shared Access Signatures erteilt werden.

Dieser Artikel bietet eine Übersicht über jede dieser wichtigsten Sicherheitsfunktionen, die mit Azure Storage verwendet werden können. Es sind jeweils Links zu Artikeln mit weiteren Informationen zu jedem Feature angegeben.

Eine ausführlichere Betrachtung der Sicherheit in Azure Storage finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage/storage-security-guide.md).

Hier sind die wichtigsten Features aufgeführt, die in diesem Artikel behandelt werden:

- Rollenbasierte Access Control
- Verwalten von Speicherkontoschlüsseln
- Delegierter Zugriff auf Speicherobjekte
- Verschlüsselung während der Übertragung
- Verschlüsselung ruhender Daten/Storage Service Encryption
- Speicheranalyse

## Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Sie können Ihr Speicherkonto mit rollenbasierter Zugriffssteuerung (RBAC) sichern. Jedes Azure-Abonnement ist mit einem Azure Active Directory-Mandanten (AD) verknüpft. Benutzer, Gruppen und Anwendungen aus diesem Verzeichnis können Ressourcen im Azure-Abonnement verwalten. Diese Zugriffsrechte werden gewährt, indem Benutzern, Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zugewiesen wird.

Mit RBAC können Sie den Zugriff auf die Verwaltungsvorgänge Ihres Azure Storage-Kontos gewähren. Mit RBAC, können Sie steuern, wer:

- das Speicherkonto verwalten darf.
- die Schlüssel des Speicherkontos lesen und diese Schlüssel für den Zugriff auf Blobs, Warteschlangen, Tabellen und Dateien verwenden darf.
- Speicherschlüssel regenerieren darf.

Weitere Informationen:

- [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## Verwalten von Speicherkontoschlüsseln

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicherzugriffsschlüssel, die für die Authentifizierung verwendet werden, wenn der Zugriff auf das Speicherkonto erfolgt. Durch Bereitstellen von zwei Speicherzugriffsschlüsseln ermöglicht Azure Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Speicherdiensts oder Zugriff auf diesen Dienst. Sie können Verbindungen zum Speicherkonto mit einem Zugriffsschlüssel aufrechterhalten, während Sie den anderen Zugriffsschlüssel neu generieren.

Speicherkontoschlüssel können zusammen mit dem Speicherkontonamen für den Zugriff auf die im Speicherkonto gespeicherten Datenobjekte, z.B. Blobs, Entitäten innerhalb einer Tabelle, Warteschlangennachrichten und Dateien in einer Azure-Dateifreigabe verwendet werden. Mit RBAC steuern Sie den Zugriff auf die Speicherkontoschlüssel, die den Zugriff auf die Datenobjekte selbst steuern.

Weitere Informationen:

- [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md)

## Delegierter Zugriff auf Speicherobjekte

Eine Shared Access Signature (SAS) ist eine Zeichenfolge, die ein Sicherheitstoken enthält, das einem URI angefügt werden kann, der Ihnen das Delegieren des Zugriffs auf Speicherobjekte und Festlegen von Einschränkungen, z.B. Berechtigungen und den Datums- und Uhrzeitbereich für den Zugriff, ermöglicht.

Wenn Sie den Schlüssel Ihres Speicherkontos übergeben, ist dies so, als würden Sie die Schlüssel zu Ihrem „Speicherreich“ freigeben. Sie gewähren damit vollständigen Zugriff. Mit SAS können Sie einem Client genau die erforderlichen Berechtigungen für einen begrenzten Zeitraum gewähren. Sie können:

- den Zugriff auf Blobs, Container, Warteschlangennachrichten, Dateien und Tabellen gewähren. Für Tabellen können Sie die Berechtigung zum Zugriff auf eine Reihe von Entitäten in der Tabelle gewähren, indem Sie die Partitions- und Zeilenschlüsselbereiche angeben, auf die der Benutzer Zugriff haben soll.
- Geben Sie an, dass Anforderungen, die mit einer SAS erfolgen, auf eine bestimmte IP-Adresse oder einen bestimmten IP-Adressbereich außerhalb von Azure beschränkt sind.
- Fordern Sie an, dass die Anforderungen mit einem bestimmten Protokoll erfolgen (HTTPS oder HTTP/HTTPS). Dies bedeutet: Wenn Sie nur HTTPS-Datenverkehr zulassen möchten, können Sie HTTPS als erforderliches Protokoll festlegen, sodass HTTP-Datenverkehr blockiert wird.

Weitere Informationen:

- [Grundlagen zum SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md)

## Verschlüsselung während der Übertragung
Verschlüsselung während der Übertragung ist ein Mechanismus zum Schutz der Daten bei der Übertragung über Netzwerke hinweg. Mit Azure Storage können Sie die Daten sichern mit:

- Verschlüsselung auf Transportebene, wie z.B. HTTPS, wenn Sie Daten in oder aus Azure Storage übertragen.
- Wire-Verschlüsselung, wie z.B. SMB 3.0-Verschlüsselung für Azure-Dateifreigaben.
- clientseitiger Verschlüsselung, um die Daten zu verschlüsseln, bevor sie in den Speicher übertragen werden, und nach der Übertragung aus dem Speicher zu entschlüsseln.

### Verschlüsselung auf Transportebene

Um die Sicherheit Ihrer Azure Storage-Daten sicherzustellen, können Sie die Daten bei der Übertragung zwischen dem Client und Azure Storage verschlüsseln. Beim Abrufen von REST-APIs oder Zugreifen auf Objekte im Speicher können Sie [HTTPS aktivieren](../app-service-web/web-sites-configure-ssl-certificate.md). Darüber hinaus [Shared Access Signatures](../storage/storage-dotnet-shared-access-signature-part-1.md) (SAS), die zum Delegieren des Zugriffs auf Azure Storage-Objekte verwendet werden können. Zu ihnen gehört ein optionaler Parameter, „protocol“, den Sie verwenden können, um anzugeben, dass nur das HTTPS-Protokoll verwendet werden kann, wenn Sie SAS verwenden. Dadurch wird sichergestellt, dass jeder, der Links mit SAS-Token sendet, das richtige Protokoll verwendet.

### Wire-Verschlüsselung für den Zugriff auf Dateifreigaben

SMB 3.0 unterstützt die Verschlüsselung und kann mit Windows Server 2012 R2, Windows 8, Windows 8.1 und Windows 10 verwendet werden, sodass regionsübergreifender Zugriff und sogar Zugriff auf dem Desktop möglich ist.

Azure-Dateifreigaben können mit virtuellen Linux-Computern verwendet werden. Der Linux-SMB-Client unterstützt die Verschlüsselung noch nicht, sodass der Zugriff nur innerhalb einer Azure-Region zulässig ist. Unterstützung der Linux-Verschlüsselung ist in der Roadmap aufgeführt. Wenn die Verschlüsselung möglich ist, haben Sie unter Linux die gleiche Verschlüsselung auf Netzwerkebene für den Zugriff auf eine Azure-Dateifreigabe wie unter Windows.

Weitere Informationen:

- [Verwenden des Azure-Dateispeichers unter Linux](../storage/storage-how-to-use-files-linux.md)
- [Erste Schritte mit Azure File Storage unter Windows](../storage/storage-dotnet-how-to-use-files.md)
- [Azure-Dateispeicher](https://azure.microsoft.com/blog/inside-azure-file-storage/)

### Clientseitige Verschlüsselung

Die clientseitige Verschlüsselung hilft Ihnen, zu gewährleisten, dass Ihre Daten beim Übertragen zwischen einer Clientanwendung und Azure Storage sicher sind. Die Daten werden verschlüsselt, bevor sie in Azure Storage übertragen werden. Beim Abrufen der Daten aus Azure Storage werden die Daten entschlüsselt, nachdem der Client sie empfangen hat. Obwohl die Daten verschlüsselt gesendet werden, können Sie trotzdem HTTPS verwenden, um das Risiko zu reduzieren, dass Netzwerkfehler die Integrität der Daten beeinträchtigen.

Weitere Informationen:

- [Clientseitige Verschlüsselung für Microsoft Azure Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud security controls series: Encrypting Data in Transit](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/) (Cloudsicherheit steuert Serie: Verschlüsseln von Daten bei der Übertragung)

## Verschlüsselung ruhender Daten

Drei Azure-Features ermöglichen die Verschlüsselung „ruhender“ Daten:

- Storage Service Encryption
- Clientseitige Verschlüsselung
- Azure-Datenträgerverschlüsselung

### Storage Service Encryption

Mit Storage Service Encryption können Sie anfordern, dass der Speicherdienst die Daten beim Schreiben in Azure Storage automatisch verschlüsselt. Wenn Sie die Daten aus Azure Storage lesen, werden sie vom Speicherdienst vor der Rückgabe entschlüsselt. So können Sie Daten schützen, ohne dass Sie in Anwendungen Code ändern oder hinzufügen müssen.

[Azure Storage Service Encryption](https://azure.microsoft.com/services/storage/) ist für [Azure-Blobspeicher](https://azure.microsoft.com/services/storage/blobs/) verfügbar. Weitere Informationen zu anderen Typen von Azure-Speicher finden Sie unter [Datei](https://azure.microsoft.com/services/storage/files/), [Datenträger (Storage Premium)](https://azure.microsoft.com/services/storage/premium-storage/), [Tabelle](https://azure.microsoft.com/services/storage/tables/) und [Warteschlange](https://azure.microsoft.com/services/storage/queues/).

Weitere Informationen:

- [Azure Storage Service Encryption für ruhende Daten](../storage/storage-service-encryption.md)

### Clientseitige Verschlüsselung

Wir erwähnten die clientseitige Verschlüsselung bei der Erörterung der Verschlüsselung während der Übertragung. Mit diesem Feature können Sie Ihre Daten in einer Clientanwendung programmgesteuert verschlüsseln, bevor Sie sie an Azure Storage senden, und nach dem Abrufen aus Azure Storage programmgesteuert entschlüsseln.

Das Feature bietet so Verschlüsselung während der Übertragung, doch ermöglicht auch die Verschlüsselung ruhender Daten. Beachten Sie: Obwohl die Daten während der Übertragung verschlüsselt werden, können Sie trotzdem HTTPS einsetzen, um den Vorteil der integrierten Datenintegritätsprüfungen zu nutzen, die Auswirkungen von Netzwerkfehlern auf die Integrität der Daten verringern.

Weitere Informationen:

- [Clientseitige Verschlüsselung und Azure Key Vault für Microsoft Azure Storage](../storage/storage-client-side-encryption.md)

### Azure-Datenträgerverschlüsselung

Mithilfe von Azure Disk Encryption für virtuelle Computer (VMs) können Sie die Sicherheits- und Complianceanforderungen Ihrer Organisation erfüllen, indem Sie die Datenträger Ihrer virtuellen Computer (Start- und sonstige Datenträger) mit Schlüsseln und Richtlinien verschlüsseln, die Sie im [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) steuern.

Datenträgerverschlüsselung für VMs funktioniert für Linux- und Windows-Betriebssysteme. Es wird auch Key Vault verwendet, um Ihnen das Schützen, Verwalten und Überwachen der Nutzung Ihrer Datenträgerverschlüsselungs-Schlüssel zu erleichtern. Alle Daten auf den Datenträgern Ihrer virtuellen Computer werden im Ruhezustand mithilfe von Verschlüsselungstechnologien des Industriestandards in Ihren Azure Storage-Konten verschlüsselt. Die Disk Encryption-Lösung für Windows basiert auf der [Microsoft BitLocker-Laufwerkverschlüsselung](https://technet.microsoft.com/library/cc732774.aspx), die Linux-Lösung basiert auf [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Weitere Informationen:

- [Azure Disk Encryption for Windows and Linux IaaS Virtual Machines (Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer)](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## Speicheranalyse

Für jedes Speicherkonto können Sie die Azure-Speicheranalyse zur Protokollierung und Speicherung von Metrikdaten aktivieren. Dieses Tool eignet sich hervorragend, wenn Sie die Leistungsmetriken eines Speicherkontos überprüfen oder die Behandlung eines Leistungsproblems durchführen möchten.

Außerdem sehen Sie in den Speicheranalyseprotokollen die Authentifizierungsmethode, die von einem Benutzer verwendet wird, wenn er auf den Speicher zugreift. Bei Blob Storage sehen Sie z. B., ob eine SAS oder die Schlüssel des Speicherkontos verwendet wurden, oder ob das Blob, auf das zugegriffen wurde, für den öffentlichen Zugriff verfügbar war.

Weitere Informationen:

- [Speicheranalyse](../storage/storage-analytics.md)
- [Protokollformat der Speicheranalyse](https://msdn.microsoft.com/library/azure/hh343259.aspx)
- [Überwachen eines Speicherkontos im Azure-Portal](../storage/storage-monitor-storage-account.md)
- [End-to-End-Problembehandlung mit Azure-Speichermetriken und -Protokollen sowie AzCopy und Message Analyzer](../storage/storage-e2e-troubleshooting.md)

<!---HONumber=AcomDC_0601_2016-->