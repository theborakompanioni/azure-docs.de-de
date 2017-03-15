---
title: Verwalten von VM-Artefakten in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie VM-Artefakte in Azure DevTest Labs verwalten.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 576509ce-6a33-4c26-87c7-de8b40271efa
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2d298c210fa46cc4473fc7e8809e55bfc102481d
ms.openlocfilehash: c970bca8f6ab9e97741370f74efc5ee66e592468
ms.lasthandoff: 02/27/2017


---
# <a name="manage-vm-artifacts-in-azure-devtest-labs"></a>Verwalten von VM-Artefakten in Azure DevTest Labs
Mit Azure DevTest Labs-*Artefakten* können Sie *Aktionen* angeben, die beim Bereitstellen des virtuellen Computers ausgeführt werden. 

Artefaktaktionen können Prozeduren ausführen, z.B. die Ausführung von Windows PowerShell-Skripts und Bash-Befehlen und eine Softwareinstallation. 

Mit *Parametern* für Artefakte können Sie das Artefakt für ein bestimmtes Szenario anpassen.

In diesem Artikel wird die Verwaltung der Artefakte für einen virtuellen Computer in Ihrem Lab veranschaulicht.

## <a name="add-an-existing-artifact-to-a-vm"></a>Hinzufügen eines vorhandenen Artefakts zu einer VM
Beim Erstellen eines virtuellen Computers können Sie vorhandene Artefakte hinzufügen. Jedes Lab enthält Artefakte aus dem öffentlichen DevTest Labs-Artefaktrepository, sowie Artefakte, die Sie erstellt und Ihrem eigenen Artefaktrepository hinzugefügt haben.
Informationen zum Erstellen von Artefakten finden Sie im Artikel [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Labs-VM](devtest-lab-artifact-author.md).

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das Lab mit dem virtuellen Computer aus, mit dem Sie arbeiten möchten.  
1. Wählen Sie **Meine virtuellen Computer** aus.
1. Wählen Sie den gewünschten virtuellen Computer aus.
1. Wählen Sie **Artefakte**. 
1. Wählen Sie **Artefakte anwenden**.
1. Wählen Sie auf dem Blatt **Artefakte anwenden** das Artefakt aus, das Sie dem virtuellen Computer hinzufügen möchten.
1. Geben Sie auf dem Blatt **Artefakt hinzufügen** die erforderlichen Parameterwerte und alle optionalen Parameter ein, die Sie benötigen.  
1. Wählen Sie **Hinzufügen** aus, um das Artefakt hinzuzufügen, und kehren Sie zum Blatt **Artefakte anwenden** zurück.
1. Fügen Sie weiterhin nach Bedarf für Ihren virtuellen Computer Artefakte hinzu.
1. Sobald Sie Ihre Artefakte hinzugefügt haben, können Sie [die Reihenfolge ändern, in der die Artefakte ausgeführt werden](#change-the-order-in-which-artifacts-are-run). Sie können auch zum [Anzeigen oder Ändern eines Artefakts](#view-or-modify-an-artifact)zurückgehen.
1. Wenn Sie mit dem Hinzufügen von Artefakten fertig sind, wählen Sie **Anwenden**.

## <a name="change-the-order-in-which-artifacts-are-run"></a>Ändern der Reihenfolge, in der Artefakte ausgeführt werden
Standardmäßig werden die Aktionen der Artefakte in der Reihenfolge ausgeführt, in der sie der VM hinzugefügt wurden. Die folgenden Schritte veranschaulichen, wie Sie die Reihenfolge ändern, in der die Artefakte ausgeführt werden.

1. Wählen Sie oben auf dem Blatt **Artefakte anwenden** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.
   
    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Ziehen Sie die Artefakte auf dem Blatt **Ausgewählte Artefakte** per Drag & Drop in die gewünschten Reihenfolge. **Hinweis:** Wenn beim Ziehen eines Artefakts Probleme auftreten, stellen Sie sicher, dass Sie von der linken Seite des Artefakts aus ziehen. 
1. Wählen Sie nach Abschluss des Vorgangs **OK** .  

## <a name="view-or-modify-an-artifact"></a>Anzeigen oder Ändern eines Artefakts
Die folgenden Schritte veranschaulichen das Anzeigen oder Ändern der Parameter eines Artefakts:

1. Wählen Sie oben auf dem Blatt **Artefakte anwenden** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.
   
    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Wählen Sie auf dem Blatt **Ausgewählte Artefakte** das Artefakt aus, das Sie anzeigen oder bearbeiten möchten.  
1. Nehmen Sie auf dem Blatt **Artefakt hinzufügen** die erforderlichen Änderungen vor, und wählen Sie **OK**, um das Blatt **Artefakt hinzufügen** zu schließen.
1. Wählen Sie **OK**, um das Blatt **Ausgewählte Artefakte** zu schließen.

## <a name="save-azure-resource-manager-template"></a>Speichern der Azure Resource Manager-Vorlage
Eine Azure Resource Manager-Vorlage bietet eine deklarative Möglichkeit zum Definieren einer wiederholbaren Bereitstellung. Die folgenden Schritte erläutern, wie die Azure Resource Manager-Vorlage für den zu erstellenden virtuellen Computer gespeichert wird.
Nach dem Speichern können Sie die Azure Resource Manager-Vorlage zum [Bereitstellen neuer virtueller Computer mit Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment) nutzen.

1. Klicken Sie auf dem Blatt **Virtueller Computer** auf **View ARM Template** (ARM-Vorlage anzeigen).
2. Wählen Sie auf dem Blatt **Azure Resource Manager-Vorlage anzeigen** den Text der Vorlage aus.
3. Kopieren Sie den markierten Text in die Zwischenablage.
4. Wählen Sie **OK** aus, um das Blatt **View Azure Resource Manager Template** (Azure Resource Manager-Vorlage anzeigen) zu schließen.
5. Öffnen Sie einen Text-Editor.
6. Fügen Sie den Text der Vorlage aus der Zwischenablage ein.
7. Speichern Sie die Datei für eine spätere Verwendung.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über das [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Lab-VM](devtest-lab-artifact-author.md).

