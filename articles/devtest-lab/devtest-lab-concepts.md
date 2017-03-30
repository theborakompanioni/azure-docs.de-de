---
title: DevTest Labs-Konzepte | Microsoft Docs
description: "Lernen Sie die grundlegenden Konzepte von DevTest Labs kennen, und erfahren Sie, wie Sie virtuelle Azure-Computer damit einfach erstellen, verwalten und überwachen können."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 236d38fcda90643992f66c9b2f4d92180e4463f5
ms.lasthandoff: 03/21/2017


---
# <a name="devtest-labs-concepts"></a>DevTest Labs-Konzepte
> [!NOTE]
> Dieser Artikel ist Teil 3 einer dreiteiligen Reihe:
>
> 1. [Was ist DevTest Labs?](devtest-lab-overview.md)
> 2. [Warum DevTest Labs?](devtest-lab-why.md)
> 3. **[DevTest Labs-Konzepte](devtest-lab-concepts.md)**
>
>

## <a name="overview"></a>Übersicht
Die folgende Liste enthält wichtige DevTest Labs-Konzepte und Definitionen:

## <a name="labs"></a>Labs
Ein Lab ist die Infrastruktur, die eine Gruppe von Ressourcen wie z.B. virtuelle Computer (VMs) umfasst, mit der Sie diese Ressourcen besser verwalten können, indem Sie Einschränkungen und Kontingente angeben.

## <a name="virtual-machine"></a>Virtueller Computer
Ein virtueller Azure-Computer ist eine von mehreren [bedarfsgesteuerten, skalierbaren Computerressourcen](https://docs.microsoft.com/azure/app-service-web/choose-web-site-cloud-service-vm), die von Azure angeboten werden. Virtuelle Azure-Computer bieten Ihnen flexible Virtualisierungsmöglichkeiten, ohne dass Sie für die Ausführung physische Hardware erwerben und verwalten müssen. Allerdings müssen Sie den virtuellen Computer weiterhin warten, indem Sie bestimmte Aufgaben ausführen, z.B. Konfigurieren, Patchen und Installieren der Software, die auf ihm ausgeführt wird.

Im Artikel [Übersicht über virtuelle Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) erfahren Sie, was Sie vor der Erstellung eines virtuellen Computers berücksichtigen sollten und wie Sie ihn erstellen und verwalten.

## <a name="claimable-vm"></a>Anforderbarer virtueller Computer
Ein anforderbarer virtueller Azure-Computer ist ein virtueller Computer, der für jeden Benutzer mit Berechtigungen im Lab zur Verwendung verfügbar ist. Ein Lab-Administrator kann virtuelle Computer mit bestimmten Basisimages und Artefakten vorbereiten und in einem freigegebenen Pool speichern. Ein Lab-Benutzer kann dann einen funktionierenden virtuellen Computer aus dem Pool anfordern, wenn er einen Computer mit dieser spezifischen Konfiguration benötigt.

Ein anforderbarer virtueller Computer wird anfänglich keinem bestimmten Benutzer zugewiesen, er wird jedoch in der Liste jedes Benutzers unter „Claimable virtual machines“ (Anforderbare virtuelle Computer) angezeigt. Nachdem ein virtueller Computer von einem Benutzer angefordert wurde, wird er in dessen Bereich „My virtual machines“ (Meine virtuellen Computer) verschoben und kann von keinem anderen Benutzer mehr angefordert werden.

## <a name="environment"></a>Environment
In DevTest Labs bezieht sich eine Umgebung auf eine Sammlung von Azure-Ressourcen in einem Lab. In [diesem Blogeintrag](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) erfahren Sie, wie Sie auf der Grundlage von Azure Resource Manager-Vorlagen Umgebungen mit mehreren virtuellen Computern erstellen.

## <a name="base-images"></a>Basisimages
Basisimages sind ein VM-Images, in denen alle Tools und Einstellungen vorinstalliert und so konfiguriert sind, dass Sie schnell einen virtuellen Computer erstellen können. Sie können einen virtuellen Computer bereitstellen, indem Sie eine vorhandene Basis auswählen ein Artefakt hinzufügen, um Ihren Test-Agent zu installieren. Anschließend können Sie dann die bereitgestellte VM als Basis speichern, die dann verwendet werden kann, ohne den Test-Agent bei jeder Bereitstellung der VM erneut installieren zu müssen.

## <a name="artifacts"></a>Artefakte
Artefakte werden zum Bereitstellen und Konfigurieren Ihrer Anwendung nach der Bereitstellung eines virtuellen Computers verwendet. Bei Artefakten kann es sich um Folgendes handeln:

* Tools, die Sie auf dem virtuellen Computer installieren möchten, z. B. Agents, Fiddler oder Visual Studio.
* Aktionen, die auf dem virtuellen Computer ausgeführt werden sollen, z. B. das Klonen eines Repositorys.
* Anwendungen, die Sie testen möchten.

Artefakte sind [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-JSON-Dateien, die Anweisungen zum Ausführen der Bereitstellung und Übernehmen der Konfiguration enthalten.

## <a name="artifact-repositories"></a>Artefaktrepositorys
Artefaktrepositorys sind Git-Repositorys, in denen Artefakte eingecheckt werden. Artefaktrepositorys können mehreren Labs in Ihrer Organisation hinzugefügt werden, sodass sie wiederverwendet und geteilt werden können.

## <a name="formulas"></a>Formeln
Formeln bieten neben Basisimages einen Mechanismus für die schnelle Bereitstellung virtueller Computer. Eine Formel in DevTest Labs ist eine Liste standardmäßiger Eigenschaftswerte, die zum Erstellen eines virtuellen Labcomputers verwendet werden.
Mit Formeln können virtuelle Computer mit dem gleichen Satz von Eigenschaften (z.B. Basisimage, VM-Größe, virtuelles Netzwerk und Artefakte) erstellt werden, ohne dass diese Eigenschaften jedes Mal angeben werden müssen. Beim Erstellen eines virtuellen Computers aus einer Formel können die Standardwerte wie vorhanden verwendet oder geändert werden.

## <a name="policies"></a>Richtlinien
Richtlinien helfen dabei, die Kosten im Lab zu kontrollieren. Beispielsweise können Sie eine Richtlinie erstellen, um virtuelle Computer automatisch anhand eines definierten Zeitplans herunterzufahren.

## <a name="caps"></a>Obergrenzen
Obergrenzen sind ein Mechanismus zum Minimieren unnötiger Ressourcennutzung im Lab. Beispielsweise können Sie eine Obergrenze für die Anzahl der virtuellen Computer festlegen, die pro Benutzer oder in einem Lab erstellt werden können.

## <a name="security-levels"></a>Sicherheitsstufen
Der Sicherheitszugriff wird durch die rollenbasierte Zugriffssteuerung von Azure (Role-Based Access Control, RBAC) bestimmt. Die Funktionsweise des Zugriffs ist leichter zu verstehen, wenn Sie die Unterschiede zwischen Berechtigungen, Rollen und Bereichen in RBAC kennen.

* Berechtigung: Eine Berechtigung ist ein definierter Zugriff auf eine bestimmte Aktion (z.B. Lesezugriff auf alle virtuellen Computer).
* Rolle: Eine Rolle ist ein Satz von Berechtigungen, die gruppiert und einem Benutzer zugewiesen werden können. Zum Beispiel die Rolle *Abonnementbesitzer* verfügt über Zugriff auf alle Ressourcen in einem Abonnement.
* Bereich: Ein Bereich ist eine Ebene innerhalb der Azure-Ressourcenhierarchie (z.B. eine Ressourcengruppe, ein einzelnes Lab oder das gesamte Abonnement).

Innerhalb des Bereichs von DevTest Labs gibt es zwei Arten von Rollen zum Definieren von Benutzerberechtigungen: Labbesitzer und Labbenutzer.

* Labbesitzer: Ein Labbesitzer verfügt über Zugriff auf alle Ressourcen innerhalb des Labs. Daher kann ein Labbesitzer Richtlinien und das virtuelle Netzwerk ändern, verfügt über Lese- und Schreibberechtigungen für alle virtuellen Computer usw.
* Labbenutzer: Ein Labbenutzer kann alle Labressourcen anzeigen (z.B. virtuelle Computer, Richtlinien und virtuelle Netzwerke), aber keine Richtlinien oder von anderen Benutzern erstellte virtuelle Computer ändern.

Informationen zum Erstellen benutzerdefinierter Rollen in DevTest Labs finden Sie im Artikel [Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Da Bereiche hierarchisch sind, gelten die Berechtigungen, die einem Benutzer für einen bestimmten Bereich gewährt werden, automatisch für alle darin enthaltenen Bereiche auf niedrigerer Ebene. Ist ein Benutzer beispielsweise der Rolle „Abonnementbesitzer“ zugewiesen, hat er Zugriff auf alle Ressourcen in einem Abonnement, d.h. auf alle virtuellen Computer, alle virtuellen Netzwerke und alle Labs. Der Abonnementbesitzer erbt somit automatisch die Rolle des Labbesitzers. Umgekehrt ist dies jedoch nicht der Fall. Ein Labbesitzer hat Zugriff auf ein Lab, d.h. auf einen Bereich, der sich innerhalb der Hierarchie auf einer niedrigeren Ebene befindet als das Abonnement. Aus diesem Grund kann ein Labbesitzer keine virtuellen Computer, virtuellen Netzwerke oder anderen Ressourcen außerhalb des Labs anzeigen.

## <a name="arm-templates"></a>ARM-Vorlagen
Alle in diesem Artikel besprochenen Konzepte können mithilfe von Vorlagen für Azure Resource Manager (ARM) konfiguriert werden. Mit ARM-Vorlagen können Sie die Infrastruktur/Konfiguration Ihrer Azure-Lösung definieren und wiederholt in einem konsistenten Zustand bereitstellen.

[ARM-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) beschreibt die Struktur einer Azure Resource Manager-Vorlage und die Eigenschaften, die in den verschiedenen Abschnitten einer Vorlage verfügbar sind.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
[Erstellen eines Labs in Azure DevTest Labs](devtest-lab-create-lab.md)

