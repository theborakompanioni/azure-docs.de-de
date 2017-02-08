---
title: Verwalten von Formeln in Azure DevTest Labs zum Erstellen virtueller Computer | Microsoft Docs
description: Erfahren Sie, wie Sie Azure DevTest Labs-Formeln erstellen, aktualisieren, entfernen und zum Erstellen neuer virtueller Computer verwenden.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f7ea1762e3a3ffe8d3c0c6af307b846dc1f3c696


---
# <a name="manage-devtest-labs-formulas-to-create-vms"></a>Verwalten von DevTest Labs-Formeln zum Erstellen virtueller Computer
Eine Formel in Azure DevTest Labs ist eine Liste von Standardeigenschaftswerten, die zum Erstellen eines virtuellen Computers (VM, Virtual Machine) verwendet werden. Beim Erstellen eines virtuellen Computers aus einer Formel können die Standardwerte wie vorhanden verwendet oder geändert werden. Ähnlich wie [benutzerdefinierte Images](devtest-lab-create-template.md) und [Marketplace-Images](devtest-lab-configure-marketplace-images.md) bieten Formeln einen Mechanismus zur schnellen Bereitstellung virtueller Computer.  

In diesem Artikel erfahren Sie, wie Sie folgende Aufgaben ausführen:

* [Erstellen einer Formel](#create-a-formula)
* [Verwenden einer Formel zum Bereitstellen eines virtuellen Computers](#use-a-formula-to-provision-a-vm)
* [Ändern einer Formel](#modify-a-formula)
* [Löschen einer Formel](#delete-a-formula)

> [!NOTE]
> Genauso wie [benutzerdefinierte Images](devtest-lab-create-template.md) ermöglichen Ihnen Formeln, aus einer VHD-Datei ein Basisimage zu erstellen. Dieses Basisimage kann anschließend zum Bereitstellen eines neuen virtuellen Computers verwendet werden. Um leichter entscheiden zu können, welche Methode sich für Ihre Umgebung besser eignet, lesen Sie die Informationen unter [Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).
> 
> 

## <a name="create-a-formula"></a>Erstellen einer Formel
Jeder Benutzer mit der Berechtigung *Benutzer* in DevTest Labs kann virtuelle Computer auf Basis einer Formel erstellen. Es gibt zwei Möglichkeiten, um Formeln zu erstellen: 

* Anhand einer Basis: Entscheiden Sie sich für diese Option, wenn Sie alle Merkmale der Formel definieren möchten.
* Aus einem vorhandenen virtuellen Labcomputer: Entscheiden Sie sich für diese Option, wenn Sie eine Formel basierend auf den Einstellungen eines vorhandenen virtuellen Computers erstellen möchten.

### <a name="create-a-formula-from-a-base"></a>Erstellen einer Formel anhand einer Basis
In den folgenden Schritten wird beschrieben, wie Sie eine Formel auf der Basis eines benutzerdefinierten Images, eines Marketplace-Images oder auf Grundlage einer anderen Formel erstellen.

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  
4. Wählen Sie auf dem Blatt für das Lab die Option **Formulas (reusable bases)**(Formeln (wiederverwendbare Basis)) aus.
   
    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Wählen Sie auf dem Blatt **Labformeln** **+ Hinzufügen**.
   
    ![Formel hinzufügen](./media/devtest-lab-manage-formulas/add-formula.png)
6. Wählen Sie auf dem Blatt **Choose a base** (Basis auswählen) die Grundlage aus, anhand derer Sie die Formel erstellen möchten (also das benutzerdefinierte Image, das Marketplace-Image oder die Formel).
   
    ![Basisliste](./media/devtest-lab-manage-formulas/base-list.png)
7. Geben Sie auf dem Blatt **Formel erstellen** folgende Werte an:
   
   * **Formelname**: Geben Sie einen Namen für die Formel ein. Dieser Wert wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen. Der Name wird während der Eingabe überprüft. Falls er nicht gültig ist, werden Sie in einer Meldung über die Anforderungen für einen gültigen Namen informiert.
   * **Beschreibung** : Geben Sie eine aussagekräftige Beschreibung für Ihre Formel ein. Dieser Wert steht über das Kontextmenü der Formel zur Verfügung, wenn Sie einen virtuellen Computer erstellen.
   * **Benutzername** : Geben Sie einen Benutzernamen ein, dem Administratorrechte erteilt werden.
   * **Kennwort**: Geben Sie einen Wert ein, der dem geheimen Schlüssel bzw. dem Kennwort zugeordnet ist, das für den angegebenen Benutzer verwendet werden soll, oder wählen Sie diesen Wert aus der Dropdownliste aus.  
   * **Image** : Dieses Feld zeigt den Namen des Basisimage an, das Sie auf dem vorherigen Blatt ausgewählt haben. 
   * **Größe des virtuellen Computers** : Wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
   * **Virtuelles Netzwerk** : Geben Sie das gewünschte virtuelle Netzwerk an.
   * **Subnetz** : Geben Sie das gewünschte Subnetz an.
   * **Öffentliche IP-Adresse**: Wenn die Labrichtlinie öffentliche IP-Adressen für das ausgewählte Subnetz zulässt, geben Sie durch Auswahl von **Ja** oder **Nein** an, ob die IP-Adresse öffentlich sein soll. Andernfalls ist diese Option deaktiviert und als **Nein**festgelegt.
   * **Artefakte** : Wählen Sie die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie. Die Werte von sichere Zeichenfolgen werden nicht in der Formel gespeichert. Wenn es sich bei Artefaktparametern um sichere Zeichenfolgen handelt, werden sie deshalb nicht angezeigt. 
     
       ![Formel erstellen](./media/devtest-lab-manage-formulas/create-formula.png)
8. Wählen Sie **Erstellen** , um die Formel zu erstellen.

### <a name="create-a-formula-from-a-vm"></a>Erstellen einer Formel aus einem virtuellen Computer
Die folgenden Schritte führen Sie durch den Prozess der Erstellung einer neuen Formel aus einem vorhandenen virtuellen Labcomputer. 

> [!NOTE]
> Damit Sie anhand eines virtuellen Computers eine Formel erstellen können, muss dieser virtuelle Computer nach dem 30. März 2016 erstellt worden sein. 
> 
> 

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  
4. Wählen Sie auf dem Blatt **Übersicht** des Labs den virtuellen Computer aus, aus dem Sie die Formel erstellen möchten.
   
    ![Virtuelle Labs-Computer](./media/devtest-lab-manage-formulas/my-vms.png)
5. Wählen Sie auf dem Blatt für den virtuellen Computer die Option **Create formula (reusable base)**(Formel erstellen (wiederverwendbare Basis)) aus.
   
    ![Formel erstellen](./media/devtest-lab-manage-formulas/create-formula-menu.png)
6. Geben Sie auf dem Blatt **Create Formula** (Formel erstellen) einen **Namen** und eine **Beschreibung** für die neue Formel ein.
   
    ![Blatt „Formel erstellen“](./media/devtest-lab-manage-formulas/create-formula-blade.png)
7. Wählen Sie **OK** , um die Formel zu erstellen.

## <a name="use-a-formula-to-provision-a-vm"></a>Verwenden einer Formel zum Bereitstellen eines virtuellen Computers
Nachdem Sie eine Formel erstellt haben, können Sie diese als Grundlage zum Erstellen eines virtuellen Computers verwenden. Die genaue Vorgehensweise wird im Abschnitt [Hinzufügen einer VM mit Artefakten](devtest-lab-add-vm-with-artifacts.md#add-a-vm-with-artifacts) beschrieben.

## <a name="modify-a-formula"></a>Ändern einer Formel
Um eine Formel zu ändern, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  
4. Wählen Sie auf dem Blatt für das Lab die Option **Formulas (reusable bases)**(Formeln (wiederverwendbare Basis)) aus.
   
    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Wählen Sie auf dem Blatt **Labformeln** die Formel, die Sie ändern möchten.
6. Nehmen Sie auf dem Blatt **Formel aktualisieren** die gewünschten Änderungen vor, und wählen Sie **Aktualisieren**.

## <a name="delete-a-formula"></a>Löschen einer Formel
Um eine Formel zu löschen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  
4. Wählen Sie auf dem Blatt **Einstellungen** für das Lab **Formeln**.
   
    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Wählen Sie auf dem Blatt **Labformeln** die Auslassungspunkte rechts neben der Formel, die Sie löschen möchten.
   
    ![Menü „Formel“](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Wählen Sie im Kontextmenü der Formel die Option **Löschen**aus.
   
    ![Kontextmenü „Formel“](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Wählen Sie im Bestätigungsdialogfeld **Ja** .

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge
* [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine Formel erstellt haben, die zum Erstellen virtueller Computer verwendet werden soll, besteht der nächste Schritt darin, [Ihrem Lab einen virtuellen Computer hinzuzufügen](devtest-lab-add-vm-with-artifacts.md).




<!--HONumber=Nov16_HO3-->


