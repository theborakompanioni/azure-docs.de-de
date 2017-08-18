---
title: Erfassen eines Image einer Linux-VM in Azure mit CLI 2.0 | Microsoft-Dokumentation
description: "Erfassen Sie ein Image von einer Azure-VM, das für Massenbereitstellungen mit Azure CLI 2.0 verwendet werden soll."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 19b573f77f2ee84600955d00d30bdb16c84e3623
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Vorgehensweise zum Erstellen eines Image von einem virtuellen Computer oder einer VHD

<!-- generalize, image - extended version of the tutorial-->

Um mehrere Kopien eines virtuellen Computers (VM) für die Verwendung in Azure zu erstellen, erfassen Sie ein Image von der VM oder der Betriebssystem-VHD. Zum Erstellen eines Image müssen Sie persönliche Kontoinformationen entfernen, um für Sicherheit bei mehrmalige Bereitstellungen zu sorgen. In den folgenden Schritten heben Sie die Bereitstellung einer vorhandenen VM auf, heben die Zuordnung der VM auf und erstellen ein Image. Sie können dieses Image verwenden, um virtuelle Computer über jede Ressourcengruppe innerhalb Ihres Abonnements hinweg zu erstellen.

Wenn Sie zum Sichern oder Debuggen eine Kopie Ihrer vorhandenen Linux-VM erstellen oder eine spezialisierte Linux-VHD über eine lokale VM hochladen möchten, lesen Sie den Abschnitt [Hochladen und Erstellen einer Linux-VM über benutzerdefinierte Datenträgerimages](upload-vhd.md).  

Sie können Ihre benutzerdefinierte Konfiguration auch mit **Packer** erstellen. Weitere Informationen zur Verwendung von Packer finden Sie unter [Vorgehensweise zum Erstellen von Images von virtuellen Linux-Computern in Azure mit Packer](build-image-with-packer.md).


## <a name="before-you-begin"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Sie müssen mit verwalteten Datenträgern eine Azure-VM im Resource Manager-Bereitstellungsmodell erstellen. Wenn Sie keine Linux-VM erstellt haben, können Sie das [Portal](quick-create-portal.md), die [Azure CLI](quick-create-cli.md) oder [Resource Manager-Vorlagen](create-ssh-secured-vm-from-template.md) dazu verwenden. Konfigurieren Sie die VM den Anforderungen entsprechend. Sie können beispielsweise [Datenträger hinzufügen](add-disk.md), Updates einspielen und Anwendungen installieren. 

* Zudem muss die neueste [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

## <a name="quick-commands"></a>Schnellbefehle

Eine vereinfachte Version dieses Themas zum Testen, Auswerten oder Erhalten von Informationen über VMs in Azure finden Sie unter [Erstellen eines benutzerdefinierten Image von einer Azure-VM mithilfe der CLI](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>Schritt 1: Aufheben der Bereitstellung der VM
Sie heben die Bereitstellung der VM mithilfe des Azure-VM-Agents auf, um computerspezifische Dateien und Daten zu löschen. Verwenden Sie den Befehl `waagent` mit dem Parameter *-deprovision+user* auf der Linux-Quell-VM. Weitere Informationen erhalten Sie im [Benutzerhandbuch für Azure Linux-Agent](../windows/agent-user-guide.md).

1. Stellen Sie mit einem SSH-Client eine Verbindung mit Ihrer Linux-VM her.
2. Geben Sie im SSH-Fenster den folgenden Befehl ein:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Führen Sie diesen Befehl nur auf einem virtuellen Computer aus, den Sie als Image erfassen möchten. Dies garantiert nicht, dass alle vertraulichen Informationen aus dem Image gelöscht werden oder dass es für eine erneute Verteilung genutzt werden kann. Der Parameter *+user* entfernt auch das zuletzt bereitgestellte Benutzerkonto. Wenn Sie die Anmeldeinformationen auf der VM beibehalten möchten, verwenden Sie einfach *-deprovision*, um das Benutzerkonto beizubehalten.
 
3. Geben Sie **y** ein, um fortzufahren. Sie können den **-force** -Parameter hinzufügen, um diesen Bestätigungsschritt zu vermeiden.
4. Geben Sie nach Abschluss des Befehls **exit** ein. Dieser Schritt schließt den SSH-Client.

## <a name="step-2-create-vm-image"></a>Schritt 2: Erstellen des VM-Image
Verwenden Sie die Azure CLI 2.0, um die VM als generalisiert zu kennzeichnen und das Image zu erfassen. Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind u.a. *myResourceGroup*, *myVnet* und *myVM*.

1. Heben Sie die Zuordnung des virtuellen Computers auf, dessen Bereitstellung Sie mit [az vm deallocate](/cli//azure/vm#deallocate) aufgehoben haben. Im folgenden Beispiel wird die Zuordnung für die VM *myVM* in der Ressourcengruppe *myResourceGroup* aufgehoben:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. Kennzeichnen Sie die VM mit [az vm generalize](/cli//azure/vm#generalize) als generalisiert. Im folgenden Beispiel wird die VM *myVM* in der Ressourcengruppe *myResourceGroup* als generalisiert gekennzeichnet:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Erstellen Sie jetzt ein Image aus der VM-Ressource mit [az image create](/cli//azure/image#create). Im folgenden Beispiel wird ein Image namens *myImage* in der Ressourcengruppe *myResourceGroup* mit der VM-Ressource *myVM* erstellt:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Das Image wird in derselben Ressourcengruppe wie der virtuelle Quellcomputer erstellt. Sie können aus diesem Image virtuelle Computer in einer beliebigen Ressourcengruppe in Ihrem Abonnement erstellen. Angesichts der Verwaltung können Sie eine spezifische Ressourcengruppe für die VM-Ressourcen und Images erstellen.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Schritt 3: Bereitstellen eines virtuellen Computers anhand des erfassten Images
Erstellen Sie aus dem erstellten Image einen virtuellen Computer mit [az vm create](/cli/azure/vm#create). Im folgenden Beispiel wird eine VM namens *myVMDeployed* anhand des Image *myImage* erstellt:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Erstellen der VM in einer anderen Ressourcengruppe 

Sie können VMs aus einem Image in jeder Ressourcengruppe innerhalb Ihres Abonnements erstellen. Zum Erstellen eines virtuellen Computers in einer anderen Ressourcengruppe als dem Image geben Sie die vollständige Ressourcen-ID im Image an. Verwenden Sie [az image list](/cli/azure/image#list), um eine Liste von Images anzuzeigen. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Im folgenden Beispiel wird [az vm create](/cli/azure/vm#create) zum Erstellen eines virtuellen Computers in einer anderen Ressourcengruppe als dem Quellimage verwendet, indem die Imageressourcen-ID angegeben wird:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>Schritt 4: Überprüfen der Bereitstellung

Stellen Sie über SSH eine Verbindung zum erstellen virtuellen Computer her, um die Bereitstellung zu prüfen und den neuen virtuellen Computer zu verwenden. Zum Herstellen einer Verbindung über SSH suchen Sie die IP-Adresse oder den FQDN Ihres virtuellen Computers mit [az vm show](/cli/azure/vm#show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Nächste Schritte
Sie können mehrere virtuelle Computer aus dem Image des virtuellen Quellcomputers erstellen. Falls Sie Änderungen an Ihrem Image vornehmen müssen: 

- Erstellen Sie einen virtuellen Computer aus Ihrem Image.
- Nehmen Sie sämtliche Updates oder Änderungen an der Konfiguration vor.
- Führen Sie erneut die Schritte zum Aufheben der Bereitstellung, zum Aufheben der Zuordnung, zum Generalisieren und zum Erstellen eines Images aus.
- Verwenden Sie dieses neue Image für zukünftige Bereitstellungen. Löschen Sie bei Bedarf das ursprüngliche Image.

Weitere Informationen zum Verwalten Ihrer virtuellen Computer mit der Befehlszeilenschnittstelle finden Sie unter [Azure CLI 2.0](/cli/azure/overview).

