---
title: Erneutes Bereitstellen von virtuellen Linux-Computern | Microsoft Docs
description: Beschreibt, wie Sie virtuelle Linux-Computer zum Beheben von Problemen mit der SSH-Verbindung bereitstellen.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 1cd81944ff1e8b6048315946220adee4bf68e576


---
# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen
Wenn Sie Schwierigkeiten mit der Problembehandlung bei SSH oder dem Anwendungszugriff auf einen virtuellen Azure-Computer haben, lassen sich diese u. U. durch das erneute Bereitstellen des virtuellen Computers beseitigen. Wenn Sie einen virtuellen Computer erneut bereitstellen, wird er innerhalb der Azure-Infrastruktur auf einen neuen Knoten verschoben und dann wieder eingeschaltet. Dabei werden alle Ihre Konfigurationsoptionen und zugehörigen Ressourcen beibehalten. In diesem Artikel erfahren Sie, wie ein virtueller Computer mithilfe der Azure-Befehlszeilenschnittstelle oder dem Azure-Portal erneut bereitgestellt wird.

> [!NOTE]
> Nachdem Sie einen virtuellen Computer erneut bereitgestellt haben, geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert. 
> 
> 

## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Stellen Sie sicher, dass die [aktuelle Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) auf Ihrem Computer installiert ist und Sie sich im Resource Manager-Modus (`azure config mode arm`) befinden.

Verwenden Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle, um Ihren virtuellen Computer erneut bereitzustellen:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Der Status der Änderung des virtuellen Computers wird während des Vorgangs der erneuten Bereitstellung angezeigt. Während der virtuelle Computer den Vorgang der erneuten Bereitstellung auf einem neuen Host durchläuft, ändert sich sein `PowerState` von „Wird ausgeführt“ zu „Wird aktualisiert“, dann zu „Wird gestartet“ und schließlich wieder zu „Wird ausgeführt“. Überprüfen Sie den Status der virtuellen Computer innerhalb einer Ressourcengruppe mit:

```bash
azure vm list -g <resourcegroup>
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Nächste Schritte
Falls beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, finden Sie spezifische Hilfe unter [Problembehandlung bei SSH-Verbindungen](virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Ausführliche Schritte zur Problembehandlung bei SSH](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können auch die Informationen zur [Problembehandlung bei der Anwendung](virtual-machines-linux-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lesen, wenn Sie auf eine Anwendung, die auf Ihrem virtuellen Computer ausgeführt wird, nicht zugreifen können.




<!--HONumber=Nov16_HO3-->


