---
title: Erfassen eines virtuellen Azure-Computers unter Linux zur Verwendung als Vorlage | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit dem Azure Resource Manager-Bereitstellungsmodell ein Image eines Linux-basierten virtuellen Azure-Computers erfassen und generalisieren können."
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
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 4505bb5f572add13c21df06fc7997358eaae6352
ms.lasthandoff: 03/21/2017


---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Erfassen eines virtuellen Linux-Computers, der in Azure ausgeführt wird
Befolgen Sie die Anweisungen in diesem Artikel zum Generalisieren und Erfassen Ihrer Azure-Linux-VM im Resource Manager-Bereitstellungsmodell. Wenn Sie den virtuellen Computer generalisieren, entfernen Sie persönliche Kontoinformationen und bereiten die VM auf die Nutzung als Image vor. Sie erfassen dann eine generalisierte virtuelle Festplatte (Virtual Hard Disk, VHD) für das Betriebssystem, VHDs für angefügte Datenträger und eine [Resource Manager-Vorlage](../azure-resource-manager/resource-group-overview.md) für neue VM-Bereitstellungen. Dieser Artikel erläutert, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle 1.0 ein VM-Image für einen virtuellen Computer erfassen, der nicht verwaltete Datenträger verwendet. Sie können auch [mithilfe von Azure CLI 2.0 einen virtuellen Computer erfassen, der Azure Managed Disks verwendet](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Verwaltete Datenträger werden von der Azure-Plattform verarbeitet und erfordern weder Vorbereitung und noch einen Speicherort. Weitere Informationen finden Sie in der [Übersicht über Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Um VMs mithilfe des Images zu erstellen, richten Sie die Netzwerkressourcen für jede neue VM ein. Nutzen Sie die Vorlage (eine JSON-Datei [JavaScript Object Notation]), um sie aus den erfassten VHD-Images bereitzustellen. Auf diese Weise können Sie eine VM mit der aktuellen Softwarekonfiguration ähnlich wie bei Verwenden von Images in Azure Marketplace replizieren.

> [!TIP]
> Wenn Sie eine Kopie Ihrer vorhandenen Linux-VM mit spezialisiertem Zustand für das Sichern oder Debuggen erstellen möchten, finden Sie unter [Erstellen einer Kopie eines virtuellen Linux-Computers, der in Azure ausgeführt wird](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) entsprechende Anweisungen. Wenn Sie eine Linux-VHD aus einer lokalen VM hochladen möchten, lesen Sie [Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträgerimage](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#before-you-begin): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="before-you-begin"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* **Im Resource Manager-Bereitstellungsmodell erstellte Azure-VM**: Wenn Sie keine Linux-VM erstellt haben, können Sie das [Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), die [Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Resource Manager-Vorlagen](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dazu verwenden. 
  
    Konfigurieren Sie die VM den Anforderungen entsprechend. Sie können beispielsweise [Datenträger hinzufügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), Updates einspielen und Anwendungen installieren. 
* **Azure-Befehlszeilenschnittstelle**: Installieren Sie die [Azure-CLI](../cli-install-nodejs.md) auf einem lokalen Computer.

## <a name="step-1-remove-the-azure-linux-agent"></a>Schritt 1: Entfernen des Azure Linux-Agents
Führen Sie zunächst den Befehl **waagent** mit dem Parameter **deprovision** auf der Linux-VM aus. Dieser Befehl löscht Dateien und Daten, um den virtuellen Computer für die Generalisierung bereit zu machen. Weitere Informationen finden Sie im [Benutzerhandbuch für Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

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
Verwenden Sie Azure-CLI zum Generalisieren und Erfassen des virtuellen Computers. Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind u.a. **myResourceGroup**, **myVnet** und **myVM**.

1. Öffnen Sie auf dem lokalen Computer die Azure-CLI, und [melden Sie sich bei Ihrem Azure-Abonnement an](../xplat-cli-connect.md). 
2. Stellen Sie sicher, dass Sie sich im Resource Manager-Modus befinden.
   
    ```azurecli
    azure config mode arm
    ```
3. Fahren Sie den bereits außer Betrieb genommenen virtuellen Computer mithilfe des folgenden Befehls herunter:
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. Generalisieren Sie den virtuellen Computer mit dem folgenden Befehl:
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. Führen Sie nun den Befehl **azure vm capture** aus, der den virtuellen Computer erfasst. Im folgenden Beispiel werden die Image-VHDs mit Namen erfasst, die mit **MyVHDNamePrefix** beginnen. Die Option **-t** gibt einen Pfad zur Vorlage **MyTemplate.json** an. 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > Die Image-VHD-Dateien werden standardmäßig im selben Speicherkonto erstellt wie der ursprünglich verwendete virtuellen Computer. Verwenden Sie *dasselbe Speicherkonto* zum Speichern der VHDs aller neuen VMs, die Sie anhand des Images erstellen. 

6. Um den Speicherort eines erfassten Images zu finden, öffnen Sie die JSON-Vorlage in einem Texteditor. Suchen Sie in **storageProfile** im Container **system** den Wert von **uri** für **image**. Der URI des Images des Betriebssystem-Datenträgers lautet ähnlich wie `https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Schritt 3: Bereitstellen eines virtuellen Computers anhand des erfassten Images
Jetzt können Sie das Image mit einer Vorlage zum Erstellen eines virtuellen Linux-Computers verwenden. Diese Schritte zeigen, wie Sie mit der Azure-CLI und JSON-Dateivorlage, die Sie erfasst haben, den virtuellen Computer in einem neuen virtuellen Netzwerk erstellen.

### <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
Um die Vorlage zu verwenden, müssen Sie zunächst ein virtuelles Netzwerk und eine NIC für Ihren neuen virtuellen Computer einrichten. Es wird empfohlen, eine Ressourcengruppe für diese Ressourcen an dem Speicherort zu erstellen, an dem Ihr VM-Image gespeichert ist. Führen Sie Befehle ähnlich dem folgenden aus, indem Sie die Namen durch Ihre Ressourcen und einen entsprechenden Azure-Speicherort (in diesen Befehlen „centralus“) ersetzen:

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-the-id-of-the-nic"></a>Abrufen der ID der Netzwerkkarte (NIC)
Zum Bereitstellen eines virtuellen Computers aus dem Image mithilfe der während der Erfassung gespeicherten JSON-Datei benötigen Sie die ID der NIC. Diese ermitteln Sie, indem Sie den folgenden Befehl ausführen:

```azurecli
azure network nic show myResourceGroup1 myNIC
```

Die **ID** in der Ausgabe sieht in etwa aus wie `/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`.

### <a name="create-a-vm"></a>Erstellen einer VM
Führen Sie nun den folgenden Befehl aus, um Ihren virtuellen Computer aus dem erfassten VM-Image zu erstellen. Verwenden Sie den Parameter **-f**, um den Pfad zur JSON-Vorlagendatei anzugeben, die Sie gespeichert haben.

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

In der Befehlsausgaben werden Sie aufgefordert, einen neuen Namen für den virtuellen Computer, den Admin-Benutzernamen und das Kennwort sowie die ID der Netzwerkkarte anzugeben, die Sie zuvor erstellt haben.

```bash
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

Das folgende Beispiel zeigt, was Sie bei einer erfolgreichen Bereitstellung sehen:

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment to complete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung
Stellen Sie über SSH eine Verbindung zum erstellen virtuellen Computer her, um die Bereitstellung zu prüfen und den neuen virtuellen Computer zu verwenden. Um eine Verbindung über SSH herzustellen, suchen Sie die IP-Adresse des erstellten virtuellen Computers, indem Sie folgenden Befehl ausführen:

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

Die öffentliche IP-Adresse wird in der Befehlsausgabe aufgeführt. Die Verbindung zum virtuellen Linux-Computer erfolgt standardmäßig über SSH auf Port 22.

## <a name="create-additional-vms"></a>Erstellen zusätzlicher VMs
Verwenden Sie das erfasste Image und die Vorlage zum Bereitstellen zusätzlicher virtueller Computer mithilfe der im vorherigen Abschnitt beschriebenen Schritte. Zu den weitere Optionen zum Erstellen von VMs aus dem Image zählen das Verwenden einer Schnellstartvorlage oder Ausführen des Befehls **azure vm create**.

### <a name="use-the-captured-template"></a>Verwenden der erfassten Vorlage
Wenn Sie das erfasste Image und die erfasste Vorlage nutzen möchten, führen Sie diese Schritte aus (die im vorherigen Abschnitt detailliert vorgestellt wurden):

* Stellen Sie sicher, dass sich das Image des virtuellen Computers im gleichen Speicherkonto wie die VHD Ihres virtuellen Computers befindet.
* Kopieren Sie die JSON-Vorlagendatei, und geben Sie einen eindeutigen Namen für den Betriebssystem-Datenträger der VHD bzw. VHDs der neuen VM an. Geben Sie beispielsweise in **storageProfile** unter **vhd** in **uri** einen eindeutigen Namen für die VHD **osDisk** ein, z.B. `https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
* Erstellen Sie eine Netzwerkkarte im gleichen oder einem anderen virtuellen Netzwerk.
* Erstellen Sie eine Bereitstellung in der Ressourcengruppe, in der Sie das virtuelle Netzwerk mithilfe der JSON-Dateivorlage einrichten.

### <a name="use-a-quickstart-template"></a>Mit einer Schnellstartvorlage
Wenn Sie möchten, dass das Netzwerk beim Erstellen eines virtuellen Computers automatisch eingerichtet wird, können Sie diese Ressourcen in einer Vorlage angeben. Siehe beispielsweise die Vorlage [101-vm-from-user-image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) auf GitHub. Diese Vorlage erstellt einen virtuellen Computer aus Ihrem benutzerdefinierten Image sowie das erforderliche virtuelle Netzwerk, die öffentliche IP-Adresse und die NIC-Ressourcen. Eine exemplarische Vorgehensweise zur Verwendung der Vorlage im Azure-Portal finden Sie unter [How to create a virtual machine from a custom image using a Resource Manager template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/)(Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image mithilfe einer Resource Manager-Vorlage).

### <a name="use-the-azure-vm-create-command"></a>Verwenden des „azure vm create“-Befehls
Sie sollten der Einfachheit halber eine Resource Manager-Vorlage verwenden, um einen virtuellen Computer aus dem Image zu erstellen. Allerdings können Sie den virtuellen Computer *imperativ* erstellen, indem Sie den Befehl **azure vm create** mit dem Parameter**-Q** (**--image-urn**) verwenden. Bei dieser Methode übergeben Sie zudem den Parameter **-d** (**--os-disk-vhd**), um den Speicherort der VHD-Datei des Betriebssystems für den neuen virtuellen Computer anzugeben. Diese Datei muss sich im Container „vhds“ des Speicherkontos befinden, in dem die VHD-Imagedatei gespeichert ist. Der Befehl kopiert die virtuelle Festplatte für den neuen virtuellen Computer automatisch in den Container **vhds**.

Führen Sie vor dem Ausführen von **azure vm create** mit dem Image folgende Schritte aus:

1. Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe für die Bereitstellung aus.
2. Erstellen Sie eine öffentliche IP-Adressressource und eine NIC-Ressource für den neuen virtuellen Computer. Informationen zum Erstellen eines virtuellen Netzwerks, einer öffentlichen IP-Adresse und einer Netzwerkkarte mithilfe der Befehlszeilenschnittstelle finden Sie weiter oben in diesem Artikel. (**azure vm create** kann auch eine neue Netzwerkkarte erstellen. Jedoch müssen Sie zusätzliche Parameter für ein virtuelles Netzwerk und Subnetz übergeben.)

Führen Sie einen Befehl zum Übergeben von URIs an die neue VHD-Betriebssystemdatei und das vorhandene Image aus. Bei diesem Beispiel wird eine VM der Größe „Standard_A1“ in der Region „USA, Osten“ erstellt.

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

Führen Sie für zusätzliche Befehlsoptionen `azure help vm create`aus.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwaltung Ihrer virtuellen Computer mit der CLI finden Sie unter [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


