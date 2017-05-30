---
title: Entwicklerhandbuch zu Azure Key Vault | Microsoft-Dokumentation
description: "Mit Azure-Schlüsseltresor können Entwickler kryptografische Schlüssel in der Microsoft Azure-Umgebung verwalten."
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 05/10/2017
ms.author: bruceper
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: b046e95e2167009727f6ea8f3dd237619c61434f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-key-vault-developers-guide"></a>Entwicklerhandbuch zu Azure-Schlüsseltresor

Mithilfe des Schlüsseltresors können Sie in Ihren Anwendungen sicher auf vertrauliche Daten zugreifen:

- Schlüssel und geheime Schlüssel werden geschützt, ohne dass Sie den Code selbst schreiben müssen, und können auf einfache Weise in Ihren Anwendungen verwendet werden.
- Ihre Kunden können die Verantwortung für ihre eigenen Schlüssel übernehmen und diese selbst verwalten, sodass Sie sich auf die Bereitstellung der Hauptfunktionen der Software konzentrieren können. Auf diese Weise müssen Ihre Anwendungen weder die Verantwortung noch die Haftung für die Mandantenschlüssel und geheimen Mandantenschlüssel Ihrer Kunden übernehmen.
- Ihre Anwendung kann Signatur- und Verschlüsselungsschlüssel verwenden, wobei die Schlüssel außerhalb der Anwendung verwaltet werden, damit die Lösung auch für eine geografisch verteilte App geeignet ist.
- Mit der Key Vault-Version vom September 2016 können Ihre Anwendungen nun auch Key Vault-[Zertifikate](https://docs.microsoft.com/rest/api/keyvault/certificate-operations) nutzen. Weitere Informationen finden Sie im Artikel [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates).

Allgemeine Informationen zu Azure Key Vault finden Sie unter [Was ist der Schlüsseltresor?](key-vault-whatis.md).

## <a name="public-preview---may-10-2017"></a>Öffentliche Vorschau – 10. Mai 2017

>[!NOTE]
>Bei dieser Vorschauversion von Azure Key Vault ist nur die Funktion für **vorläufiges Löschen** in der Vorschau. Insgesamt ist Azure Key Vault ein vollständiger Produktionsdienst.

Diese Vorschau enthält unsere neue Funktion für vorläufiges Löschen, das wiederherstellbare Löschen von Schlüsseltresoren und Key Vault-Objekten und aktualisierte Schnittstellen für Entwickler, [.NET/C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/), [REST](https://docs.microsoft.com/rest/api/keyvault/) und [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/). 

Weitere Informationen zur neuen Funktion für vorläufiges Löschen finden Sie unter [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](key-vault-ovw-soft-delete.md).

## <a name="videos"></a>Videos

Dieses Video zeigt Ihnen, wie Sie selbst einen Schlüsseltresor erstellen und diesen in der Beispielanwendung „Hello Key Vault“ verwenden.

- [Key Vault-Entwickler: Schnellstarthandbuch](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

Im Video erwähnte Ressourcen:

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure Key Vault-Beispielcode](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Erstellen und Verwalten von Schlüsseltresoren

Bevor Sie mit Azure Key Vault in Ihrem Code arbeiten, können Sie Tresore über REST, Resource Manager-Vorlagen, PowerShell oder die Befehlszeilenschnittstelle (CLI) erstellen und verwalten, wie in den folgenden Artikeln beschrieben:

- [Erstellen und Verwalten von Schlüsseltresoren über REST](https://docs.microsoft.com/rest/api/keyvault/)
- [Erstellen und Verwalten von Schlüsseltresoren mit PowerShell](key-vault-get-started.md)
- [Erstellen und Verwalten von Schlüsseltresoren über die Befehlszeilenschnittstelle](key-vault-manage-with-cli2.md)
- [Erstellen eines Schlüsseltresors und Hinzufügen eines Geheimnisses über eine Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> Vorgänge für Schlüsseltresore werden über AAD authentifiziert und über eine eigene Zugriffsrichtlinie des Schlüsseltresors autorisiert, die pro Tresor definiert wird.

## <a name="coding-with-key-vault"></a>Programmieren mit dem Schlüsseltresor

Das Key Vault-Verwaltungssystem für Programmierer besteht aus mehreren Schnittstellen auf Grundlage von REST. Über die REST-Schnittstelle können Sie auf alle Ihre Ressourcen für Schlüsseltresore zugreifen: Schlüssel, Geheimnisse und Zertifikate. [Referenz für die Key Vault-REST-API](https://docs.microsoft.com/rest/api/keyvault/). 

### <a name="supported-programming-languages"></a>Unterstützte Programmiersprachen

#### <a name="net"></a>.NET

- [.NET-API-Referenz für Key Vault](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

Weitere Informationen über die .NET SDK-Version 2.x finden Sie in den [Anmerkungen zu dieser Version](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

- [Java SDK für Key Vault](https://docs.microsoft.com/java/api/com.microsoft.azure.keyvault)

#### <a name="nodejs"></a>Node.js

- [Node.js-API-Referenz für die Key Vault-Verwaltung](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Node.js-API-Referenz für den Key Vault-Betrieb](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/) 

### <a name="quick-start"></a>Schnellstart

- [Erstellen eines Schlüsseltresors](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Erste Schritte mit Key Vault in Node.js](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)

### <a name="code-examples"></a>Codebeispiele

Vollständige Beispiele für die Verwendung von Key Vault mit Ihren Anwendungen finden Sie hier:

- [Azure Key Vault-Codebeispiele](http://www.microsoft.com/download/details.aspx?id=45343): .NET-Beispielanwendung *HelloKeyVault* und ein Azure-Webdienstbeispiel. 
- [Verwenden von Azure Key Vault in einer Webanwendung](key-vault-use-from-web-application.md) In diesem Tutorial erfahren Sie, wie Sie Azure Key Vault in einer Webanwendung in Azure verwenden. 

## <a name="how-tos"></a>Vorgehensweisen

Die folgenden Artikel und Szenarien bieten aufgabenspezifische Anleitungen für die Arbeit mit Azure Key Vault:

- [Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung:](key-vault-subscription-move-fix.md) Wenn Sie Ihr Azure-Abonnement aus Mandant A in Mandant B verschieben, können die Prinzipale (Benutzer und Anwendungen) in Mandant B nicht auf Ihre vorhandenen Schlüsseltresore zugreifen. Gehen Sie wie folgt vor, um dies zu beheben.
- [Zugreifen auf Schlüsseltresore hinter der Firewall:](key-vault-access-behind-firewall.md) Für den Zugriff auf einen Schlüsseltresor muss Ihre Schlüsseltresor-Clientanwendung auf mehrere Endpunkte für unterschiedliche Funktionen zugreifen können.
- [Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault](key-vault-hsm-protected-keys.md) : Verwenden Sie diesen Artikel zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit Azure Key Vault.
- [Übergeben sicherer Werte während der Bereitstellung:](../azure-resource-manager/resource-manager-keyvault-parameter.md) Wenn Sie während der Bereitstellung einen sicheren Wert (z.B. ein Kennwort) als Parameter übergeben müssen, können Sie diesen Wert als Geheimnis in Azure Key Vault speichern und in anderen Resource Manager-Vorlagen auf ihn verweisen.
- [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](https://msdn.microsoft.com/library/dn198405.aspx) : Der SQL Server-Connector für Azure Key Vault ermöglicht SQL Server und SQL-in-a-VM die Nutzung des Diensts Azure Key Vault als EKM-Anbieter (Extensible Key Management), um die Verschlüsselungsschlüssel für Anwendungslinks zu schützen – transparente Datenverschlüsselung, Verschlüsseln von Sicherungen und Verschlüsselung auf Spaltenebene.
- [Bereitstellen von Zertifikaten für VMs aus Key Vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) : Eine in einer VM in Azure ausgeführte Cloudanwendung benötigt ein Zertifikat. Wie wird dieses Zertifikat derzeit für eine solche VM bereitgestellt?
- [Einrichten des Schlüsseltresors mit End-to-End-Schlüsselrotation und Überwachung](key-vault-key-rotation-log-monitoring.md) bietet eine exemplarische Vorgehensweise zum Einrichten der Schlüsselrotation und Protokollüberwachung mit Azure Key Vault.
- [Deploying Azure Web App Certificate through Key Vault]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) (Bereitstellen von Azure-Web-App-Zertifikaten über Key Vault) enthält schrittweise Anleitungen zum Bereitstellen von Zertifikaten, die im Schlüsseltresor gespeichert sind, im Rahmen des [App Service Certificate](https://azure.microsoft.com/blog/internals-of-app-service-certificate/)-Angebots.
- [Grant permission to many applications to access a key vault](key-vault-group-permissions-for-apps.md) (Erteilen von Berechtigungen für viele Anwendungen für den Zugriff auf einen Schlüsseltresor):Die Key Vault-Zugriffssteuerungsrichtlinie unterstützt nur 16 Einträge. Sie können aber eine Azure Active Directory-Sicherheitsgruppe erstellen. Fügen Sie dieser Sicherheitsgruppe alle zugeordneten Dienstprinzipale hinzu, und gewähren Sie dieser Sicherheitsgruppe dann Zugriff auf Key Vault.

Aufgabenspezifische Anleitungen zum Integrieren und Verwenden von Schlüsseltresoren mit Azure finden Sie in den [Beispielen für Azure Resource Manager-Vorlagen für Key Vault von Ryan Jones](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Key Vault-Integration

Diese Artikel enthalten Informationen zu anderen Szenarien und Diensten, in denen Key Vault oder eine Key Vault-Integration zum Einsatz kommt.

- Bei der [Azure Disk Encryption](../security/azure-security-disk-encryption.md) werden das Branchenstandardfeature [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) von Windows und das Feature [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux verwendet, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die geheimen Schlüssel unter Ihrem Schlüsseltresorabonnement steuern und verwalten können. Gleichzeitig können Sie sicherstellen, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand in Ihrem Azure-Speicher verschlüsselt sind.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) bietet eine Option zur Verschlüsselung von Daten, die im Konto gespeichert sind. Für die Schlüsselverwaltung bietet Data Lake Store zwei Modi zur Verwaltung Ihrer Master-Verschlüsselungsschlüssel (Master Encryption Keys, MEKs), die zum Entschlüsseln von Daten erforderlich sind, die im Data Lake Store gespeichert sind. Sie können die MEKs entweder von Data Lake Store für sich verwalten lassen, oder Sie verwenden das Azure Key Vault-Konto und bleiben im Besitz der MEKs. Beim Erstellen eines Data Lake Store-Kontos geben Sie die Art der Schlüsselverwaltung an. 
- [Azure Information Protection](/information-protection/plan-design/plan-implement-tenant-key) ermöglicht Ihnen, Ihren eigenen Mandantenschlüssel zu verwalten. Anstatt z.B. Ihren Mandantenschlüssel von Microsoft verwalten zu lassen (Standard), können Sie Ihren eigenen Mandantenschlüssel verwalten, um bestimmte Vorschriften zu erfüllen, die in Ihrer Organisation gelten. Das Verwalten Ihres eigenen Mandantenschlüssels wird auch als „Bring Your Own Key“ (BYOK) bezeichnet.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault-Übersichten und -Konzepte

- [Key Vault-Sicherheitsumgebungen (Security Worlds)](key-vault-ovw-security-worlds.md)
- [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](key-vault-ovw-soft-delete.md)

## <a name="social"></a>Soziale Netzwerke

- [Key Vault Blog](http://aka.ms/kvblog)
- [Key Vault Forum](http://aka.ms/kvforum)


## <a name="supporting-libraries"></a>Unterstützung von Bibliotheken

- Die [Microsoft Azure Key Vault-Kernbibliothek](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) bietet die Schnittstellen **IKey** und **IKeyResolver**, um Schlüssel von Bezeichnern zu ermitteln und mit diesen Schlüsseln Vorgänge auszuführen.
- [Microsoft Azure Key Vault-Erweiterungen](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) bieten erweiterte Funktionen für Azure Key Vault.

## <a name="other-key-vault-resources"></a>Weitere Ressourcen für Schlüsseltresore


