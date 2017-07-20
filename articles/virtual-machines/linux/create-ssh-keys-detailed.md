---
title: "Ausführliche Schritte zum Erstellen eines SSH-Schlüsselpaars für virtuelle Linux-Computer in Azure| Microsoft-Dokumentation"
description: "Hier finden Sie zusätzliche Schritte zum Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer in Azure mit bestimmten Zertifikaten für verschiedene Anwendungsfälle."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 0cb70d36bd6e8d4cf5fcd5ed4a3e85c42f3cf81d
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

# <a name="detailed-walk-through-to-create-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a>Ausführliche exemplarische Vorgehensweise zum Erstellen eines SSH-Schlüsselpaars mit zusätzlichen Zertifikaten für einen virtuellen Linux-Computer in Azure
Mit einem SSH-Schlüsselpaar können Sie virtuelle Computer in Azure erstellen, bei deren Authentifizierung standardmäßig SSH-Schlüssel verwendet werden, sodass zum Anmelden keine Kennwörter mehr erforderlich sind. Kennwörter können erraten werden und machen Ihre virtuellen Computer anfällig für entsprechende Brute-Force-Angriffe. Virtuelle Computer, die mit der Azure CLI oder Resource Manager-Vorlagen erstellt werden, können Ihren öffentlichen SSH-Schlüssel als Teil der Bereitstellung enthalten, sodass eine nachträgliche Deaktivierung von Kennwortanmeldungen für SSH entfällt. Dieser Artikel enthält detaillierte Schritte und zusätzliche Beispiele zum Generieren von Zertifikaten, z.B. zur Verwendung mit virtuellen Linux-Computern. Wenn Sie schnell ein SSH-Schlüsselpaar erstellen und verwenden möchten, finden Sie Informationen dazu unter [Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer in Azure](mac-create-ssh-keys.md).

## <a name="understanding-ssh-keys"></a>Grundlegendes zu SSH-Schlüssels

Die Verwendung von öffentlichen und privaten SSH-Schlüsseln ist die einfachste Möglichkeit der Anmeldung an Ihren Linux-Servern. [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) (Public-Key-Kryptosystem) ist ein Weg mit deutlich mehr Sicherheit für die Anmeldung an Ihrer Linux- oder BSD-VM in Azure als Kennwörter, die viel leichter einem Brute-Force-Angriff unterzogen werden können.

Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur).  Der private SSH-Schlüssel muss mit einem [sehr sicheren Kennwort](https://www.xkcd.com/936/)(source:[xkcd.com](https://xkcd.com)) geschützt werden.  Dieses Kennwort gilt nur für den Zugriff auf die private SSH-Schlüsseldatei und ist **nicht** das Kennwort für das Benutzerkonto.  Wenn Sie Ihren SSH-Schlüssel mit einem Kennwort versehen, wird der private Schlüssel mit AES 128 Bit verschlüsselt, sodass er ohne das Kennwort nicht verwendet werden kann.  Falls ein Angreifer Ihren privaten Schlüssel entwendet und dafür kein Kennwort konfiguriert wurde, kann sich der Angreifer mithilfe des privaten Schlüssels bei jedem Server anmelden, der über den entsprechenden öffentlichen Schlüssel verfügt.  Wenn der private Schlüssel mit einem Kennwort geschützt ist, kann er vom Angreifer nicht verwendet werden, und Ihre Infrastruktur unter Azure verfügt über eine weitere Sicherheitsebene.

In diesem Artikel wird ein öffentlich-privates SSH-Schlüsseldateipaar (Protokollversion 2 RSA, auch als „SSH-RSA-Schlüssel“ bezeichnet) erstellt, das für Bereitstellungen mit Azure Resource Manager empfohlen wird. *ssh-rsa*-Schlüssel werden im [Portal](https://portal.azure.com) sowohl für klassische Bereitstellungen als auch für Resource Manager-Bereitstellungen benötigt.

## <a name="ssh-keys-use-and-benefits"></a>Verwendung und Vorteile von SSH-Schlüsseln

Azure erfordert öffentlich-private Schlüssel im SSH-Format (Protokollversion 2 RSA) mit mindestens 2048 Bit; die öffentliche Schlüsseldatei weist das Containerformat `.pub` auf. Verwenden Sie zum Erstellen der Schlüssel `ssh-keygen`. Hierbei wird eine Reihe von Fragen gestellt, und anschließend werden ein privater Schlüssel und ein passender öffentlicher Schlüssel geschrieben. Beim Erstellen eines virtuellen Azure-Computers kopiert Azure den öffentlichen Schlüssel in den Ordner `~/.ssh/authorized_keys` auf dem virtuellen Computer. SSH-Schlüssel in `~/.ssh/authorized_keys` werden verwendet, um vom Client zu fordern, für eine SSH-Anmeldeverbindung den entsprechenden privaten Schlüssel bereitzustellen.  Wenn bei der Erstellung eines virtuellen Azure-Computers unter Linux SSH-Schlüssel für die Authentifizierung verwendet werden, konfiguriert Azure den SSHD-Server so, dass nur SSH-Schlüssel (und keine kennwortbasierten Anmeldungen) möglich sind.  Wenn Sie also virtuelle Azure Linux-Computer mit SSH-Schlüsseln erstellen, können Sie zur Sicherung der Bereitstellung des virtuellen Computers beitragen, und der üblicherweise nachträglich auszuführende Konfigurationsschritt zum Deaktivieren von Kennwörtern in der Datei **sshd_config** entfällt.

## <a name="using-ssh-keygen"></a>Verwenden von ssh-keygen

Mit diesem Befehl wird ein kennwortgeschütztes (verschlüsseltes) SSH-Schlüsselpaar per RSA mit 2048 Bit erstellt und kommentiert, um eine einfache Identifizierung zu ermöglichen.  

SSH-Schlüssel werden standardmäßig im Verzeichnis `~/.ssh` gespeichert.  Wenn Sie nicht über das Verzeichnis `~/.ssh` verfügen, können Sie es mithilfe des Befehls `ssh-keygen` gleich mit den richtigen Berechtigungen erstellen.

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

*Befehlsbeschreibung*

`ssh-keygen` = zum Erstellen der Schlüssel verwendetes Programm

`-t rsa` = Typ des zu erstellenden Schlüssels, also RSA-Format [Wikipedia](https://de.wikipedia.org/wiki/RSA-Kryptosystem)

`-b 2048` = Bits des Schlüssels

`-C "azureuser@myserver"` = ein Kommentar, der zur einfachen Identifizierung am Ende der Datei mit dem öffentlichen Schlüssel angefügt wird.  Normalerweise wird eine E-Mail-Adresse als Kommentar verwendet, aber Sie können je nach Eignung für Ihre Infrastruktur auch etwas anderes angeben.

## <a name="classic-deploy-using-asm"></a>Klassische Bereitstellung mithilfe von `asm`

Bei Verwendung des klassischen Bereitstellungsmodells (`asm`-Modus in der CLI) können Sie einen öffentlichen SSH-RSA-Schlüssel oder einen Schlüssel im RFC4716-Format in einem PEM-Container verwenden.  Der öffentliche SSH-RSA-Schlüssel wurde weiter oben in diesem Artikel mit `ssh-keygen` erstellt.

So erstellen Sie einen Schlüssel im RFC4716-Format aus einem vorhandenen öffentlichen SSH-Schlüssel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Beispiel für ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
The keys randomart image is:
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

`Enter file in which to save the key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

Der Schlüsselpaarname für diesen Artikel.  Standardmäßig wird ein Schlüsselpaar mit dem Namen **id_rsa** verwendet, und einige Tools erwarten den Namen **id_rsa** für die Datei mit dem privaten Schlüssel. Es ist also ratsam, dies zu berücksichtigen. Das Verzeichnis `~/.ssh/` ist der Standardspeicherort für SSH-Schlüsselpaare und die SSH-Konfigurationsdatei.  Wenn kein vollständiger Pfad angegeben wird, erstellt `ssh-keygen` die Schlüssel im aktuellen Arbeitsverzeichnis anstatt im Standardverzeichnis `~/.ssh`.

Eine Auflistung für das Verzeichnis `~/.ssh` .

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

Kennwort für den Schlüssel:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` bezieht sich auf ein Kennwort für den privaten Schlüssel als „Passphrase“.  Es wird *dringend* empfohlen, Ihrem privaten Schlüssel ein Kennwort hinzuzufügen. Wenn die Schlüsseldatei nicht mit einem Kennwort geschützt ist, kann es von Personen, die über die Datei verfügen, zum Anmelden an allen Servern mit dem entsprechenden öffentlichen Schlüssel verwendet werden. Das Hinzufügen eines Kennworts (Passphrase) sorgt für mehr Schutz. Wenn sich eine Person Zugang zu Ihrer Datei mit dem privaten Schlüssel verschafft, haben Sie so mehr Zeit, die Schlüssel für die Authentifizierung zu ändern.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Verwenden von „ssh-agent“ zum Speichern des Kennworts für den privaten Schlüssel

Sie können `ssh-agent` zum Zwischenspeichern des Kennworts für die private Schlüsseldatei verwenden, damit Sie das Kennwort nicht bei jeder SSH-Anmeldung eingeben müssen. Bei Verwendung eines Macintosh-Computers werden die Kennwörter für private Schlüssel sicher im „Schlüsselbund“ von OSX gespeichert, wenn Sie `ssh-agent`aufrufen.

Überprüfen und verwenden Sie „ssh-agent“ und „ssh-add“, um das SSH-System über die Schlüsseldateien zu informieren, damit die Passphrase nicht interaktiv verwendet werden muss.

```bash
eval "$(ssh-agent -s)"
```

Fügen Sie anschließend den privaten Schlüssel `ssh-agent` mit dem Befehl `ssh-add` hinzu.

```bash
ssh-add ~/.ssh/id_rsa
```

Das Kennwort für den privaten Schlüssel ist jetzt unter `ssh-agent` gespeichert.

## <a name="using-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>Kopieren des Schlüssels auf einen vorhandenen virtuellen Computer mithilfe von `ssh-copy-id`
Wenn Sie bereits einen virtuellen Computer erstellt haben, können Sie den neuen öffentlichen SSH-Schlüssel für den virtuellen Linux-Computer wie folgt installieren:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

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
  User ahmet
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

`Host` = Name des Hosts, der im Terminal aufgerufen wird.  `ssh fedora22` weist `SSH` an, die Werte im Einstellungsblock mit der Bezeichnung `Host fedora22` zu verwenden. HINWEIS: Der Host kann eine beliebige Bezeichnung sein, die sich für Ihr Szenario anbietet und nicht den tatsächlichen Hostnamen eines Servers darstellt.

`Hostname 102.160.203.241` = IP-Adresse oder DNS-Name für den Server, auf den zugegriffen wird.

`User ahmet` = Remotebenutzerkonto, das bei der Serveranmeldung verwendet wird.

`PubKeyAuthentication yes` = weist SSH an, dass Sie einen SSH-Schlüssel für die Anmeldung verwenden möchten.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` = der private SSH-Schlüssel und dazugehörige öffentliche Schlüssel für die Authentifizierung.

## <a name="ssh-into-linux-without-a-password"></a>SSH für Linux ohne Kennwort

Da Sie jetzt über ein SSH-Schlüsselpaar und eine konfigurierte SSH-Konfigurationsdatei verfügen, können Sie sich schnell und sicher an Ihrer Linux-VM anmelden. Beim ersten Anmelden an einem Server mit einem SSH-Schlüssel werden Sie vom Befehl zur Eingabe der Passphrase für die Schlüsseldatei aufgefordert.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Befehlsbeschreibung

Wenn `ssh fedora22` ausgeführt wird, ermittelt und lädt SSH zuerst die Einstellungen aus dem Block `Host fedora22` und lädt dann alle restlichen Einstellungen aus dem letzten Block `Host *`.

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt besteht darin, Azure-Linux-VMs mit dem neuen öffentlichen SSH-Schlüssel zu erstellen.  Virtuelle Azure-Computer, die mit einem öffentlichen SSH-Schlüssel als Anmeldung erstellt werden, sind besser gesichert als VMs, die mit der Standardanmeldemethode und Kennwörtern erstellt werden.  Mit SSH-Schlüsseln erstellte Azure-VMs werden standardmäßig mit deaktivierten Kennwörtern konfiguriert, um Brute-Force-Angriffe zum Erraten von Kennwörtern auszuschließen.

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit dem Azure-Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit der Azure-Befehlszeilenschnittstelle](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

