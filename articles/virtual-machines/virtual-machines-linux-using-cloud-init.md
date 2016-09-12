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
    ms.date="08/30/2016"
    ms.author="v-livech"
/>

# Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung

In diesem Artikel wird gezeigt, wie ein Cloud-Init-Skript zum Festlegen des Hostnamens, Aktualisieren installierter Pakete und Verwalten von Benutzerkonten erstellt wird. Diese Cloud-Init-Skripts werden dann während der Erstellung der virtuellen Computer über die Azure-Befehlszeilenschnittstelle aufgerufen.

## Voraussetzungen

Voraussetzungen: [Ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [öffentliche und private SSH-Schlüssel](virtual-machines-linux-mac-create-ssh-keys.md) und die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), die mit `azure config mode arm` in den Azure Resource Manager-Modus geschaltet wurde.

## Schnellbefehle

Erstellen Sie ein cloud-init.txt-Skript, das den Hostnamen festlegt, alle Pakete aktualisiert und Linux einen Sudo-Benutzer hinzufügt.

```bash
#cloud-config
hostname: exampleServerName
apt_upgrade: true
users:
  - name: exampleUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==exampleuser@slackwarelaptop
```

Erstellen Sie eine Linux-VM mit Cloud-Init, die beim Start konfiguriert wird.

```bash
azure group create cloudinitexample westus
```

```bash
azure vm create \
--resource-group cloudinitexample \
--name cloudinitexample \
--location westus \
--os-type Linux \
--nic-name cloudinitnicexample \
--vnet-name cloudinitvnetexample \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name cloudinitvsubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
--admin-username ahmet \
--custom-data cloud-init.txt

```

## Einführung

Wenn Sie eine neue Linux-VM starten, erhalten Sie eine Standard-Linux-VM ohne Anpassung an Ihre Bedürfnisse. [Cloud-Init](https://cloudinit.readthedocs.org) ist eine Standardmethode, ein Skript oder Konfigurationseinstellungen in diese Linux-VM einzufügen, wenn sie zum ersten Mal gestartet wird.

Azure bietet drei verschiedene Möglichkeiten, bei Start oder Bereitstellung Änderungen an einer Linux-VM vorzunehmen.

- Einfügen von Skripts mit Cloud-Init.
- Einfügen von Skripts mithilfe der Azure-[VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md).
- Eine Azure-Vorlage, die Cloud-Init nutzt.
- Eine Azure-Vorlage, die [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md) nutzt.

So fügen Sie Skripts zu einem beliebigen Zeitpunkt nach dem Start ein:

- SSH zur direkten Befehlsausführung
- Fügt Skripts mithilfe der Azure-[VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md) ein, imperativ oder in einer Azure-Vorlage.
- Konfigurationsverwaltungstools wie Ansible, Salt, Chef und Puppet.

>[AZURE.NOTE]\: Die VMAccess-Erweiterung führt ein Skript in gleicher Weise als Root aus, wie es mit SSH möglich ist. Allerdings aktiviert die Verwendung der VM-Erweiterung mehrere Funktionen von Azure, die abhängig von Ihrem Szenario hilfreich sein können.

### Cloud-Init-Verfügbarkeit in Azure-VM-Schnellerstellungs-Imagealiasen:

| Alias | Herausgeber | Angebot | SKU | Version | cloud-init |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS | OpenLogic | Centos | 7\.2 | neueste | no |
| CoreOS | CoreOS | CoreOS | Stable | neueste | Ja |
| Debian | credativ | Debian | 8 | neueste | no |
| openSUSE | SUSE | openSUSE | 13\.2 | neueste | no |
| RHEL | Redhat | RHEL | 7\.2 | neueste | no |
| UbuntuLTS | Canonical | UbuntuServer | 14\.04.4-LTS | neueste | Ja |

Microsoft arbeitet mit den Partnern zusammen, damit Cloud-Init einbezogen wird und in den Images arbeitet, die sie in Azure bereitstellen.


## Ausführliche exemplarische Vorgehensweise

### Hinzufügen eines Cloud-Init-Skripts zu der VM-Erstellung mit der Azure-Befehlszeilenschnittstelle

Um beim Erstellen eines virtuellen Computers in Azure ein Cloud-Init-Skript zu starten, geben Sie die Cloud-Init-Datei mit der Option `--custom-data` der Azure-Befehlszeilenschnittstelle an.

```bash
azure group create cloudinitexample westus
```

```bash
azure vm create \
--resource-group cloudinitexample \
--name cloudinitexample \
--location westus \
--os-type Linux \
--nic-name cloudinitnicexample \
--vnet-name cloudinitvnetexample \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name cloudinitvsubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
--admin-username ahmet \
--custom-data cloud-init.txt

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
--resource-group cloudinitexample \
--name cloudinitexample \
--location westus \
--os-type Linux \
--nic-name cloudinitnicexample \
--vnet-name cloudinitvnetexample \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name cloudinitvsubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
--admin-username ahmet \
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

Nachdem Linux gestartet wurde, werden alle installierten Pakete über `apt-get` aktualisiert.

```bash
azure vm create \
--resource-group cloudinitexample \
--name cloudinitexample \
--location westus \
--os-type Linux \
--nic-name cloudinitnicexample \
--vnet-name cloudinitvnetexample \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name cloudinitvsubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
--admin-username ahmet \
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

Eine der ersten Aufgaben auf jeder neuen Linux-VM ist, das Sie einen Benutzer für sich selbst hinzufügen, um die Verwendung von `root` zu vermeiden. SSH-Schlüssel sind eine bewährte Methode für die Sicherheit und Verwendbarkeit und werden der `~/.ssh/authorized_keys` -Datei mit diesem Cloud-Init-Skript hinzugefügt.

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

Nachdem Linux gestartet wurde, werden alle aufgeführten Benutzer erstellt und der Sudo-Gruppe hinzugefügt.

```bash
azure vm create \
--resource-group cloudinitexample \
--name cloudinitexample \
--location westus \
--os-type Linux \
--nic-name cloudinitnicexample \
--vnet-name cloudinitvnetexample \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name cloudinitvsubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
--admin-username ahmet \
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

## Nächste Schritte

Cloud-Init wird eine standardmäßige Möglichkeit, Ihre Linux-VM beim Start zu ändern. Azure verfügt außerdem über VM-Erweiterungen, mit denen Sie Ihre LinuxVM beim Start oder während der Ausführung ändern können. Sie können z.B. mit der Azure-VMAccess-Erweiterung SSH oder Benutzerinformationen zurücksetzen, während die VM ausgeführt wird. Bei Cloud-Init müssten Sie einen Neustart durchführen, um das Kennwort zurückzusetzen.

[Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md)

[Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf Azure-Linux-VMs mit der VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md)

<!---HONumber=AcomDC_0831_2016-->