<properties
    pageTitle="Zurücksetzen des Zugriffs auf Azure-Linux-VMs mit der VMAccess-Erweiterung | Microsoft Azure"
    description="Zurücksetzen des Zugriffs auf Azure-Linux-VMs mit der VMAccess-Erweiterung."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/29/2016"
    ms.author="v-livech"
/>

# Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf Azure-Linux-VMs mit der VMAccess-Erweiterung

Dieser Artikel veranschaulicht die Verwendung der VMAcesss-VM-Erweiterung [(Github)](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess), um einen Datenträger zu überprüfen oder zu reparieren, den Benutzerzugriff zurückzusetzen, Benutzerkonten zu verwalten oder die SSHD-Konfiguration unter Linux zurückzusetzen. Dieser Artikel erfordert [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [SSH-Schlüssel](virtual-machines-linux-mac-create-ssh-keys.md), eine Azure-Linux-VM, und dass die Azure-Befehlszeilenschnittstelle installiert ist und mithilfe von `azure config mode arm` in den ARM-Modus umgeschaltet wurde.

## Schnellbefehle

Es gibt zwei Möglichkeiten, VMAccess auf Ihren virtuellen Linux-Computern zu verwenden:

- Verwenden der Azure-Befehlszeilenschnittstelle und der erforderlichen Parameter.
- Verwenden von JSON-Rohdatendateien, die VMAccess verarbeitet und dann darauf reagiert.

Für den Schnellbefehlsabschnitt verwenden wir die `azure vm reset-access`-Methode mit der Azure-Befehlszeilenschnittstelle. Ersetzen Sie in den folgenden Befehlsbeispielen die Werte zwischen &lt; und &gt; durch die Werte aus Ihrer eigenen Umgebung.

## Zurücksetzen des Stammkennworts

So setzen Sie das Stammkennwort zurück:

```bash
azure vm reset-access -g <resource group> -n <vm name> -u root -p <examplePassword>
```

## SSH-Schlüsselrücksetzung

So setzen Sie den SSH-Schlüssel eines Nichtstammbenutzers zurück:

```bash
azure vm reset-access -g <resource group> -n <vm name> -u <exampleUser> -M <~/.ssh/azure_id_rsa.pub>
```

## Erstellen eines Benutzers

So erstellen Sie einen neuen Benutzer:

```bash
azure vm reset-access -g <resource group> -n <vm name> -u <exampleNewUserName> -p <examplePassword>
```

## Entfernen eines Benutzers

```bash
azure vm reset-access -g <resource group> -n <vm name> -R <exampleNewUserName>
```

## Zurücksetzen von SSHD

So setzen Sie die SSHD-Konfiguration zurück:

```bash
azure vm reset-access -g <resource group> -n <vm name> -r
```


## Ausführliche exemplarische Vorgehensweise

### VMAccess-definiert:

Der Datenträger auf Ihrer Linux-VM zeigt Fehler an. Aus irgendeinem Grund haben Sie das Stammkennwort für Ihre Linux-VM zurückgesetzt oder Ihren privaten SSH-Schlüssel versehentlich gelöscht. Wenn so etwas in dunkler Vorzeit im Rechenzentrum geschah, mussten Sie dorthin fahren, Ihren Handabdruck scannen lassen, um die Tür zu entriegeln, in den Käfig gelangen und den KVM-Switch knacken, um an die Serverkonsole zu gelangen. Stellen Sie sich die Azure-VMAccess-Erweiterung als diesen KVM-Switch vor, mit dem Sie Zugriff auf die Konsole haben, um den Zugriff auf Linux zurückzusetzen oder Wartung auf Datenträgerebene durchzuführen.

Für die ausführliche exemplarische Vorgehensweise verwenden wir die Langform von VMAccess, die unformatierte JSON-Rohdateien verwendet. Diese VMAccess-JSON-Dateien können auch aus Azure-Vorlagen heraus aufgerufen werden.

### Verwenden von VMAccess zum Überprüfen oder Reparieren des Datenträgers einer Linux-VM

Mit VMAccess können Sie eine FSCK-Prüfung auf dem Datenträger Ihrer Linux-VM ausführen. Sie können den Datenträger überprüfen und reparieren, wenn Sie Fehler finden.

Um den Datenträger zu überprüfen und zu reparieren, verwenden Sie dieses VMAccess-Skript:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Führen Sie das VMAccess-Skript aus mit:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### Verwendung von VMAccess zum Zurücksetzen des Benutzerzugriffs auf Linux

Wenn Sie den Zugriff auf das Stammverzeichnis Ihres virtuellen Linux-Computers verloren haben, können Sie ein VMAccess-Skript zum Zurücksetzen des Stammkennworts starten.

Verwenden Sie zum Zurücksetzen des Stammkennworts dieses VMAccess-Skript:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"exampleNewPassword",   
}
```

Führen Sie das VMAccess-Skript aus mit:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Verwenden Sie zum Zurücksetzen des SSH-Schlüssels eines Nichtstammbenutzers dieses VMAccess-Skript:

`reset_ssh_key.json`

```json
{
  "username":"exampleUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== exampleUser@exampleServer",   
}
```

Führen Sie das VMAccess-Skript aus mit:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### Verwenden von VMAccess zum Verwalten von Benutzerkonten unter Linux

VMAccess ist ein Python-Skript, mit dem Sie Benutzer auf Ihrer Linux-VM ohne Anmeldung und Verwendung von Sudo- oder Stammkonto verwalten können.

Verwenden Sie zum Erstellen eines neuen Benutzers dieses VMAccess-Skript:

`create_new_user.json`

```json
{
"username":"exampleNewUserName",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== exampleUser@exampleServer",
"password":"examplePassword",
}
```

Führen Sie das VMAccess-Skript aus mit:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Verwenden Sie zum Erstellen eines neuen Benutzers dieses VMAccess-Skript:

`remove_user.json`

```json
{
"remove_user":"exampleUser",
}
```

Führen Sie das VMAccess-Skript aus mit:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### Verwenden von VMAccess beim Zurücksetzen der SSHD-Konfiguration unter Linux

Wenn Sie Änderungen an der SSHD-Konfiguration der Linux-VMs vornehmen und die SSH-Verbindung vor dem Überprüfen der Änderungen schließen, können Sie vielleicht keine SSH-Verbindung mehr herstellen. Mit VMAccess kann die SSHD-Konfiguration auf einen als funktionierend bekannten Zustand zurückgesetzt werden.

Verwenden Sie zum Zurücksetzen der SSHD-Konfiguration dieses VMAccess-Skript:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Führen Sie das VMAccess-Skript aus mit:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

<!---HONumber=AcomDC_0803_2016-->