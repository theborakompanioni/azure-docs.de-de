---
title: "Erstellen und Verwenden eines SSH-Schlüsselpaars für virtuelle Linux-Computer in Azure| Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie ein SSH-Schlüsselpaar (öffentlicher und privater Schlüssel) für virtuelle Linux-Computer in Azure erstellen und verwenden, um die Sicherheit des Authentifizierungsprozesses zu verbessern."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/07/2017
ms.author: iainfou
experimental: true
experiment_id: rasquill-ssh-20170308
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 49541306c66e066a9d9f75d90bed2f4dfd21ce9d
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---

<a id="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure" class="xliff"></a>

# Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure
Mit einem SSH-Schlüsselpaar (Secure Shell) können Sie virtuelle Computer (VMs) in Azure erstellen, bei deren Authentifizierung SSH-Schlüssel verwendet werden, sodass zum Anmelden keine Kennwörter mehr erforderlich sind. In diesem Artikel erfahren Sie, wie Sie für virtuelle Linux-Computer eine Datei mit einem SSH-Schlüsselpaar (ein öffentlicher und ein privater Schlüssel) der Protokollversion 2 RSA generieren. Ausführliche Schritte und weitere Beispiele finden Sie unter [Ausführliche exemplarische Vorgehensweise zum Erstellen eines SSH-Schlüsselpaars mit zusätzlichen Zertifikaten für einen virtuellen Linux-Computer in Azure](create-ssh-keys-detailed.md).

<a id="create-an-ssh-key-pair" class="xliff"></a>

## Erstellen eines SSH-Schlüsselpaars
Verwenden Sie den Befehl `ssh-keygen`, um Dateien mit einem öffentlichen und einem privaten SSH-Schlüssel zu erstellen. Standardmäßig werden die Dateien im Verzeichnis `~/.ssh` erstellt, aber Sie können auch einen anderen Speicherort und eine zusätzliche Passphrase (Kennwort zum Zugreifen auf die Datei mit dem privaten Schlüssel) angeben, wenn Sie dazu aufgefordert werden. Führen Sie den folgenden Befehl in einer Bash-Shell aus, und geben Sie Ihre Informationen ein, wenn die entsprechenden Aufforderungen angezeigt werden.

```bash
ssh-keygen -t rsa -b 2048
```

<a id="use-the-ssh-key-pair" class="xliff"></a>

## Verwenden des SSH-Schlüsselpaars
Der öffentliche Schlüssel, den Sie auf Ihrer Linux-VM in Azure platzieren, wird standardmäßig in `~/.ssh/id_rsa.pub` gespeichert, sofern Sie den Speicherort nicht bei der Erstellung geändert haben. Wenn Sie [Azure CLI 2.0](/cli/azure) zum Erstellen Ihrer VM nutzen, müssen Sie den Speicherort dieses öffentlichen Schlüssels angeben, falls Sie [az vm create](/cli/azure/vm#create) mit der Option `--ssh-key-path` verwenden. Stellen Sie sicher, dass Sie keine zusätzlichen Leerzeichen kopieren, wenn Sie den Inhalt der Datei mit dem öffentlichen Schlüssel für die Verwendung im Azure-Portal oder in einer Resource Manager-Vorlage kopieren und einfügen. Bei Nutzung von OS X können Sie die Datei mit dem öffentlichen Schlüssel (standardmäßig **~/.ssh/id_rsa.pub**) beispielsweise per Pipe mit **pbcopy** verknüpfen, um den Inhalt zu kopieren (für diesen Zweck gibt es auch andere Linux-Programme, z.B. `xclip`).

Falls Sie nicht mit öffentlichen SSH-Schlüsseln vertraut sein sollten, können Sie Ihren öffentlichen Schlüssel anzeigen, indem Sie `cat` wie unten angegeben ausführen. Ersetzen Sie hierbei `~/.ssh/id_rsa.pub` durch den Speicherort Ihrer eigenen Datei mit dem öffentlichen Schlüssel:

```bash
cat ~/.ssh/id_rsa.pub
```

Stellen Sie mit dem öffentlichen Schlüssel auf Ihrer Azure-VM eine SSH-Verbindung mit der VM her, indem Sie die IP-Adresse oder den DNS-Namen Ihrer VM verwenden (ersetzen Sie `azureuser` und `myvm.westus.cloudapp.azure.com` unten durch den Administratorbenutzernamen und den vollqualifizierten Domänennamen oder die IP-Adresse):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Falls Sie beim Erstellen des Schlüsselpaars eine Passphrase angegeben haben, müssen Sie die Passphrase eingeben, wenn Sie während des Anmeldeprozesses dazu aufgefordert werden. (Der Server wird dem Ordner `~/.ssh/known_hosts` hinzugefügt, und Sie werden erst wieder zum Herstellen der Verbindung aufgefordert, wenn sich der öffentliche Schlüssel auf Ihrer Azure-VM ändert oder der Servername unter `~/.ssh/known_hosts` entfernt wird.)

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

VMs, die mithilfe von SSH-Schlüsseln erstellt werden, werden standardmäßig mit deaktivierten Kennwörtern konfiguriert, um Brute-Force-Angriffe, bei denen Kennwörter erraten werden sollen, deutlich aufwändiger und somit schwieriger zu machen. In diesem Thema wird beschrieben, wie Sie schnell ein einfaches SSH-Schlüsselpaar erstellen können. Wenn Sie weitere Hilfe bei der Erstellung Ihres SSH-Schlüsselpaars oder zusätzliche Zertifikate benötigen finden Sie entsprechende Informationen unter [Ausführliche exemplarische Vorgehensweise zum Erstellen eines SSH-Schlüsselpaars mit zusätzlichen Zertifikaten für einen virtuellen Linux-Computer in Azure](create-ssh-keys-detailed.md).

Sie können VMs erstellen, für die Ihr SSH-Schlüsselpaar verwendet wird, indem Sie das Azure-Portal, die CLI oder Vorlagen nutzen:

* [Erstellen einer geschützten Linux-VM mit dem Azure-Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM per Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

