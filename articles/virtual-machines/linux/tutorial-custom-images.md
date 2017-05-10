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
ms.date: 05/02/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 2ce92b3f0a21f80eb4294161d6d3a5275c992600
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe der Befehlszeilenschnittstelle

In diesem Tutorial erfahren Sie, wie Sie ein eigenes benutzerdefiniertes Image eines virtuellen Azure-Computers definieren. Benutzerdefinierte Images ermöglichen Ihnen die Erstellung von virtuellen Computern unter Verwendung eines Images, das Sie bereits konfiguriert haben. Benutzerdefinierte Images können zum Bootstrapping des Vorabladens von Binärdateien und Anwendungen, Anwendungskonfigurationen, VM-Datenträgerdefinitionen und anderen Betriebssystemkonfigurationen verwendet werden. Beim Erstellen eines benutzerdefinierten Images werden der virtuelle Computer, den Sie anpassen, und alle angefügten Datenträger in das Image einbezogen.

Die Schritte in diesem Tutorial können mit der neuesten Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) ausgeführt werden.

## <a name="before-you-begin"></a>Voraussetzungen

In den folgenden Schritten wird erläutert, wie Sie eine vorhandene VM in ein wiederverwendbares benutzerdefiniertes Image umwandeln, mit dem Sie neue VM-Instanzen erstellen können.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

## <a name="prepare-vm"></a>Vorbereiten der VM

Zum Erstellen eines Images eines virtuellen Computers müssen Sie den virtuellen Computer vorbereiten, indem Sie die Bereitstellung und Zuordnung aufheben und dann den virtuellen Quellcomputer als generalisiert markieren.

### <a name="deprovision-the-vm"></a>Aufheben der Bereitstellung des virtuellen Computers 

Durch Aufheben der Bereitstellung werden computerspezifische Informationen entfernt und der virtuelle Computer dadurch generalisiert. Durch diese Generalisierung können viele virtuelle Computer aus einem einzelnen Image bereitgestellt werden. Beim Aufheben der Bereitstellung wird der Hostname auf *localhost.localdomain* zurückgesetzt. Ebenfalls gelöscht werden SSH-Hostschlüssel, Namenserverkonfigurationen, das Stammkennwort und zwischengespeicherte DHCP-Leases.

Verwenden Sie zum Aufheben der Bereitstellung des virtuellen Computers den Azure-VM-Agent (waagent). Der Azure-VM-Agent ist auf dem virtuellen Computer installiert und verwaltet die Bereitstellung und Interaktion mit dem Azure Fabric Controller. Weitere Informationen erhalten Sie im [Benutzerhandbuch für Azure Linux-Agent](agent-user-guide.md).

Stellen Sie eine Verbindung mit dem virtuellen Computer über SSH her, und führen Sie den Befehl zum Aufheben der Bereitstellung des virtuellen Computers aus. Mit dem Argument `+user` werden auch das zuletzt bereitgestellte Benutzerkonto und alle zugehörigen Daten gelöscht. Ersetzen Sie die IP-Beispieladresse durch die öffentliche IP-Adresse Ihres virtuellen Computers.

Stellen Sie eine SSH-Verbindung mit der VM her.
```bash
ssh azureuser@52.174.34.95
```
Heben Sie die Bereitstellung der VM auf.

```bash
sudo waagent -deprovision+user -force
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

Legen Sie schließlich den Status der VM mit [az vm generalize](/cli//azure/vm#generalize) auf „generalisiert“ fest, damit die Azure-Plattform weiß, dass die VM generalisiert wurde. Sie können nur anhand einer generalisierten VM ein Image erstellen.
   
```azurecli
az vm generalize --resource-group myResourceGroupImages --name myVM
```

## <a name="create-the-image"></a>Erstellen des Images

Jetzt können Sie mit [az image create](/cli//azure/image#create) ein Image des virtuellen Computers erstellen. Im folgenden Beispiel wird ein Image mit dem Namen *myImage* vom virtuellen Computer *myVM* erstellt.
   
```azurecli
az image create \
    --resource-group myResourceGroupImages \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Erstellen von VMs anhand des Images

Nachdem Sie ein Image erstellt haben, können Sie mithilfe von [az vm create](/cli/azure/vm#create) anhand des Images eine oder mehrere neue VMs erstellen. Im folgenden Beispiel wird eine VM namens *myVMfromImage* anhand des Images *myImage* erstellt.

```azurecli
az vm create \
    --resource-group myResourceGroupImages \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zum Erstellen von benutzerdefinierten VM-Images erhalten. Im nächsten Tutorial erhalten Sie Informationen zu hoch verfügbaren virtuellen Computern.

[Erstellen von hoch verfügbaren virtuellen Computern](tutorial-availability-sets.md)


