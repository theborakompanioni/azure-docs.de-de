---
title: "Übersicht über das Erstellen und Bereitstellen eines Angebots im Marketplace | Microsoft Docs"
description: "Beschreibung der Schritte, die ausgeführt werden müssen, um sich als zugelassener Microsoft-Entwickler zu qualifizieren und Angebote für VM-Images, Vorlagen, Datendienste oder Entwicklerdienste im Azure Marketplace zu erstellen und bereitzustellen"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 87bd4e1d2b51857526b9137c09b7013c5221c698
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>Veröffentlichen und Verwalten eines Angebots im Azure Marketplace
Dieser Artikel soll Entwickler dabei unterstützen, ihre im Azure Marketplace aufgeführten Lösungen zu erstellen, bereitzustellen und zu verwalten, damit sie von anderen Azure-Kunden und -Partnern erworben und genutzt werden können.

## <a name="marketplace-publishing"></a>Marketplace-Veröffentlichung
Als Azure-Herausgeber können Sie Ihre innovative Lösung oder Ihren innovativen Dienst an andere Entwickler, ISVs und IT-Profis im Marketplace verteilen und verkaufen. Über den Marketplace können Sie Kunden erreichen, die schnell eigene cloudbasierte Anwendungen und mobile Lösungen entwickeln möchten. Falls sich Ihre Lösung an Geschäftsbenutzer richtet, sollten Sie ggf. den [AppSource](http://appsource.microsoft.com)-Marketplace in Betracht ziehen.


## <a name="supported-types-of-solutions"></a>Unterstützte Lösungsarten
Als Herausgeber einer Lösung müssen Sie zunächst definieren, welche Art von Lösung Ihr Unternehmen anbietet. Der Marketplace unterstützt folgende Angebotsarten:

|Lösungstyp|Virtueller Computer|Lösungsvorlage|
|---|---|---|
|**Definition**|Vorkonfigurierte Images mit einem vollständig installierten Betriebssystem und mindestens einer Anwendung. Ein Image eines virtuellen Computers stellt die erforderlichen Informationen zum Erstellen und Bereitstellen virtueller Computer im Azure Virtual Machines-Dienst bereit.|Eine Datenstruktur, die auf einen oder mehrere bestimmte Azure-Dienste verweisen kann, unter anderem auf von anderen Verkäufern veröffentlichte Dienste. Azure-Abonnenten können diese verwenden, um ein oder mehrere Angebote zentral auf koordinierte Weise bereitzustellen.|
|**Beispiel**|Als Azure-Herausgeber haben Sie eine VM mit einem innovativen Datenbankdienst erstellt und überprüft. Andere Azure-Abonnenten möchten diese VM erwerben und in ihren Clouddienstumgebungen bereitstellen.|Als Azure-Herausgeber haben Sie einige Dienste aus Azure zusammengefasst und können nun im Handumdrehen Clouddienste mit Lastenausgleich, erweiterter Sicherheit und hoher Verfügbarkeit bereitstellen. Andere Azure-Abonnenten können Zeit sparen, indem sie die Lösungsvorlage erwerben, die ihr Ziel erfüllt. Sie müssen dann nicht dieselben oder ähnliche Azure-Dienste manuell suchen, beschaffen, bereitstellen und konfigurieren.|

> [!NOTE]
> Einige Schritte sind bei den unterschiedlichen Lösungstypen gleich, andere sind lösungstypabhängig. In diesem Artikel erhalten Sie eine kurze Übersicht darüber, welche Schritte für die einzelnen Lösungstypen erforderlich sind.

## <a name="publish-a-solution"></a>Veröffentlichen einer Lösung
![Nominieren, Registrieren, Veröffentlichen](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Nominieren Ihrer Lösung für eine Vorabgenehmigung
Um einen virtuellen Computer als [Lösung](https://createopportunity.azurewebsites.net) im Marketplace zu veröffentlichen, füllen Sie das **Solution Nomination Form** für Microsoft Azure Certified aus.

>[!NOTE]
> Wenn Sie mit einem Partner Account Manager oder einem DX Partner Manager zusammenarbeiten, bitten Sie diesen, Ihre Lösung für das Azure Certified-Programm zu nominieren. Sie können auch die Webseite [Microsoft Azure Certified](http://createopportunity.azurewebsites.net) besuchen und das Bewerbungsformular ausfüllen. Geben Sie die E-Mail-Adresse Ihres Partner Account Managers oder DX Partner Managers in das Feld **Microsoft Sponsor Contact** ein.

Wenn Sie die Teilnahmekriterien gemäß den [Teilnahmerichtlinien für Microsoft Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833) erfüllen und Ihre Bewerbung angenommen wird, arbeiten wir gemeinsam mit Ihnen an der Aufnahme Ihrer Lösung in den Marketplace.

### <a name="register-your-account-as-a-microsoft-seller"></a>Registrieren Ihres Kontos als Microsoft-Verkäufer
Registrieren Sie Ihr Microsoft-Konto als [Microsoft Developer-Konto](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Veröffentlichen Ihrer Lösung
Gehen Sie folgendermaßen vor, um eine Lösung im Marketplace zu veröffentlichen:
1. Stellen Sie sicher, dass die nicht technischen Voraussetzungen erfüllt sind.

    a. Stellen Sie sicher, dass die [nicht technischen Voraussetzungen](marketplace-publishing-pre-requisites.md) erfüllt sind.

    b. Stellen Sie sicher, dass die [technischen Voraussetzungen für die VM](marketplace-publishing-vm-image-creation-prerequisites.md) erfüllt sind.

    c. Stellen Sie sicher, dass die [technischen Voraussetzungen für die Lösungsvorlage](marketplace-publishing-solution-template-creation-prerequisites.md) erfüllt sind.

2. Erstellen Sie Ihr Angebot.

    a. Erstellen Sie ein Angebot eines [virtuellen Computers](marketplace-publishing-vm-image-creation.md).

    b. Erstellen Sie ein Angebot einer [Lösungsvorlage](marketplace-publishing-solution-template-creation.md).

3. Erstellen Sie die [Marketinginhalte](marketplace-publishing-push-to-staging.md) zu Ihrem Angebot.

4. Testen Sie Ihr Angebot in der Stagingumgebung.

    a. Testen Sie Ihr VM-Angebot in der [Stagingumgebung](marketplace-publishing-vm-image-test-in-staging.md).

    b. Testen Sie Ihr Angebot einer Lösungsvorlage in der [Stagingumgebung](marketplace-publishing-solution-template-test-in-staging.md).

5. Stellen Sie Ihr Angebot im [Marketplace](marketplace-publishing-push-to-production.md) bereit.


### <a name="create-and-manage-a-virtual-machine-image"></a>Erstellen und Verwalten eines Images eines virtuellen Computers
Erstellen und verwalten Sie ein VM-Image mithilfe der folgenden Ressourcen:
* Erstellen Sie ein [lokales](marketplace-publishing-vm-image-creation-on-premise.md) VM-Image.
* Erstellen Sie einen [virtuellen Windows-Computer im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Erstellen Sie einen [virtuellen Linux-Computer im Azure-Portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Behandeln Sie häufig auftretende Probleme bei der [Erstellung virtueller Festplatten](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Verwalten Ihrer Lösung
Verwalten Sie Ihre Lösung mithilfe folgender Ressourcen:
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
* [Anzeigen und Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter im Azure Marketplace](marketplace-publishing-csp-incentive.md)
* [Grundlegendes zu Azure Marketplace-Auszahlungsberichten](marketplace-publishing-report-payout.md)
* [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md)

