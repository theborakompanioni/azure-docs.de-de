---
title: "Erstellen von SSH-Schlüsseln unter Linux und Mac | Microsoft Docs"
description: "Generieren und Verwenden von SSH-Schlüsseln unter Linux und Mac für die Ressourcen-Manager- und klassischen Bereitstellungsmodelle in Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: b80fb49929ce53f33b9a4064ba6ad14d0108d690
ms.openlocfilehash: c879231fac37ae3c69bac4d201b01469ca18240c


---
# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Erstellen von SSH-Schlüsseln unter Linux und Mac für Linux-VMs in Azure
Mit einem SSH-Schlüsselpaar können Sie virtuelle Computer (VMs) in Azure erstellen, für die zur Authentifizierung standardmäßig SSH-Schlüssel verwendet werden, sodass zum Anmelden keine Kennwörter mehr erforderlich sind.  Kennwörter können erraten werden und machen Ihre VMs anfällig für entsprechende Brute-Force-Angriffe. VMs, die mit Azure-Vorlagen oder der `azure-cli` erstellt werden, können Ihren öffentlichen SSH-Schlüssel im Rahmen der Bereitstellung enthalten, sodass eine Konfiguration nach der Bereitstellung entfällt.  Wenn Sie unter Windows eine Verbindung mit einem virtuellen Linux-Computer herstellen, helfen Ihnen die Informationen in [Erstellen von SSH-Schlüsseln unter Windows](virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="quick-commands"></a>Schnellbefehle
Ersetzen Sie die Beispiele in den folgenden Befehlen durch Ihre eigenen Werte.

SSH-Schlüssel werden standardmäßig im Verzeichnis `.ssh` gespeichert.  

```bash
cd ~/.ssh/
```

Wenn Sie nicht über das Verzeichnis `~/.ssh` verfügen, können Sie es mit dem Befehl `ssh-keygen` gleich mit den richtigen Berechtigungen erstellen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Geben Sie den Namen der Datei ein, die im `~/.ssh/`-Verzeichnis gespeichert wird:

```bash
id_rsa
```

Geben Sie die Passphrase für id_rsa ein:

```bash
correct horse battery staple
```

Das Verzeichnis `~/.ssh` enthält jetzt das SS-Schlüsselpaar `id_rsa` und `id_rsa.pub`.

```bash
ls -al ~/.ssh
```

Fügen Sie den neu erstellten Schlüssel `ssh-agent` unter Linux und Mac hinzu (auch im Schlüsselbund von OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopieren Sie den öffentlichen SSH-Schlüssel auf Ihren Linux-Server:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Testen Sie die Anmeldung mithilfe von Schlüsseln anstelle eines Kennworts:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
Die Verwendung von öffentlichen und privaten SSH-Schlüsseln ist die einfachste Möglichkeit der Anmeldung an Ihren Linux-Servern. [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) (Public-Key-Kryptosystem) ist ein Weg mit deutlich mehr Sicherheit für die Anmeldung an Ihrer Linux- oder BSD-VM in Azure als Kennwörter, die viel leichter einem Brute-Force-Angriff unterzogen werden können. Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur).  Der private SSH-Schlüssel muss mit einem [sehr sicheren Kennwort](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) geschützt werden.  Dieses Kennwort gilt nur für den Zugriff auf den privaten SSH-Schlüssel und ist **nicht** das Kennwort für das Benutzerkonto.  Wenn Sie Ihren SSH-Schlüssel mit einem Kennwort versehen, wird der private Schlüssel verschlüsselt, sodass er ohne das Kennwort nicht verwendet werden kann.  Falls ein Angreifer Ihren privaten Schlüssel entwendet und dafür kein Kennwort konfiguriert wurde, kann sich der Angreifer mithilfe des privaten Schlüssels bei jedem Server anmelden, der über den entsprechenden öffentlichen Schlüssel verfügt.  Wenn der private Schlüssel mit einem Kennwort geschützt ist, kann er vom Angreifer nicht verwendet werden, und Ihre Infrastruktur unter Azure verfügt über eine weitere Sicherheitsebene.

In diesem Artikel werden Dateien mit Schlüsseln im *ssh-rsa* -Format erstellt, die für Bereitstellungen im Resource Manager empfohlen werden.  *ssh-rsa* -Schlüssel werden im [Portal](https://portal.azure.com) sowohl für klassische Bereitstellungen als auch für Resource Manager-Bereitstellungen benötigt.

## <a name="create-the-ssh-keys"></a>Erstellen der SSH-Schlüssel
Für Azure sind öffentliche und private Schlüssel mit mindestens 2048 Bit im Format „ssh-rsa“ erforderlich. Verwenden Sie zum Erstellen der Schlüssel `ssh-keygen`. Hierbei wird eine Reihe von Fragen gestellt, und anschließend werden ein privater Schlüssel und ein passender öffentlicher Schlüssel geschrieben. Bei der Erstellung einer Azure-VM wird der öffentliche Schlüssel in `~/.ssh/authorized_keys` kopiert.  SSH-Schlüssel in `~/.ssh/authorized_keys` werden verwendet, um vom Client zu fordern, für eine SSH-Anmeldeverbindung den entsprechenden privaten Schlüssel bereitzustellen.

## <a name="using-ssh-keygen"></a>Verwenden von ssh-keygen
Mit diesem Befehl wird ein kennwortgeschütztes (verschlüsseltes) SSH-Schlüsselpaar per RSA mit 2.048 Bit erstellt und kommentiert, um die einfache Identifizierung zu ermöglichen.  

Beginnen Sie mit einem Wechsel des Verzeichnisses, damit alle SSH-Schlüssel in diesem Verzeichnis erstellt werden.

```bash
cd ~/.ssh
```

Wenn Sie nicht über das Verzeichnis `~/.ssh` verfügen, können Sie es mit dem Befehl `ssh-keygen` gleich mit den richtigen Berechtigungen erstellen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

*Befehlsbeschreibung*

`ssh-keygen` = zum Erstellen der Schlüssel verwendetes Programm

`-t rsa` = Typ des zu erstellenden Schlüssels, also [RSA-Format](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = Bits des Schlüssels

`-C "myusername@myserver"` = ein Kommentar, der zur einfachen Identifizierung am Ende der Datei mit dem öffentlichen Schlüssel angefügt wird.  Normalerweise wird eine E-Mail-Adresse als Kommentar verwendet, aber Sie können je nach Eignung für Ihre Infrastruktur auch etwas anderes angeben.

### <a name="using-pem-keys"></a>Verwenden von PEM-Schlüsseln
Wenn Sie das klassische Bereitstellungsmodell verwenden (klassisches Azure-Portal oder Befehlszeilenschnittstelle der Azure-Dienstverwaltung (`asm`)), müssen Sie unter Umständen SSH-Schlüssel im PEM-Format für den Zugriff auf Ihre Linux-VMs einsetzen.  Hier wird beschrieben, wie Sie einen PEM-Schlüssel aus einem vorhandenen öffentlichen SSH-Schlüssel und einem vorhandenen x509-Zertifikat erstellen.

Gehen Sie wie folgt vor, um einen Schlüssel im PEM-Format aus einem vorhandenen öffentlichen SSH-Schlüssel zu erstellen:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Beispiel für ssh-keygen
```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Gespeicherte Schlüsseldateien:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Der Schlüsselpaarname für diesen Artikel.  Standardmäßig wird ein Schlüsselpaar mit dem Namen **id_rsa** verwendet, und einige Tools erwarten den Namen **id_rsa** für die Datei mit dem privaten Schlüssel. Es ist also ratsam, dies zu berücksichtigen. Das Verzeichnis `~/.ssh/` ist der Standardspeicherort für SSH-Schlüsselpaare und die SSH-Konfigurationsdatei.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Eine Auflistung für das Verzeichnis `~/.ssh` . `ssh-keygen` erstellt das Verzeichnis `~/.ssh`, falls es nicht vorhanden ist, und legt auch die richtige Eigentümerschaft und die Dateimodi fest.

Kennwort für den Schlüssel:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` bezieht sich auf ein Kennwort als „Passphrase“.  Es wird *dringend* empfohlen, Ihren Schlüsselpaaren ein Kennwort hinzuzufügen. Wenn das Schlüsselpaar nicht mit einem Kennwort geschützt ist, kann es von Personen, die über die Datei mit dem privaten Schlüssel verfügen, zum Anmelden an allen Servern mit dem entsprechenden öffentlichen Schlüssel verwendet werden. Das Hinzufügen eines Kennworts sorgt für mehr Schutz. Wenn sich eine Person Zugang zu Ihrer Datei mit dem privaten Schlüssel verschafft, haben Sie so mehr Zeit, die Schlüssel für die Authentifizierung zu ändern.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Verwenden von „ssh-agent“ zum Speichern des Kennworts für den privaten Schlüssel
Sie können `ssh-agent` zum Zwischenspeichern des Kennworts für die private Schlüsseldatei verwenden, damit Sie das Kennwort nicht bei jeder SSH-Anmeldung eingeben müssen. Bei Verwendung eines Macintosh-Computers werden die Kennwörter für private Schlüssel sicher im „Schlüsselbund“ von OSX gespeichert, wenn Sie `ssh-agent`aufrufen.

Überprüfen Sie zunächst, ob `ssh-agent` ausgeführt wird.

```bash
eval "$(ssh-agent -s)"
```

Fügen Sie anschließend den privaten Schlüssel `ssh-agent` mit dem Befehl `ssh-add` hinzu.

```bash
ssh-add ~/.ssh/id_rsa
```

Das Kennwort für den privaten Schlüssel ist jetzt unter `ssh-agent` gespeichert.

## <a name="create-and-configure-an-ssh-config-file"></a>Erstellen und Konfigurieren einer SSH-Konfigurationsdatei
Es ist eine empfohlene bewährte Methode, eine `~/.ssh/config`-Datei zu erstellen und zu konfigurieren, um Anmeldungen zu beschleunigen und das Verhalten Ihres SSH-Clients zu optimieren.

Im folgenden Beispiel wird eine Standardkonfiguration veranschaulicht.

### <a name="create-the-file"></a>Erstellen der Datei
```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bearbeiten der Datei zum Hinzufügen der neuen SSH-Konfiguration
```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Beispieldatei für `~/.ssh/config`:
```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Bei dieser SSH-Konfiguration werden Abschnitte für jeden Server bereitgestellt, damit jeweils ein eigenes dediziertes Schlüsselpaar verwendet werden kann. Die Standardeinstellungen (`Host *`) gelten für alle Hosts, die nicht den spezifischen Hosts auf höheren Ebenen in der Konfigurationsdatei entsprechen.

### <a name="config-file-explained"></a>Beschreibung der Config-Datei
`Host` = Name des Hosts, der im Terminal aufgerufen wird.  `ssh fedora22` weist `SSH` an, die Werte im Einstellungsblock mit der Bezeichnung `Host fedora22` zu verwenden. HINWEIS: Dies kann eine beliebige Bezeichnung sein, die für Ihre Nutzung logisch ist und nicht den tatsächlichen Hostnamen eines Servers darstellt.

`Hostname 102.160.203.241` = IP-Adresse oder DNS-Name für den Server, auf den zugegriffen wird.

`User myusername` = Remotebenutzerkonto, das beim Anmelden auf dem Server verwendet wird.

`PubKeyAuthentication yes` = weist SSH an, dass Sie einen SSH-Schlüssel für die Anmeldung verwenden möchten.

`IdentityFile /home/myusername/.ssh/id_id_rsa` = der private SSH-Schlüssel und dazugehörige öffentliche Schlüssel für die Authentifizierung.

## <a name="ssh-into-linux-without-a-password"></a>SSH für Linux ohne Kennwort
Da Sie jetzt über ein SSH-Schlüsselpaar und eine konfigurierte SSH-Konfigurationsdatei verfügen, können Sie sich schnell und sicher an Ihrer Linux-VM anmelden. Beim ersten Anmelden an einem Server mit einem SSH-Schlüssel werden Sie vom Befehl zur Eingabe der Passphrase für die Schlüsseldatei aufgefordert.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Befehlsbeschreibung
Wenn `ssh fedora22` ausgeführt wird, ermittelt und lädt SSH zuerst die Einstellungen aus dem Block `Host fedora22` und lädt dann alle restlichen Einstellungen aus dem letzten Block `Host *`.

## <a name="next-steps"></a>Nächste Schritte
Der nächste Schritt besteht darin, Azure-Linux-VMs mit dem neuen öffentlichen SSH-Schlüssel zu erstellen.  Virtuelle Azure-Computer, die mit einem öffentlichen SSH-Schlüssel als Anmeldung erstellt werden, sind besser gesichert als VMs, die mit der Standardanmeldemethode und Kennwörtern erstellt werden.  Mit SSH-Schlüsseln erstellte Azure-VMs werden standardmäßig mit deaktivierten Kennwörtern konfiguriert, um Brute-Force-Angriffe zum Erraten von Kennwörtern auszuschließen.

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Nov16_HO3-->


