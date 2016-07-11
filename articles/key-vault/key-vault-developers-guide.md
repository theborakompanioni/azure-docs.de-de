<properties
   pageTitle="Entwicklerhandbuch zu Schlüsseltresor | Microsoft Azure"
   description="Mit Azure-Schlüsseltresor können Entwickler kryptografische Schlüssel in der Microsoft Azure-Umgebung verwalten. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="05/25/2016"
   ms.author="bruceper" />

# Entwicklerhandbuch zu Azure-Schlüsseltresor
Mithilfe des Schlüsseltresors können Sie in Ihren Anwendungen sicher auf vertrauliche Daten zugreifen:

- Schlüssel und geheime Schlüssel werden geschützt, ohne dass Sie den Code selbst schreiben müssen, und können auf einfache Weise in Ihren Anwendungen verwendet werden.
- Ihre Kunden können die Verantwortung für ihre eigenen Schlüssel übernehmen und diese selbst verwalten, sodass Sie sich auf die Bereitstellung der Hauptfunktionen der Software konzentrieren können. Auf diese Weise müssen Ihre Anwendungen weder die Verantwortung noch die Haftung für die Mandantenschlüssel und geheimen Mandantenschlüssel Ihrer Kunden übernehmen.
- Ihre Anwendung kann Signatur- und Verschlüsselungsschlüssel verwenden, wobei die Schlüssel außerhalb der Anwendung verwaltet werden, damit die Lösung auch für eine geografisch verteilte Anwendung geeignet ist.

Allgemeine Informationen zu Azure Key Vault finden Sie unter [Was ist der Schlüsseltresor?](key-vault-whatis.md).

## Videos
Dieses Video zeigt Ihnen, wie Sie selbst einen Schlüsseltresor erstellen und diesen in der Beispielanwendung „Hello Key Vault“ verwenden.

Links zu den im Video erwähnten Ressourcen:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure Key Vault-Beispielcode](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Um weitere Informationen zu erhalten, folgen Sie dem [Key Vault Blog](http://aka.ms/kvblog), und nehmen Sie am [Key Vault Forum](http://aka.ms/kvforum) teil.



> [AZURE.VIDEO azure-key-vault-developer-quick-start]



## Erstellen und Verwalten von Schlüsseltresoren

Bevor Sie mit Azure Key Vault in Ihrem Code arbeiten, können Sie Tresore über REST, Resource Manager-Vorlagen, PowerShell oder die Befehlszeilenschnittstelle (CLI) erstellen und verwalten, wie in den folgenden Artikeln beschrieben:

- [Erstellen und Verwalten von Schlüsseltresoren über REST](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Erstellen und Verwalten von Schlüsseltresoren mit PowerShell](key-vault-get-started.md)
- [Erstellen und Verwalten von Schlüsseltresoren über die Befehlszeilenschnittstelle](key-vault-manage-with-cli.md)
- [Erstellen eines Schlüsseltresors und Hinzufügen eines geheimen Schlüssels über eine ARM-Vorlage](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Vorgänge für Schlüsseltresore werden über AAD authentifiziert und über eine eigene Zugriffsrichtlinie des Schlüsseltresors autorisiert, die pro Tresor definiert wird.

## Programmieren mit dem Schlüsseltresor

Das Schlüsseltresor-Verwaltungssystem für Programmierer besteht aus mehreren Schnittstellen auf der Grundlage von REST: [Key Vault REST API Reference](https://msdn.microsoft.com/library/azure/dn903609.aspx) (Referenz für die Schlüsseltresor-REST-API).

Nach erfolgreicher Autorisierung können Sie folgende Aufgaben ausführen:

- Verwalten kryptografischer Schlüssel unter Verwendung von [Create](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Import](https://msdn.microsoft.com/library/azure/dn903626.aspx), [Update](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Delete](https://msdn.microsoft.com/library/azure/dn903611.aspx) und anderer Befehle

- Verwalten geheimer Schlüssel unter Verwendung von [Get](https://msdn.microsoft.com/library/azure/dn903633.aspx), [Update](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Delete](https://msdn.microsoft.com/library/azure/dn903613.aspx) und anderer Befehle

- Verwenden kryptografischer Schlüssel unter von [Sign](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Verify](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) und [Encrypt](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[Decrypt](https://msdn.microsoft.com/library/azure/dn878097.aspx)

Die folgenden SDKs sind für die Arbeit mit dem Schlüsseltresor verfügbar:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/azure/dn903301.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[Dokumentation zum .NET SDK](https://msdn.microsoft.com/library/azure/dn903301.aspx)|[Dokumentation zum Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK-Paket](https://azure.microsoft.com/documentation/api/)|[Node.js SDK-Paket](https://www.npmjs.com/package/azure-keyvault)|


Vollständige Beispiele für die Verwendung des Schlüsseltresors mit Ihren Anwendungen finden Sie hier:

- .NET-Beispielanwendung *HelloKeyVault* und ein Azure-Webdienstbeispiel. [Codebeispiele für Azure-Schlüsseltresor](http://www.microsoft.com/download/details.aspx?id=45343)
- Tutorial zur Verwendung von Azure Key Vault aus einer Webanwendung in Azure. [Verwenden des Azure-Schlüsseltresors aus einer Webanwendung](key-vault-use-from-web-application.md)

## Vorgehensweisen

Die folgenden Artikel und Szenarios bieten aufgabenspezifische Anleitungen für die Arbeit mit Azure Key Vault:

- [Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault](key-vault-hsm-protected-keys.md): Verwenden Sie diesen Artikel zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit Azure Key Vault.
- [Übergeben sicherer Werte während der Bereitstellung](../resource-manager-keyvault-parameter.md): Wenn Sie während der Bereitstellung einen sicheren Wert (z.B. ein Kennwort) als Parameter übergeben müssen, können Sie diesen Wert als geheimen Schlüssel in Azure Key Vault speichern und in anderen Resource Manager-Vorlagen auf ihn verweisen.
- [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](https://msdn.microsoft.com/library/dn198405.aspx): Der SQL Server-Connector für Azure Key Vault ermöglicht SQL Server und SQL-in-a-VM die Nutzung des Diensts Azure Key Vault als EKM-Anbieter (Extensible Key Management), um die Verschlüsselungsschlüssel für Anwendungslinks zu schützen – transparente Datenverschlüsselung, Verschlüsseln von Sicherungen und Verschlüsselung auf Spaltenebene.
- [Bereitstellen von Zertifikaten für VMs aus Key Vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/): Eine in einer VM in Azure ausgeführte Cloudanwendung benötigt ein Zertifikat. Wie wird dieses Zertifikat derzeit für eine solche VM bereitgestellt?
- [Integrieren von Key Vault mit Azure Disk Encryption für Windows- und Linux-IaaS-VMs](../azure-security-disk-encryption.md): Die Lösung ist in Azure Key Vault integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die geheimen Schlüssel unter Ihrem Schlüsseltresorabonnement steuern und verwalten können. Gleichzeitig können Sie sicherstellen, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand in Ihrem Azure-Speicher verschlüsselt sind.

Aufgabenspezifische Anleitungen zum Integrieren und Verwenden von Schlüsseltresoren mit Azure finden Sie unter [Ryan Jones ARM template examples for Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples) (ARM-Vorlagenbeispiele von Ryan Jones für den Schlüsseltresor).

## Key Vault-Integration

Diese Artikel enthalten Informationen zu anderen Szenarien und Diensten, in denen Key Vault oder eine Key Vault-Integration zum Einsatz kommt.

- [Azure Disk Encryption](../azure-security-disk-encryption.md) nutzt das branchenübliche Feature [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) von Windows und das Feature [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die geheimen Schlüssel unter Ihrem Schlüsseltresorabonnement steuern und verwalten können. Gleichzeitig können Sie sicherstellen, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand in Ihrem Azure-Speicher verschlüsselt sind.


## Unterstützung von Bibliotheken

- Die [Microsoft Azure Key Vault Core Library](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/1.0.0) (Microsoft Azure Key Vault-Kernbibliothek) bietet `IKey`- und `IKeyResolver`-Schnittstellen, um Schlüssel von Bezeichnern zu ermitteln und mit diesen Schlüsseln Vorgänge auszuführen.

- [Microsoft Azure-Schlüsseltresorerweiterungen](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/1.0.0) bieten erweiterte Funktionen für den Azure-Schlüsseltresor.

## Weitere Ressourcen für Schlüsseltresore
- [Key Vault Blog](http://aka.ms/kvblog)
- [Key Vault Forum](http://aka.ms/kvforum)

<!---HONumber=AcomDC_0629_2016-->