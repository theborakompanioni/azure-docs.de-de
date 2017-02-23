---
title: Erfassen eines virtuellen Linux-Computers mit der Azure CLI 2.0 (Vorschau) | Microsoft-Dokumentation
description: "Erfassen und Generalisieren eines Images eines Linux-basierten virtuellen Azure-Computers mit verwalteten Datenträgern, die mit der Azure CLI 2.0 (Vorschau) erstellt wurden"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 4620ace217e8e3d733129f69a793d3e2f9e989b2
ms.openlocfilehash: 64b829de4389ba6aa46dc51afd0cff3f40265d68


---
# <a name="how-to-generalize-and-capture-a-linux-virtual-machine-using-the-azure-cli-20-preview"></a>Generalisieren und Erfassen eines virtuellen Linux-Computers mithilfe der Azure CLI 2.0 (Vorschau)
Zum Wiederverwenden von virtuellen Computern (VMs), die in Azure bereitgestellt und konfiguriert wurden, erfassen Sie ein Image des virtuellen Computers. Der Prozess umfasst auch das Verallgemeinern des virtuellen Computers zum Entfernen persönlicher Kontoinformationen, bevor Sie neue virtuelle Computer aus dem Image bereitstellen. In diesem Artikel wird erläutert, wie Sie mithilfe der verwalteten Azure-Datenträger ein Image eines virtuellen Computers mit der Azure CLI 2.0 (Vorschau) für einen virtuellen Computer erfassen. Diese Datenträger werden von der Azure-Plattform verarbeitet und erfordern keine Vorbereitung und keinen Speicherort zur Aufbewahrung. Weitere Informationen finden Sie in der [Übersicht über Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!TIP]
> Wenn Sie eine Kopie Ihrer vorhandenen Linux-VM mit spezialisiertem Zustand für das Sichern oder Debuggen erstellen möchten, finden Sie unter [Erstellen einer Kopie eines virtuellen Linux-Computers, der in Azure ausgeführt wird](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) entsprechende Anweisungen. Wenn Sie eine Linux-VHD aus einer lokalen VM hochladen möchten, lesen Sie [Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträgerimage](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](virtual-machines-linux-capture-image-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau) – Azure Managed Disks:](#quick-commands) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)

## <a name="before-you-begin"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* **Im Resource Manager-Bereitstellungsmodell erstellte Azure-VM**: Wenn Sie keine Linux-VM erstellt haben, können Sie das [Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), die [Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Resource Manager-Vorlagen](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dazu verwenden. Konfigurieren Sie die VM den Anforderungen entsprechend. Sie können beispielsweise [Datenträger hinzufügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), Updates einspielen und Anwendungen installieren. 

Zudem muss die neueste [Azure CLI 2.0 (Vorschau)](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

## <a name="quick-commands"></a>Schnellbefehle
Im folgenden Abschnitt werden die grundlegenden Befehle zum Erstellen eines Images eines virtuellen Linux-Computers in Azure beschrieben, falls Sie die Aufgabe schnell durchführen müssen. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](#detailed-steps)). Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `myVM` und `myImage` verwendet.

1. Aufheben der Bereitstellung Ihres virtuellen Quellcomputers:

    ```bash
    ssh ops@myvm.westus.cloudapp.azure.com
    sudo waagent -deprovision+user -force
    exit
    ```

2. Heben Sie die Zuordnung des virtuellen Computers mit [az vm deallocate](/cli/azure/vm#deallocate) auf:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Generalisieren Sie den virtuellen Computer mit [az vm generalize](/cli/azure/vm#generalize):
   
    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ```

4. Erstellen Sie ein Image aus der VM-Ressource mit [az image create](/cli/azure/image#create):
   
    ```azurecli
    az image create --resource-group myResourceGroup --name myImage --source myVM
    ```

5. Erstellen Sie einen virtuellen Computer aus Ihrer Imageressource mit [az vm create](/cli/azure/vm#create):

    ```azurecli
    az vm create --resource-group myResourceGroup --name myVMDeployed --image myImage
        --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub
    ```

## <a name="detailed-steps"></a>Ausführliche Schritte
In den folgenden Schritten heben Sie die Bereitstellung eines vorhandenen virtuellen Computers auf, geben die virtuelle Computerressource frei und verallgemeinern sie und erstellen dann ein Image. Sie können dieses Image verwenden, um virtuelle Computer über jede Ressourcengruppe innerhalb Ihres Abonnements hinweg zu erstellen. Dieser Vorgang bietet [Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) einen Vorteil gegenüber nicht verwalteten Datenträgern. Mit nicht verwalteten Datenträgern erstellen Sie eine Blobkopie der zugrunde liegenden virtuellen Festplatte (VHD) und sind dann auf die Erstellung von virtuellen Computern im selben Speicherkonto wie das kopierte VHD-Blob beschränkt. Mit verwalteten Datenträgern erstellen Sie eine Imageressource, die in Ihrem gesamten Abonnement bereitgestellt werden kann.

## <a name="step-1-remove-the-azure-linux-agent"></a>Schritt 1: Entfernen des Azure Linux-Agents
Zur Vorbereitung des virtuellen Computers für das Verallgemeinern heben Sie die Bereitstellung des virtuellen Computers mithilfe des Azure-VM-Agents auf, um Dateien und Daten zu löschen. Verwenden Sie den Befehl **waagent** mit dem Parameter **deprovision** auf dem virtuellen Linux-Zielcomputer. Weitere Informationen erhalten Sie im [Benutzerhandbuch für Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Stellen Sie mit einem SSH-Client eine Verbindung mit Ihrer Linux-VM her.
2. Geben Sie im SSH-Fenster den folgenden Befehl ein:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Führen Sie diesen Befehl nur auf einem virtuellen Computer aus, den Sie als Image erfassen möchten. Dies garantiert nicht, dass alle vertraulichen Informationen aus dem Image gelöscht werden oder dass es für eine erneute Verteilung genutzt werden kann.
 
3. Geben Sie **y** ein, um fortzufahren. Sie können den **-force** -Parameter hinzufügen, um diesen Bestätigungsschritt zu vermeiden.
4. Geben Sie nach Abschluss des Befehls **exit** ein. Dieser Schritt schließt den SSH-Client.

## <a name="step-2-capture-the-vm"></a>Schritt 2: Erfassen der VM
Verwenden Sie die Azure CLI 2.0 (Vorschau) zum Verallgemeinern und Erfassen des virtuellen Computers. Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind u.a. **myResourceGroup**, **myVnet** und **myVM**.

1. Heben Sie die Zuordnung des virtuellen Computers auf, dessen Bereitstellung Sie mit [az vm deallocate](/cli//azure/vm#deallocate) aufgehoben haben. Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Verallgemeinern Sie den virtuellen Computer mit [az vm generalize](/cli//azure/vm#generalize). Im folgenden Beispiel wird der virtuelle Computer namens `myVM` in der Ressourcengruppe namens `myResourceGroup` verallgemeinert:
   
    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ```

3. Erstellen Sie jetzt ein Image aus der VM-Ressource mit [az image create](/cli//azure/image#create). Im folgenden Beispiel wird ein Image namens `myImage` in der Ressourcengruppe namens `myResourceGroup` mit der VM-Ressource namens `myVM` erstellt:
   
    ```azurecli
    az image create --resource-group myResourceGroup --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Das Image wird in derselben Ressourcengruppe wie der virtuelle Quellcomputer erstellt. Sie können aus diesem Image virtuelle Computer in einer beliebigen Ressourcengruppe in Ihrem Abonnement erstellen. Angesichts der Verwaltung können Sie eine spezifische Ressourcengruppe für die VM-Ressourcen und Images erstellen.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Schritt 3: Bereitstellen eines virtuellen Computers anhand des erfassten Images
Erstellen Sie aus dem erstellten Image einen virtuellen Computer mit [az vm create](/cli/azure/vm#create). Im folgenden Beispiel wird ein virtueller Computer mit dem Namen `myVMDeployed` aus dem Image namens `myImage` erstellt:

```azurecli
az vm create --resource-group myResourceGroup --name myVMDeployed --image myImage
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub
```

Mit verwalteten Datenträgern können Sie virtuelle Computer aus einem Image in jeder Ressourcengruppe innerhalb Ihres Abonnements erstellen. Dies ist eine Änderung gegenüber nicht verwalteten Datenträgern, bei denen Sie virtuelle Computer nur im selben Speicherkonto wie die Quell-VHD erstellen konnten. Zum Erstellen eines virtuellen Computers in einer anderen Ressourcengruppe als dem Image geben Sie die vollständige Ressourcen-ID im Image an. Verwenden Sie [az image list](/cli/azure/image#list), um eine Liste von Images anzuzeigen. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Im folgenden Beispiel wird **az vm create** zum Erstellen eines virtuellen Computers in einer anderen Ressourcengruppe als dem Quellimage verwendet, indem die Imageressourcen-ID angegeben wird:

```azurecli
az vm create --resource-group myOtherResourceGroup --name myOtherVMDeployed 
    --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage"
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub
```


### <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung
Stellen Sie über SSH eine Verbindung zum erstellen virtuellen Computer her, um die Bereitstellung zu prüfen und den neuen virtuellen Computer zu verwenden. Zum Herstellen einer Verbindung über SSH suchen Sie die IP-Adresse oder den FQDN Ihres virtuellen Computers mit [az vm show](/cli/azure/vm#show):

```azurecli
az vm show --resource-group myResourceGroup --name myVM --show-details
```

## <a name="next-steps"></a>Nächste Schritte
Sie können mehrere virtuelle Computer aus dem Image des virtuellen Quellcomputers erstellen. Falls Sie Änderungen an Ihrem Image vornehmen müssen: 

- Schalten Sie die Quell-VM-Ressource ein.
- Nehmen Sie sämtliche Updates oder Änderungen an der Konfiguration vor.
- Führen Sie erneut die Schritte zum Aufheben der Bereitstellung, zum Aufheben der Zuordnung, zum Verallgemeinern und zum Erfassen des virtuellen Computers aus. 

Weitere Informationen zum Verwalten Ihrer virtuellen Computer mit der CLI finden Sie unter [Azure CLI 2.0 (Vorschau)](/cli/azure/overview).



<!--HONumber=Feb17_HO2-->


