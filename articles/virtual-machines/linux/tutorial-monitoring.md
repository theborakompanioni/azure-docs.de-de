---
title: "Überwachen virtueller Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie etwas zum Überwachen der Startdiagnose und von Leistungsmetriken für einen virtuellen Linux-Computer in Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: c5eab88f1b2311d52e582a0aa1121c8001437376
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="how-to-monitor-a-linux-virtual-machine-in-azure"></a>Überwachen eines virtuellen Linux-Computers in Azure

Um sicherzustellen, dass die virtuellen Computer (VMs) in Azure ordnungsgemäß ausgeführt werden, können Sie die Startdiagnose und Leistungsmetriken überprüfen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren der Startdiagnose auf dem virtuellen Computer
> * Anzeigen der Startdiagnose
> * Anzeigen von Hostmetriken
> * Aktivieren der Diagnoseerweiterung auf dem virtuellen Computer
> * Anzeigen von Metriken des virtuellen Computers
> * Erstellen von Benachrichtigungen basierend auf Diagnosemetriken
> * Einrichten der erweiterten Überwachung


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-vm"></a>Erstellen eines virtuellen Computers

Um die Diagnose und die Metriken in Aktion anzuzeigen, benötigen Sie einen virtuellen Computer. Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt die Ressourcengruppe *myResourceGroupMonitor* am Standort *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Jetzt können Sie mit [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) einen virtuellen Computer erstellen. Im folgenden Beispiel wird ein virtueller Computer namens *myVM* erstellt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Aktivieren der Startdiagnose

Wenn virtuelle Linux-Computer starten, erfasst die Startdiagnoseerweiterung die Startausgabe und speichert sie in Azure Storage. Diese Daten können zum Beheben von Startproblemen bei virtuellen Computern verwendet werden. Die Startdiagnose wird nicht automatisch aktiviert, wenn Sie eine Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle erstellen.

Vor dem Aktivieren der Startdiagnose muss ein Speicherkonto zum Speichern der Startprotokolle erstellt werden. Speicherkonten benötigen einen global eindeutigen Namen, der zwischen 3 und 24 Zeichen lang sein muss und nur Ziffern und Kleinbuchstaben enthalten darf. Sie erstellen ein Speicherkonto mit dem Befehl [az storage account create](/cli/azure/storage/account#create). In diesem Beispiel wird eine zufällige Zeichenfolge verwendet, um einen eindeutigen Speicherkontonamen zu erstellen. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Bei der Aktivierung der Startdiagnose wird der URI zum Blob Storage-Container benötigt. Der folgende Befehl fragt das Speicherkonto ab, um diesen URI zurückzugeben. Der URI-Wert wird in der Variable *bloburi* gespeichert, die im nächsten Schritt verwendet wird.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Aktivieren Sie nun die Startdiagnose mit [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). Der `--storage`-Wert ist der Blob-URI, der im vorherigen Schritt erfasst wurde.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Anzeigen der Startdiagnose

Wenn die Startdiagnose aktiviert ist, werden bei jedem Beenden und Starten des virtuellen Computers Informationen zum Startvorgang in eine Protokolldatei geschrieben. Heben Sie in diesem Beispiel zunächst die Zuordnung des virtuellen Computers mit dem Befehl [az vm deallocate](/cli/azure/vm#deallocate) wie folgt auf:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Starten Sie nun den virtuellen Computer mit dem Befehl [az vm start]( /cli/azure/vm#stop) wie folgt:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Sie erhalten die Startdiagnosedaten für *myVM* mithilfe des Befehls [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) wie folgt:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Anzeigen von Hostmetriken

Eine Linux-VM verfügt über einen dedizierten Host in Azure, mit dem sie interagiert. Es werden automatisch Metriken für den Host gesammelt, die folgendermaßen im Azure-Portal angezeigt werden können:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroupMonitor** und dann in der Ressourcenliste **myVM** aus.
1. Um die Leistung der Host-VM anzuzeigen, klicken Sie dem Blatt der VM auf **Metriken**, und wählen Sie dann eine der *[Host]*-Metriken unter **Verfügbare Metriken** aus.

    ![Anzeigen von Hostmetriken](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Installieren der Diagnoseerweiterung

> [!IMPORTANT]
> Dieses Dokument beschreibt Version 2.3 der Linux-Diagnoseerweiterung, die veraltet ist. Version 2.3 wird bis zum 30. Juni 2018 unterstützt.
>
> Stattdessen kann Version 3.0 der Linux-Diagnoseerweiterung aktiviert werden. Weitere Informationen finden Sie in der [Dokumentation](./diagnostic-extension.md).

Die grundlegenden Hostmetriken sind direkt verfügbar. Wenn Sie aber detailliertere und VM-spezifische Metriken anzeigen möchten, müssen Sie die Azure-Diagnose-Erweiterung auf dem virtuellen Computer installieren. Die Azure-Diagnose-Erweiterung ermöglicht das Abrufen zusätzlicher Überwachungs- und Diagnosedaten von virtuellen Computern. Sie können diese Leistungsmetriken anzeigen und basierend auf der VM-Leistung Benachrichtigungen erstellen. Die Diagnose-Erweiterung wird wie folgt über das Azure-Portal installiert:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
1. Klicken Sie auf **Diagnoseeinstellungen**. Die Liste zeigt, dass die *Startdiagnose* bereits im vorherigen Abschnitt aktiviert wurde. Klicken Sie auf das Kontrollkästchen für *Grundlegende Metriken*.
1. Wählen Sie im Abschnitt *Speicherkonto* das Konto *mydiagdata[1234]* aus, das im vorherigen Abschnitt erstellt wurde.
1. Klicken Sie auf die Schaltfläche **Save** .

    ![Anzeigen von Diagnosemetriken](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Anzeigen von Metriken des virtuellen Computers

Sie können die VM-Metriken auf die gleiche Weise anzeigen, wie Sie die Metriken zur Host-VM angezeigt haben:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
1. Um die Leistung des virtuellen Computers anzuzeigen, klicken Sie dem Blatt der VM auf **Metriken**, und wählen Sie dann eine der Diagnosemetriken unter **Verfügbare Metriken** aus.

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

Wenn Sie auf das OMS-Portal zugreifen, finden Sie den Arbeitsbereichsschlüssel und die Arbeitsbereichs-ID auf dem Blatt „Einstellungen“. Ersetzen Sie <workspace-key> und <workspace-id> durch die Werten aus Ihrem OMS-Arbeitsbereich, und verwenden Sie dann **az vm extension set**, um dem virtuellen Computer die OMS-Erweiterung hinzuzufügen:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

Auf dem Blatt „Protokollsuche“ im OMS-Portal sehen Sie *myVM* wie in der folgenden Abbildung dargestellt:

![OMS-Blatt](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie virtuelle Computer mit Azure Security Center konfiguriert und überprüft. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Aktivieren der Startdiagnose auf dem virtuellen Computer
> * Anzeigen der Startdiagnose
> * Anzeigen von Hostmetriken
> * Aktivieren der Diagnoseerweiterung auf dem virtuellen Computer
> * Anzeigen von Metriken des virtuellen Computers
> * Erstellen von Benachrichtigungen basierend auf Diagnosemetriken
> * Einrichten der erweiterten Überwachung

Im nächsten Tutorial erhalten Sie Informationen zu Azure Security Center.

> [!div class="nextstepaction"]
> [Verwalten der VM-Sicherheit](./tutorial-azure-security.md)
