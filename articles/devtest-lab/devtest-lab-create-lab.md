---
title: Erstellen eines Labs in Azure DevTest Labs | Microsoft Docs
description: "Erstellen eines Labs in Azure DevTest Labs für virtuelle Computer"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 265a968f902f53c7561c8c7e937f8eacfdb37167
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Erstellen eines Labs in Azure DevTest Labs
Ein Lab in Azure DevTest Labs ist die Infrastruktur, die eine Gruppe von Ressourcen wie z.B. virtuelle Computer (VMs) umfasst, mit der Sie diese Ressourcen besser verwalten können, indem Sie Einschränkungen und Kontingente angeben. Dieser Artikel beschreibt das Erstellen eines Labs mit dem Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen eines Labs benötigen Sie Folgendes:

* Ein Azure-Abonnement. Weitere Informationen zu den Azure-Kaufoptionen finden Sie unter [Azure erwerben](https://azure.microsoft.com/pricing/purchase-options/) oder [Kostenlose Testversion (1 Monat)](https://azure.microsoft.com/pricing/free-trial/). Zum Erstellen des Labs müssen Sie der Besitzer des Abonnements sein.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Schritte zum Erstellen eines Labs in Azure DevTest Labs
Die folgenden Schritte beschreiben, wie Sie mithilfe des Azure-Portals ein Lab in Azure DevTest Labs erstellen. 

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie im Hauptmenü links **Weitere Dienste** (unten auf der Liste).

    ![Menüoption „Weitere Dienste“](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. Wählen Sie in der Liste der verfügbaren Dienste **DevTest Labs**.
1. Wählen Sie auf dem Blatt **DevTest Labs** die Option **Hinzufügen** aus.
   
    ![Hinzufügen eines Labs](./media/devtest-lab-create-lab/add-lab-button.png)

1. Führen Sie auf dem Blatt **DevTest Lab erstellen** folgende Schritte aus:
   
    1. Geben Sie einen **Labnamen** für das neue Lab ein.
    2. Wählen Sie das **Abonnement** aus, das mit dem Lab verknüpft werden soll.
    3. Wählen Sie einen **Speicherort** für das Lab aus.
    4. Geben Sie mithilfe von **Auto-shutdown** (Automatisch herunterfahren) an, ob Sie das automatische Herunterfahren für alle virtuellen Computer im Lab aktivieren und die entsprechenden Parameter festlegen möchten. Die Funktion zum automatischen Herunterfahren dient in erster Linie zum Kostensparen. Mit dieser Funktion können Sie angeben, wann der virtuelle Computer automatisch heruntergefahren werden soll. Nach dem Erstellen des Labs können Sie die Einstellungen zum automatischen Herunterfahren ändern, indem Sie die Schritte im Artikel [Verwalten aller Richtlinien für ein Lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown) ausführen.
    5. Wählen Sie **An Dashboard anheften**, wenn auf dem Portaldashboard eine Verknüpfung des Labs angezeigt werden soll.
    6. Wählen Sie **Automatisierungsoptionen**, um Azure Resource Manager-Vorlagen für die Konfigurationsautomatisierung abzurufen. 
    7. Klicken Sie auf **Erstellen**. Nach Auswählen von **Erstellen** wird das Blatt **DevTest Labs** angezeigt. Sie können den Status der Lab-Erstellung im Bereich **Benachrichtigungen** überwachen. Aktualisieren Sie nach Abschluss der Erstellung die Seite, um das neu erstellte Lab in der Liste der Labs anzuzeigen.  
    
    ![Erstellen eines Lab-Blatts](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem das Lab erstellt wurde, sollten Sie ggf. die folgenden Schritte ausführen:

* [Sicherer Zugriff auf ein Lab](devtest-lab-add-devtest-user.md).
* [Festlegen von Labrichtlinien](devtest-lab-set-lab-policy.md).
* [Erstellen einer Labvorlage](devtest-lab-create-template.md).
* [Erstellen benutzerdefinierter Artefakte für Ihre VMs](devtest-lab-artifact-author.md).
* [Hinzufügen einer VM mit Artefakten zu einem Lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).


