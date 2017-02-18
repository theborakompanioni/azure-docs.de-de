---
title: "Ressourcengruppen für Linux-VMs in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung von Ressourcengruppen in Azure-Infrastrukturdiensten."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 93ab9d93-965a-46b9-9c87-a10d652a6422
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 233116deaaaf2ac62981453b05c4a5254e836806
ms.openlocfilehash: adf382fef7a83d31f35321187c7c5d20c48db6ef


---
# <a name="azure-resource-group-guidelines"></a>Richtlinien für Azure-Ressourcengruppen
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

In diesem Artikel erfahren Sie, wie Sie Ihre Umgebung logisch aufbauen und alle Komponenten in Ressourcengruppen zusammenfassen.

## <a name="implementation-guidelines-for-resource-groups"></a>Implementierungsrichtlinien für Ressourcengruppen
Entscheidungen:

* Werden Sie Ressourcengruppen anhand der zentralen Infrastrukturkomponenten oder für eine vollständige Anwendungsbereitstellung aufbauen?
* Müssen Sie den Zugriff auf Ressourcengruppen mithilfe der rollenbasierten Zugriffssteuerung beschränken?

Aufgaben:

* Legen Sie fest, welche zentralen Infrastrukturkomponenten und dedizierten Ressourcengruppen Sie benötigen.
* Prüfen Sie, wie Resource Manager-Vorlagen für konsistente, reproduzierbare Bereitstellungen implementiert werden können.
* Legen Sie fest, welche Benutzerzugriffsrollen Sie zur Steuerung des Zugriffs auf Ressourcengruppen benötigen.
* Erstellen Sie den Satz von Ressourcengruppen anhand Ihrer Benennungskonvention. Sie können die Azure-Befehlszeilenschnittstelle oder das Portal verwenden.

## <a name="resource-groups"></a>Ressourcengruppen
In Azure fassen Sie zusammengehörige Ressourcen wie Speicherkonten, virtuelle Netzwerke und virtuelle Computer (VMs) logisch zusammen, um sie als eine Einheit bereitzustellen, zu verwalten und zu warten. Auf diese Weise können Sie Anwendungen leichter bereitstellen und gleichzeitig alle zugehörigen Ressourcen verwaltungstechnisch zusammenhalten oder anderen Benutzern Zugriff auf die Ressourcengruppe gewähren. Ausführlichere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

Ressourcengruppen bietet den wesentlichen Vorteil, dass Sie Ihre Umgebung mithilfe einer JSON-Datei erstellen können, die Speicher-, Netzwerk- und Computeressourcen deklariert. Sie können auch zugehörige benutzerdefinierte Skripts oder anzuwendende Konfigurationen definieren. Mithilfe solcher JSON-Vorlagen erstellen Sie konsistente, reproduzierbare Bereitstellungen für Ihre Anwendungen. Dieser Ansatz ermöglicht die Erstellung einer Umgebung für die Entwicklung und die anschließende Verwendung derselben Vorlage für die Erstellung einer Produktionsbereitstellung (oder umgekehrt). Um die Verwendung von Vorlagen besser zu verstehen, können Sie die [exemplarische Vorgehensweise zu Vorlagen](../resource-manager-template-walkthrough.md) durchlaufen, die Sie schrittweise durch die Erstellung einer JSON-Vorlage führt.

Sie können beim Entwerfen Ihrer Umgebung mit Ressourcengruppen zwischen zwei verschiedenen Ansätzen wählen:

* Ressourcengruppen für jede Anwendungsbereitstellung, die Speicherkonten, virtuelle Netzwerke und Subnetze, virtuelle Computer, Load Balancer usw. kombinieren.
* Zentralisierte Ressourcengruppen, die Ihre wesentlichen virtuellen Netzwerke und Subnetze bzw. Speicherkonten enthalten. Ihre Anwendungen befinden sich dann in eigenen Ressourcengruppen, die nur virtuelle Computer, Load Balancer, Netzwerkschnittstellen usw. enthalten.

Wenn Sie Ihre Umgebung horizontal hochskalieren möchten, vereinfachen zentralisierte Ressourcengruppen für Ihre virtuellen Netzwerke und Subnetze die Erstellung von standortübergreifenden Netzwerkverbindungen für hybride Verbindungsoptionen. Der alternative Ansatz für jede Anwendung ist das Arbeiten mit einem eigenen virtuellen Netzwerk, das konfiguriert und gewartet werden muss. [Rollenbasierte Zugriffssteuerungen](../active-directory/role-based-access-control-what-is.md) bieten eine differenzierte Möglichkeit, den Zugriff auf Ressourcengruppen zu steuern. Bei Produktionsanwendungen können Sie steuern, welche Benutzer auf diese Ressourcen zugreifen dürfen. Für die zentralen Infrastrukturressourcen können Sie festlegen, dass ausschließlich Infrastrukturtechniker mit Infrastrukturressourcen arbeiten dürfen. Ihre Anwendungsbesitzer haben nur Zugriff auf die Anwendungskomponenten innerhalb ihrer Ressourcengruppe, nicht aber auf die zentrale Azure-Infrastruktur Ihrer Umgebung. Berücksichtigen Sie beim Entwerfen Ihrer Umgebung die Benutzer, die auf die Ressourcen zugreifen müssen, und entwerfen Sie Ihre Ressourcengruppen entsprechend. 

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Jan17_HO5-->


