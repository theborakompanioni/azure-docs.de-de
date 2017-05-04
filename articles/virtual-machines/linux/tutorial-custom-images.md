---
title: Erstellen von benutzerdefinierten VM-Images mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: 'Tutorial: Erstellen eines benutzerdefinierten VM-Images mithilfe der Azure-Befehlszeilenschnittstelle.'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/21/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: f7fd7d63e6bb1da7022cef782e3b84ea20a64c31
ms.lasthandoff: 04/26/2017

---

# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe der Befehlszeilenschnittstelle

In diesem Tutorial erfahren Sie, wie Sie ein eigenes benutzerdefiniertes Image eines virtuellen Azure-Computers erstellen. Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Starten von Konfigurationen verwendet werden, z.B. zum Vorabladen von Anwendungen, Anwendungskonfigurationen und anderen Betriebssystemkonfigurationen. Beim Erstellen eines benutzerdefinierten Images sind der virtuelle Computer und alle angefügten Datenträger im Image enthalten. 

Die Schritte in diesem Tutorial können mit der neuesten Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) ausgeführt werden.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

## <a name="create-an-image"></a>Erstellen eines Images

Zum Erstellen eines Images eines virtuellen Computers müssen Sie den virtuellen Computer vorbereiten, indem Sie die Bereitstellung und Zuordnung aufheben und dann den virtuellen Quellcomputer als generalisiert markieren.

### <a name="deprovision-the-vm"></a>Aufheben der Bereitstellung des virtuellen Computers 

Durch Aufheben der Bereitstellung werden computerspezifische Informationen entfernt und der virtuelle Computer dadurch generalisiert. Durch diese Generalisierung können viele virtuelle Computer aus einem einzelnen Image bereitgestellt werden. Beim Aufheben der Bereitstellung wird der Hostname auf `localhost.localdomain` zurückgesetzt. Ebenfalls gelöscht werden SSH-Hostschlüssel, Namenserverkonfigurationen, das Stammkennwort und zwischengespeicherte DHCP-Leases.

Verwenden Sie zum Aufheben der Bereitstellung des virtuellen Computers den Azure-VM-Agent (waagent). Der Azure-VM-Agent ist auf dem virtuellen Computer installiert und verwaltet die Bereitstellung und Interaktion mit dem Azure Fabric Controller. Weitere Informationen erhalten Sie im [Benutzerhandbuch für Azure Linux-Agent](agent-user-guide.md).

Stellen Sie eine Verbindung mit dem virtuellen Computer über SSH her, und führen Sie den Befehl zum Aufheben der Bereitstellung des virtuellen Computers aus. Mit dem Argument `+user` werden auch das zuletzt bereitgestellte Benutzerkonto und alle zugehörigen Daten gelöscht. Ersetzen Sie die IP-Beispieladresse durch die öffentliche IP-Adresse Ihres virtuellen Computers.

```bash
ssh azureuser@52.174.34.95 sudo waagent -deprovision+user -force
```
Schließen Sie die SSH-Sitzung.

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Aufheben der Zuordnung und Markieren des virtuellen Computers als generalisiert

Zum Erstellen eines Images muss die Zuordnung des virtuellen Computers aufgehoben werden. Heben Sie die Zuordnung des virtuellen Computers mit [az vm deallocate](/cli//azure/vm#deallocate) auf. 
   
```azurecli
az vm deallocate --resource-group myRGCaptureImage --name myVM
```

Legen Sie schließlich den Status des virtuellen Computers mit [az vm generalize](/cli//azure/vm#generalize) als generalisiert fest.
   
```azurecli
az vm generalize --resource-group myResourceGroupImages --name myVM
```

### <a name="capture-the-image"></a>Erfassen des Images

Jetzt können Sie mit [az image create](/cli//azure/image#create) ein Image des virtuellen Computers erstellen. Im folgenden Beispiel wird ein Image mit dem Namen `myImage` von dem virtuellen Computer `myVM` erstellt.
   
```azurecli
az image create --resource-group myResourceGroupImages --name myImage --source myVM
```
 
## <a name="create-a-vm-from-an-image"></a>Erstellen eines virtuellen Computers aus einem Image

Mit [az vm create](/cli/azure/vm#create) können Sie einen virtuellen Computer über ein Image erstellen. Im folgenden Beispiel wird ein virtueller Computer mit dem Namen `myVMfromImage` aus dem Image `myImage` erstellt.

```azurecli
az vm create --resource-group myResourceGroupImages --name myVMfromImage --image myImage --admin-username azureuser --generate-ssh-keys
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zum Erstellen von benutzerdefinierten VM-Images erhalten. Im nächsten Tutorial erhalten Sie Informationen zu hoch verfügbaren virtuellen Computern.

[Erstellen von hoch verfügbaren virtuellen Computern](tutorial-availability-sets.md)


