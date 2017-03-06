---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Windows Server 2016-VM mit IIS und DSC | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Windows Server 2016-VM mit IIS und DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: 8c5874e88fe70bef2b6b1bda666c2c69142e376d
ms.openlocfilehash: 3fdc27c54ad52dfce42f6d039a8453cdfdce00ee
ms.lasthandoff: 03/01/2017

---

# <a name="create-a-vm-with-iis-using-dsc"></a>Erstellen einer VM mit IIS und DSC

Dieses Skript erstellt einen virtuellen Computer und verwendet anschließend die benutzerdefinierte Azure-VM-DSC-Skripterweiterung, um die Internetinformationsdienste (IIS) zu installieren und zu konfigurieren. 

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde. Darüber hinaus sollten Sie die $AdminPassword-Variable zu Beginn des Skripts ändern, damit dieses eindeutig ist und den Kennwortkomplexitätsanforderungen entspricht.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines-windows-cli-options.md).

## <a name="create-vm-sample"></a>Erstellen einer Beispiel-VM

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Schnelles Erstellen einer VM")]

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
| [az vm extension set](https://docs.microsoft.com/cli/azure/vm#create) | Fügt der VM die benutzerdefinierte Skripterweiterung hinzu, die ein Skript zum Installieren von IIS aufruft. |
| [az vm open-port](https://docs.microsoft.com/cli/azure/vm#open-port) | Erstellt eine Netzwerksicherheitsgruppenregel zum Zulassen von eingehendem Datenverkehr. In diesem Beispiel wird Port 80 für HTTP-Datenverkehr geöffnet. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
