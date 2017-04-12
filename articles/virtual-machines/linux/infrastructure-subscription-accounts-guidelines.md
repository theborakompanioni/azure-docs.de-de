---
title: "Abonnement und Konto für Linux-VMs in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für Abonnements und Konten in Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19343826-7eef-42a1-98be-4ec65b0f377a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: b26c1c83d56d75bdcffc44c831eefb9d08e6f04a
ms.lasthandoff: 04/03/2017


---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a>Richtlinien für Azure-Abonnements und -Konten für Linux-VMs

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

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

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Sie können beispielsweise die folgende Struktur verwenden:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Wenn sich eine Region dazu entscheidet, mehr als ein Abonnement einer bestimmten Gruppe zuzuordnen, sollte die Benennungskonvention eine Methode enthalten, mit der die zusätzlichen Daten entweder im Kontonamen oder im Abonnementnamen codiert werden. Diese Organisation ermöglicht Abrechnungsdaten, mit denen die neuen Hierarchieebenen bei den Abrechnungsberichten generiert werden:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Die Organisation kann wie im folgenden Beispiel aussehen:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Wir stellen ausführliche Abrechnungen in einer herunterladbaren Datei für einzelne Konten oder für alle Konten in einem Enterprise Agreement zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]


