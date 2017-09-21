---
title: Entfernen von Azure Automation Hybrid-Runbook Workern | Microsoft-Dokumentation
description: "Dieser Artikel enthält Informationen zum Verwalten bereitgestellter Azure Automation-Hybrid Runbook Worker, mit denen Sie Runbooks auf Computern in Ihrem lokalen Datencenter oder einer Cloudumgebung ausführen können."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: bf4f7befa4a66b739e72d0ea3d632b95f9b31c47
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---

# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Entfernen von Azure Automation-Hybrid-Runbook Workern

Sie haben folgende Möglichkeiten: 

## <a name="removing-hybrid-runbook-worker"></a>Entfernen von Hybrid Runbook Worker

Je nach ihren Anforderungen können Sie eine oder mehrere Hybrid Runbook Worker aus einer Gruppe entfernen, oder Sie können die Gruppe entfernen, je nach Ihren Anforderungen.  Wenn Sie einen Hybrid Runbook Worker von einem lokalen Computer entfernen möchten, führen Sie die folgenden Schritte aus.

1. Navigieren Sie im Azure-Portal zu Ihrem Automation-Konto.  
2. Wählen Sie auf dem Blatt **Einstellungen** die Option **Schlüssel** aus, und notieren Sie sich die Werte für die Felder **URL** und **Primärer Zugriffsschlüssel**.  Sie benötigen diese Informationen im nächsten Schritt.
3. Öffnen Sie eine PowerShell-Sitzung im Administratormodus, und führen Sie den folgenden Befehl aus: `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Verwenden Sie den Schalter **-Verbose** , um ein ausführliches Protokoll zum Entfernungsvorgang zu erhalten.

> [!NOTE]
> Dadurch wird nicht Microsoft Monitoring Agent von diesem Computer entfernt, sondern nur die Funktionen und die Konfiguration der Hybrid Runbook-Workerrolle.  

## <a name="remove-hybrid-worker-groups"></a>Entfernen von Hybrid Worker-Gruppen

Um eine Gruppe zu entfernen, müssen Sie zuerst den Hybrid Runbook Worker von allen Computern entfernen, die Mitglied der Gruppe sind. Verwenden Sie dazu das weiter oben gezeigte Verfahren, und führen Sie dann die folgenden Schritte aus, um die Gruppe zu entfernen.  

1. Wählen Sie im Azure-Portal das Automation-Konto aus.
1. Wählen Sie die Kachel **Hybrid Worker-Gruppen** Kachel und auf dem Blatt **Hybrid Worker-Gruppen** die Gruppe, die Sie löschen möchten.  Nach dem Auswählen der Gruppe wird das Blatt mit den Eigenschaften der **Hybrid Worker-Gruppen** angezeigt.<br> ![Blatt „Hybrid-Runbook-Workergruppen“](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
1. Klicken Sie im Fenster mit den Eigenschaften der ausgewählten Gruppe auf **Löschen**.  Eine Meldung wird angezeigt, in der Sie aufgefordert werden, diese Aktion zu bestätigen. Wählen Sie **Ja**, wenn Sie sicher sind, dass Sie fortfahren möchten.<br> ![Bestätigungsdialogfeld zum Löschen der Gruppe](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie sich [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.
* Informationen zum Installieren von Windows-Hybrid Runbook Workern finden Sie unter [Azure Automation-Hybrid Runbook Worker (Windows)](automation-windows-hrw-install.md).
* Informationen zum Installieren von Linux-Hybrid Runbook Workern finden Sie unter [Bereitstellen eines Linux-Hybrid Runbook Workers](automation-linux-hrw-install.md).
