---
title: Verwalten von grundlegenden Labrichtlinien in Azure DevTest Labs | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie in DevTest Labs einige grundlegende Richtlinien (Einstellungen) für ein Lab festlegen."
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
ms.date: 06/29/2017
ms.author: tarcher
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: ed35d081b191ec41ed9e5970515057a4715c0d59
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---

# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Verwalten von grundlegenden Richtlinien für ein Lab in Azure DevTest Labs

Mit Azure DevTest Labs können Sie Kosten und unnötigen Aufwand in Ihren Labs minimieren, indem Sie Richtlinien (Einstellungen) für jedes Lab verwalten. Dieser Artikel dient als Einstieg in die Verwendung von Richtlinien. Sie erfahren, wie Sie zwei der wichtigsten Richtlinien festlegen: Begrenzen der Anzahl von virtuellen Computern, die von einem einzelnen Benutzer erstellt oder angefordert werden können, und Konfigurieren des automatischen Herunterfahrens. Informationen zum Einrichten der einzelnen Labrichtlinien finden Sie im Artikel [Definieren von Labrichtlinien in Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Zugriff auf Labrichtlinien in Azure DevTest Labs
Die folgenden Schritte führen Sie durch das Einrichten von Richtlinien für ein Lab in Azure DevTest Labs:

Um die Richtlinien für ein Lab anzuzeigen (und zu ändern), gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   

1. Wählen Sie **Konfiguration und Richtlinien** aus.

    ![Blatt „Richtlinieneinstellungen“](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Das Blatt **Konfiguration und Richtlinien** enthält ein Menü mit Einstellungen, die Sie angeben können. Dieser Artikel erläutert nur die Einstellungen für **Virtuelle Computer pro Benutzer** und **Automatisch herunterfahren**. Informationen zu den weiteren Einstellungen finden Sie unter [Verwalten aller Richtlinien für ein Lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Festlegen der virtuellen Computer pro Benutzer
Mit der Richtlinie für **Virtual machines per user** (VMs pro Benutzer) können Sie die maximale Anzahl von virtuellen Computern angeben, die von einem einzelnen Benutzer erstellt werden können. Wenn ein Benutzer versucht, einen virtuellen Computer zu erstellen oder anzufordern, und das Benutzerlimit erreicht wurde, wird in einer Fehlermeldung darauf hingewiesen, dass der virtuelle Computer nicht erstellt/angefordert werden kann. 

1. Wählen Sie im Menü **Konfiguration und Richtlinien** des Labs die Option **Virtuelle Computer pro Benutzer**.
   
    ![Virtual machines per user](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Wählen Sie **Ja**, um die Anzahl von virtuellen Computern pro Benutzer zu begrenzen. Wenn Sie die Anzahl von virtuellen Computern pro Benutzer nicht begrenzen möchten, wählen Sie **Nein**. Wenn Sie **Ja** auswählen, geben Sie einen numerischen Wert für die maximale Anzahl von virtuellen Computern ein, die von einem Benutzer erstellt oder angefordert werden können. 

1. Wählen Sie **Ja**, um die Anzahl von virtuellen Computern zu begrenzen, die eine SSD (Solid-State Disk) verwenden können. Wenn Sie die Anzahl von virtuellen Computern, die eine SSD verwenden können, nicht begrenzen möchten, wählen Sie **Nein**. Wenn Sie **Ja** auswählen, geben Sie einen Wert für die maximale Anzahl von virtuellen Computern ein, die mit einer SSD erstellt werden können. 

1. Wählen Sie **Speichern**aus.

## <a name="set-auto-shutdown"></a>Festlegen des automatischen Herunterfahrens
Die Auto-Shutdown-Richtlinie hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die Uhrzeit anzugeben, zu der die virtuellen Computer für dieses Lab heruntergefahren werden.

1. Wählen Sie auf dem Blatt **Konfiguration und Richtlinien** des Labs die Option **Automatisch herunterfahren** aus.
   
    ![Auto-shutdown](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

1. Wenn Sie diese Richtlinie aktivieren, geben Sie die Uhrzeit (und die Zeitzone) an, zu der alle virtuellen Computer im aktuellen Lab heruntergefahren werden sollen.

1. Geben Sie für die Option, mit der 15 Minuten vor der angegebenen Uhrzeit des automatischen Herunterfahrens eine Benachrichtigung gesendet wird, entweder **Ja** oder **Nein** an. Wenn Sie **Ja** festlegen, geben Sie einen Webhook-URL-Endpunkt ein, der die Benachrichtigung empfangen soll. Weitere Informationen zu Webhooks finden Sie unter [Erstellen eines Webhooks oder einer API-Azure-Funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Wählen Sie **Speichern**aus.

    Standardmäßig gilt diese Richtlinie nach der Aktivierung für alle virtuellen Computer im aktuellen Lab. Um diese Einstellung von einem bestimmten virtuellen Computer zu entfernen, öffnen Sie das Blatt des virtuellen Computers, und ändern Sie die Einstellung **Auto shutdown** (Automatisches Herunterfahren). 

## <a name="set-auto-start"></a>Festlegen des automatischen Startens
Mit der Autostart-Richtlinie können Sie angeben, wann die virtuellen Computer im aktuellen Lab gestartet werden sollen.  

1. Wählen Sie auf dem Blatt **Konfiguration und Richtlinien** des Labs die Option **Automatisch starten** aus.
   
    ![Auto-start](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.

3. Wenn Sie diese Richtlinie aktivieren, geben Sie die geplante Startzeit, die Zeitzone und die Wochentage an, an denen die Uhrzeit gelten soll. 

4. Wählen Sie **Speichern**aus.

    Nach der Aktivierung wird diese Richtlinie nicht automatisch auf alle virtuellen Computer im aktuellen Lab angewendet. Um diese Einstellung auf einen bestimmten virtuellen Computer anzuwenden, öffnen Sie das Blatt des virtuellen Computers, und ändern Sie die Einstellung **Auto start** (Automatisches Starten). 

## <a name="next-steps"></a>Nächste Schritte

- [Definieren von Labrichtlinien in Azure DevTest Labs](devtest-lab-set-lab-policy.md): Informationen zum Ändern anderer Labrichtlinien 

