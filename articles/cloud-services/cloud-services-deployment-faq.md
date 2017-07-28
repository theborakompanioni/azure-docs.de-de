---
title: "Probleme mit der Bereitstellung in Microsoft Azure Cloud Services – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Dieser Artikel behandelt häufig gestellte Fragen zur Bereitstellung in Microsoft Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 9b788b1d95c821a4bb76cd4dea1d689d36e2f92b
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Probleme mit der Bereitstellung in Microsoft Azure Cloud Services – Häufig gestellte Fragen (FAQs)

Dieser Artikel enthält häufig gestellte Fragen zu Problemen mit der Bereitstellung in [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Informationen zur Größe finden sie auch unter [Größen für Clouddienste](cloud-services-sizes-specs.md) .

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Warum tritt bei der Bereitstellung eines Clouddiensts in einem Stagingslot manchmal ein Fehler bei der Ressourcenzuteilung auf, wenn bereits eine Bereitstellung im Produktionsslot vorhanden ist?
Verfügt ein Clouddienst über eine Bereitstellung in einem der Slots, wird der gesamte Clouddienst mit einem bestimmten Cluster verknüpft. Wenn also bereits eine Bereitstellung im Produktionsslot vorhanden ist, kann eine neue Stagingbereitstellung nur demselben Cluster wie der Produktionsslot zugeordnet werden.

Fehler bei der Zuordnung treten auf, wenn der Cluster, in dem sich der Clouddienst befindet, nicht über ausreichend physische Computeressourcen verfügt, um die Bereitstellungsanfrage zu erfüllen.

Hilfe zur Behandlung solcher Fehler bei der Zuordnung finden Sie unter [Zuordnungsfehler bei Cloud Services: Lösungen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Warum führt das Hochskalieren oder das horizontale Skalieren einer Clouddienstbereitstellung manchmal zu Zuordnungsfehlern?
Wenn ein Clouddienst bereitgestellt ist, wird er normalerweise an einen bestimmten Cluster angeheftet. Das bedeutet, dass beim horizontalen oder Hochskalieren eines vorhandenen Clouddiensts eine Zuweisung neuer Instanzen in demselben Cluster erfolgen muss. Wenn der Cluster seine Kapazität nahezu erreicht hat, oder die gewünschte Größe/der gewünschte Typ des virtuellen Computers nicht verfügbar ist, schlägt die Anforderung möglicherweise fehl.

Hilfe zur Behandlung solcher Fehler bei der Zuordnung finden Sie unter [Zuordnungsfehler bei Cloud Services: Lösungen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Warum führt die Bereitstellung eines Clouddiensts in einer Affinitätsgruppe manchmal zu einem Fehler bei der Zuordnung?
Eine neue Bereitstellung für einen leeren Clouddienst kann von dem Fabric in jedem Cluster in dieser Region zugeordnet werden, es sei denn, der Clouddienst ist mit einer Affinitätsgruppe verknüpft. Bereitstellungen in derselben Affinitätsgruppe werden auch im selben Cluster versucht. Wenn der Cluster seine Kapazität nahezu erreicht hat, schlägt die Anforderung möglicherweise fehl.

Hilfe zur Behandlung solcher Fehler bei der Zuordnung finden Sie unter [Zuordnungsfehler bei Cloud Services: Lösungen](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Warum führt das Ändern der Größe eines virtuellen Computers oder das Hinzufügen eines neuen virtuellen Computers zu einem vorhandenen Clouddienst manchmal zu einem Zuordnungsfehler?
Die Cluster in einem Datencenter können über verschiedene Konfigurationen der Computertypen verfügen (z.B. A-Serie, Av2-Serie, D-Serie, Dv2-Serie, G-Serie, H-Serie, usw.). Allerdings verfügen möglicherweise nicht alle Cluster über alle Arten von virtuellen Computern. Wenn Sie z.B. versuchen, einen virtuellen Computer der D-Serie einem Clouddienst zuzuordnen, der bereits in einem Cluster der A-Serie bereitgestellt ist, tritt ein Zuordnungsfehler auf. Dies geschieht auch dann, wenn Sie versuchen, die SKU-Größe des virtuellen Computers zu ändern (z.B. durch Wechsel von einer A-Serie zu einer D-Serie).

Hilfe zur Behandlung solcher Fehler bei der Zuordnung finden Sie unter [Zuordnungsfehler bei Cloud Services: Lösungen](cloud-services-allocation-failures.md#solutions).

Besuchen Sie die Website [Microsoft Azure: Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services), um die verfügbaren Größen in Ihrer Region zu überprüfen.

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Warum schlägt die Bereitstellung eines Clouddiensts manchmal aufgrund von Grenzwerten/Kontingenten/Einschränkungen in meinem Abonnement oder Dienst fehl?
Die Bereitstellung eines Clouddiensts schlägt manchmal fehl, wenn die erforderlichen Ressourcen für die Zuweisung das maximale oder standardmäßige Kontingent überschreiten, das für Ihren Dienst in Ihrer Region/Datencenterebene zulässig ist. Weitere Informationen finden Sie unter [Grenzwerte für Clouddienste](../azure-subscription-service-limits.md#cloud-services-limits).

Sie können die aktuelle Nutzung oder das aktuelle Kontingent Ihres Abonnements im Portal überprüfen: Azure-Portal => Abonnements => \<entsprechendes Abonnement => „Nutzung + Kontingent“.

Informationen zur Ressourcenauslastung und zum Verbrauch können auch über die Azure-Abrechnungs-APIs abgerufen werden. Weitere Informationen finden Sie unter [Azure-API zur Ressourcennutzung (Preview)](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Wie kann ich die Größe eines bereitgestellten virtuellen Clouddienstcomputers ändern, ohne ihn erneut bereitzustellen?
Die Größe eines virtuellen Clouddienstcomputers kann nicht ohne erneute Bereitstellung geändert werden. Die Größe des virtuellen Computers ist in CSDEF festgelegt. Diese Datei kann nur durch eine erneute Bereitstellung aktualisiert werden.

Weitere Informationen finden Sie unter [Aktualisieren eines Clouddiensts](cloud-services-update-azure-service.md).

 

