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
ms.date: 05/21/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: de8ffb5ef81ac9ef4a9217f275f2c96973948eb1
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe der Befehlszeilenschnittstelle

Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Starten von Konfigurationen verwendet werden, z.B. zum Vorabladen von Anwendungen, Anwendungskonfigurationen und anderen Betriebssystemkonfigurationen. In diesem Tutorial erstellen Sie ein eigenes benutzerdefiniertes Image eines virtuellen Azure-Computers. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Aufheben der Bereitstellung und Generalisieren von virtuellen Computern
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

Für dieses Tutorial ist mindestens Version 2.0.4 der Azure CLI erforderlich. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. Sie können auch [Cloud Shell](/azure/cloud-shell/quickstart) in Ihrem Browser verwenden.

## <a name="before-you-begin"></a>Voraussetzungen

In den folgenden Schritten wird erläutert, wie Sie eine vorhandene VM in ein wiederverwendbares benutzerdefiniertes Image umwandeln, mit dem Sie neue VM-Instanzen erstellen können.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

## <a name="create-a-custom-image"></a>Erstellen eines benutzerdefinierten Images

Zum Erstellen eines Images eines virtuellen Computers müssen Sie den virtuellen Computer vorbereiten, indem Sie die Bereitstellung und Zuordnung aufheben und dann den virtuellen Quellcomputer als generalisiert markieren. Nachdem Sie den virtuellen Computer vorbereitet haben, können Sie ein Image erstellen.

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
az vm deallocate --resource-group myResourceGroup --name myVM
```

Legen Sie schließlich den Status der VM mit [az vm generalize](/cli//azure/vm#generalize) auf „generalisiert“ fest, damit die Azure-Plattform weiß, dass die VM generalisiert wurde. Sie können nur anhand einer generalisierten VM ein Image erstellen.
   
```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>Erstellen des Images

Jetzt können Sie mit [az image create](/cli//azure/image#create) ein Image des virtuellen Computers erstellen. Im folgenden Beispiel wird ein Image mit dem Namen *myImage* vom virtuellen Computer *myVM* erstellt.
   
```azurecli
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Erstellen von VMs anhand des Images

Nachdem Sie ein Image erstellt haben, können Sie mithilfe von [az vm create](/cli/azure/vm#create) anhand des Images eine oder mehrere neue VMs erstellen. Im folgenden Beispiel wird eine VM namens *myVMfromImage* anhand des Images *myImage* erstellt.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Verwaltung von Images 

Hier sind einige Beispiele für allgemeine Aufgaben der Imageverwaltung und ihre Ausführung mithilfe der Azure CLI.

Auflisten aller Images nach dem Namen im Tabellenformat.

```azurecli
az image list \
  --resource-group myResourceGroup
```

Löschen eines Images. Dieses Beispiel löscht das Image mit dem Namen *myOldImage* aus *myResourceGroup*.

```azurecli
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein benutzerdefiniertes Image eines virtuellen Computers erstellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Aufheben der Bereitstellung und Generalisieren von virtuellen Computern
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

Im nächsten Tutorial erhalten Sie Informationen zu hoch verfügbaren virtuellen Computern.

> [!div class="nextstepaction"]
> [Erstellen von hoch verfügbaren virtuellen Computern](tutorial-availability-sets.md)


