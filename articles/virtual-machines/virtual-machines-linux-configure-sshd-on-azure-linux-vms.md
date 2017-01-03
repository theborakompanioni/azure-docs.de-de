---
title: Konfigurieren von SSHD auf virtuellen Azure Linux-Computern | Microsoft Docs
description: "Konfigurieren Sie SSHD für bewährte Sicherheitsmethoden, oder um SSH auf virtuellen Azure Linux-Computern zu sperren."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: fa6c9b6b60aef689568c58699eeb89d14b1ae2bf
ms.openlocfilehash: 73c47bcfbe018947930bb23fbbcc201b91a49944


---

# <a name="configure-sshd-on-azure-linux-vms"></a>Konfigurieren von SSHD auf virtuellen Azure Linux-Computern

In diesem Artikel wird beschrieben, wie der SSH-Server unter Linux gesperrt wird, um bewährte Sicherheitsmethoden bereitzustellen und den SSH-Anmeldevorgang mithilfe von SSH-Schlüsseln statt mit Kennwörtern zu beschleunigen.  Um SSHD zu sperren, werden wir dafür sorgen, das sich der Root-Benutzer nicht anmelden kann. Wir werden die Anzahl der Benutzer begrenzen, die sich über eine genehmigte Gruppenliste anmelden kann. Hierzu deaktivieren wir die SSH-Protokollversion 1, legen eine Mindestanzahl für Schlüsselbits fest und konfigurieren die automatische Abmeldung von inaktiven Benutzern.  Für diesen Artikel gelten folgende Voraussetzungen: ein Azure-Konto ([testen Sie es kostenlos](https://azure.microsoft.com/pricing/free-trial/)) sowie [Dateien mit öffentlichen und privaten SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Schnellbefehle

Konfigurieren Sie `/etc/ssh/sshd_config` mit den folgenden Einstellungen:

### <a name="disable-password-logins"></a>Deaktivieren von Anmeldungen mit Kennwort

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-the-root-user"></a>Deaktivieren der Anmeldung durch den Root-Benutzer

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a>Liste mit zulässigen Gruppen

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a>Liste mit zulässigen Benutzern

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a>Deaktivieren der SSH-Protokollversion 1

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a>Mindestanzahl für Schlüsselbits

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a>Trennen von inaktiven Benutzern

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

SSHD ist der SSH-Server, der auf der Linux-VM ausgeführt wird.  SSH ist ein Client, der von der Shell auf Ihrer MacBook- oder Linux-Arbeitsstation oder von einem Bash unter Windows aus ausgeführt wird.  SSH ist auch das Protokoll, das zum Schützen und Verschlüsseln der Kommunikation zwischen Ihrer Arbeitsstation und der Linux-VM verwendet wird. Dadurch wird SSH zu einem virtuellen privaten Netzwerk (VPN, Virtual Private Network).

Für diesen Artikel ist es sehr wichtig, dass Sie eine Anmeldung bei Ihrer Linux-VM für die gesamte Vorgehensweise offen halten.  Sobald eine SSH-Verbindung hergestellt ist, bleibt diese Sitzung offen, bis das Fenster geschlossen wird.  Wenn ein Terminal angemeldet ist, können Sie am SSHD-Dienst Änderungen vornehmen, ohne dass Sie beim Durchführen einer grundlegenden Änderung gesperrt werden.  Wenn Sie für Ihre Linux-VM aufgrund einer fehlerhaften SSHD-Konfiguration gesperrt werden, bietet Azure die Möglichkeit, die Konfiguration mit der [Azure-VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) zurückzusetzen.

Aus diesem Grund werden wir zwei Terminals und eine SSH-Verbindung mit der Linux-VM für beide öffnen.  Wir verwenden das erste Terminal, um die Konfigurationsdatei der SSHDs zu ändern und den SSHD-Dienst neu zu starten.  Wir verwenden das zweite Terminal, um diese Änderungen zu testen, sobald der Dienst neu gestartet wurde.  Da wir SSH-Kennwörter deaktivieren und uns strikt auf SSH-Schlüssel verlassen, wird die VM dauerhaft gesperrt, wenn Ihre SSH-Schlüssel nicht korrekt sind, und Sie die Verbindung mit der VM schließen, und niemand wird in der Lage sein, sich bei ihr anzumelden. Sie muss dann also gelöscht und neu erstellt werden.

## <a name="disable-password-logins"></a>Deaktivieren von Anmeldungen mit Kennwort

Die schnellste Möglichkeit zum Schutz der Linux-VM ist die Deaktivierung der Anmeldung mit Kennwort.  Wenn Anmeldungen mit Kennwort aktiviert werden, beginnen Bots, die das Web durchsuchen, sofort damit, das Kennwort für die Linux-VM, die SSH verwendet, mittels Brute-Force zu erraten.  Wenn Anmeldungen mit Kennwort vollständig deaktiviert werden, kann der SSH-Server alle Anmeldeversuche mit Kennwort ignorieren.

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-the-root-user"></a>Deaktivieren der Anmeldung durch den Root-Benutzer

Gemäß den bewährten Methoden von Linux darf sich der `root`-Benutzer nicht über SSH oder mithilfe von `sudo su` anmelden.  Alle Befehle, für die Berechtigungen auf Stammebene erforderlich sind, müssen immer über den Befehl `sudo` ausgeführt werden. Damit werden alle Aktionen für die spätere Überwachung aufgezeichnet.  Zu verhindern, dass sich der `root`-Benutzer mittels SSH anmelden kann, ist eine bewährte Sicherheitsmethode, mit der sichergestellt wird, dass nur autorisierte Benutzer auf SSH zugreifen können.

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a>Liste mit zulässigen Gruppen

SSH bietet eine Möglichkeit, die Benutzer und Gruppen einzuschränken, die sich über SSH anmelden dürfen.  Für diese Funktion werden Listen verwendet, um bestimmten Benutzern und Gruppen die Anmeldung zu erlauben bzw. zu verweigern.  Wenn die Gruppe namens „wheel“ in die `AllowGroups`-Liste aufgenommen wird, werden genehmigte Anmeldungen über SSH auf Benutzerkonten beschränkt, die sich in der wheel-Gruppe befinden.

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a>Liste mit zulässigen Benutzern

SSH-Anmeldungen auf Benutzer zu beschränken, ist im Vergleich zu `AllowGroups` eine speziellere Methode, dieselbe Aufgabe durchzuführen.  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a>Deaktivieren der SSH-Protokollversion 1

SSH-Protokollversion 1 ist unsicher und sollte deaktiviert werden.  SSH-Protokollversion 2 ist die aktuelle Version, die eine sichere Methode zum Herstellen einer SSH-Verbindung mit Ihrem Server bietet.  Wenn SSH-Version 1 deaktiviert wird, werden SSH-Clients abgewiesen, die versuchen, eine Verbindung mit einem SSH-Server mithilfe von SSH-Version 1 herzustellen.  Nur über SSH-Version 2 können Verbindungen mit dem SSH-Server ausgehandelt werden.

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a>Mindestanzahl für Schlüsselbits

Gemäß bewährten Sicherheitsmethoden sollen SSH-Anmeldungen mit Kennwort deaktiviert werden und nur SSH-Schlüssel sollen zur Authentifizierung beim SSH-Server verwendet werden dürfen.  Diese SSH-Schlüssel können mit unterschiedlicher Länge erstellt werden, die in Bit gemessen wird.  Bewährte Methoden besagen, dass Schlüssel für eine akzeptable Schlüsselstärke mindestens eine Länge von 2048 Bit aufweisen sollten.  Schlüssel mit weniger als 2048 Bit könnten theoretisch entschlüsselt werden.  Wenn `ServerKeyBits` auf `2048` festgelegt wird, können für Verbindungen Schlüssel mit mindestens 2048 Bit verwendet und Verbindungen mit weniger als 2048 Bit abgelehnt werden.

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a>Trennen von inaktiven Benutzern

Mit SSH ist es möglich, Benutzer mit offenen Verbindungen zu trennen, die für eine bestimmte Zeitdauer inaktiv waren, wobei die Zeitdauer in Sekunden angegeben wird.  Dadurch, dass offene Sitzungen nur aktiven Benutzern vorbehalten sind, wird die Linux-VM geschützt.

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a>Neustarten von SSHD

Um die Einstellungen in `/etc/ssh/sshd_config` zu aktivieren, starten Sie den SSHD-Prozess neu. Dadurch wird der SSH-Server neu gestartet.  Das Terminalfenster, das für den Neustart des SSH-Servers verwendet wird, bleibt geöffnet und die offene SSH-Sitzung bleibt erhalten.  Verwenden Sie zum Testen der neuen SSH-Servereinstellungen ein zweites Terminalfenster oder eine zweite Registerkarte.  Wenn Sie zum Testen der SSH-Verbindung ein separates Terminal verwenden, können Sie zurückkehren und an `/etc/ssh/sshd_config` weitere Änderungen im ersten Terminal vornehmen, ohne durch eine grundlegende SSHD-Änderung gesperrt zu werden.  

### <a name="on-redhat-centos-and-fedora"></a>Bei Redhat, Centos und Fedora

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a>Bei Debian und Ubuntu

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a>Zurücksetzen von SSHD mithilfe von Azure reset-access

Wenn Sie aufgrund einer grundlegenden Änderung an der SSHD-Konfiguration gesperrt wurden, können Sie die Azure VMAccess-Erweiterung verwenden, um die SSHD-Konfiguration auf die ursprüngliche Konfiguration zurückzusetzen.

Ersetzen Sie alle Beispielnamen durch Ihre eigenen.

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a>Installieren von Fail2ban

Es wird dringend empfohlen, die Open-Source-App Fail2ban zu installieren und einzurichten. Mit dieser App werden wiederholte Versuche, sich über SSH mittels Brute Force bei Ihrer Linux-VM anzumelden, blockiert.  Mit Fail2ban werden wiederholte, fehlgeschlagene Versuche, sich über SSH anzumelden, protokolliert. Anschließend werden Firewallregeln erstellt, mit denen die IP-Adresse gesperrt wird, von denen die Versuche ausgehen.

* [Fail2ban-Homepage](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den SSH-Server auf Ihrer Linux-VM konfiguriert und gesperrt haben, können Sie noch weitere bewährte Sicherheitsmethoden befolgen.  

* [Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf Azure-Linux-VMs mit der VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-encrypt-disks?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Zugriff und Sicherheit in Azure Resource Manager-Vorlagen](virtual-machines-linux-dotnet-core-3-access-security?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Nov16_HO5-->


