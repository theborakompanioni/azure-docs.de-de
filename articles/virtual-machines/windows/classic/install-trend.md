---
title: Installieren von Trend Micro Deep Security auf einem virtuellen Computer | Microsoft Docs
description: Dieser Artikel beschreibt, wie Sie Trend Micro Security auf einem mit dem klassischen Bereitstellungsmodell erstellten virtuellen Computer in Azure installieren und konfigurieren.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: cb10c04d76e957f88d67a791e3b4a19797704eea
ms.lasthandoff: 03/27/2017


---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem virtuellen Windows-Computer
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

Dieser Artikel zeigt Ihnen das Installieren und Konfigurieren von Trend Micro Deep Security als einen Dienst auf einem neuen oder vorhandenen virtuellen Computer unter Windows Server. Deep Security as a Service umfasst den Antischadsoftwareschutz, eine Firewall, ein Eindringschutzsystem und die Integritätsüberwachung.

Der Client wird als Sicherheitserweiterung über den VM-Agent installiert. Auf einem neuen virtuellen Computer installieren Sie den VM-Agent zusammen mit Deep Security Agent. Auf einem vorhandenen virtuellen Computer, der nicht über den VM-Agent verfügt, müssen Sie diesen zunächst herunterladen und installieren. Dieser Artikel deckt beide Situationen ab.

Wenn Sie über ein vorhandenes Abonnement von Trend Micro für eine lokale Lösung verfügen, können Sie es zum Schützen Ihrer virtuellen Azure-Computer verwenden. Wenn Sie noch kein Kunde sind, können Sie sich für ein Testabonnement registrieren. Weitere Informationen zu dieser Lösung finden Sie im Trend Micro-Blogbeitrag [Microsoft Azure VM Agent Extension For Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945)(Microsoft Azure-VM-Agent-Erweiterung für Deep Security, in englischer Sprache).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Installieren von Deep Security Agent auf einem neuen virtuellen Computer
Mit dem [klassischen Azure-Portal](http://manage.windowsazure.com) können Sie den VM-Agent und die Trend Micro-Sicherheitserweiterung installieren, wenn Sie die Option **Aus Katalog** zum Erstellen des virtuellen Computers verwenden. Wenn Sie einen einzelnen virtuellen Computer erstellen, ist die Verwendung des Portals eine einfache Möglichkeit, den Trend Micro-Schutz hinzuzufügen.

Die Option **Aus Katalog** öffnet einen Assistenten, der Ihnen beim Einrichten des virtuellen Computers hilft. Sie verwenden die letzte Seite des Assistenten zum Installieren des VM-Agents und der Trend Micro-Sicherheitserweiterung. Allgemeine Anweisungen dazu finden Sie unter [Erstellen eines virtuellen Computers unter Windows im klassischen Azure-Portal](tutorial.md). Führen Sie auf der letzten Seite des Assistenten folgende Schritte aus:

1. Aktivieren Sie unter **VM-Agent** die Option **VM-Agent installieren**.
2. Aktivieren Sie unter **Sicherheitserweiterungen** die Option **Trend Micro Deep Security Agent**.
   
   ![Installieren des VM-Agents und von Deep Security Agent](./media/install-trend/InstallVMAgentandTrend.png)
3. Aktivieren Sie das Kontrollkästchen, um den virtuellen Computer zu erstellen.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Installieren von Deep Security Agent auf einem vorhandenen virtuellen Computer
Um den Agent auf einem vorhandenen virtuellen Computer zu installieren, benötigen Sie Folgendes:

* Das Azure PowerShell-Modul Version 0.8.2 oder höher muss auf Ihrem lokalen Computer installiert sein. Sie können die installierte Version von Azure PowerShell mit dem Befehl **Get-Module azure | format-table version** überprüfen. Anweisungen und einen Link zur neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). Melden Sie sich mit `Add-AzureAccount`bei Ihrem Azure-Abonnement an.
* Der VM-Agent muss auf dem virtuellen Zielcomputer installiert sein.

Überprüfen Sie zunächst, ob der VM-Agent bereits installiert ist. Füllen Sie den Clouddienstnamen und den Namen des virtuellen Computers aus, und führen Sie dann die folgenden Befehle an einer Azure PowerShell-Eingabeaufforderung mit Administratorrechten aus. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen < und >.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Wenn Sie den Namen des Clouddiensts und des virtuellen Computers nicht kennen, führen Sie **Get-AzureVM** aus, um diese Informationen für alle virtuellen Computer im aktuellen Abonnement anzuzeigen.

Wenn der Befehl **write-host** den Wert **True** zurückgibt, ist der VM-Agent installiert. Wenn **False**zurückgegeben wird, nutzen Sie die Anweisungen und den Link zum Download im Azure-Blogbeitrag [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947)(VM-Agent und Erweiterungen, Teil 2, in englischer Sprache).

Wenn der VM-Agent installiert ist, führen Sie diese Befehle aus.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte
Es dauert einige Minuten, bis der Agent gestartet wird, wenn er installiert ist. Danach müssen Sie Deep Security auf dem virtuellen Computer aktivieren, sodass es durch einen Deep Security Manager verwaltet werden kann. Weitere Anweisungen finden Sie in folgenden Artikeln:

* Artikel von Trend zu dieser Lösung [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* [Windows PowerShell-Beispielskript](http://go.microsoft.com/fwlink/?LinkId=404100) zum Konfigurieren des virtuellen Computers
* [Anweisungen](http://go.microsoft.com/fwlink/?LinkId=404099) für das Beispiel

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird]

[Azure-VM-Erweiterungen und -Features]

<!--Link references-->
[Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird]:connect-logon.md
[Azure-VM-Erweiterungen und -Features]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409

