---
title: "Leitfaden für das Erstellen einer Lösungsvorlage für den Marketplace | Microsoft Docs"
description: "Detaillierte Anweisungen zum Erstellen, Zertifizieren und Bereitstellen einer Multi-VM-Lösungsvorlage für den Verkauf im Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 4e6d276eb7f7ec1cd4c2c67912d522b383ac3ae2


---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Leitfaden zum Erstellen einer Lösungsvorlage für den Azure Marketplace
Nach Abschluss von Schritt 1, [Erstellen und Registrieren eines Kontos][link-acct-creation], wurden Sie unter [Technische Voraussetzungen für das Erstellen einer Lösungsvorlage](marketplace-publishing-solution-template-creation-prerequisites.md) durch die Erstellung einer mit Azure kompatiblen Lösungsvorlage geleitet. Jetzt werden Sie die Schritte zum Erstellen einer Lösungsvorlage für mehrere virtuelle Computer im [Veröffentlichungsportal][link-pubportal] für den Azure Marketplace durchlaufen.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Erstellen eines Angebots für Ihre Lösungsvorlage im Veröffentlichungsportal
Gehen Sie zu [https://publish.windowsazure.com](http://publish.windowsazure.com). Verwenden Sie bei der erstmaligen Anmeldung beim [Veröffentlichungsportal](https://publish.windowsazure.com/)das gleiche Konto, mit dem das Verkäuferprofil für Ihr Unternehmen registriert wurde. Sie können später im Veröffentlichungsportal einen beliebigen Mitarbeiter Ihres Unternehmens als Co-Administrator hinzufügen.

### <a name="1-select-solution-templates"></a>1. Auswählen von „Lösungsvorlagen“
  ![Abbildung][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Erstellen einer neuen Lösungsvorlage
  ![Abbildung][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Beginnen mit Topologien
Eine Lösungsvorlage ist allen zugehörigen Topologien übergeordnet. Sie können in einem Angebot/einer Lösungsvorlage mehrere Topologien definieren. Wenn ein Angebot in die Stagingumgebung überführt wird, werden alle Topologien einbezogen. Nachfolgend sind die Schritte zum Definieren Ihres Angebots aufgeführt:     

* Erstellen einer Topologie: Der „Topologiebezeichner“ ist in der Regel der Name der Topologie für die Lösungsvorlage. Der Topologiebezeichner wird wie nachstehend gezeigt in der URL verwendet:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}

  Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}
* Fügen Sie eine neue Version hinzu.

### <a name="4-get-your-topology-versions-certified"></a>4. Zertifizieren Ihrer Topologieversionen
Laden Sie eine ZIP-Datei hoch, die alle erforderlichen Dateien zum Bereitstellen der jeweiligen Version der Topologie enthält. Diese ZIP-Datei muss Folgendes enthalten:

* Die Dateien *mainTemplate.json* und *createUiDefinition.json* im Stammverzeichnis.
* Alle verknüpften Vorlagen und erforderlichen Skripts

  > [!TIP]
  > Während Ihre Entwickler die Topologien für die Lösungsvorlagen erstellen und diese zertifizieren lassen, können die Business-, Marketing- und/oder Rechtsabteilungen Ihres Unternehmens mit der Arbeit an den Marketinginhalten und den rechtlichen Inhalten beginnen.
  >
  >

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun Ihre Lösungsvorlage erstellt und die Zipdatei hochgeladen haben, befolgen Sie bitte die Anweisungen in [Abschließen der Angebotserstellung mit Marketinginhalten](marketplace-publishing-push-to-staging.md) , bevor Sie das Angebot in die Stagingumgebung überführen. Alle Artikel zum Veröffentlichen in Marketplace finden Sie unter [Erste Schritte: Veröffentlichen eines Angebots im Microsoft Azure Marketplace](marketplace-publishing-getting-started.md).

Folgende Artikel könnten für Sie ebenfalls von Interesse sein:

* VM-Images: [Grundlegendes zu VM-Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* VM-Erweiterungen: Übersicht über [VM-Agent und VM-Erweiterungen](https://msdn.microsoft.com/library/azure/dn832621.aspx) und [Azure-VM-Erweiterungen und Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)
* Azure Resource Manager: [Erstellen von Azure ARM-Vorlagen](../resource-group-authoring-templates.md) und [Einfache ARM-Vorlagenbeispiele](https://github.com/rjmax/ArmExamples)
* Drosseln von Speicherkonten: [Überwachen der Speicherkontodrosselung](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) und [Storage Premium](../storage/storage-premium-storage.md#premium-storage-scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com



<!--HONumber=Dec16_HO2-->


