<properties
    pageTitle="Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung | Microsoft Azure"
    description="Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung."
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

# Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung

In diesem Artikel wird gezeigt, wie ein Cloud-Init-Skript zum Festlegen des Hostnamens, Aktualisieren installierter Pakete und Verwalten von Benutzerkonten erstellt wird. Diese Cloud-Init-Skripts werden dann während der Erstellung der virtuellen Computer über [die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) verwendet.

## Voraussetzungen

Voraussetzungen: [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [öffentliche und private SSH-Schlüssel](virtual-machines-linux-mac-create-ssh-keys.md), eine Azure-Ressourcengruppe, in der die Linux VMs gestartet werden, und die installierte Azure-Befehlszeilenschnittstelle, die mithilfe von `azure config mode arm` in den ARM-Modus umgeschaltet wurde.

## Einführung

Wenn Sie eine neue Linux-VM starten, erhalten Sie eine Standard-Linux-VM ohne Anpassung an Ihre Bedürfnisse. [Cloud-Init](https://cloudinit.readthedocs.org) ist eine Standardmethode, ein Skript oder Konfigurationseinstellungen in diese Linux-VM einzufügen, wenn sie zum ersten Mal gestartet wird.

Azure bietet drei verschiedene Möglichkeiten, beim Start Änderungen an einer Linux-VM vorzunehmen.

- Sie können Skripts mit Cloud-Init einfügen.
- Sie können Skripts mit Azure [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md) einfügen.
- Sie können benutzerdefinierte Einstellungen in einer Azure-Vorlage angeben und sie zum Starten und Anpassen Ihrer Linux-VM verwenden, was sowohl die Unterstützung für Cloud-Init als auch für die CustomScript-VM-Erweiterung und viele andere einschließt.

Um Skripts zu einem beliebigen Zeitpunkt einzufügen, können Sie:

- mit SSH Befehle direkt ausführen. Sie können die Azure [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md) entweder imperativ oder in einer Azure-Vorlage verwenden, oder Sie können allgemeine Konfigurationsverwaltungstools wie Ansible, Salt, Chef und Puppet verwenden, die wiederum nach Abschluss des Starts des virtuellen Computers selbst über SSH funktionieren.

HINWEIS: Obwohl eine [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md) lediglich ein Skript auf die gleiche Weise als Stamm ausführt, wie es mit SSH möglich ist, aktiviert die Verwendung der VM-Erweiterung mehrere Features von Azure, die abhängig von Ihrem Szenario hilfreich sein können.

## Schnellbefehle

Erstellen eines Cloud-Init-Skripts für Hostnamen

```bash
#cloud-config
hostname: exampleServerName
```

Erstellen eines Cloud-Init-Skripts für Linux-Updates beim ersten Start für die Debian-Familie

```bash
#cloud-config
apt_upgrade: true
```

Erstellen eines Cloud-Init-Skripts zum Hinzufügen eines Benutzers

```bash
#cloud-config
users:
  - name: exampleUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==exampleuser@slackwarelaptop
```

## Ausführliche exemplarische Vorgehensweise

### Hinzufügen eines Cloud-Init-Skripts zu der VM-Erstellung mit der Azure-Befehlszeilenschnittstelle

Um beim Erstellen eines virtuellen Computers in Azure ein Cloud-Init-Skript zu starten, geben Sie die Cloud-Init-Datei mit der Option `--custom-data` der Azure-Befehlszeilenschnittstelle an.

HINWEIS: Obwohl in diesem Artikel die Verwendung der Option `--custom-data` für Cloud-Init-Dateien erläutert wird, können Sie auch nach Belieben Code oder Dateien mit dieser Option übergeben. Wenn die Linux-VM bereits weiß, was sie mit diesen Dateien tun soll, werden sie automatisch ausgeführt.

```bash
azure vm create \
--resource-group exampleRG \
--name exampleVM \
--location westus \
--admin-username exampleAdminUserName \
--os-type Linux \
--nic-name exampleNIC \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--custom-data cloud_init_script.txt
```

### Erstellen eines Cloud-Init-Skripts zum Festlegen des Hostnamens einer Linux-VM

Eine der einfachsten und wichtigsten Einstellungen für jede Linux-VM ist die des Hostnamens. Wir können ihn mit diesem Skript leicht mit Cloud-Init festlegen.

#### Beispiel: Cloud-Init-Skript namens `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: exampleServerName
```

Während des ersten Starts des virtuellen Computers legt dieses Cloud-Init-Skript den Hostnamen auf `exampleServerName` fest.

```bash
azure vm create \
--resource-group exampleRG \
--name exampleVM \
--location westus \
--admin-username exampleAdminUserName \
--os-type Linux \
--nic-name exampleNIC \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--custom-data cloud_config_hostname.txt
```

Melden Sie sich an und überprüfen Sie den Hostnamen des neuen virtuellen Computers.

```bash
ssh exampleVM
hostname
exampleServerName
```

### Erstellen eines Cloud-Init-Skripts zum Aktualisieren von Linux

Aus Sicherheitsgründen soll Ihre Ubuntu-VM beim ersten Start aktualisiert werden. Bei Verwendung von Cloud-Init ist dies abhängig von der Linux-Distribution, die Sie verwenden, mit dem folgenden Skript möglich.

#### Beispiel: Cloud-Init-Skript `cloud_config_apt_upgrade.txt` für die Debian-Familie

```bash
#cloud-config
apt_upgrade: true
```

Nach dem Start der neuen Linux-VM werden sofort die installierten Pakete über `apt-get` aktualisiert.

```bash
azure vm create \
--resource-group exampleRG \
--name exampleVM \
--location westus \
--admin-username exampleAdminUserName \
--os-type Linux \
--nic-name exampleNIC \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--custom-data cloud_config_apt_upgrade.txt
```

Melden Sie sich an,und überprüfen Sie, ob alle Pakete aktualisiert sind.

```bash
ssh exampleVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

### Erstellen eines Cloud-Init-Skripts zum Hinzufügen eines Benutzers zu Linux

Eine der ersten Aufgaben auf jeder neuen Linux-VM ist, das Sie einen Benutzer für sich selbst hinzufügen, um die Verwendung von `root` zu vermeiden. Dies ist ein großer Vorteil für Sicherheit und Benutzerfreundlichkeit, sobald Sie Ihren öffentlichen SSH-Schlüssel der `~/.ssh/authorized_keys`-Datei des Benutzers für Anmeldungen mit weniger Kennwörtern und sichere SSH-Anmeldungen hinzufügen.

#### Beispiel: Cloud-Init-Skript `cloud_config_add_users.txt` für die Debian-Familie

```bash
#cloud-config
users:
  - name: exampleUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==exampleuser@slackwarelaptop
```

Nach dem Start der neuen Linux-VM wird der neue Benutzer erstellt und der Sudo-Gruppe hinzugefügt.

```bash
azure vm create \
--resource-group exampleRG \
--name exampleVM \
--location westus \
--admin-username exampleAdminUserName \
--os-type Linux \
--nic-name exampleNIC \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--custom-data cloud_config_add_users.txt
```

Melden Sie sich an und überprüfen Sie den neu erstellten Benutzer.

```bash
cat /etc/group
```

Ausgabe

```bash
root:x:0:
<snip />
sudo:x:27:exampleUser
<snip />
exampleUser:x:1000:
```

<!---HONumber=AcomDC_0504_2016-->