---
title: "Erstellen eines SSH-Schlüsselpaars für virtuelle Linux-Computer in Azure| Microsoft-Dokumentation"
description: "Erstellen Sie auf sichere Weise ein öffentlich-privates SSH-Schlüsselpaar für virtuelle Azure Linux-Computer."
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
ms.date: 2/6/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 6776fe5cf1faadfbcfc9bbd6c69ec1380c2429c7
ms.openlocfilehash: 67dce2c226f5ec5f396ef51cd59781d532d76795
ms.lasthandoff: 02/27/2017


---

# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer

In diesem Artikel erfahren Sie, wie Sie ein öffentlich-privates SSH-Schlüsselpaar für die Verwendung mit virtuellen Linux-Computern generieren.  Mit einem SSH-Schlüsselpaar können Sie virtuelle Computer in Azure erstellen, bei deren Authentifizierung standardmäßig SSH-Schlüssel verwendet werden, sodass zum Anmelden keine Kennwörter mehr erforderlich sind.  Kennwörter können erraten werden und machen Ihre virtuellen Computer anfällig für entsprechende Brute-Force-Angriffe. Virtuelle Computer, die mit Azure-Vorlagen oder der `azure-cli` erstellt werden, können Ihren öffentlichen SSH-Schlüssel im Rahmen der Bereitstellung enthalten, sodass eine nachträgliche Deaktivierung von Kennwortanmeldungen für SSH entfällt.

## <a name="quick-commands"></a>Schnellbefehle

Führen Sie die folgenden Befehle über eine Bash-Shell aus, und ersetzen Sie dabei die Beispiele durch Ihre eigenen Werte.

SSH-Schlüssel werden standardmäßig im Verzeichnis `~/.ssh` gespeichert.  Wenn Sie nicht über das Verzeichnis `~/.ssh` verfügen, können Sie es mithilfe des Befehls `ssh-keygen` gleich mit den richtigen Berechtigungen erstellen.  Das `-N`-Argument legt das Kennwort zum Verschlüsseln des privaten SSH-Schlüssels fest und ist *nicht* Ihr Benutzerkennwort.

```bash
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -N mypassword
```

Fügen Sie `ssh-agent` den neu erstellten Schlüssel hinzu:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!NOTE] 
> Die oben angegebenen Befehle eignen sich für nahezu alle Linux-Distributionen, aber nicht unbedingt für Container, da die Umgebung sehr stark eingeschränkt werden kann.

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Die Verwendung von öffentlichen und privaten SSH-Schlüsseln ist die einfachste Möglichkeit der Anmeldung an Ihren Linux-Servern. [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) (Public-Key-Kryptosystem) ist ein Weg mit deutlich mehr Sicherheit für die Anmeldung an Ihrer Linux- oder BSD-VM in Azure als Kennwörter, die viel leichter einem Brute-Force-Angriff unterzogen werden können.

Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur).  Der private SSH-Schlüssel muss mit einem [sehr sicheren Kennwort](https://www.xkcd.com/936/)(source:[xkcd.com](https://xkcd.com)) geschützt werden.  Dieses Kennwort gilt nur für den Zugriff auf den privaten SSH-Schlüssel und ist **nicht** das Kennwort für das Benutzerkonto.  Wenn Sie Ihren SSH-Schlüssel mit einem Kennwort versehen, wird der private Schlüssel mit AES 128 Bit verschlüsselt, sodass er ohne das Kennwort nicht verwendet werden kann.  Falls ein Angreifer Ihren privaten Schlüssel entwendet und dafür kein Kennwort konfiguriert wurde, kann sich der Angreifer mithilfe des privaten Schlüssels bei jedem Server anmelden, der über den entsprechenden öffentlichen Schlüssel verfügt.  Wenn der private Schlüssel mit einem Kennwort geschützt ist, kann er vom Angreifer nicht verwendet werden, und Ihre Infrastruktur unter Azure verfügt über eine weitere Sicherheitsebene.

In diesem Artikel werden Dateien mit Schlüsseln im *ssh-rsa* -Format erstellt, die für Bereitstellungen im Resource Manager empfohlen werden.  *ssh-rsa*-Schlüssel werden im [Portal](https://portal.azure.com) sowohl für klassische Bereitstellungen als auch für Resource Manager-Bereitstellungen benötigt.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Deaktivieren von SSH-Kennwörtern durch Verwendung von SSH-Schlüsseln

Für Azure sind öffentliche und private Schlüssel mit mindestens 2048 Bit im Format „ssh-rsa“ erforderlich. Verwenden Sie zum Erstellen der Schlüssel `ssh-keygen`. Hierbei wird eine Reihe von Fragen gestellt, und anschließend werden ein privater Schlüssel und ein passender öffentlicher Schlüssel geschrieben. Bei der Erstellung einer Azure-VM wird der öffentliche Schlüssel in `~/.ssh/authorized_keys` kopiert.  SSH-Schlüssel in `~/.ssh/authorized_keys` werden verwendet, um vom Client zu fordern, für eine SSH-Anmeldeverbindung den entsprechenden privaten Schlüssel bereitzustellen.  Wenn bei der Erstellung eines virtuellen Azure-Computers unter Linux SSH-Schlüssel für die Authentifizierung verwendet werden, konfiguriert Azure den SSHD-Server so, dass nur SSH-Schlüssel (und keine kennwortbasierten Anmeldungen) möglich sind.  Wenn Sie also virtuelle Azure Linux-Computer mit SSH-Schlüsseln erstellen, können Sie zur Sicherung der Bereitstellung des virtuellen Computers beitragen, und der üblicherweise nachträglich auszuführende Konfigurationsschritt zum Deaktivieren von Kennwörtern in der Konfigurationsdatei „sshd_config“ entfällt.

## <a name="using-ssh-keygen"></a>Verwenden von ssh-keygen

Mit diesem Befehl wird ein kennwortgeschütztes (verschlüsseltes) SSH-Schlüsselpaar per RSA mit 2048 Bit erstellt und kommentiert, um eine einfache Identifizierung zu ermöglichen.  

SSH-Schlüssel werden standardmäßig im Verzeichnis `~/.ssh` gespeichert.  Wenn Sie nicht über das Verzeichnis `~/.ssh` verfügen, können Sie es mithilfe des Befehls `ssh-keygen` gleich mit den richtigen Berechtigungen erstellen.

```bash
ssh-keygen \
-t rsa \
-b 2048 \
-C "ahmet@myserver" \
-f ~/.ssh/id_rsa \
-N mypassword
```

*Befehlsbeschreibung*

`ssh-keygen` = zum Erstellen der Schlüssel verwendetes Programm

`-t rsa` = Typ des zu erstellenden Schlüssels, also RSA-Format [Wikipedia](https://de.wikipedia.org/wiki/RSA-Kryptosystem)

`-b 2048` = Bits des Schlüssels

`-C "myusername@myserver"` = ein Kommentar, der zur einfachen Identifizierung am Ende der Datei mit dem öffentlichen Schlüssel angefügt wird.  Normalerweise wird eine E-Mail-Adresse als Kommentar verwendet, aber Sie können je nach Eignung für Ihre Infrastruktur auch etwas anderes angeben.

## <a name="classic-portal-and-x509-certs"></a>Klassisches Portal und X.509-Zertifikate

Bei Verwendung des [klassischen Portals](https://manage.windowsazure.com/) von Azure sind X.509-Zertifikate für die SSH-Schlüssel erforderlich.  Öffentliche SSH-Schlüssel eines anderen Typs sind nicht zulässig, es *müssen* X.509-Zertifikate verwendet werden.

So erstellen Sie ein X.509-Zertifikat aus Ihrem vorhandenen privaten SSH-RSA-Schlüssel:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Klassische Bereitstellung mithilfe von `asm`

Bei Verwendung des klassischen Bereitstellungsmodells (Befehlszeilenschnittstelle der Azure-Dienstverwaltung `asm`) können Sie einen öffentlichen SSH-RSA-Schlüssel oder einen Schlüssel im RFC4716-Format in einem PEM-Container verwenden.  Der öffentliche SSH-RSA-Schlüssel wurde weiter oben in diesem Artikel mit `ssh-keygen` erstellt.

So erstellen Sie einen Schlüssel im RFC4716-Format aus einem vorhandenen öffentlichen SSH-Schlüssel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Beispiel für ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
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

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

Der Schlüsselpaarname für diesen Artikel.  Standardmäßig wird ein Schlüsselpaar mit dem Namen **id_rsa** verwendet, und einige Tools erwarten den Namen **id_rsa** für die Datei mit dem privaten Schlüssel. Es ist also ratsam, dies zu berücksichtigen. Das Verzeichnis `~/.ssh/` ist der Standardspeicherort für SSH-Schlüsselpaare und die SSH-Konfigurationsdatei.  Wenn kein vollständiger Pfad angegeben wird, erstellt `ssh-keygen` die Schlüssel im aktuellen Arbeitsverzeichnis anstatt im Standardverzeichnis `~/.ssh`.

Eine Auflistung für das Verzeichnis `~/.ssh` .

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Kennwort für den Schlüssel:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` bezieht sich auf ein Kennwort als „Passphrase“.  Es wird *dringend* empfohlen, Ihren Schlüsselpaaren ein Kennwort hinzuzufügen. Wenn das Schlüsselpaar nicht mit einem Kennwort geschützt ist, kann es von Personen, die über die Datei mit dem privaten Schlüssel verfügen, zum Anmelden an allen Servern mit dem entsprechenden öffentlichen Schlüssel verwendet werden. Das Hinzufügen eines Kennworts sorgt für mehr Schutz. Wenn sich eine Person Zugang zu Ihrer Datei mit dem privaten Schlüssel verschafft, haben Sie so mehr Zeit, die Schlüssel für die Authentifizierung zu ändern.

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

## <a name="using-ssh-copy-id-to-install-the-new-key"></a>Installieren des neuen Schlüssels mithilfe von `ssh-copy-id`
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

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer geschützten Linux-VM mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

