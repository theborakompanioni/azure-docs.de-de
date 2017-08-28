---
title: "Zurücksetzen des Zugriffs auf einen virtuellen Azure-Linux-Computer | Microsoft-Dokumentation"
description: "Informationen zum Verwalten von Benutzern und Zurücksetzen des Zugriffs auf virtuellen Linux-Computern mithilfe der VMAccess-Erweiterung und der Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 587c73278a9a92776276a811c5c4c8d3db773de3
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a>Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf virtuellen Azure-Linux-Computern mit der VMAccess-Erweiterung und der Azure CLI 2.0
Der Datenträger auf Ihrer Linux-VM zeigt Fehler an. Aus irgendeinem Grund haben Sie das Stammkennwort für Ihre Linux-VM zurückgesetzt oder Ihren privaten SSH-Schlüssel versehentlich gelöscht. Wenn dies früher zu Rechenzentrumszeiten geschah, mussten Sie dorthin fahren und den KVM öffnen, um an die Serverkonsole zu gelangen. Stellen Sie sich die Azure-VMAccess-Erweiterung als diesen KVM-Switch vor, mit dem Sie Zugriff auf die Konsole haben, um den Zugriff auf Linux zurückzusetzen oder Wartung auf Datenträgerebene durchzuführen.

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure-VMAccess-Erweiterung einen Datenträger überprüfen oder reparieren, den Benutzerzugriff zurücksetzen, Benutzerkonten verwalten oder die SSH-Konfiguration unter Linux zurücksetzen. Sie können diese Schritte auch mit der [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.


## <a name="ways-to-use-the-vmaccess-extension"></a>Verschiedene Verwendungsmöglichkeiten für die VMAccess-Erweiterung
Es gibt zwei Möglichkeiten, die VMAccess-Erweiterung auf Ihren virtuellen Linux-Computern zu verwenden:

* Verwenden Sie die Azure CLI 2.0 und die erforderlichen Parameter.
* [Verwenden Sie JSON-Rohdatendateien, die von der VMAccess-Erweiterung verarbeitet](#use-json-files-and-the-vmaccess-extension) und dann verwendet werden können.

In den folgenden Beispielen werden Befehle vom Typ [az vm user](/cli/azure/vm/user) verwendet. Zum Ausführen dieser Schritte muss die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

## <a name="reset-ssh-key"></a>Zurücksetzen des SSH-Schlüssels
Das folgende Beispiel setzt den SSH-Schlüssel für den Benutzer `azureuser` auf dem virtuellen Computer `myVM` zurück:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a>Zurücksetzen des Kennworts
Das folgende Beispiel setzt das Kennwort für den Benutzer `azureuser` auf dem virtuellen Computer `myVM` zurück:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>Neustarten von SSH
Im folgenden Beispiel wird auf einem virtuellen Computer namens `myVM` der SSH-Daemon neu gestartet und die SSH-Konfiguration auf Standardwerte zurückgesetzt:

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a>Erstellen eines Benutzers
Das folgende Beispiel erstellt einen Benutzer namens `myNewUser` und verwendet dabei einen SSH-Schlüssel zur Authentifizierung auf dem virtuellen Computer `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Löschen eines Benutzers
Das folgende Beispiel löscht einen Benutzer namens `myNewUser` vom virtuellen Computer `myVM`:

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a>Verwenden von JSON-Dateien und der VMAccess-Erweiterung
Die folgenden Beispiele verwenden JSON-Rohdatendateien. Verwenden Sie [az vm extension set](/cli/azure/vm/extension#set), um Ihre JSON-Dateien aufzurufen. Diese JSON-Dateien können auch aus Azure-Vorlagen heraus aufgerufen werden. 

### <a name="reset-user-access"></a>Zurücksetzen des Benutzerzugriffs
Wenn Sie nicht mehr auf das Stammverzeichnis Ihres virtuellen Linux-Computers zugreifen können, können Sie ein VMAccess-Skript starten, um den SSH-Schlüssel oder das Kennwort eines Benutzers zurückzusetzen.

Um den öffentlichen SSH-Schlüssel eines Benutzers zurückzusetzen, erstellen Sie eine Datei namens `reset_ssh_key.json` und fügen ihr Einstellungen im folgenden Format hinzu. Geben Sie für die Parameter `username` und `ssh_key` Ihre eigenen Werte an:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

Um ein Benutzerkennwort zurückzusetzen, erstellen Sie eine Datei namens `reset_user_password.json` und fügen ihr Einstellungen im folgenden Format hinzu. Geben Sie für die Parameter `username` und `password` Ihre eigenen Werte an:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>Neustarten von SSH
Erstellen Sie zum Neustarten des SSH-Daemons und zum Wiederherstellen der Standardwerte der SSH-Konfiguration eine Datei namens `reset_sshd.json`. Fügen Sie folgenden Inhalt hinzu:

```json
{
  "reset_ssh": true
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a>Verwalten von Benutzern

Erstellen Sie zum Erstellen eines Benutzers, der einen SSH-Schlüssel für die Authentifizierung verwendet, eine Datei namens `create_new_user.json`, und fügen Sie Einstellungen im folgenden Format hinzu. Geben Sie für die Parameter `username` und `ssh_key` Ihre eigenen Werte an:

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Erstellen Sie zum Löschen eines Benutzers eine Datei namens `delete_user.json`, und fügen Sie folgenden Inhalt hinzu. Geben Sie für den Parameter `remove_user` Ihren eigenen Wert an:

```json
{
  "remove_user":"myNewUser"
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a>Überprüfen oder Reparieren des Datenträgers
Mithilfe von „VMAccess“ können Sie auch einen Datenträger überprüfen und reparieren, den Sie dem virtuellen Linux-Computer hinzugefügt haben.

Erstellen Sie zum Überprüfen und anschließenden Reparieren des Datenträgers eine Datei namens `disk_check_repair.json`, und fügen Sie ihr Einstellungen im folgenden Format hinzu. Geben Sie für den Namen von `repair_disk` Ihren eigenen Wert an:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a>Nächste Schritte
Die Aktualisierung von Linux mit der Azure-VMAccess-Erweiterung ist eine Möglichkeit, um Änderungen an einem aktiven virtuellen Linux-Computer vorzunehmen. Sie können auch Tools wie Cloud-Init und Azure Resource Manager-Vorlagen verwenden, um Ihren virtuellen Linux-Computer beim Start zu ändern.

[Informationen zu Erweiterungen und Features für virtuelle Computer für Linux](extensions-features.md)

[Erstellen von Azure Resource Manager-Vorlagen mit Linux-VM-Erweiterungen](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](using-cloud-init.md)


