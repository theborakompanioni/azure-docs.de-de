---
title: "Schnellstart für Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Schnellstart für Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: 
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: juluk
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 347984aae91a9fa99f88001effe2cba49f92deb9
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="quickstart-for-using-the-azure-cloud-shell"></a>Schnellstart für Azure Cloud Shell

Dieses Dokument erläutert die Verwendung von Azure Cloud Shell im [Azure-Portal](https://ms.portal.azure.com/).

## <a name="start-cloud-shell"></a>Starten von Cloud Shell
1. Starten Sie **Cloud Shell** aus dem oberen Navigationsbereich im Azure-Portal. <br>
![](media/shell-icon.png)
2. Wählen Sie ein Abonnement aus, in dem ein Speicherkonto und eine Azure-Dateifreigabe erstellt werden solle. <br>
![](media/storage-prompt.png)
3. Wählen Sie „Speicher erstellen“ aus.

> [!TIP]
> Sie werden in jeder Sitzung automatisch bei der Azure CLI 2.0 authentifiziert.

### <a name="set-your-subscription"></a>Festlegen des Abonnements
1. Führen Sie die Abonnements auf, auf die Sie Zugriff haben: <br>
`az account list`
2. Legen Sie Ihr bevorzugtes Abonnement fest: <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> Ihr Abonnement wird mithilfe von `azureProfile.json` in Ihrem $Home-Verzeichnis gespeichert.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie eine neue Ressourcengruppe in „USA, Westen“ namens „MyRG“: <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a>Erstellen eines virtuellen Linux-Computers
Erstellen Sie einen virtuellen Ubuntu-Computer in Ihrer neuen Ressourcengruppe. Die Azure CLI 2.0 erstellt SSH-Schlüssel und richtet den virtuellen Computer damit ein. <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> Die öffentlichen und privaten Schlüssel, mit denen Ihr virtueller Computer authentifiziert wird, werden von der Azure CLI 2.0 standardmäßig in `/User/.ssh/id_rsa` und `/User/.ssh/id_rsa.pub` gespeichert. Der SSH-Ordner wird im 5 GB großen Image der angefügten Azure-Dateifreigabe beibehalten.

Ihr Benutzername auf diesem virtuellen Computer ist der Benutzername, der in Cloud Shell verwendet wird ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>Herstellen einer SSH-Verbindung mit Ihrem virtuellen Linux-Computer
1. Suchen Sie über die Suchleiste des Azure-Portals nach dem Namen Ihres virtuellen Computers.
2. Klicken Sie auf „Verbinden“ und führen Sie `ssh username@ipaddress` aus.

![](media/sshcmd-copy.png)

Nach dem Herstellen der SSH-Verbindung sollte der Willkommensbildschirm von Ubuntu angezeigt werden.
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Bereinigen 
Löschen Sie Ihre Ressourcengruppe und alle darin befindlichen Ressourcen: <br>
Führen Sie `az group delete -n MyRG` aus.

## <a name="next-steps"></a>Nächste Schritte
[Informationen zum Beibehalten von Speicher in Cloud Shell](persisting-shell-storage.md) [Informationen zur Azure CLI 2.0] (https://docs.microsoft.com/cli/azure/) [Informationen zu Azure File Storage] (https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) 

