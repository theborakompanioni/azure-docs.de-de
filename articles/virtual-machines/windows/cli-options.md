---
title: Verwenden der Azure CLI unter Windows | Microsoft-Dokumentation
description: Verwenden der Azure CLI unter Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5554097904598a7bdb7bb0148f3a00923da2cfa7
ms.lasthandoff: 04/03/2017

---

# <a name="using-the-azure-cli-on-windows"></a>Verwenden der Azure CLI unter Windows

Die Azure-Befehlszeilenschnittstelle (CLI) stellt eine Befehlszeile und eine Skriptumgebung für das Erstellen und Verwalten von Azure-Ressourcen bereit. Die Azure CLI steht für Mac OS X-, Linux- und Windows-Betriebssysteme zur Verfügung. Die CLI-Befehle sind für all diese Betriebssysteme identisch, die Skriptsyntax kann jedoch je nach Betriebssystem abweichen.

In dieser Dokumentation wird erläutert, wie die Azure CLI unter Windows installiert und ausgeführt werden kann, und es werden Überlegungen zur Syntax beleuchtet. Eine ausführliche Erörterung der Azure CLI finden Sie in der [Azure CLI-Dokumentation]( https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="windows-subsystem-for-linux"></a>Windows-Subsystem für Linux

Das Windows-Subsystem für Linux (WSL) bietet eine Ubuntu Linux-Umgebung für die Windows 10 Anniversary-Edition. Bei Aktivierung bietet WSL eine native Bash-Oberfläche, die zum Erstellen und Ausführen von Azure CLI-Skripts verwendet werden kann. Da WSL eine native Bash-Oberfläche bereitstellt, können Azure CLI-Skripts ohne Änderungen unter Mac OS X, Linux und Windows eingesetzt werden.

Führen Sie die folgenden Schritte aus, um die Azure CLI in WSL zu verwenden.

|Aufgabe | Anleitung |
|---|---|
| Aktivieren von WSL | [Dokumentation zum Installieren von WSL](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| Installieren der Azure CLI |[Installieren der CLI für WSL / Ubuntu 14.04](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

Die Azure CLI kann in Windows nativ ausgeführt werden. In dieser Konfiguration wird das Azure CLI-Paket auf dem Windows-Betriebssystem ausgeführt, und Befehle können über PowerShell ausgeführt werden. Azure CLI-Befehle und Skripts können in dieser Konfiguration auf jeder unterstützten Windows-Version ausgeführt werden, es ist jedoch eine plattformspezifische Skriptsyntax erforderlich. Aufgrund dieser Tatsache können Skripts möglicherweise nicht ohne Änderungen unter Mac OS X, Linux und Windows eingesetzt werden.

Installieren Sie das Paket mithilfe der Anweisungen unter [Installieren der CLI unter Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows), um die Azure CLI unter Windows einzusetzen.

## <a name="docker-image"></a>Docker-Image

Bei Einsatz von Docker für Windows kann ein Docker-Image gestartet werden, das die Azure CLI umfasst. Dieses Image basiert auf Linux und bietet eine native Bash-Oberfläche.  Bei Einsatz von Docker für Windows und dem Azure CLI-Image können Skripts sowohl unter Mac OS X als auch unter Linux und Windows verwendet werden. 

Um die Azure CLI unter Docker für Windows zu verwenden, müssen Sie zunächst sicherstellen, dass Docker für Windows ausgeführt wird. Führen Sie dann den folgenden Befehl aus.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

Durch den Befehl wird eine Bash-Sitzung gestartet, in der die Azure CLI-Tools bereits geladen sind.

## <a name="next-steps"></a>Nächste Schritte

[Azure CLI-Beispiele für Linux-VMs](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure CLI Samples](../../app-service-web/app-service-cli-samples.md) (Azure CLI-Beispiele)

[Azure CLI samples for Azure SQL Database](../../sql-database/sql-database-cli-samples.md) (Azure CLI-Beispiele für Azure SQL-Datenbank)

