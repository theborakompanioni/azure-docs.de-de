---
title: "Überwachen von Azure und von virtuellen Windows-Computern | Microsoft-Dokumentation"
description: 'Tutorial: Verwalten eines virtuellen Windows-Computers mit Azure PowerShell'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 2bf6e3cf2f039a9c92617203a3d4cf2aac8901ce
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017

---

# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a>Überwachen eines virtuellen Windows-Computers mit Azure PowerShell

Bei der Überwachung von Azure werden Agents verwendet, um Start- und Leistungsdaten von Azure-VMs zu sammeln, in Azure zu speichern und über das Portal, das Azure PowerShell-Modul und die Azure-Befehlszeilenschnittstelle verfügbar zu machen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren der Startdiagnose auf einem virtuellen Computer
> * Anzeigen der Startdiagnose
> * Anzeigen der VM-Hostmetriken
> * Bereitstellen der Diagnoseerweiterung
> * Anzeigen von Metriken des virtuellen Computers
> * Erstellen einer Warnung
> * Einrichten der erweiterten Überwachung

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren und Konfigurieren von Azure PowerShell) Informationen dazu.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe, des virtuellen Computers und des Speicherorts.

## <a name="view-boot-diagnostics"></a>Anzeigen der Startdiagnose

Beim Start von virtuellen Windows-Computern erfasst der Startdiagnose-Agent Bildschirmausgaben, die zur Fehlerbehebung verwendet werden können. Diese Funktionalität ist standardmäßig aktiviert. Die erfassten Screenshots werden in einem Azure-Speicherkonto gespeichert, das auch standardmäßig erstellt wird. 

Sie erhalten die Startdiagnosedaten mit dem Befehl [Get AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata). Im folgenden Beispiel wird die Startdiagnose in den Stammordner des Laufwerks *c:\* heruntergeladen. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Anzeigen von Hostmetriken

Eine Windows-VM verfügt über eine dedizierte Host-VM in Azure, mit der sie interagiert. Es werden automatisch Metriken für den Host gesammelt, die im Azure-Portal angezeigt werden können.

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
2. Klicken Sie in dem Blatt der VM auf **Metriken**, und wählen Sie dann eine der Host-Metriken aus **Verfügbare Metriken**  aus.

    ![Anzeigen von Hostmetriken](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Installieren der Diagnoseerweiterung

Die grundlegenden Hostmetriken sind direkt verfügbar. Wenn Sie aber detailliertere und VM-spezifische Metriken anzeigen möchten, müssen Sie die Azure-Diagnose-Erweiterung auf dem virtuellen Computer installieren. Die Azure-Diagnose-Erweiterung ermöglicht das Abrufen zusätzlicher Überwachungs- und Diagnosedaten von virtuellen Computern. Sie können diese Leistungsmetriken anzeigen und basierend auf der VM-Leistung Benachrichtigungen erstellen. Die Diagnose-Erweiterung wird wie folgt über das Azure-Portal installiert:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
2. Klicken Sie auf **Diagnoseeinstellungen**. Die Liste zeigt, dass die *Startdiagnose* bereits im vorherigen Abschnitt aktiviert wurde. Klicken Sie auf das Kontrollkästchen für *Grundlegende Metriken*.
3. Klicken Sie auf die Schaltfläche **Überwachung auf Gastebene aktivieren**.

    ![Anzeigen von Diagnosemetriken](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Anzeigen von Metriken des virtuellen Computers

Sie können die VM-Metriken auf die gleiche Weise anzeigen, wie Sie die Metriken zur Host-VM angezeigt haben:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
2. Um die Leistung des virtuellen Computers anzuzeigen, klicken Sie dem Blatt der VM auf **Metriken**, und wählen Sie dann eine der Diagnosemetriken unter **Verfügbare Metriken** aus.

    ![Anzeigen von Metriken des virtuellen Computers](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Erstellen von Warnungen

Sie können Warnungen auf Grundlage von bestimmten Leistungsmetriken erstellen. Warnungen können für Benachrichtigungen verwendet werden, wenn z.B. die durchschnittliche CPU-Auslastung einen bestimmten Schwellenwert überschreitet oder wenn der verfügbare freie Speicherplatz unter einen bestimmten Wert fällt. Warnungen werden im Azure-Portal angezeigt oder können per E-Mail gesendet werden. Sie können auch als Reaktion auf die generierten Warnungen Azure Automation-Runbooks oder Azure Logic Apps auslösen.

Das folgende Beispiel erstellt eine Warnung für die durchschnittliche CPU-Auslastung.

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
2. Klicken Sie auf dem Blatt des virtuellen Computers auf **Warnungsregeln** und dann am oberen Rand des Warnungsblatts auf **Metrikwarnung hinzufügen**.
4. Geben Sie einen **Namen** für die Warnung ein, z.B. *myAlertRule*.
5. Um eine Warnung auszulösen, wenn der CPU-Prozentsatz 1.0 für fünf Minuten überschreitet, belassen Sie alle anderen Standardeinstellungen ausgewählt.
6. Aktivieren Sie optional das Kontrollkästchen *E-Mail-Besitzer, Mitwirkende und Leser*, um E-Mail-Benachrichtigungen zu senden. Als Standardaktion wird im Portal eine Benachrichtigung angezeigt.
7. Klicken Sie auf die Schaltfläche **OK**.

## <a name="advanced-monitoring"></a>Erweiterte Überwachung 

Mithilfe der [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) können Sie eine noch umfassendere Überwachung Ihrer virtuellen Computer umsetzen. Wenn Sie dies noch nicht getan haben, können Sie sich für eine [kostenlose Testversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) von Operations Management Suite registrieren.

Wenn Sie auf das OMS-Portal zugreifen können, finden Sie den Arbeitsbereichsschlüssel und die Arbeitsbereichs-ID auf dem Blatt „Einstellungen“. Verwenden Sie den Befehl [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension), um die OMS-Erweiterung zu dem virtuellen Computer hinzufügen. Aktualisieren Sie die Variablenwerte im nachstehenden Beispiel gemäß Ihrem OMS-Arbeitsbereichsschlüssel und Ihrer Arbeitsbereichs-ID.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Nach wenigen Minuten sollte der neue virtuelle Computer im OMS-Arbeitsbereich angezeigt werden. 

![OMS-Blatt](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie virtuelle Computer mit Azure Security Center konfiguriert und überprüft. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen einer Ressourcengruppe und VM 
> * Aktivieren der Startdiagnose auf dem virtuellen Computer
> * Anzeigen der Startdiagnose
> * Anzeigen von Hostmetriken
> * Bereitstellen der Diagnoseerweiterung
> * Anzeigen von Metriken des virtuellen Computers
> * Erstellen einer Warnung
> * Einrichten der erweiterten Überwachung

Im nächsten Tutorial erhalten Sie Informationen zu Azure Security Center.

> [!div class="nextstepaction"]
> [Verwalten der VM-Sicherheit](./tutorial-azure-security.md)
