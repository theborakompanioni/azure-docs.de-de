---
title: "Zurücksetzen des Zugriffs mit der VMAccess-Erweiterung und der Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Informationen zum Verwalten von Benutzern und Zurücksetzen des Zugriffs auf virtuellen Linux-Computern mithilfe der VMAccess-Erweiterung und der Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: debdb8a16c8cfd6a137bd2a7c3b82cfdbedb0d8c
ms.openlocfilehash: 4fac98d37dde195af69d8bd03fd796c6eeae3734
ms.lasthandoff: 02/27/2017


---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a>Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf virtuellen Azure-Linux-Computern mit der VMAccess-Erweiterung und der Azure CLI 2.0
Der Datenträger auf Ihrer Linux-VM zeigt Fehler an. Aus irgendeinem Grund haben Sie das Stammkennwort für Ihre Linux-VM zurückgesetzt oder Ihren privaten SSH-Schlüssel versehentlich gelöscht. Wenn dies früher zu Rechenzentrumszeiten geschah, mussten Sie dorthin fahren und den KVM öffnen, um an die Serverkonsole zu gelangen. Stellen Sie sich die Azure-VMAccess-Erweiterung als diesen KVM-Switch vor, mit dem Sie Zugriff auf die Konsole haben, um den Zugriff auf Linux zurückzusetzen oder Wartung auf Datenträgerebene durchzuführen.

Dieser Artikel veranschaulicht die Verwendung der Azure-VMAcesss-Erweiterung zum Überprüfen oder Reparieren eines Datenträgers, Zurücksetzen des Benutzerzugriffs, Verwalten von Benutzerkonten oder Zurücksetzen der SSHD-Konfiguration unter Linux. Sie können diese Schritte auch mit der [Azure CLI 1.0](virtual-machines-linux-using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.


## <a name="ways-to-use-the-vmaccess-extension"></a>Verschiedene Verwendungsmöglichkeiten für die VMAccess-Erweiterung
Es gibt zwei Möglichkeiten, die VMAccess-Erweiterung auf Ihren virtuellen Linux-Computern zu verwenden:

* Verwenden Sie die Azure CLI 2.0 und die erforderlichen Parameter.
* [Verwenden Sie JSON-Rohdatendateien, die von der VMAccess-Erweiterung verarbeitet](#use-json-files-and-the-vmaccess-extension) und dann verwendet werden können.

Die folgenden Beispiele verwenden [az vm access](/cli/azure/vm/access) mit den geeigneten Parametern. Zum Ausführen dieser Schritte muss die neueste Version der [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

## <a name="reset-ssh-key"></a>Zurücksetzen des SSH-Schlüssels
Das folgende Beispiel setzt den SSH-Schlüssel für den Benutzer `azureuser` auf dem virtuellen Computer `myVM` zurück:

```azurecli
az vm access set-linux-user \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a>Zurücksetzen des Kennworts
Das folgende Beispiel setzt das Kennwort für den Benutzer `azureuser` auf dem virtuellen Computer `myVM` zurück:

```azurecli
az vm access set-linux-user \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="reset-sshd"></a>Zurücksetzen von SSHD
Das folgende Beispiel setzt die SSHD-Konfiguration auf dem virtuellen Computer `myVM` zurück:

```azurecli
az vm access reset-linux-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a>Erstellen eines Benutzers
Das folgende Beispiel erstellt einen Benutzer namens `myNewUser` und verwendet dabei einen SSH-Schlüssel zur Authentifizierung auf dem virtuellen Computer `myVM`:

```azurecli
az vm access set-linux-user \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="deletes-a-user"></a>Löschen eines Benutzers
Das folgende Beispiel löscht einen Benutzer namens `myNewUser` vom virtuellen Computer `myVM`:

```azurecli
az vm access delete-linux-user \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a>Verwenden von JSON-Dateien und der VMAccess-Erweiterung
Die folgenden Beispiele verwenden JSON-Rohdatendateien. Verwenden Sie [az vm extension set](/cli/azure/vm/extension#set), um Ihre JSON-Dateien aufzurufen. Diese JSON-Dateien können auch aus Azure-Vorlagen heraus aufgerufen werden. 

### <a name="reset-user-access"></a>Zurücksetzen des Benutzerzugriffs
Wenn Sie den Zugriff auf das Stammverzeichnis Ihres virtuellen Linux-Computers verloren haben, können Sie ein VMAccess-Skript zum Zurücksetzen eines Benutzerkennworts starten.

Um den SSH-Schlüssel eines Benutzers zurückzusetzen, erstellen Sie eine Datei namens `reset_ssh_key.json`, und fügen Sie folgenden Inhalt hinzu:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
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

Um ein Benutzerkennwort zurückzusetzen, erstellen Sie eine Datei namens `reset_user_password.json`, und fügen Sie folgenden Inhalt hinzu:

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

### <a name="reset-ssh"></a>Zurücksetzen von SSH
Wenn Sie Änderungen an der SSHD-Konfiguration der Linux-VMs vornehmen und die SSH-Verbindung vor dem Überprüfen der Änderungen schließen, können Sie vielleicht keine SSH-Verbindung mehr herstellen.  Mit VMAccess kann die SSHD-Konfiguration ohne Anmeldung über SSH auf einen als funktionierend bekannten Zustand zurückgesetzt werden.

Um die SSHD-Konfiguration zurückzusetzen, erstellen Sie eine Datei namens `reset_sshd.json`, und fügen Sie folgenden Inhalt hinzu:

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
VMAccess ist ein Python-Skript, mit dem Sie Benutzer auf Ihrer Linux-VM ohne Anmeldung und Verwendung von Sudo- oder Stammkonto verwalten können.

Um einen Benutzer zu erstellen, erstellen Sie eine Datei namens `create_new_user.json`, und fügen Sie folgenden Inhalt hinzu:

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

Um einen Benutzer zu löschen, erstellen Sie eine Datei namens `delete_user.json`, und fügen Sie folgenden Inhalt hinzu:

```json
{
  "remove_user":"myDeleteUser"
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
Mit VMAccess können Sie eine FSCK-Prüfung auf dem Datenträger Ihrer Linux-VM ausführen. Sie können auch eine Überprüfung des Datenträgers und eine Reparatur mithilfe von VMAccess durchführen.

Um den Datenträger mit diesem VMAccess-Skript zu überprüfen und dann zu reparieren, erstellen Sie eine Datei namens `disk_check_repair.json`, und fügen Sie folgenden Inhalt hinzu:

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
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
Die Aktualisierung von Linux mit Azure-VMAccess-Erweiterungen ist eine Methode, Änderungen an einem ausgeführten virtuellen Linux-Computer vorzunehmen. Sie können auch Tools wie Cloud-Init und Azure Resource Manager-Vorlagen verwenden, um Ihren virtuellen Linux-Computer beim Start zu ändern.

[Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Erstellen von Azure Resource Manager-Vorlagen mit Linux-VM-Erweiterungen](virtual-machines-linux-extensions-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](virtual-machines-linux-using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


