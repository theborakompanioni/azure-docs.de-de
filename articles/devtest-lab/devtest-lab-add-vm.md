---
title: "Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs | Microsoft-Dokumentation"
description: "Informationen zum Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: tarcher
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 449bffb040dafc8edd0b8b0afd80dbea35cd28ac
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
Wenn Sie bereits [Ihren ersten virtuellen Computer erstellt haben](devtest-lab-create-first-vm.md), haben Sie dazu wahrscheinlich ein vorkonfiguriertes [Marketplace-Image](devtest-lab-configure-marketplace-images.md) verwendet. Wenn Sie Ihrem Lab nun weitere virtuelle Computer hinzufügen möchten, können Sie auch eine *Basis* auswählen, die entweder ein [benutzerdefiniertes Image](devtest-lab-create-template.md) oder eine [Formel](devtest-lab-manage-formulas.md) ist. Dieses Tutorial führt Sie durch die Verwendung des Azure-Portals zum Hinzufügen eines virtuellen Computers zu einem Lab in DevTest Labs.

In diesem Artikel wird außerdem die Verwaltung der Artefakte für einen virtuellen Computer in Ihrem Lab veranschaulicht.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Schritte zum Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer (VM, Virtual Machine) erstellen möchten.  
1. Wählen Sie auf dem Blatt **Übersicht** des Labs die Option **+Hinzufügen** aus.  

    ![Schaltfläche zum Hinzufügen eines virtuellen Computers](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Wählen Sie auf dem Blatt **Basisdesign auswählen** eine Basis für den virtuellen Computer aus.
1. Geben Sie auf dem Blatt **Virtueller Computer** im Textfeld **Name des virtuellen Computers** einen Namen für den neuen virtuellen Computer ein.

    ![Blatt „Lab-VM“](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Geben Sie einen **Benutzernamen** ein, dem Administratorrechte auf dem virtuellen Computer erteilt werden.  
1. Wenn Sie ein Kennwort verwenden möchten, das in Ihrem [geheimen Speicher](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store) hinterlegt ist, wählen Sie **Gespeichertes Geheimnis verwenden**, und geben Sie einen Schlüsselwert an, der Ihrem Geheimnis (Kennwort) entspricht. Geben Sie andernfalls im Textfeld **Geben Sie einen Wert ein** ein Kennwort ein.
1. Der **Datenträgertyp des virtuellen Computers** bestimmt, welcher Typ von Speicherdatenträgern für die virtuellen Computer im Lab zulässig ist.
1. Wählen Sie **Größe des virtuellen Computers** , und wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
1. Wählen Sie **Artefakte** , und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie.
    **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, lesen Sie den Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](#add-an-existing-artifact-to-a-vm), und kehren Sie dann hierher zurück.
1. Wählen Sie **Erweiterte Einstellungen**, um die Netzwerkoptionen und Ablaufoptionen des virtuellen Computers zu konfigurieren. 

   Wählen Sie zum Festlegen einer Ablaufoption das Symbol „Kalender“ aus, um ein Datum anzugeben, an dem der virtuelle Computer automatisch gelöscht wird.  Standardmäßig läuft der virtuelle Computer nie ab. 
1. Wenn Sie die Azure Resource Manager-Vorlage anzeigen oder kopieren möchten, lesen Sie den Abschnitt [Speichern der Azure Resource Manager-Vorlage](#save-azure-resource-manager-template), und kehren Sie anschließend hierher zurück.
1. Wählen Sie **Erstellen** , um den angegebene virtuellen Computer dem Lab hinzuzufügen.
1. Auf dem Blatt für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Wird erstellt**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.

> [!NOTE]
> In [Hinzufügen eines anforderbaren virtuellen Computers](devtest-lab-add-claimable-vm.md) wird erläutert, wie Sie den virtuellen Computer als anforderbar festlegen, sodass er für jeden Benutzer im Lab zur Verwendung verfügbar ist.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Hinzufügen eines vorhandenen Artefakts zu einer VM
Beim Erstellen eines virtuellen Computers können Sie vorhandene Artefakte hinzufügen. Jedes Lab enthält Artefakte aus dem öffentlichen DevTest Labs-Artefaktrepository, sowie Artefakte, die Sie erstellt und Ihrem eigenen Artefaktrepository hinzugefügt haben.

* Azure DevTest Labs-*Artefakte* ermöglichen Ihnen die Angabe von *Aktionen*, die ausgeführt werden, wenn der virtuelle Computer bereitgestellt wird, beispielsweise Ausführen von Windows PowerShell-Skripts, Ausführen von Bash-Befehlen und Installieren von Software.
* Mit *Parametern* für Artefakte können Sie das Artefakt für ein bestimmtes Szenario anpassen.

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

### <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie **Verbinden** auf dem Blatt des virtuellen Computers wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Informieren Sie sich über das [Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Lab-VM](devtest-lab-artifact-author.md).
* Erkunden Sie den [DevTest Labs-Azure Resource Manager-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).

