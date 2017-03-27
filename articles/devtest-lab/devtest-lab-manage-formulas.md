---
title: Verwalten von Formeln in Azure DevTest Labs zum Erstellen virtueller Computer | Microsoft Docs
description: Erfahren Sie, wie Sie Azure DevTest Labs-Formeln aktualisieren und entfernen.
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
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: fd334dc8cd22990456888146f41389054f2e8b29
ms.lasthandoff: 03/15/2017


---
# <a name="manage-azure-devtest-labs-formulas"></a>Verwalten von Azure DevTest Labs-Formeln

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Der Artikel [Erstellen von Azure DevTest Labs-Formeln](devtest-lab-create-formulas.md#create-a-formula) führt Sie durch die Erstellung einer Formel in Azure DevTest Labs. Wenn Sie über eine Formel verfügen, informiert Sie der vorliegende Artikel über die Verwaltung von Formeln.

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


