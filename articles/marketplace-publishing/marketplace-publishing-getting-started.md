<properties
   pageTitle="Übersicht über das Erstellen und Bereitstellen eines Angebots im Marketplace | Microsoft Azure"
   description="Beschreibung der Schritte, die ausgeführt werden müssen, um sich als zugelassener Microsoft-Entwickler zu qualifizieren und Angebote für VM-Images, Vorlagen, Datendienste oder Entwicklerdienste im Azure Marketplace zu erstellen und bereitzustellen."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/22/2016"
   ms.author="hascipio" />

# Veröffentlichen und Verwalten eines Angebots im Azure Marketplace
Dieser Artikel soll Entwickler dabei unterstützen, ihre im Azure Marketplace aufgeführten Lösungen zu erstellen, bereitzustellen und zu verwalten, damit sie von anderen Azure-Kunden und -Partnern erworben und genutzt werden können.

## Was ist der Azure Marketplace?
Im Azure Marketplace finden Azure-Abonnenten Dienste, die ihnen die Entwicklung lokaler oder cloudbasierter Lösungen und Anwendungen erleichtern. [Azure Certified](http://azure.com/certified)-Dienste dienen als Bausteine für die schnelle Entwicklung einer innovativen Anwendung oder eines Diensts für Branche sowie für andere Azure-Abonnenten.

Als Azure-Herausgeber können Sie Ihre innovative Lösung oder Ihren Dienst über den Azure Marketplace an andere Entwickler, ISVs und IT-Fachleute verkaufen, die ihre cloudbasierten Anwendungen und mobilen Lösungen schnell entwickeln möchten.

## Unterstützte Angebotstypen
Als Herausgeber einer Lösung müssen Sie zunächst definieren, welche Art von Lösung Ihr Unternehmen anbietet. Der Azure Marketplace unterstützt drei Arten von Angeboten:

- **Images virtueller Computer** sind mit einem vollständig installierten Betriebssystem und mindestens einer Anwendung vorkonfiguriert. Ein Image eines virtuellen Computers stellt die erforderlichen Informationen zum Erstellen und Bereitstellen virtueller Computer im Azure Virtual Machines-Dienst bereit.

    >[AZURE.NOTE] **Beispiel**: Als Azure-Herausgeber haben Sie einen virtuellen Computer mit einem innovativen Datenbankdienst erstellt und validiert. Dieser ist für andere Azure-Abonnenten recht interessant, sodass sie diesen virtuellen Computer gerne in ihren Clouddienstumgebungen bereitstellen möchten.

- **Entwicklerdienste** sind vollständig verwaltete Dienste, die bei der Anwendungsentwicklung oder bei der Systemverwaltung verwendet werden können. Sie bieten Funktionen, die eine schnelle Entwicklung von Cloudanwendungen auf Azure ermöglichen.

    >[AZURE.NOTE] **Beispiel**: Als Azure-Herausgeber haben Sie einen Dienst mit API-Zugriff (auf Azure oder woanders gehostet) entwickelt, der Prognosen anhand historischer Daten erstellt. Diesen Dienst möchten auch andere Azure-Abonnenten nutzen, die Lösungen erstellen. Sie können den Dienst im Azure Marketplace bereitstellen, damit er von anderen Abonnenten gefunden, bereitgestellt und in ihrem Dienst verwendet werden kann.

- Eine **Lösungsvorlage** ist eine Datenstruktur, die auf verschiedene Azure-Dienste verweisen kann, darunter Dienste, die von anderen Verkäufern veröffentlicht wurden, um Azure-Abonnenten die Bereitstellung von einem oder auch mehreren Angeboten auf zentrale, koordinierte Weise zu ermöglichen.

    >[AZURE.NOTE] **Beispiel**: Als Azure-Herausgeber haben Sie Dienste aus Azure in einem Satz zusammengestellt, der Ihnen mit nur wenigen Klicks das Erstellen eines sicheren, hochverfügbaren Clouddiensts mit Lastenausgleich ermöglicht. Andere Azure-Abonnenten sparen Zeit, wenn sie diese Lösungsvorlage bereitstellen anstatt die gleichen oder ähnliche Azure-Dienste manuell suchen und konfigurieren zu müssen.

Einige Schritte müssen für alle Lösungstypen ausgeführt werden. In diesem Artikel erhalten Sie eine kurze Übersicht darüber, welche Schritte für die einzelnen Lösungstypen erforderlich sind.

## 1\. Voraussetzungen

> [AZURE.NOTE] Bevor Sie im Azure Marketplace arbeiten können, müssen Sie über eine [Vorabgenehmigung](http://azure.com/certified) verfügen.

1. [Anfordern einer Vorabgenehmigung für das Microsoft Azure Certified-Programm](marketplace-publishing-azure-certification.md)
2. [Erstellen eines Microsoft-Entwicklerkontos](marketplace-publishing-accounts-creation-registration.md)
3. [Allgemeine Voraussetzungen für das Erstellen eines Angebots für den Azure Marketplace](marketplace-publishing-pre-requisites.md)

## 2\. Veröffentlichen Ihres Angebots
### 2\.1 Angebotsspezifische technische Voraussetzungen
- [Technische Voraussetzungen für das Erstellen eines VM-Images für den Azure Marketplace](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Technische Voraussetzungen für das Erstellen einer Lösungsvorlage für den Azure Marketplace](marketplace-publishing-solution-template-creation-prerequisites.md)

### 2\.2 Erstellen Ihres Angebots
1. Erstellen Sie das Angebot, indem Sie die unten angegebenen speziellen Anleitungen verwenden.
    - [Anleitung zum Erstellen eines VM-Images für Azure Marketplace](marketplace-publishing-vm-image-creation.md)
    - [Leitfaden zum Erstellen einer Lösungsvorlage für den Azure Marketplace](marketplace-publishing-solution-template-creation.md)
2. [Abschließen der Angebotserstellung mit Marketinginhalten](marketplace-publishing-push-to-staging.md)

### 2\.3 Testen des Angebots in der Stagingphase
- [Testen des VM-Angebots in der Stagingphase](marketplace-publishing-vm-image-test-in-staging.md)
- [Testen Ihres Angebots einer Lösungsvorlage in der Stagingumgebung](marketplace-publishing-solution-template-test-in-staging.md)

### 2\.4 Bereitstellen des Angebots im Marketplace
- [Bereitstellen Ihres Angebots im Azure Marketplace](marketplace-publishing-push-to-production.md)

### VM-Images ###
- [Erstellen eines lokalen VM-Images](marketplace-publishing-vm-image-creation-on-premise.md)
- [Erstellen eines virtuellen Computers unter Windows im Azure-Vorschauportal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


- [Beheben häufiger Probleme bei der Veröffentlichung im Marketplace](marketplace-publishing-support-common-issues.md)
- Weitere Informationen zu den verwendeten Portalen finden Sie unter [Portale, die Sie benötigen](marketplace-publishing-portals.md).


## 3\. Verwaltung des Angebots nach der Veröffentlichung
- [Postproduktionsleitfaden für Virtual Machine-Angebote im Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md)
- [Aktualisieren der nicht technischen Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku)
- [Aktualisieren der technischen Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#1-how-to-update-the-technical-details-of-a-sku)
- [Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot](marketplace-publishing-vm-image-post-publishing.md#3-how-to-add-a-new-sku-under-a-listed-offer)
- [Ändern der Anzahl der Datenträger für eine aufgelistete SKU](marketplace-publishing-vm-image-post-publishing.md#4-how-to-change-the-data-disk-count-for-a-listed-sku)
- [Löschen eines aufgelisteten Angebots aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#5-how-to-delete-a-listed-offer-from-the-azure-marketplace)
- [Löschen einer aufgelisteten SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#6-how-to-delete-a-listed-sku-from-the-azure-marketplace)
- [Löschen der aktuellen Version einer aufgelisteten SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace)
- [Zurücksetzen des Listenpreises auf Produktionswerte](marketplace-publishing-vm-image-post-publishing.md#8-how-to-revert-listing-price-to-production-values)
- [Zurücksetzen des Abrechnungsmodells auf Produktionswerte](marketplace-publishing-vm-image-post-publishing.md#9-how-to-revert-billing-model-to-production-values)
- [Zurücksetzen der Sichtbarkeitseinstellung einer aufgelisteten SKU auf den Produktionswert](marketplace-publishing-vm-image-post-publishing.md#10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value)
- [Gewusst wie: Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter](marketplace-publishing-csp-incentive.md)
- [Grundlegendes zu Verkäufer-Insights-Berichten](marketplace-publishing-report-seller-insights.md)
- [Grundlegendes zu Azure Marketplace-Auszahlungsberichten](marketplace-publishing-report-payout.md)
- [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)

## Weitere Ressourcen
- [Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md)

<!---HONumber=AcomDC_0824_2016-->