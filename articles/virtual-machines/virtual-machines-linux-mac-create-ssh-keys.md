<properties
	pageTitle="Erstellen von SSH-Schlüsseln unter Linux und Mac | Microsoft Azure"
	description="Generieren und Verwenden von SSH-Schlüsseln unter Linux und Mac für die Ressourcen-Manager- und klassischen Bereitstellungsmodelle in Azure."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="vlivech"
	manager="timlt"
	editor=""
	tags="" />

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/12/2016"
	ms.author="v-livech"/>

# Erstellen von SSH-Schlüsseln unter Linux und Mac für Linux-VMs in Azure

Auf Ihrer Arbeitsstation muss ein Terminal geöffnet sein, wenn Sie einen per Kennwort geschützten öffentlichen und privaten SSH-Schlüssel erstellen möchten. Nachdem Sie über SSH-Schlüssel verfügen, können Sie standardmäßig neue VMs mit diesem Schlüssel erstellen oder den öffentlichen Schlüssel vorhandenen VMs hinzufügen, indem Sie die Azure-Befehlszeilenschnittstelle und Azure-Vorlagen verwenden.

## Kurze Befehlsliste

Ersetzen Sie in den folgenden Befehlsbeispielen die Werte zwischen &lt; und &gt; durch die Werte aus Ihrer eigenen Umgebung.

```bash
[ahmet@fedora ~]$ ssh-keygen -t rsa -b 2048 -C "<your_user@yourdomain.com>"

#Enter the name of the file that will be saved in the `~/.ssh/` directory.
<azure_fedora_id_rsa>

#Enter passphrase for azure_fedora_id_rsa:
<correct horse battery staple>

#Add the newly created key to `ssh-agent` on Linux and Mac (also added to OSX Keychain).
[ahmet@fedora ~]$ eval "$(ssh-agent -s)"
[ahmet@fedora ~]$ ssh-add ~/.ssh/azure_fedora_id_rsa

#Copy the SSH public key to your Linux Server.
[ahmet@fedora ~]$ ssh-copy-id -i ~/.ssh/azure_fedora_id_rsa.pub <youruser@yourserver.com>

#Test the login using keys instead of a password.
[ahmet@fedora ~]$ ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/azure_fedora_id_rsa <youruser@yourserver.com>

Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
[ahmet@fedora ~]$

```

## Einführung

Die Verwendung von öffentlichen und privaten SSH-Schlüsseln ist die einfachste Möglichkeit, sich an Ihren Linux-Servern anzumelden. Aber [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) (Public-Key-Kryptosystem) ist ein Weg mit deutlich mehr Sicherheit für die Anmeldung an Ihrer Linux- oder BSD-VM in Azure als Kennwörter, die viel leichter einem Brute-Force-Angriff unterzogen werden können. Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur). Der erstellte private SSH-Schlüssel verfügt als Schutz über ein [sicheres Kennwort](https://www.xkcd.com/936/). Dieses Kennwort ist nur für den Zugriff auf den privaten SSH-Schlüssel bestimmt und dient **nicht** als Kennwort für das Benutzerkonto. Alle Personen, die im Besitz eines privaten Schlüssels ohne Kennwort sind, können auf alle Server mit installiertem öffentlichem Schlüssel zugreifen. Ohne Kennwort kann der private Schlüssel nicht verwendet werden.


In diesem Artikel werden Dateien mit Schlüsseln im *ssh-rsa*-Format erstellt, da diese für Bereitstellungen im Resource Manager empfohlen werden und im [Portal](https://portal.azure.com) sowohl für klassische Bereitstellungen als auch für Resource Manager-Bereitstellungen benötigt werden.


## Erstellen der SSH-Schlüssel

Für Azure sind öffentliche und private Schlüssel mit mindestens 2048 Bit im Format „ssh-rsa“ erforderlich. Zum Erstellen des Schlüsselpaars verwenden wir `ssh-keygen`. Hiermit wird eine Reihe von Fragen gestellt, und anschließend werden ein privater Schlüssel und ein passender öffentlicher Schlüssel geschrieben. Beim Erstellen Ihrer Azure-VM übergeben Sie den Inhalt des öffentlichen Schlüssels, der dann auf die Linux-VM kopiert und mit Ihrem lokalen und sicher gespeicherten privaten Schlüssel verwendet wird, um Sie beim Anmelden zu authentifizieren.

### Verwenden von `ssh-keygen`

Mit diesem Befehl wird ein kennwortgeschütztes SSH-Schlüsselpaar per RSA mit 2048 Bit erstellt und kommentiert, um die einfache Identifizierung zu ermöglichen.

```
ahmet@fedora$ ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
```

##### Befehlsbeschreibung

`ssh-keygen` = zum Erstellen der Schlüssel verwendetes Programm

`-t rsa` = zu erstellender Schlüsseltyp, also [RSA-Format](https://en.wikipedia.org/wiki/RSA_(cryptosystem))

`-b 2048` = Bits des Schlüssels

`-C "ahmet@fedoraVMAzure"` = ein Kommentar, der zur einfachen Identifizierung am Ende der Datei mit dem öffentlichen Schlüssel angefügt wird. Normalerweise wird eine E-Mail-Adresse als Kommentar verwendet, aber Sie können je nach Eignung für Ihre Infrastruktur auch etwas anderes angeben.

#### Exemplarische Vorgehensweise für `ssh-keygen`

```bash
ahmet@fedora$ ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in azure_fedora_id_rsa.
Your public key has been saved in azure_fedora_id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@fedoraVMAzure
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

ahmet@fedora$ ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 azure_fedora_id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 azure_fedora_id_rsa.pub
```

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa` Der Schlüsselpaarname für diesen Artikel. Der Name **id\_rsa** für ein Schlüsselpaar ist die Standardeinstellung. Einige Tools erwarten für die Datei mit dem privaten Schlüssel ggf. den Namen **id\_rsa**, sodass es ratsam ist, wenn er vorhanden ist. (`~/.ssh/` ist der typische Standardspeicherort für alle SSH-Schlüsselpaare und die SSH-Konfigurationsdatei.)

`Enter passphrase (empty for no passphrase):` Es wird dringend empfohlen, Ihre Schlüsselpaare mit einem Kennwort zu versehen (unter `ssh-keygen` wird dies als „Passphrase“ bezeichnet). Wenn das Schlüsselpaar nicht mit einem Kennwort geschützt ist, kann es von Personen, die über eine Kopie der Datei mit dem privaten Schlüssel verfügen, zum Anmelden an allen Servern verwendet werden. Dies gilt für Server, die über den entsprechenden öffentlichen Schlüssel verfügen. Das Hinzufügen eines Kennworts sorgt also für deutlich mehr Schutz. Wenn sich eine Person Zugang zu Ihrer Datei mit dem privaten Schlüssel verschafft, haben Sie so mehr Zeit, die Schlüssel für die Authentifizierung zu ändern.

`ahmet@fedora$ ls -al ~/.ssh` Hier werden Ihre neuen Schlüsselpaare und die dazugehörigen Berechtigungen angezeigt. `ssh-keygen` erstellt das Verzeichnis `~/.ssh`, falls es nicht vorhanden ist, und legt auch die richtige Eigentümerschaft und die Dateimodi fest.

## Verwenden von „ssh-agent“ zum Speichern des Kennworts für den privaten Schlüssel

Um zu vermeiden, dass Sie das Kennwort für die Datei mit dem privaten Schlüssel bei jeder SSH-Anmeldung eingeben müssen, können Sie `ssh-agent` zum Zwischenspeichern des Kennworts für die Datei mit dem privaten Schlüssel verwenden. Die Anmeldung an Ihrer Linux-VM ist dann schnell und sicher möglich. Bei Verwendung von OSX werden die Kennwörter für Ihren privaten Schlüssel sicher im „Schlüsselbund“ gespeichert, wenn Sie `ssh-agent` aufrufen.

Überprüfen Sie zunächst, ob `ssh-agent` ausgeführt wird.

`[ahmet@fedora ~]$ eval "$(ssh-agent -s)"`

Fügen Sie nun `ssh-agent` den privaten Schlüssel mit dem Befehl `ssh-add` hinzu. OSX startet wieder den Schlüsselbund, um die Anmeldeinformationen zu speichern.

`[ahmet@fedora ~]$ ssh-add ~/.ssh/azure_fedora_id_rsa`

Das Kennwort für den privaten Schlüssel ist jetzt gespeichert, und Sie müssen das Schlüsselkennwort nicht mehr bei jeder SSH-Anmeldung eingeben.

## Erstellen und Konfigurieren einer SSH-Konfigurationsdatei

Auch wenn diese Maßnahme für die ersten Schritte mit einer Linux-VM nicht unbedingt erforderlich ist, gilt Folgendes: Es ist eine bewährte Methode, die Datei `~/.ssh/config` zu erstellen und zu konfigurieren. Damit soll verhindert werden, dass Sie versehentlich Kennwörter zum Anmelden an Ihren VMs verwenden. Außerdem können Sie die Automatisierung mit unterschiedlichen Schlüsselpaaren für unterschiedliche Azure-VMs durchführen und andere Programme konfigurieren, z.B. **git**, um mehrere Server zu erreichen.

Im folgenden Beispiel wird eine Standardkonfiguration veranschaulicht.

### Erstellen der Datei

```bash
ahmet@fedora$ touch ~/.ssh/config
```

### Bearbeiten der Datei zum Hinzufügen der neuen SSH-Konfiguration

```bash
ahmet@fedora$ vim ~/.ssh/config

#Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
  PubkeyAuthentication yes
  IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa
# ./Azure Keys
# GitHub keys
Host github.com
  Hostname github.com
  User git
  PubKeyAuthentication yes
  IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa
Host github.private
  Hostname github.com
  User git
  PubKeyAuthentication yes
  IdentityFile /home/ahmet/.ssh/private_repo_azure_fedora_id_rsa
# ./Github Keys
# Default Settings
Host *
  PubkeyAuthentication=no
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath /home/ahmet/.ssh/Connections/ssh-%r@%h:%p
  ControlPersist 4h
  StrictHostKeyChecking=no
  IdentityFile /home/ahmet/.ssh/id_rsa
  UseRoaming=no
```

Bei dieser SSH-Konfiguration werden Abschnitte für jeden Dienst bereitgestellt, damit jeweils ein eigenes dediziertes Schlüsselpaar verwendet werden kann. Die Standardeinstellungen gelten für alle Hosts, an denen Sie angemeldet sind, die nicht mit einer der obigen Gruppen übereinstimmen. Außerdem können Sie bei der SSH-Konfiguration zwei separate [GitHub](https://github.com)-Anmeldungen verwenden, und zwar eine für die öffentliche Arbeit und eine zweite nur für private Repositorys, die Sie ggf. nutzen.


##### Beschreibung der Config-Datei

`Host` = Name des Hosts, der im Terminal aufgerufen wird. `ssh fedora22` weist `SSH` an, die Werte im Einstellungsblock mit der Bezeichnung `Host fedora22` zu verwenden. HINWEIS: Dies kann eine beliebige Bezeichnung sein, die für Ihre Nutzung logisch ist und nicht den tatsächlichen Hostnamen eines Servers darstellt.

`Hostname 102.160.203.241` = IP-Adresse oder DNS-Name für den Server, für den die Anmeldung besteht. Wird für die Weiterleitung an den Server verwendet und kann eine externe IP-Adresse sein, die einer internen IP-Adresse zugeordnet ist.

`User git` = Remotebenutzerkonto, das beim Anmelden an der Azure-VM verwendet wird.

`PubKeyAuthentication yes` = Weist SSH an, dass Sie einen SSH-Schlüssel für die Anmeldung verwenden möchten.

`IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa` = Weist SSH an, welches Schlüsselpaar zur Authentifizierung bei der Anmeldung für den Server bereitgestellt werden soll.


## SSH für eine Linux-VM ohne Kennwort

Da Sie jetzt über ein SSH-Schlüsselpaar und eine konfigurierte SSH-Konfigurationsdatei verfügen, können Sie sich schnell und sicher an Ihrer Linux-VM anmelden. Beim ersten Anmelden an einem Server mit einem SSH-Schlüssel werden Sie vom Befehl zur Eingabe der Passphrase für die Schlüsseldatei aufgefordert.

`ahmet@fedora$ ssh fedora22`

##### Befehlsbeschreibung

Wenn `ahmet@fedora$ ssh fedora22` ausgeführt wird, ermittelt und lädt SSH zuerst die Einstellungen aus dem Block `Host fedora22` und lädt dann alle restlichen Einstellungen aus dem letzten Block `Host *`.

## Nächste Schritte

Sie können die SSH-Schlüsseldateien jetzt für folgende Zwecke nutzen:

- [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Erstellen einer geschützten Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md)
- [Erstellen einer geschützten Linux-VM mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md)

<!---HONumber=AcomDC_0420_2016-->