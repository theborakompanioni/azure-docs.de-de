---
title: Aktualisieren des Azure Linux-Agents von GitHub | Microsoft Docs
description: "Erfahren Sie, wie Sie den Azure Linux-Agent für Ihren virtuellen Linux-Computer in Azure auf die neueste Version von GitHub aktualisieren."
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/14/2015
ms.author: mingzhan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: c9cea69fc6462f69c8627219aca11272bbdc493f
ms.lasthandoff: 04/03/2017


---
# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Aktualisieren des Azure Linux-Agent auf einem virtuellen Computer auf die neueste Version von GitHub
Zum Aktualisieren Ihres [Azure Linux-Agents](https://github.com/Azure/WALinuxAgent) auf einem virtuellen Linux-Computer benötigen Sie:

1. Einen ausgeführten virtuellen Linux-Computer in Azure.
2. Eine SSH-Verbindung mit diesem virtuellen Linux-Computer.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [!NOTE]
> Wenn Sie diese Aufgabe auf einem Windows-Computer ausführen, können Sie mit PuTTY eine SSH-Verbindung mit dem Linux-Computer herstellen. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Linux ausgeführt wird](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

Linux-Distributionen mit Azure-Unterstützung haben das Azure Linux Agent-Paket in ihren Repositorys. Bitte überprüfen Sie dies und installieren Sie nach Möglichkeit die neueste Version aus dem Distrbutions-Repository zuerst.  

Für Ubuntu, geben Sie einfach ein:

```bash
sudo apt-get install walinuxagent
```

Für CentOS geben Sie ein:

```bash
sudo yum install waagent
```

Stellen Sie bei Oracle Linux sicher, dass das `Addons` -Repository aktiviert ist. Bearbeiten Sie die Datei `/etc/yum.repos.d/public-yum-ol6.repo` (Oracle Linux 6) oder `/etc/yum.repos.d/public-yum-ol7.repo` (Oracle Linux), und ändern Sie in dieser Datei unter **[ol6_addons]** oder **[ol7_addons]** die Zeile `enabled=0` in `enabled=1`.

Installieren Sie anschließend die aktuelle Version des Azure Linux-Agents. Geben Sie dazu Folgendes ein:

```bash
sudo yum install WALinuxAgent
```

Wenn Sie das Add-On-Repository nicht finden, können Sie diese Zeilen einfach am Ende der REPO-Datei Ihrer Oracle Linux-Version hinzufügen:

Für virtuelle Oracle Linux 6-Computer:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Für virtuelle Oracle Linux 7-Computer:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

Geben Sie anschließend Folgendes ein:

```bash
sudo yum update WALinuxAgent
```

Normalerweise benötigen Sie nicht mehr. Wenn Sie aus irgendeinem Grund jedoch die Installation von https://github.com direkt ausführen müssen, befolgen Sie diese Schritte.

## <a name="install-wget"></a>Installieren von wget
Melden Sie sich in Ihrem virtuellen Computer über SSH an.

Installieren Sie wget (bei einigen Distributionen nicht standardmäßig installiert, wie z. B. Redhat CentOS und Oracle Linux-Versionen 6.4 und 6.5) durch Eingabe von `#sudo yum install wget` in der Befehlszeile.

## <a name="download-the-latest-version"></a>Herunterladen der aktuellen Version
Öffnen Sie [die Version des Azure Linux-Agent auf GitHub](https://github.com/Azure/WALinuxAgent/releases) auf einer Webseite, und finden Sie die neueste Versionsnummer heraus. (Die aktuelle Version finden Sie durch Eingabe von `#waagent --version`.)

### <a name="for-version-20x-type"></a>Geben Sie für Version 2.0.x Folgendes ein:

```bash
wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent
```

In der folgenden Zeile wird als Beispiel Version 2.0.14 verwendet:

```bash
wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent
```

### <a name="for-version-21x-or-later-type"></a>Geben Sie für Version 2.1.x oder höher Folgendes ein:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
unzip WALinuxAgent-[version].zip
cd WALinuxAgent-[version]
```

In der folgenden Zeile wird als Beispiel Version 2.1.0 verwendet:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
unzip WALinuxAgent-2.1.0.zip  
cd WALinuxAgent-2.1.0
```

## <a name="install-the-azure-linux-agent"></a>Installieren des Azure Linux-Agents
### <a name="for-version-20x-use"></a>Verwenden Sie für Version 2.0.x Folgendes:
Waagent ausführbar machen:

```bash
chmod +x waagent
```

Kopieren Sie die neue ausführbare Datei nach „/usr/sbin/“.

Verwenden Sie für die meisten Linux-Distributionen Folgendes:

```bash
sudo cp waagent /usr/sbin
```

Für CoreOS verwenden Sie:

```bash
sudo cp waagent /usr/share/oem/bin/
```

Bei einer neuen Installation des Azure Linux-Agents führen Sie Folgendes aus:

```bash
sudo /usr/sbin/waagent -install -verbose
```

### <a name="for-version-21x-use"></a>Verwenden Sie für Version 2.1.x Folgendes:
Möglicherweise müssen Sie zuerst das Paket `setuptools` installieren (weitere Informationen finden Sie [hier](https://pypi.python.org/pypi/setuptools)). Führen Sie anschließend Folgendes aus:

```bash
sudo python setup.py install
```

## <a name="restart-the-waagent-service"></a>Neustarten des Waagent-Diensts
Für die meisten Linux-Distributionen:

```bash
sudo service waagent restart
```

Für Ubuntu verwenden Sie:

```bash
sudo service walinuxagent restart
```

Für CoreOS verwenden Sie:

```bash
sudo systemctl restart waagent
```

## <a name="confirm-the-azure-linux-agent-version"></a>Bestätigen der Azure Linux Agent-Version
    
```bash
waagent -version
```

Für CoreOS funktioniert oben genannter Befehl möglicherweise nicht.

Sie sehen, dass die Azure Linux Agent-Version auf die neue Version aktualisiert wurde.

Weitere Informationen zum Azure Linux-Agent finden Sie in der [Azure Linux-Agent-Infodatei](https://github.com/Azure/WALinuxAgent).


