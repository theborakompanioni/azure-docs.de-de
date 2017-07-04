---
title: "Abonnement und Konten für Windows-VMs in Azure | Microsoft-Dokumentation"
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
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: b210c73d577016f465de6d323de7b43f2baf760a
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="azure-subscription-and-accounts-guidelines-for-windows-vms"></a>Richtlinien für Azure-Abonnements und -Konten für Windows-VMs

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

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

Die Organisation kann wie im folgenden Beispiel aussehen:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Wir stellen ausführliche Abrechnungen in einer herunterladbaren Datei für einzelne Konten oder für alle Konten in einem Enterprise Agreement zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


