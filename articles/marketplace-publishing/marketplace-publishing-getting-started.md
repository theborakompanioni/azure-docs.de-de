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
   ms.date="07/05/2016"
   ms.author="hascipio" />

# Veröffentlichen und Verwalten eines Angebots im Azure Marketplace
Dieser Artikel soll Entwickler dabei unterstützen, ihre im Azure Marketplace aufgeführten Lösungen zu erstellen, bereitzustellen und zu verwalten, damit sie von anderen Azure-Kunden und -Partnern erworben und genutzt werden können.

Als Herausgeber einer Lösung müssen Sie zunächst definieren, welche Art von Lösung Ihr Unternehmen anbietet. Der Azure Marketplace unterstützt verschiedene Lösungen, und für jeden Lösungstyp müssen für eine erfolgreiche Veröffentlichung im Marketplace geringfügig andere Schritte ausgeführt werden.

## Angebotstypen
|Angebotstyp| Definition |
|---|---|
|VM-Image | Vorkonfiguriertes Image eines virtuellen Computers mit einem vollständig installierten Betriebssystem und mindestens einer Anwendung. VM-Imageangebote können ein einzelnes VM-Image umfassen oder aus mehreren VM-Images bestehen, die mittels einer Lösungsvorlage verbunden sind. Ein Image eines virtuellen Computers („Image“) stellt die erforderlichen Informationen zum Erstellen und Bereitstellen virtueller Computer im Azure Virtual Machines-Dienst bereit. Ein Image besteht aus einer virtuellen Festplatte mit einem Betriebssystem und ggf. einer oder mehreren virtuellen Festplatten für Daten. Kunden können mit einem einzelnen Image eine beliebige Anzahl von virtuellen Computern bereitstellen.|
|Entwicklerdienst| Vollständig verwalteter Dienst für Information Worker, Geschäftsanalysten, Entwickler oder IT-Experten zur Verwendung bei der Entwicklung von Kundenanwendungen oder bei der Systemverwaltung. Entwicklerdienste bieten Funktionen, mit denen Kunden im Handumdrehen cloudfähige Anwendungen in Azure entwickeln können. Für den Kauf von Entwicklerdiensten benötigen Kunden ein Azure-Abonnement. Die Herausgeber sind dafür verantwortlich, die Nutzung von Entwicklerdiensten durch Kunden zu messen und Nutzungsinformationen an Microsoft zu melden, wie in der Microsoft Azure Marketplace-Herausgebervereinbarung festgelegt.|
|Lösungsvorlage|Eine „ARM-Lösungsvorlage“ (Azure Resource Manager) ist eine Datenstruktur, die auf verschiedene Angebote verweisen kann, darunter Angebote, die von anderen Herausgebern veröffentlicht wurden, um Azure-Kunden die Bereitstellung von einem oder auch mehreren Angeboten auf zentrale, koordinierte Weise zu ermöglichen.|

Einige Schritte müssen für alle Lösungstypen ausgeführt werden. In diesem Artikel erhalten Sie eine kurze Übersicht darüber, welche Schritte für die einzelnen Lösungstypen erforderlich sind.

## 1\. Voraussetzungen

> [AZURE.NOTE] Bevor Sie im Azure Marketplace arbeiten können, müssen Sie über eine Vorabgenehmigung verfügen. Dies gilt nicht für Herausgeber von Datendiensten.

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
- [Post-production guide for virtual machine offers in the Azure Marketplace (Postproduktionsleitfaden für VM-Angebote in Azure Marketplace)](marketplace-publishing-vm-image-post-publishing.md)
- [Aktualisieren der nicht technischen Details eines Angebots oder einer SKU](marketplace-publishing-vm-image-post-publishing.md#2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku)
- [Löschen eines Liveangebots oder einer SKU aus dem Azure Marketplace](marketplace-publishing-vm-image-post-publishing.md#4-how-to-delete-a-live-offer-or-sku-from-the-azure-marketplace)
- [Gewusst wie: Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter](marketplace-publishing-csp-incentive.md)
- [Grundlegendes zu Verkäufer-Insights-Berichten](marketplace-publishing-report-seller-insights.md)
- [Grundlegendes zu Azure Marketplace-Auszahlungsberichten](marketplace-publishing-report-payout.md)
- [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)

## Zusätzliche Ressourcen
- [Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md)

<!---HONumber=AcomDC_0706_2016-->