---
title: Diagnostizieren von Artefaktfehlern in Azure DevTest Labs-VM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme aufgrund von Artefaktfehlern in DevTest Labs behandeln.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: e4f2946d0ba0756f36622aded0e8594acabb9527
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Diagnostizieren von Artefaktfehlern im Lab 
Nachdem Sie ein Artefakt erstellt haben, können Sie überprüfen, ob es erfolgreich war oder nicht. Artefaktprotokolle in DevTest Labs bieten Informationen, die Ihnen beim Untersuchen eines Artefaktfehlers helfen. Es gibt verschiedene Möglichkeiten zum Anzeigen der Informationen im Artefaktprotokoll einer Windows-VM.

> [!NOTE]
> Um sicherzustellen, dass Fehler ordnungsgemäß bestimmt und erläutert werden, ist es wichtig, dass das Artefakt ordnungsgemäß strukturiert ist. Informationen dazu, wie ein Artefakt ordnungsgemäß erstellt wird, finden Sie unter [Erstellen benutzerdefinierter Artefakte](devtest-lab-artifact-author.md). Ein Beispiel eines ordnungsgemäß strukturierten Artefakts ist dieses Artefakt zum [Testen von Parametertypen](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes).

## <a name="troubleshoot-artifact-failures-using-the-azure-portal"></a>Problembehandlung bei Artefaktfehlern im Azure-Portal
Gehen Sie folgendermaßen vor, um im Azure-Portal Fehler bei der Erstellung von Artefakten zu untersuchen:

1. Wählen Sie in der Liste mit den Ressourcen Ihr Lab aus.

2. Wählen Sie die Windows-VM, die das Artefakt enthält, das Sie untersuchen möchten.

3. Im linken Bereich unter **ALLGEMEIN** wählen Sie **Artefakte** aus. Eine Liste der Artefakte, die dieser VM zugeordnet sind, wird mit der Angabe des Namens des Artefakts und seines Status angezeigt.

   ![Beispiel-Git-Repository für Artefakte](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Wählen Sie ein Artefakt mit dem Status **Fehler** aus. Das Artefakt wird geöffnet. Es wird eine ausführliche Meldung mit Details zum Fehler des Artefakts angezeigt.

   ![Beispiel-Git-Repository für Artefakte](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-vm"></a>Beheben von Artefaktfehlern innerhalb der VM
Zum Anzeigen der Artefaktprotokolle in der VM gehen Sie wie folgt vor:

1. Melden Sie sich an der VM mit dem Artefakt an, das Sie untersuchen möchten.

2. Navigieren Sie zu „C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status“, wobei „1.9“ die CSE-Versionsnummer ist.

   ![Beispiel-Git-Repository für Artefakte](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Öffnen Sie die Datei **status**, um Informationen anzuzeigen, die Ihnen beim Untersuchen von Artefaktfehlern bei dieser VM helfen.




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge
* [Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs) (Einbinden eines virtuellen Computers in eine vorhandenen AD-Domäne mithilfe einer Resource Manager-Vorlage in Azure DevTest Labs)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie [ein Git-Repository einem Lab hinzufügen](devtest-lab-add-artifact-repo.md).


