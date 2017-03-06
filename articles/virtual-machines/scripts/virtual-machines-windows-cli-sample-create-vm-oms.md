---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Windows Server 2016-VM mit OMS-Überwachung | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Windows Server 2016-VM mit OMS-Überwachung"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: a172d73732354d31d717d8e2f3a5c5c43cbbd6dc
ms.openlocfilehash: 3c479ed2450927a4e7c6cd07250873c556735b8b
ms.lasthandoff: 03/01/2017

---

# <a name="monitor-a-vm-with-operations-management-suite"></a>Überwachen einer VM mit der Operations Management Suite

Dieses Skript erstellt eine Azure-VM, installiert den OMS-Agent (Operations Management Suite) und registriert das System bei einem OMS-Arbeitsbereich. Nach Ausführung des Skripts wird der virtuelle Computer in der OMS-Konsole angezeigt.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde. Darüber hinaus sollten Sie die $AdminPassword-Variable zu Beginn des Skripts ändern, damit dieses eindeutig ist und den Kennwortkomplexitätsanforderungen entspricht.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts unter Windows finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines-windows-cli-options.md).

## <a name="create-vm-sample-with-operations-management-suite"></a>Erstellen einer Beispiel-VM mit der Operations Management Suite

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "Schnelles Erstellen einer VM")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die VM und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension#set) | Führt einen VM-Erweiterungs-Agent für einen virtuellen Computer aus. In diesem Fall wird die Operations Management Suite-Agent-Erweiterung verwendet, um den OMS-Agent zu installieren und die VM in einem OMS-Arbeitsbereich zu registrieren. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

