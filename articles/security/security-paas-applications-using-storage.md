---
title: "Schützen von PaaS-Anwendungen mit Azure Storage | Microsoft-Dokumentation"
description: " Informationen zu den Best Practices in Azure Storage zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: TomShinder
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 62c7b6706268e3c6e329f90651125fe299f61d67
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="securing-paas-web-and-mobile-applications-using-azure-storage"></a>Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure Storage
In diesem Artikel erläutern wir eine Sammlung empfohlener Vorgehensweisen in Azure Storage zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit Azure und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Der [Azure Storage security guide](../storage/common/storage-security-guide.md) (Azure Storage-Sicherheitsleitfaden, in englischer Sprache) ist eine hervorragende Quelle für ausführliche Informationen zu Azure Storage und Sicherheit.  In diesem Artikel werden in allgemeiner Form einige der Konzepte im Sicherheitsleitfaden erläutert. Er enthält zudem Links zum Sicherheitsleitfaden und anderen Quellen, die weitere Informationen bieten.

## <a name="azure-storage"></a>Azure Storage
Azure bietet Methoden zum Bereitstellen und Verwenden von Speicher, die lokal nicht möglich sind. Sie können in Azure Storage mit relativ geringem Aufwand ein hohes Maß an Skalierbarkeit und Verfügbarkeit erzielen. Azure Storage ist nicht nur die Grundlage für virtuelle Windows- und Linux-Computer in Azure, sondern unterstützt auch große verteilte Anwendungen.

Azure Storage umfasst vier Dienste: Blob Storage, Table Storage, Queue Storage und File Storage. Weitere Informationen finden Sie unter [Introduction to Microsoft Azure Storage](../storage/storage-introduction.md) (Einführung in Microsoft Azure, in englischer Sprache).

## <a name="best-practices"></a>Bewährte Methoden
Dieser Artikel behandelt die folgenden Best Practices:

- Zugriffsschutz:
   - Shared Access Signatures (SAS)
   - Managed Disk
   - Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

- Speicherverschlüsselung:
   - Clientseitige Verschlüsselung wertvoller Daten
   - Azure Disk Encryption für virtuelle Computer (VMs)
   - Storage Service Encryption

## <a name="access-protection"></a>Zugriffsschutz
### <a name="use-shared-access-signature-instead-of-a-storage-account-key"></a>Verwenden von Shared Access Signature statt eines Speicherkontoschlüssels

In einer IaaS-Lösung, in der normalerweise virtuelle Windows Server- oder Linux-Computer ausgeführt werden, erfolgt der Schutz der Dateien vor Offenlegung und Manipulation durch Mechanismen der Zugriffssteuerung. Unter Windows verwenden Sie [Zugriffssteuerungslisten (Access Control Lists, ACLs)](../virtual-network/virtual-networks-acl.md) und unter Linux wahrscheinlich [chmod](https://en.wikipedia.org/wiki/Chmod). Dies sind im Wesentlichen die Methoden, mit denen heute Dateien auf einem Server im eigenen Rechenzentrum geschützt werden.

Dies ist bei PaaS anders. Eine der am häufigsten verwendeten Methoden zum Speichern von Dateien in Microsoft Azure ist [Azure Blob Storage](../storage/storage-dotnet-how-to-use-blobs.md). Blob Storage und andere Arten von File Storage unterscheiden sich in der Datei-E/A und deren Schutzmethoden.

Die Zugriffssteuerung ist von wesentlicher Bedeutung. Zum Steuern des Zugriffs auf Azure Storage erzeugt das System 512-Bit-Speicherkontoschlüssel (Storage Account Keys, SAKs), wenn Sie [ein Speicherkonto erstellen](../storage/common/storage-create-storage-account.md). Der Grad der Schlüsselredundanz ermöglicht Ihnen die Vermeidung von Dienstunterbrechungen während der regelmäßigen Schlüsselrotation.

Speicherzugriffsschlüssel sind Geheimnisse von hoher Priorität, und nur die Personen, die für die Speicherzugriffssteuerung verantwortlich sind, sollten Zugriff auf sie haben. Falls die falschen Personen Zugriff auf diese Schlüssel erlangen, haben sie umfassende Kontrolle über den Speicher und können Dateien im Speicher ersetzen, löschen oder hinzufügen. Hierzu zählen auch Malware und andere Typen von Inhalten, die Ihre Organisation oder Ihre Kunden potenziell gefährden.

Dennoch benötigen Sie ein Verfahren, um Zugriff auf Objekte im Speicher zu gewähren. Für einen genauer abgestimmten Zugriff können Sie die [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS) verwenden. Die SAS ermöglicht es Ihnen, bestimmte Objekte im Speicher für ein vordefiniertes Zeitintervall und mit bestimmten Berechtigungen freizugeben. Mit einer Shared Access Signature können Sie Folgendes definieren:

- Intervallzeitraum der SAS-Gültigkeit, einschließlich Startzeit und Ablaufzeit.
- Berechtigungen, die von der SAS gewährt werden. Mit einer SAS auf einem Blob können einem Benutzer beispielsweise Lese- und Schreibberechtigungen für das Blob gewährt werden, aber keine Löschberechtigungen.
- Eine optionale IP-Adresse oder ein optionaler IP-Adressbereich, für den Azure Storage die SAS akzeptiert. Beispielsweise können Sie einen Bereich mit IP-Adressen angeben, der von Ihrer Organisation genutzt wird. Dies ist eine weitere Sicherheitsmaßnahme für Ihre SAS.
- Das Protokoll, über das Azure Storage die SAS akzeptiert. Sie können diesen optionalen Parameter verwenden, um den Zugriff auf Clients per HTTPS einzuschränken.

Mit SAS können Sie Inhalte auf die gewünschte Weise freigeben, ohne die Speicherkontoschlüssel mitzuteilen. Wenn Sie in Ihrer Anwendung immer SAS verwenden, verfügen Sie über eine sichere Methode zur Freigabe Ihrer Speicherressourcen, ohne dass Sie Kompromisse bei der Sicherheit der Speicherkontoschlüssel eingehen müssen.

Weitere Informationen finden Sie unter [Using shared access signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (Verwenden von Shared Access Signatures (SAS), in englischer Sprache). Weitere Informationen zu potenziellen Risiken und Empfehlungen zum Reduzieren dieser Risiken finden Sie unter [Using shared access signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (Verwenden von Shared Access Signatures (SAS), in englischer Sprache).

### <a name="use-managed-disks-for-vms"></a>Verwenden von Managed Disks für virtuelle Computer

Bei Auswahl von [Azure Managed Disks](../storage/storage-managed-disks-overview.md) übernimmt Azure die Verwaltung der Speicherkonten, die Sie für Ihre VM-Datenträger verwenden. Sie müssen lediglich den Typ des Datenträgers (Premium oder Standard) und die Datenträgergröße auswählen. Der Rest erfolgt automatisch durch Azure Storage. Sie müssen sich keine Gedanken über Beschränkungen der Skalierbarkeit machen, die andernfalls bei mehreren Speicherkonten beachtet werden müssen.

Weitere Informationen finden Sie unter [Frequently Asked Questions about managed and unmanaged premium disks](../storage/storage-faq-for-disks.md) (Häufig gestellte Fragen zu verwalteten und nicht verwalteten Premium-Datenträgern, in englischer Sprache).

### <a name="use-role-based-access-control"></a>Verwenden der rollenbasierten Zugriffssteuerung

Weiter oben wurde erläutert, wie Sie mit einer Shared Access Signature (SAS) anderen Clients eingeschränkten Zugriff auf Objekte in Ihrem Speicherkonto gewähren, ohne Ihren Speicherkontoschlüssel verfügbar zu machen. Manchmal überwiegen die Risiken einer bestimmten Operation für Ihr Speicherkonto gegenüber den Vorzügen von SAS. Manchmal gibt es einfachere Möglichkeiten der Zugriffsverwaltung.

Die Zugriffsverwaltung kann auch mit [rollenbasierter Zugriffssteuerung in Azure](../active-directory/role-based-access-control-what-is.md) (Azure Role-Based Access Control, RBAC) erfolgen. Mit RBAC gewähren Sie Mitarbeitern auf Need-to-Know-Basis und auf Grundlage des Sicherheitsprinzips der geringsten Rechte genau die Berechtigungen, die sie benötigen. Zu viele Berechtigungen können ein Konto zum leichten Angriffsziel machen. Wenn die Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. RBAC begegnet diesem Problem dadurch, dass eine präzise Zugriffsverwaltung für Azure ermöglicht wird. Dies ist für Organisationen zwingend erforderlich, die Sicherheitsrichtlinien für den Datenzugriff durchsetzen möchten.

Sie können integrierte RBAC-Rollen in Azure verwenden, um Benutzern Berechtigungen zuzuweisen. Ziehen Sie die Verwendung von „Speicherkontomitwirkender“ für Cloudoperatoren in Betracht, die Speicherkonten verwalten müssen, und nutzen Sie die Rolle „Klassischer Speicherkontomitwirkender“, um klassische Speicherkonten zu verwalten. Cloudoperatoren, die virtuelle Computer, jedoch nicht das virtuelle Netzwerk oder das Speicherkonto, mit dem sie verbunden sind, verwalten müssen, können Sie der Rolle „Mitwirkender für virtuelle Computer“ zuweisen.

Organisationen, die keine Datenzugriffssteuerung mithilfe von Funktionen wie RBAC erzwingen, erteilen Ihren Benutzern möglicherweise mehr Berechtigungen als erforderlich. Dies kann zur Kompromittierung der Daten führen, da einigen Benutzern Zugriff auf Daten gewährt wird, auf die sie von vorneherein keinen Zugriff haben sollten.

Weitere Informationen über RBAC:

- [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md)
- [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md)
- [Azure Storage security guide](../storage/common/storage-security-guide.md) (Azure Storage-Sicherheitsleitfaden, in englischer Sprache) mit Informationen zum Sichern des Speicherkontos mit RBAC

## <a name="storage-encryption"></a>Speicherverschlüsselung
### <a name="use-client-side-encryption-for-high-value-data"></a>Verwenden von clientseitiger Verschlüsselung für wertvolle Daten

Die clientseitige Verschlüsselung ermöglicht Ihnen das programmgesteuerte Verschlüsseln von Daten während der Übertragung, bevor sie in Azure Storage hochgeladen werden, und das programmgesteuerte Entschlüsseln von Daten beim Abrufen aus dem Speicher.  So können Daten während der Übertragung, jedoch auch ruhende Daten verschlüsselt werden.  Die clientseitige Verschlüsselung ist die sicherste Methode zum Verschlüsseln der Daten, sie erfordert jedoch programmgesteuerte Änderungen an Ihrer Anwendung und die Platzierung von Schlüsselverwaltungsprozessen.

Die clientseitige Verschlüsselung bietet Ihnen außerdem die alleinige Kontrolle über die Verschlüsselungsschlüssel.  Sie können eigene Verschlüsselungsschlüssel generieren und verwalten.  Bei der clientseitigen Verschlüsselung wird ein Umschlagverfahren verwendet. Dabei generiert die Azure Storage-Clientbibliothek einen Inhaltsverschlüsselungsschlüssel (Content Encryption Key, CEK), der dann mit dem Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) umschlossen wird. Der KEK wird anhand eines Schlüsselbezeichners identifiziert und kann ein asymmetrisches Schlüsselpaar oder ein symmetrischer Schlüssel sein. Er kann lokal verwaltet oder im [Azure Key Vault](../key-vault/key-vault-whatis.md) gespeichert werden.

Die clientseitige Verschlüsselung ist in den Java- und .NET-Speicherclientbibliotheken integriert.  Informationen zum Verschlüsseln von Daten in Clientanwendungen sowie zum Generieren und Verwalten eigener Verschlüsselungsschlüssel finden Sie unter [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../storage/storage-client-side-encryption.md) (Clientseitige Verschlüsselung und Azure Key Vault für Microsoft Azure Storage, in englischer Sprache).

### <a name="azure-disk-encryption-for-vms"></a>Azure Disk Encryption für virtuelle Computer
Azure Disk Encryption ist eine Funktion, mit der Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln können. Bei der Azure Disk Encryption werden die Branchenstandardfunktion BitLocker von Windows und die Funktion DM-Crypt von Linux verwendet, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, damit Sie die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement steuern und verwalten können. Diese Lösung stellt außerdem sicher, dass alle ruhenden Daten auf den Datenträgern der virtuellen Computer in Azure Storage verschlüsselt sind.

Siehe [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](azure-security-disk-encryption.md).

### <a name="storage-service-encryption"></a>Storage Service Encryption
Wenn [Speicherdienstverschlüsselung](../storage/storage-service-encryption.md) für File Storage aktiviert ist, werden die Daten automatisch mit AES-256-Verschlüsselung verschlüsselt. Microsoft behandelt die gesamte Verschlüsselung, Entschlüsselung und Schlüsselverwaltung. Dieses Feature ist für die Redundanztypen LRS und GRS verfügbar.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde Ihnen eine Sammlung empfohlener Vorgehensweisen in Azure Storage zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen vorgestellt. Weitere Informationen zum Schutz Ihrer PaaS-Bereitstellungen finden Sie unter:

- [Schützen von PaaS-Bereitstellungen](security-paas-deployments.md)
- [Securing PaaS web and mobile applications using Azure App Services](security-paas-applications-using-app-services.md) (Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Services)
- [Schützen von PaaS-Datenbanken in Azure](security-paas-applications-using-sql.md)

