---
title: Azure Automation-Hybrid Runbook Worker (Linux) | Microsoft-Dokumentation
description: "Dieser Artikel enthält Informationen zum Installieren eines Azure Automation-Hybrid Runbook Workers, mit dem Sie Runbooks auf Linux-basierten Computern in Ihrem lokalen Datencenter oder einer Cloudumgebung ausführen können."
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
ms.date: 09/07/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: d01d9b8723e321ca0f2f9073d5b99b984e30d971
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---

# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Bereitstellen eines Linux-Hybrid Runbook Workers
Runbooks in Azure Automation können nicht auf Ressourcen in anderen Clouds oder in Ihrer lokalen Umgebung zugreifen, da sie in der Azure-Cloud ausgeführt werden.  Die Azure Automation-Funktion „Hybrid Runbook Worker“ ermöglicht das Ausführen von Runbooks direkt auf dem Computer, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Runbooks werden in Azure Automation gespeichert und verwaltet und an einen oder mehrere dafür vorgesehene Computer übermittelt.  

Diese Funktionalität wird in der folgenden Abbildung veranschaulicht:<br>  

![Hybrid-Runbook-Worker – Übersicht](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Eine technische Übersicht über die Hybrid Runbook Worker-Rolle finden Sie unter [Übersicht über die Automation-Architektur](automation-offering-get-started.md#automation-architecture-overview). Lesen Sie sich vor dem Bereitstellen eines Hybrid Runbook Worker die folgenden Informationen zu [Hardware- und Softwareanforderungen](automation-offering-get-started.md#hybrid-runbook-worker) und [Informationen zur Vorbereitung Ihres Netzwerks](automation-offering-get-started.md#network-planning) durch.  Lesen Sie sich nach dem erfolgreichen Bereitstellen eines Runbook Workers [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.     

## <a name="hybrid-runbook-worker-groups"></a>Hybrid-Runbook-Workergruppen
Jeder Hybrid-Runbook-Worker ist ein Mitglied einer Hybrid-Runbook-Workergruppe, die Sie beim Installieren des Agents angeben.  Eine Gruppe kann einen einzelnen Agent umfassen, aber für eine höhere Verfügbarkeit können Sie mehrere Agents installieren.

Wenn Sie ein Runbook auf einen Hybrid Runbook Worker starten, geben Sie die Gruppe an, in der das Runbook ausgeführt werden soll.  Die Mitglieder der Gruppe legen fest, welcher Worker die Anforderung verarbeitet.  Sie können keinen bestimmten Worker angeben.

## <a name="installing-linux-hybrid-runbook-worker"></a>Installieren eines Linux Hybrid Runbook Worker
Wenn Sie einen Hybrid Runbook Worker auf einem Linux-Computer installieren und konfigurieren möchten, nutzen Sie einen sehr unkomplizierten Vorgang zur manuellen Installation und Konfiguration der Rolle.   Dazu ist die Aktivierung der Lösung **Automation Hybrid Worker** in Ihrem OMS-Arbeitsbereich erforderlich sowie das Ausführen einiger Befehle, um den Computer als Worker zu registrieren und ihn zu einer neuen oder vorhandenen Gruppe hinzuzufügen. 

Bevor Sie den Vorgang fortsetzen, müssen Sie den Log Analytics-Arbeitsbereich, mit dem Ihr Automation-Konto verknüpft ist, sowie den Primärschlüssel für Ihr Automation-Konto notieren.  Sie finden beides im Portal. Wählen Sie Ihr Automation-Konto aus, und klicken Sie für die Arbeitsbereichs-ID auf **Arbeitsbereich** und für den Primärschlüssel auf **Schlüssel**.  

1.  Aktivieren Sie die Lösung „Automation Hybrid Worker“ in OMS. Dies kann durch Folgendes geschehen:

   1. Aktivieren Sie die Lösung **Automation Hybrid Worker** im Lösungskatalog des [OMS-Portals](https://mms.microsoft.com).
   2. Führen Sie das folgende Cmdlet aus:

        ```powershell
         $null = Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

2.  Führen Sie den folgenden Befehl aus. Ändern Sie die Werte für die Parameter *-w* und *-k* auf Ihrem Linux-Computer.
    
    ```
    sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <OMSworkspaceId> -k <AutomationSharedKey> --groupname <hybridgroupname> -e <automationendpoint>
    ```
3. Nach Ausführung des Befehls werden auf dem Blatt „Hybrid Worker-Gruppen“ im Azure-Portal die neue Gruppe und die Anzahl von Mitgliedern angezeigt. Im Falle einer bereits vorhandenen Gruppe wird die Anzahl der Mitglieder erhöht.  Sie können die Gruppe aus der Liste auf dem Blatt **Hybrid Worker-Gruppen** auswählen und dann die Kachel **Hybrid Worker** auswählen.  Auf dem Blatt **Hybrid Worker** werden die einzelnen Mitglieder der Gruppe aufgelistet.  

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie sich [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.
* Anweisungen zum Entfernen von Hybrid Runbook Workern finden Sie unter [Entfernen von Azure Automation-Hybrid-Runbook Workern](automation-remove-hrw.md).
