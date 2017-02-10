---
title: "Zurücksetzen des Zugriffs auf Azure-Linux-VMs mit der VMAccess-Erweiterung | Microsoft Docs"
description: "Zurücksetzen des Zugriffs auf Azure-Linux-VMs mit der VMAccess-Erweiterung."
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
ms.date: 10/25/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 1225e6e3910d12921208a619c7052521071c55be


---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf Azure-Linux-VMs mit der VMAccess-Erweiterung
Dieser Artikel veranschaulicht die Verwendung der Azure-VMAcesss-Erweiterung zum Überprüfen oder Reparieren eines Datenträgers, Zurücksetzen des Benutzerzugriffs, Verwalten von Benutzerkonten oder Zurücksetzen der SSHD-Konfiguration unter Linux. Zum Ausführen der Schritte in diesem Artikel ist Folgendes erforderlich:

* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), angemeldet mit `azure login`
* Die Azure-Befehlszeilenschnittstelle *muss* im Azure Resource Manager-Modus `azure config mode arm` ausgeführt werden.

## <a name="quick-commands"></a>Schnellbefehle
Es gibt zwei Möglichkeiten, VMAccess auf Ihren virtuellen Linux-Computern zu verwenden:

* Verwenden der Azure-Befehlszeilenschnittstelle und der erforderlichen Parameter.
* Verwenden von JSON-Rohdatendateien, die VMAccess verarbeitet und dann darauf reagiert.

Für den Schnellbefehlsabschnitt verwenden wir die `azure vm reset-access` -Methode mit der Azure-Befehlszeilenschnittstelle. Ersetzen Sie in den folgenden Befehlsbeispielen die Werte, die „example“ enthalten, durch die Werte aus Ihrer eigenen Umgebung.

## <a name="create-a-resource-group-and-linux-vm"></a>Erstellen einer Ressourcengruppe und Linux-VM
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Erstellen eines virtuellen Debian-Computers
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a>Zurücksetzen des Stammkennworts
So setzen Sie das Stammkennwort zurück:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH-Schlüsselrücksetzung
So setzen Sie den SSH-Schlüssel eines Nichtstammbenutzers zurück:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Erstellen eines Benutzers
So erstellen Sie einen Benutzer:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a>Benutzer entfernen
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a>Zurücksetzen von SSHD
So setzen Sie die SSHD-Konfiguration zurück:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
### <a name="vmaccess-defined"></a>VMAccess-definiert:
Der Datenträger auf Ihrer Linux-VM zeigt Fehler an. Aus irgendeinem Grund haben Sie das Stammkennwort für Ihre Linux-VM zurückgesetzt oder Ihren privaten SSH-Schlüssel versehentlich gelöscht. Wenn dies früher zu Rechenzentrumszeiten geschah, mussten Sie dorthin fahren und den KVM öffnen, um an die Serverkonsole zu gelangen. Stellen Sie sich die Azure-VMAccess-Erweiterung als diesen KVM-Switch vor, mit dem Sie Zugriff auf die Konsole haben, um den Zugriff auf Linux zurückzusetzen oder Wartung auf Datenträgerebene durchzuführen.

Für die ausführliche exemplarische Vorgehensweise verwenden wir die Langform von VMAccess, die unformatierte JSON-Dateien verwendet.  Diese VMAccess-JSON-Dateien können auch aus Azure-Vorlagen heraus aufgerufen werden.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Verwenden von VMAccess zum Überprüfen oder Reparieren des Datenträgers einer Linux-VM
Mit VMAccess können Sie eine FSCK-Prüfung auf dem Datenträger Ihrer Linux-VM ausführen.  Sie können auch eine Überprüfung des Datenträgers und eine Reparatur mithilfe von VMAccess durchführen.

So überprüfen und reparieren Sie den Datenträger mit diesem VMAccess-Skript:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Verwendung von VMAccess zum Zurücksetzen des Benutzerzugriffs auf Linux
Wenn Sie den Zugriff auf das Stammverzeichnis Ihres virtuellen Linux-Computers verloren haben, können Sie ein VMAccess-Skript zum Zurücksetzen des Stammkennworts starten.

Verwenden Sie zum Zurücksetzen des Stammkennworts dieses VMAccess-Skript:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

Verwenden Sie zum Zurücksetzen des SSH-Schlüssels eines Nichtstammbenutzers dieses VMAccess-Skript:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Verwenden von VMAccess zum Verwalten von Benutzerkonten unter Linux
VMAccess ist ein Python-Skript, mit dem Sie Benutzer auf Ihrer Linux-VM ohne Anmeldung und Verwendung von Sudo- oder Stammkonto verwalten können.

Verwenden Sie zum Erstellen eines Benutzers dieses VMAccess-Skript:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

Verwenden Sie zum Löschen eines Benutzers dieses VMAccess-Skript:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Verwenden von VMAccess beim Zurücksetzen der SSHD-Konfiguration
Wenn Sie Änderungen an der SSHD-Konfiguration der Linux-VMs vornehmen und die SSH-Verbindung vor dem Überprüfen der Änderungen schließen, können Sie vielleicht keine SSH-Verbindung mehr herstellen.  Mit VMAccess kann die SSHD-Konfiguration ohne Anmeldung über SSH auf einen als funktionierend bekannten Zustand zurückgesetzt werden.

Verwenden Sie zum Zurücksetzen der SSHD-Konfiguration dieses VMAccess-Skript:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Führen Sie das VMAccess-Skript aus mit:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Nächste Schritte
Die Aktualisierung von Linux mit Azure-VMAccess-Erweiterungen ist eine Methode, Änderungen an einer ausgeführten Linux-VM vorzunehmen.  Sie können auch Tools wie Cloud-Init und Azure-Vorlagen zum Ändern Ihrer Linux-VM beim Start verwenden.

[Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Erstellen von Azure Resource Manager-Vorlagen mit Linux-VM-Erweiterungen](virtual-machines-linux-extensions-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](virtual-machines-linux-using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)




<!--HONumber=Nov16_HO3-->


