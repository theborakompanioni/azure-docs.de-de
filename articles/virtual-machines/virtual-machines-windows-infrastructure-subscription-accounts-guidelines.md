---
title: "Richtlinien für Abonnements und Konten | Microsoft Docs"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für Abonnements und Konten in Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 761fa847-78b0-4078-a33a-d95d198d1029
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b410436de11550aab2160421abfb27107f7874b0


---
# <a name="subscription-and-accounts-guidelines"></a>Richtlinien für Abonnements und Konten
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel wird erläutert, wie Sie die Abonnement- und-Kontoverwaltung bewältigen können, wenn die Umgebung und die Benutzerbasis wachsen.

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementierungsrichtlinien für Konten und Abonnements
Entscheidungen:

* Welche Abonnementgruppen und Konten benötigen Sie, um Ihre IT-Workload oder -Infrastruktur zu hosten?
* Wie unterteilen Sie die Hierarchie passend für Ihre Organisation?

Aufgaben:

* Definieren Sie die logische Organisationshierarchie so, wie Sie sie über die Abonnementebene verwalten möchten.
* Definieren Sie die erforderlichen Konten und die Abonnements in jedem Konto entsprechend dieser logischen Hierarchie.
* Erstellen Sie die Gruppe von Abonnements und Konten mit Ihrer Benennungskonvention.

## <a name="subscriptions-and-accounts"></a>Abonnements und Konten
Für die Arbeit mit Azure benötigen Sie mindestens ein Azure-Abonnement. Ressourcen, wie virtuelle Computer (VMs) oder virtuelle Netzwerke, sind in diesen Abonnements vorhanden.

* Unternehmenskunden verwenden i. d. R. eine Unternehmensanmeldung, die die oberste Ressource in der Hierarchie darstellt und einem oder mehreren Konten zugeordnet ist.
* Für Benutzer und Kunden ohne Unternehmensanmeldung ist die oberste Ressource das Konto.
* Abonnements sind Konten zugeordnet, und jedes Konto kann über mehrere Abonnements verfügen. Azure zeichnet Abrechnungsinformationen auf Abonnementebene auf.

Aufgrund der maximal zwei Hierarchieebenen für die Konto-/Abonnement-Beziehung ist es wichtig, die Benennungskonvention für Konten und Abonnements an die Abrechnungsanforderungen anzupassen. Wenn z.B. ein globales Unternehmen Azure verwendet, nutzt dieses pro Region möglicherweise ein Konto, und die Abonnements werden auf Regionsebene verwaltet:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Sie können beispielsweise die folgende Struktur verwenden:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Wenn sich eine Region dazu entscheidet, mehr als ein Abonnement einer bestimmten Gruppe zuzuordnen, sollte die Benennungskonvention eine Methode enthalten, mit der die zusätzlichen Daten entweder im Kontonamen oder im Abonnementnamen codiert werden. Diese Organisation ermöglicht Abrechnungsdaten, mit denen die neuen Hierarchieebenen bei den Abrechnungsberichten generiert werden:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Die Organisation kann folgendermaßen aussehen:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Wir stellen ausführliche Abrechnungen in einer herunterladbaren Datei für einzelne Konten oder für alle Konten in einem Enterprise Agreement zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Nov16_HO3-->


