---
title: "Azure CLI-Skriptbeispiel – Installieren von IIS | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Installieren von IIS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 82d40c30c92f5da090e7ec4e2f25ead3908cc603
ms.openlocfilehash: ecf51ea9bf7e74c7847b4394803c2b95a3774446
ms.lasthandoff: 03/02/2017

---

# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a>Schnelles Erstellen einer VM mit der Azure CLI

Dieses Skript erstellt einen virtuellen Azure-Computer mit Windows Server 2016 und verwendet anschließend die benutzerdefinierte Azure-VM-DSC-Skripterweiterung, um die Internetinformationsdienste (IIS) zu installieren. Nachdem das Skript ausgeführt wurde, kann die standardmäßige IIS-Website über die öffentliche IP-Adresse des virtuellen Computers aufgerufen werden.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde. Darüber hinaus sollten Sie die $AdminPassword-Variable zu Beginn des Skripts ändern, damit dieses eindeutig ist und den Kennwortkomplexitätsanforderungen entspricht.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts unter Windows finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Schnelles Erstellen einer VM")]

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
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az vm open-port](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Erstellt eine Netzwerksicherheitsgruppenregel zum Zulassen von eingehendem Datenverkehr. In diesem Beispiel wird Port 80 für HTTP-Datenverkehr geöffnet. |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension#set) | Fügt eine VM-Erweiterung zu einem virtuellen Computer hinzu und führt diese aus. In diesem Beispiel wird die benutzerdefinierte Skripterweiterung zum Installieren von IIS verwendet.|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

