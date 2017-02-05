---
title: "Hinzufügen einer VM mit Artefakten zu einem Lab in Azure DevTest Labs | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine VM mit Artefakten in Azure DevTest Labs hinzufügen."
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
ms.date: 11/25/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: f480b8155c7bee797f1fed0f80200eec500e95a2
ms.openlocfilehash: aa6c87ef6e1f87cb47f68f9480ea068aa6262bf9


---
# <a name="add-a-vm-with-artifacts-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen einer VM mit Artefakten zu einem Lab in Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/How-to-create-VMs-with-Artifacts-in-a-DevTest-Lab/player]
> 
> 

Sie erstellen einen virtuellen Computer in einem Lab aus einer *Basis*, bei der es sich entweder um ein [benutzerdefiniertes Image](devtest-lab-create-template.md), eine [Formel](devtest-lab-manage-formulas.md) oder ein [Marketplace-Image](devtest-lab-configure-marketplace-images.md) handelt.

Mit DevTest Labs-*Artefakten* können Sie *Aktionen* angeben, die beim Erstellen des virtuellen Computers ausgeführt werden. 

Artefaktaktionen können Prozeduren ausführen, z.B. die Ausführung von Windows PowerShell-Skripts und Bash-Befehlen und eine Softwareinstallation. 

Mit *Parametern* für Artefakte können Sie das Artefakt für ein bestimmtes Szenario anpassen.

In diesem Artikel wird die Erstellung eines virtuellen Computers in Ihrem Lab mithilfe von Artefakten veranschaulicht.

## <a name="add-a-vm-with-artifacts"></a>Hinzufügen einer VM mit Artefakten
1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer (VM, Virtual Machine) erstellen möchten.  
4. Wählen Sie auf dem Blatt **Übersicht** die Option **+ Virtueller Computer** aus.  
    ![Schaltfläche zum Hinzufügen eines virtuellen Computers](./media/devtest-lab-add-vm-with-artifacts/devtestlab-home-blade-add-vm.png)
5. Wählen Sie auf dem Blatt **Basisdesign auswählen** eine Basis für den virtuellen Computer aus.
6. Geben Sie auf dem Blatt **Virtueller Computer** im Textfeld **Name des virtuellen Computers** einen Namen für den neuen virtuellen Computer ein.
   
    ![Blatt „Lab-VM“](./media/devtest-lab-add-vm-with-artifacts/devtestlab-lab-vm-blade.png)
7. Geben Sie einen **Benutzernamen** ein, dem Administratorrechte auf dem virtuellen Computer erteilt werden.  
8. Wenn Sie ein Kennwort verwenden möchten, das in Ihrem *geheimen Speicher*hinterlegt ist, wählen Sie **Geheime Schlüssel aus meinem geheimen Speicher verwenden**, und geben Sie einen Schlüsselwert an, der ihrem geheimen Schlüssel (Kennwort) entspricht. Andernfalls geben Sie einfach im Textfeld **Geben Sie einen Wert ein**ein Kennwort ein.
9. Wählen Sie **Größe des virtuellen Computers** , und wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
10. Wählen Sie **Virtuelles Netzwerk** , und wählen Sie das gewünschte virtuelle Netzwerk aus.
11. Wählen Sie **Subnetz** , und wählen Sie das Subnetz aus.
12. Wenn die Labrichtlinie öffentliche IP-Adressen für das ausgewählte Subnetz zulässt, geben Sie durch Auswahl von **Ja** oder **Nein** an, ob die IP-Adresse öffentlich sein soll. Andernfalls ist diese Option deaktiviert und als **Nein**festgelegt. 
13. Wählen Sie **Artefakte** , und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie. 
    **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, fahren Sie mit dem Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](#add-an-existing-artifact-to-a-vm) fort, und kehren Sie dann hierher zurück.
14. Wenn Sie die Azure Resource Manager-Vorlage anzeigen oder kopieren möchten, fahren Sie mit dem Abschnitt [Speichern der Azure Resource Manager-Vorlage](#save-azure-resource-manager-template) fort, und kehren Sie anschließend hierher zurück.
15. Wählen Sie **Erstellen** , um den angegebene virtuellen Computer dem Lab hinzuzufügen.
16. Auf dem Blatt für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Wird erstellt**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.
17. Gehen Sie zum Abschnitt [Nächste Schritte](#next-steps) . 

## <a name="add-an-existing-artifact-to-a-vm"></a>Hinzufügen eines vorhandenen Artefakts zu einer VM
Beim Erstellen eines virtuellen Computers können Sie vorhandene Artefakte hinzufügen. Jedes Lab enthält Artefakte aus dem öffentlichen DevTest Labs-Artefaktrepository, sowie Artefakte, die Sie erstellt und Ihrem eigenen Artefaktrepository hinzugefügt haben.
Informationen zum Erstellen von Artefakten finden Sie im Artikel [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Labs-VM](devtest-lab-artifact-author.md).

1. Klicken Sie auf dem Blatt **Virtueller Computer** auf **Artefakte**. 
2. Wählen Sie auf dem Blatt **Artefakte hinzufügen** das gewünschte Artefakt aus.  
   
    ![Blatt „Artefakte hinzufügen“](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifact-blade.png)
3. Geben Sie die erforderlichen Parameterwerte und alle optionalen Parameter ein, die Sie benötigen.  
4. Wählen Sie **Hinzufügen** aus, um das Artefakt hinzuzufügen und zum Blatt **Artefakte hinzufügen** zurückzukehren.
5. Fügen Sie weiterhin nach Bedarf für Ihren virtuellen Computer Artefakte hinzu.
6. Sobald Sie Ihre Artefakte hinzugefügt haben, können Sie [die Reihenfolge ändern, in der die Artefakte ausgeführt werden](#change-the-order-in-which-artifacts-are-run). Sie können auch zum [Anzeigen oder Ändern eines Artefakts](#view-or-modify-an-artifact)zurückgehen.

## <a name="change-the-order-in-which-artifacts-are-run"></a>Ändern der Reihenfolge, in der Artefakte ausgeführt werden
Standardmäßig werden die Aktionen der Artefakte in der Reihenfolge ausgeführt, in der sie der VM hinzugefügt wurden. Die folgenden Schritte veranschaulichen, wie Sie die Reihenfolge ändern, in der die Artefakte ausgeführt werden.

1. Wählen Sie oben auf dem Blatt **Artefakte hinzufügen** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.
   
    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
2. Um die Reihenfolge für die Ausführung der Artefakte anzugeben, ziehen Sie die Artefakte in die gewünschte Reihenfolge. **Hinweis:** Wenn beim Ziehen eines Artefakts Probleme auftreten, stellen Sie sicher, dass Sie von der linken Seite des Artefakts aus ziehen. 
3. Wählen Sie nach Abschluss des Vorgangs **OK** .  

## <a name="view-or-modify-an-artifact"></a>Anzeigen oder Ändern eines Artefakts
Die folgenden Schritte veranschaulichen das Anzeigen oder Ändern der Parameter eines Artefakts:

1. Wählen Sie oben auf dem Blatt **Artefakte hinzufügen** den Link, der die Anzahl der Artefakte angibt, die dem virtuellen Computer hinzugefügt wurden.
   
    ![Anzahl der Artefakte, die dem virtuellen Computer hinzugefügt wurden](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
2. Wählen Sie auf dem Blatt **Ausgewählte Artefakte** das Artefakt aus, das Sie anzeigen oder bearbeiten möchten.  
3. Nehmen Sie auf dem Blatt **Artefakt hinzufügen** die erforderlichen Änderungen vor, und wählen Sie **OK** aus, um das Blatt **Artefakt hinzufügen** zu schließen.
4. Wählen Sie **OK** aus, um das Blatt **Ausgewählte Artefakte** zu schließen.

## <a name="save-azure-resource-manager-template"></a>Speichern der Azure Resource Manager-Vorlage
Eine Azure Resource Manager-Vorlage bietet eine deklarative Möglichkeit zum Definieren einer wiederholbaren Bereitstellung. Die folgenden Schritte erläutern, wie die Azure Resource Manager-Vorlage für den zu erstellenden virtuellen Computer gespeichert wird.
Nach dem Speichern können Sie die Azure Resource Manager-Vorlage zum [Bereitstellen neuer virtueller Computer mit Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment) nutzen.

1. Klicken Sie auf dem Blatt **Virtueller Computer** auf **View ARM Template** (ARM-Vorlage anzeigen).
2. Markieren Sie auf dem Blatt **View Azure Resource Manager Template**(Azure Resource Manager-Vorlage anzeigen) den Text der Vorlage.
3. Kopieren Sie den markierten Text in die Zwischenablage.
4. Wählen Sie **OK** aus, um das Blatt **View Azure Resource Manager Template** (Azure Resource Manager-Vorlage anzeigen) zu schließen.
5. Öffnen Sie einen Text-Editor.
6. Fügen Sie den Text der Vorlage aus der Zwischenablage ein.
7. Speichern Sie die Datei für eine spätere Verwendung.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie **Verbinden** auf dem Blatt des virtuellen Computers wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Informieren Sie sich über das [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Lab-VM](devtest-lab-artifact-author.md).
* Erkunden Sie den [DevTest Labs-ARM-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)




<!--HONumber=Nov16_HO3-->


