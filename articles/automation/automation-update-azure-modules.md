---
title: Aktualisieren von Azure-Modulen in Azure Automation | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie jetzt häufig verwendete Azure PowerShell-Module, die standardmäßig in Azure Automation bereitgestellt werden, aktualisieren können."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: ed8c97b642d406a05817ec6c67f31a1b4bce93b0
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---

# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Aktualisieren von Azure PowerShell-Modulen in Azure Automation

Die am häufigsten verwendeten Azure PowerShell-Module werden standardmäßig in jedem Automation-Konto bereitgestellt.  Das Azure-Team aktualisiert die Azure-Module regelmäßig. Über das Automation-Konto bieten wir Ihnen eine Möglichkeit, die Module im Konto zu aktualisieren, sobald neue Versionen im Portal verfügbar sind.  

Da Module von der Produktgruppe regelmäßig aktualisiert werden, können Änderungen bei den enthaltenen Cmdlets auftreten. Je nachdem, welche Änderung ausgeführt wurde, z.B. ob nein Parameter umbenannt oder ein Cmdlet vollständig eingestellt wurde, können negative Auswirkungen auf Ihre Runbooks auftreten. Um Auswirkungen auf Ihre Runbooks und die Prozesse zu vermeiden, die sie automatisieren, wird dringend empfohlen, dass Sie Tests und Überprüfungen ausführen, bevor Sie den Vorgang fortsetzen.  Wenn Sie für diesen Zweck kein dediziertes Automation-Konto haben, sollten Sie eines erstellen. So können Sie während der Entwicklung Ihrer Runbooks viele verschiedene Szenarios und Permutationen testen und iterative Änderungen wie das Aktualisieren der PowerShell-Module ausführen.  Nachdem die Ergebnisse überprüft werden und Sie alle erforderlichen Änderungen angewendet haben, koordinieren die Migration aller Runbooks, die Änderungen erforderten, und führen Sie das Update wie unten beschrieben in der Produktion aus.     

## <a name="updating-azure-modules"></a>Aktualisieren von Azure-Modulen

1. Auf dem Blatt „Module“ Ihres Automation-Kontos gibt es eine Option namens **Azure-Module aktualisieren**.  Sie ist immer aktiviert.<br><br> ![Option zum Aktualisieren von Azure-Modulen auf dem Blatt „Module“](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Klicken Sie auf **Azure-Module aktualisieren**. Dann wird eine Bestätigungsbenachrichtigung angezeigt, in der Sie gefragt werden, ob Sie den Vorgang fortsetzen möchten.<br><br> ![Benachrichtigung zum Aktualisieren von Azure-Modulen](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klicken Sie auf **Ja**, und der Aktualisierungsvorgang für die Module wird gestartet.  Der Aktualisierungsvorgang dauert etwa 15 bis 20 Minuten, und die folgenden Module werden aktualisiert:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Wenn die Module bereits auf dem neuesten Stand sind, ist der Prozess in wenigen Sekunden abgeschlossen.  Wenn der Aktualisierungsvorgang abgeschlossen ist, werden Sie benachrichtigt.<br><br> ![Aktualisierungsstatus beim Aktualisieren der Azure-Module](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> In Azure Automation werden die neueste Module in Ihrem Automation-Konto verwendet, wenn ein neuer geplanter Auftrag ausgeführt wird.    

Wenn Sie Cmdlets aus diesen Azure PowerShell-Modulen in Ihren Runbooks zum Verwalten von Azure-Ressourcen verwenden, dann sollten Sie diesen Aktualisierungsvorgang ungefähr jeden Monat ausführen, um sicherzustellen, dass Sie die neuesten Module haben.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Integrationsmodulen und zum Erstellen benutzerdefinierter Module, um Automation weiter in andere Systeme, Dienste oder Lösungen zu integrieren, finden Sie unter [Integrationsmodule](automation-integration-modules.md).

* Sie sollten die Verwendung der Integration von Quellcodeverwaltung mit [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) oder [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) in Betracht ziehen, um Releases Ihrer Automation-Runbooks und Konfigurationsportfolios zentral zu verwalten und zu steuern.  
