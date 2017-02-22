---
title: Definieren von Labrichtlinien in Azure DevTest Labs | Microsoft Docs
description: "Erfahren Sie, wie Sie Labrichtlinien wie VM-Größen, die maximale Anzahl von VMs pro Benutzer und das automatisierte Herunterfahren definieren."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: f7656382500682898cd3ed6372630afa3c3f6350
ms.openlocfilehash: 2b16840e5e9fc6e0268d83f9177a6fc6ad02841c


---
# <a name="define-lab-policies-in-azure-devtest-labs"></a>Definieren von Labrichtlinien in Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-VM-policies-in-a-DevTest-Lab/player]
> 
> 

Mit Azure DevTest Labs können Sie Schlüsselrichtlinien angeben, mit denen Sie Kosten steuern und unnötigen Aufwand in Ihren Labs minimieren. Diese Labrichtlinien umfassen die maximale Anzahl von virtuellen Computern pro Benutzer und pro Lab sowie verschiedene Optionen für automatisches Herunterfahren und automatisches Starten. 

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Zugriff auf Labrichtlinien in Azure DevTest Labs
Die folgenden Schritte führen Sie durch das Einrichten von Richtlinien für ein Lab in Azure DevTest Labs:

Um die Richtlinien für ein Lab anzuzeigen (und zu ändern), gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   
4. Wählen Sie **Richtlinieneinstellungen**.
5. Das Blatt **Richtlinieneinstellungen** enthält ein Menü mit Einstellungen, die Sie angeben können: 
   
    ![Blatt „Richtlinieneinstellungen“](./media/devtest-lab-set-lab-policy/policies.png)
   
    Um weitere Informationen zum Festlegen einer Richtlinie zu erhalten, wählen Sie sie in der folgenden Liste aus:
   
   * [Allowed virtual machine sizes](#set-allowed-virtual-machine-sizes) (Zulässige VM-Größen): Wählen Sie die Liste der im Lab zulässigen VM-Größen aus. Ein Benutzer kann nur aus dieser Liste virtuelle Computer erstellen.
   * [Virtual machines per user](#set-virtual-machines-per-user) (VMs pro Benutzer): Geben Sie die maximale Anzahl von VMs an, die ein Benutzer erstellen kann. 
   * [Virtual machines per lab](#set-virtual-machines-per-lab) (VMs pro Lab): Geben Sie die maximale Anzahl von VMs an, die für ein Lab erstellt werden können. 
   * [Auto-shutdown](#set-auto-shutdown) (Automatisches Herunterfahren): Geben Sie den Zeitpunkt an, zu dem die VMs des aktuellen Labs automatisch herunterfahren.
   * [Auto-start](#set-auto-start) (Automatisches Starten): Geben Sie den Zeitpunkt an, zu dem die virtuellen Computer des aktuellen Labs automatisch starten.

## <a name="set-allowed-virtual-machine-sizes"></a>Festlegen der zulässigen Größen virtueller Computer
Die Richtlinie für die zulässigen VM-Größen hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die im Lab zulässigen VM-Größen anzugeben. Wenn diese Richtlinie aktiviert ist, können nur VM-Größen aus dieser Liste zum Erstellen von virtuellen Computern verwendet werden.

1. Wählen Sie auf dem Blatt **Richtlinieneinstellungen** des Labs **Allowed virtual machines sizes** (Zulässige VM-Größen).
   
    ![Allowed virtual machines sizes](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)
2. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
3. Wenn Sie diese Richtlinie aktiviert haben, wählen Sie mindestens eine VM-Größe, die im Lab erstellt werden kann.
4. Wählen Sie **Speichern**aus.

## <a name="set-virtual-machines-per-user"></a>Festlegen der virtuellen Computer pro Benutzer
Mit der Richtlinie für **Virtual machines per user** (VMs pro Benutzer) können Sie die maximale Anzahl von virtuellen Computern angeben, die von einem einzelnen Benutzer erstellt werden können. Wenn ein Benutzer versucht, einen virtuellen Computer zu erstellen, und die Benutzergrenze erreicht wurde, wird in einer Fehlermeldung darauf hingewiesen, dass der virtuelle Computer nicht erstellt werden kann. 

1. Wählen Sie auf dem Blatt **Richtlinieneinstellungen** des Labs **Virtual machines per user** (VMs pro Benutzer).
   
    ![Virtual machines per user](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)
2. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
3. Wenn Sie diese Richtlinie aktivieren, geben Sie einen numerischen Wert für die maximale Anzahl von virtuellen Computern ein, die von einem Benutzer erstellt werden können. 
   Wenn Sie eine ungültige Zahl eingeben, zeigt die Benutzeroberfläche die maximal zulässige Zahl für dieses Feld an.
4. Wählen Sie **Speichern**aus.

## <a name="set-virtual-machines-per-lab"></a>Festlegen der virtuellen Computer pro Lab
Mit der Richtlinie für **Virtual machines per lab** (VMs pro Lab) können Sie die maximale Anzahl von virtuellen Computern angeben, die für das aktuelle Lab erstellt werden können. Wenn ein Benutzer versucht, einen virtuellen Computer zu erstellen, und die Labgrenze erreicht wurde, wird in einer Fehlermeldung darauf hingewiesen, dass der virtuelle Computer nicht erstellt werden kann. 

1. Wählen Sie auf dem Blatt **Richtlinieneinstellungen** des Labs **Virtual machines per lab** (VMs pro Lab).
   
    ![Virtual machines per lab](./media/devtest-lab-set-lab-policy/total-vms-allowed.png)
2. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
3. Wenn Sie diese Richtlinie aktivieren, geben Sie einen numerischen Wert für die maximale Anzahl von virtuellen Computern ein, die für das aktuelle Lab erstellt werden können. 
   Wenn Sie eine ungültige Zahl eingeben, zeigt die Benutzeroberfläche die maximal zulässige Zahl für dieses Feld an.
4. Wählen Sie **Speichern**aus.

## <a name="set-auto-shutdown"></a>Festlegen des automatischen Herunterfahrens
Die Auto-Shutdown-Richtlinie hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die Uhrzeit anzugeben, zu der die virtuellen Computer für dieses Lab heruntergefahren werden.

1. Wählen Sie auf dem Blatt **Richtlinieneinstellungen** des Labs **Auto shutdown** (Automatisches Herunterfahren).
   
    ![Auto-shutdown](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
2. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
3. Wenn Sie diese Richtlinie aktivieren, geben Sie die lokale Uhrzeit an, zu der alle virtuellen Computer im derzeitigen Lab heruntergefahren werden sollen.
4. Wählen Sie **Speichern**aus.
5. Standardmäßig gilt diese Richtlinie nach der Aktivierung für alle virtuellen Computer im aktuellen Lab. Um diese Einstellung von einem bestimmten virtuellen Computer zu entfernen, öffnen Sie das Blatt des virtuellen Computers, und ändern Sie die Einstellung **Auto shutdown** (Automatisches Herunterfahren). 

## <a name="set-auto-start"></a>Festlegen des automatischen Startens
Mit der Autostart-Richtlinie können Sie angeben, wann die virtuellen Computer im aktuellen Lab gestartet werden sollen.  

1. Wählen Sie auf dem Blatt **Richtlinieneinstellungen** des Labs **Auto-start** (Automatisches Starten).
   
    ![Auto-start](./media/devtest-lab-set-lab-policy/auto-start.png)
2. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
3. Wenn Sie diese Richtlinie aktivieren, geben Sie die geplante lokale Startzeit und die zugeordneten Tage der Woche an. 
4. Wählen Sie **Speichern**aus.
5. Nach der Aktivierung wird diese Richtlinie nicht automatisch auf alle virtuellen Computer im aktuellen Lab angewendet. Um diese Einstellung auf einen bestimmten virtuellen Computer anzuwenden, öffnen Sie das Blatt des virtuellen Computers, und ändern Sie die Einstellung **Auto start** (Automatisches Starten). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald Sie die verschiedenen VM-Richtlinieneinstellungen für Ihr Lab definiert und angewendet haben, können Sie als Nächstes versuchen, die folgenden Aufgaben auszuführen:

* [Konfigurieren des Kostenmanagements](devtest-lab-configure-cost-management.md): Veranschaulicht die Nutzung des Diagramms **Monatlicher geschätzter Kostentrend**  
  zum Anzeigen der geschätzten Kosten des aktuellen Monats bis zum gegenwärtigen Zeitpunkt und die voraussichtlichen Kosten am Ende des Monats.
* [Erstellen eines benutzerdefinierten Images](devtest-lab-create-template.md): Wenn Sie einen virtuellen Computer erstellen, geben Sie eine Basis an. Dabei kann es sich entweder um ein benutzerdefiniertes Image oder ein Marketplace-Image handeln. In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Image aus einer VHD-Datei erstellen.
* [Konfigurieren von Marketplace-Images](devtest-lab-configure-marketplace-images.md): Azure DevTest Labs unterstützt die Erstellung virtueller Computer auf der Basis von Azure Marketplace-Images. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen virtueller Computer in einem Lab verwendet werden können.
* [Erstellen eines virtuellen Computers in einem Lab](devtest-lab-add-vm-with-artifacts.md): In diesem Artikel wird veranschaulicht, wie Sie einen virtuellen Computer aus einem Basisimage erstellen (entweder aus einem benutzerdefinierten Image oder einem Marketplace-Image), und wie Sie mit Artefakten auf dem virtuellen Computer arbeiten.




<!--HONumber=Jan17_HO2-->


