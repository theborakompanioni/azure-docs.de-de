---
title: "Übersicht über das Erstellen und Bereitstellen eines Angebots im Marketplace | Microsoft Docs"
description: "Beschreibung der Schritte, die ausgeführt werden müssen, um sich als zugelassener Microsoft-Entwickler zu qualifizieren und Angebote für VM-Images, Vorlagen, Datendienste oder Entwicklerdienste im Azure Marketplace zu erstellen und bereitzustellen."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
translationtype: Human Translation
ms.sourcegitcommit: b76185c0a4a0e17b663affee9a02b65f222fedeb
ms.openlocfilehash: d679096476406831c1fda4f695adff84e63d6ae8


---
# <a name="how-to-publish-and-manage-an-offer-in-the-azure-marketplace"></a>Veröffentlichen und Verwalten eines Angebots im Azure Marketplace
Dieser Artikel soll Entwickler dabei unterstützen, ihre im Azure Marketplace aufgeführten Lösungen zu erstellen, bereitzustellen und zu verwalten, damit sie von anderen Azure-Kunden und -Partnern erworben und genutzt werden können.

## <a name="what-is-the-azure-marketplace"></a>Was ist der Azure Marketplace?
Als Azure-Herausgeber können Sie Ihre innovative Lösung oder Ihren Dienst über den Azure Marketplace an andere Entwickler, ISVs und IT-Fachleute verkaufen, die ihre cloudbasierten Anwendungen und mobilen Lösungen schnell entwickeln möchten. Falls sich Ihre Lösung an Geschäftsbenutzer richtet, sollten Sie ggf. den [AppSource](http://appsource.microsoft.com)-Marketplace in Betracht ziehen.


## <a name="supported-types-of-solutions"></a>Unterstützte Lösungsarten
Als Herausgeber einer Lösung müssen Sie zunächst definieren, welche Art von Lösung Ihr Unternehmen anbietet. Der Azure Marketplace unterstützt folgende Angebotsarten:

|Lösungstyp|Virtual Machine|Lösungsvorlage|
|---|---|---|
|Definition|Vorkonfigurierte Images mit einem vollständig installierten Betriebssystem und mindestens einer Anwendung. Ein Image eines virtuellen Computers stellt die erforderlichen Informationen zum Erstellen und Bereitstellen virtueller Computer im Azure Virtual Machines-Dienst bereit.|Eine Datenstruktur, die auf verschiedene Azure-Dienste verweisen kann, darunter Dienste, die von anderen Verkäufern veröffentlicht wurden, um Azure-Abonnenten die Bereitstellung von einem oder auch mehreren Angeboten auf zentrale, koordinierte Weise zu ermöglichen.|
|Beispiel|**Beispiel**: Als Azure-Herausgeber haben Sie einen virtuellen Computer mit einem innovativen Datenbankdienst erstellt und validiert. Dieser ist für andere Azure-Abonnenten recht interessant, sodass sie diesen virtuellen Computer gerne in ihren Clouddienstumgebungen bereitstellen möchten.|**Beispiel**: Als Azure-Herausgeber haben Sie einige Dienste aus Azure zusammengefasst und können nun im Handumdrehen Clouddienste mit Lastenausgleich, erweiterter Sicherheit und hoher Verfügbarkeit bereitstellen. Andere Azure-Abonnenten können die Lösungsvorlage verwenden, die ihre Anforderungen erfüllt, anstatt die gleichen oder ähnliche Azure-Dienste manuell zu suchen, zu beschaffen, bereitzustellen und zu konfigurieren. Das spart Zeit.|

> [!NOTE]
> Einige Schritte sind bei den unterschiedlichen Lösungsarten gleich, andere sind lösungstypabhängig. In diesem Artikel erhalten Sie eine kurze Übersicht darüber, welche Schritte für die einzelnen Lösungstypen erforderlich sind.

## <a name="how-to-publish-a-solution"></a>Veröffentlichen einer Lösung
![Zeichnen](media/marketplace-publishing-getting-started/img01.png)

### <a name="1-nominate-your-solution-for-pre-approval"></a>1. Nominieren Ihrer Lösung für eine Vorabgenehmigung
- Füllen Sie [hier](https://createopportunity.azurewebsites.net) für Ihre Lösung das Nominierungsformular für **Microsoft Azure Certified für virtuelle Computer** aus.

>[!NOTE]
> Wenn Sie mit einem Partner Account Manager oder mit einem DX Partner Manager zusammenarbeiten, bitten Sie diesen, Ihre Lösung für das Azure Certified-Programm zu nominieren. Alternativ können Sie auch auf der Webseite [Microsoft Azure Certified](http://createopportunity.azurewebsites.net) das Bewerbungsformular ausfüllen und die E-Mail-Adresse Ihres Partner Account Managers oder DX Partner Managers in das Kontaktfeld für den Microsoft-Sponsor eingeben.

Wenn die Teilnahmekriterien gemäß den [Teilnahmerichtlinien für Microsoft Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833) erfüllt sind und Ihre Bewerbung angenommen wird, arbeiten wir gemeinsam mit Ihnen an der Aufnahme Ihrer Lösung in den Azure Marketplace.

### <a name="2-register-your-account-as-a-microsoft-seller"></a>2. Registrieren Ihres Kontos als Microsoft-Verkäufer
- [Registrieren Sie Ihr Microsoft-Konto als Microsoft Developer-Konto.](marketplace-publishing-accounts-creation-registration.md)

### <a name="3-publish-your-solution"></a>3. Veröffentlichen Ihrer Lösung
1. Erfüllen nicht technischer Voraussetzungen
  - [Allgemeine Voraussetzungen für das Erstellen eines Angebots für den Azure Marketplace](marketplace-publishing-pre-requisites.md)
  - [Technische Voraussetzungen für das Erstellen eines VM-Images für den Azure Marketplace](marketplace-publishing-vm-image-creation-prerequisites.md)
  - [Technische Voraussetzungen für das Erstellen einer Lösungsvorlage für den Azure Marketplace](marketplace-publishing-solution-template-creation-prerequisites.md)
2. Erstellen Ihres Angebots
  - [Virtueller Computer](marketplace-publishing-vm-image-creation.md)
  - [Lösungsvorlage](marketplace-publishing-solution-template-creation.md)
3. [Abschließen der Angebotserstellung mit Marketinginhalten](marketplace-publishing-push-to-staging.md)
4. Testen Ihres Angebots in der Stagingphase
  - [Testen des VM-Angebots in der Stagingphase](marketplace-publishing-vm-image-test-in-staging.md)
  - [Testen Ihres Angebots einer Lösungsvorlage in der Stagingumgebung](marketplace-publishing-solution-template-test-in-staging.md)
5. [Bereitstellen Ihres Angebots im Azure Marketplace](marketplace-publishing-push-to-production.md)


### <a name="virtual-machine-image-specific"></a>VM-Images
* [Erstellen eines lokalen VM-Images](marketplace-publishing-vm-image-creation-on-premise.md)
* [Erstellen eines virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Erstellen eines virtuellen Linux-Computers im Azure-Portal](../virtual-machines/virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Behandlung häufig auftretender Probleme bei der Erstellung virtueller Festplatten](marketplace-publishing-vm-image-creation-troubleshooting.md)

## <a name="how-to-manage-your-solution"></a>Verwalten Ihrer Lösung
* [Postproduktionsleitfaden für Virtual Machine-Angebote im Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md)
* [Aktualisieren der nicht technischen Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku)
* [Aktualisieren der technischen Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#1-how-to-update-the-technical-details-of-a-sku)
* [Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot](marketplace-publishing-vm-image-post-publishing.md#3-how-to-add-a-new-sku-under-a-listed-offer)
* [Ändern der Anzahl der Datenträger für eine aufgelistete SKU](marketplace-publishing-vm-image-post-publishing.md#4-how-to-change-the-data-disk-count-for-a-listed-sku)
* [Löschen eines aufgelisteten Angebots aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md)
* [Löschen einer aufgelisteten SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#6-how-to-delete-a-listed-sku-from-the-azure-marketplace)
* [Löschen der aktuellen Version einer aufgelisteten SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace)
* [Zurücksetzen des Listenpreises auf Produktionswerte](marketplace-publishing-vm-image-post-publishing.md#8-how-to-revert-listing-price-to-production-values)
* [Zurücksetzen des Abrechnungsmodells auf Produktionswerte](marketplace-publishing-vm-image-post-publishing.md#9-how-to-revert-billing-model-to-production-values)
* [Zurücksetzen der Sichtbarkeitseinstellung einer aufgelisteten SKU auf den Produktionswert](marketplace-publishing-vm-image-post-publishing.md#10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value)
* [Gewusst wie: Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter](marketplace-publishing-csp-incentive.md)
* [Grundlegendes zu Verkäufer-Insights-Berichten](marketplace-publishing-report-seller-insights.md)
* [Grundlegendes zu Azure Marketplace-Auszahlungsberichten](marketplace-publishing-report-payout.md)
* [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>Weitere Ressourcen
* [Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md)



<!--HONumber=Feb17_HO3-->


