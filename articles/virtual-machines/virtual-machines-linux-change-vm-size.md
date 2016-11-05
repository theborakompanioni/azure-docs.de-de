---
title: Ändern der Größe eines virtuellen Linux-Computers| Microsoft Docs
description: So skalieren Sie einen virtuellen Linux-Computer zentral hoch oder herunter, indem Sie die VM-Größe ändern.
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: ''
tags: ''

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mikewasson

---
# So ändern Sie die Größe eines virtuellen Linux-Computers
## Übersicht
Nachdem Sie einen virtuellen Computer (virtual machine; VM) bereitstellen, können Sie den virtuellen Computer zentral hoch- oder herunterskalieren, indem Sie die [VM-Größe][vm-sizes] ändern. In einigen Fällen müssen Sie zuerst die Zuordnung des virtuellen Computers aufheben. Dies ist möglicherweise der Fall, falls die neue Größe auf dem Hardwarecluster nicht verfügbar ist, auf dem die VM gehostet wird.

In diesem Artikel wird beschrieben, wie Sie die Größe eines virtuellen Linux-Computers mithilfe der [Azure-Befehlszeilenschnittstelle][azure-cli] ändern.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

klassisches Bereitstellungsmodell.

## Ändern der Größe eines virtuellen Linux-Computers
Führen Sie zum Ändern der Größe eines virtuellen Computers die folgenden Schritte aus.

1. Führen Sie den folgenden CLI-Befehl aus. Der Befehl listet die VM-Größen auf, die auf dem Hardwarecluster verfügbar sind, auf dem die VM gehostet wird.
   
    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```
2. Führen Sie den folgenden Befehl aus, um die Größe der VM zu ändern, sofern die gewünschte Größe aufgelistet ist.
   
    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```
   
    Der virtuelle Computer wird während dieses Vorgangs neu gestartet. Nach dem Neustart werden Ihr vorhandenes Betriebssystem und die Datenträger neu zugeordnet. Alle Daten auf dem temporären Datenträger gehen verloren.
   
    Verwenden Sie die Option `--enable-boot-diagnostics` um es der [Startdiagnose][boot-diagnostics] zu ermöglichen, alle Fehler zu protokollieren, die mit dem Startvorgang verbunden sind.
3. Führen Sie die folgenden Befehle aus, falls die gewünschte Größe nicht aufgelistet ist. Damit heben Sie die Zuordnung der VM auf, ändern ihre Größe und starten sie neu.
   
    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```
   
   > [!WARNING]
   > Die Aufhebung der Zuordnung der VM gibt auch jegliche dynamische IP-Adressen frei, die der VM zugewiesen sind. Das Betriebssystem und die Datenträger sind nicht betroffen.
   > 
   > 

## Nächste Schritte
Führen Sie mehrere VM-Instanzen aus, und skalieren Sie diese zentral hoch, um zusätzliche Skalierbarkeit zu erhalten. Weitere Informationen finden Sie unter [Automatisches Skalieren von Linux-Computern in einer VM-Skalierungsgruppe][scale-set].

<!-- links -->

[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/de-DE/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md
[vm-sizes]: virtual-machines-linux-sizes.md

<!---HONumber=AcomDC_0824_2016-->