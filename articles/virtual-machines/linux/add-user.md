---
title: "Hinzufügen eines Benutzers zu einer Linux-VM in Azure | Microsoft Docs"
description: "Es wird beschrieben, wie Sie einen Benutzer in Azure einer Linux-VM hinzufügen."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8aa633b-8b75-45d7-b61d-11ac112cedd5
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: v-livech
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: ddc5446fa6a43bfff05ec251b30175ddd24052c9
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="add-a-user-to-an-azure-vm"></a>Hinzufügen eines Benutzers zu einer Azure-VM
Eine der ersten Aufgaben jeder neu gestarteten Linux-VM ist die Erstellung eines neuen Benutzers.  In diesem Artikel sind die Schritte für das Erstellen eines sudo-Benutzerkontos, das Festlegen des Kennworts, das Hinzufügen von öffentlichen SSH-Schlüsseln und das Verwenden von `visudo` für die Nutzung von sudo ohne Kennwort ausführlich beschrieben.

Voraussetzungen: [Ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [öffentliche und private SSH-Schlüssel](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), eine Azure-Ressourcengruppe und die Azure-Befehlszeilenschnittstelle, die installiert und mit `azure config mode arm` in den Azure Resource Manager-Modus geschaltet wurde.

## <a name="quick-commands"></a>Schnellbefehle
```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
### <a name="introduction"></a>Einführung
Eine der ersten und häufigsten Aufgaben bei einem Server ist das Hinzufügen eines Benutzerkontos.  Stammanmeldungen sollten deaktiviert werden, und das Stammkonto selbst sollte nicht mit dem Linux-Server verwendet werden, sondern nur sudo.  Bei richtiger Verwaltung und Nutzung von Linux gewähren Sie einem Benutzer mit sudo Berechtigungen für die Stammeskalation.

Mit dem Befehl `useradd` fügen wir der Linux-VM-Benutzerkonten hinzu.  Ausführen von `useradd` ändert `/etc/passwd`, `/etc/shadow`, `/etc/group` und `/etc/gshadow`.  Wir fügen dem Befehl `useradd` ein Befehlszeilenflag hinzu, um den neuen Benutzer auch der richtigen sudo-Gruppe unter Linux hinzuzufügen.  Mit `useradd` wird zwar ein Eintrag unter `/etc/passwd` erstellt, aber für das neue Benutzerkonto wird kein Kennwort vergeben.  Wir erstellen ein Anfangskennwort für den neuen Benutzer, indem wir den einfachen Befehl `passwd` verwenden.  Im letzten Schritt ändern wir die sudo-Regeln, damit der Benutzer Befehle mit sudo-Berechtigungen ausführen kann, ohne für jeden Befehl ein Kennwort eingeben zu müssen.  Mit der Anmeldung mit dem privaten Schlüssel gehen wir davon aus, dass das Benutzerkonto vor Angreifern sicher ist, und lassen den sudo-Zugriff ohne Kennwort zu.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Hinzufügen eines einzelnen sudo-Benutzers zu einer Azure-VM
Melden Sie sich mit SSH-Schlüsseln bei der Azure-VM an.  Falls Sie den Zugriff mit einem öffentlichen SSH-Schlüssel noch nicht eingerichtet haben, sollten Sie zuerst den Artikel [Using Public Key Authentication with Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)(Verwenden der Authentifizierung mit öffentlichem Schlüssel für Azure) durcharbeiten.  

Mit dem Befehl `useradd` werden die folgenden Aufgaben durchgeführt:

* Erstellen eines neuen Benutzerkontos
* Erstellen einer neuen Benutzergruppe mit dem gleichen Namen
* Einfügen eines leeren Eintrags in `/etc/passwd`
* Einfügen eines leeren Eintrags in `/etc/gpasswd`

Mit dem Befehlszeilenflag `-G` wird das neue Benutzerkonto der richtigen Linux-Gruppe hinzugefügt, und das Konto erhält Berechtigungen für die Stammeskalation.

#### <a name="add-the-user"></a>Hinzufügen des Benutzers
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Festlegen eines Kennworts
Mit dem Befehl `useradd` wird der neue Benutzer erstellt und `/etc/passwd` und `/etc/gpasswd` jeweils ein Eintrag hinzugefügt, aber das Kennwort wird nicht festgelegt.  Das Kennwort wird dem Eintrag mit dem Befehl `passwd` hinzugefügt.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Wir verfügen jetzt über einen Benutzer mit sudo-Berechtigungen auf dem Server.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>Hinzufügen des öffentlichen SSH-Schlüssels zum neuen Benutzerkonto
Verwenden Sie auf Ihrem Computer den Befehl `ssh-copy-id` mit dem neuen Kennwort.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Verwenden von visudo, um die sudo-Nutzung ohne Kennwort zu ermöglichen
Beim Verwenden von `visudo` zum Bearbeiten der Datei `/etc/sudoers` werden einige Schutzebenen festgelegt, um zu verhindern, dass diese wichtige Datei geändert wird.  Wenn `visudo` ausgeführt wird, wird die Datei `/etc/sudoers` gesperrt. So wird sichergestellt, dass während der aktiven Bearbeitung kein anderer Benutzer Änderungen vornehmen kann.  Die Datei `/etc/sudoers` wird von `visudo` auch auf Fehler untersucht, wenn Sie versuchen, sie zu speichern oder zu beenden, sodass Sie keine beschädigte sudo-Datei speichern können.

Es befinden sich bereits Benutzer in der richtigen Standardgruppe für den sudo-Zugriff.  Jetzt aktivieren wir diese Gruppen, um sudo ohne Kennwort zu verwenden.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Überprüfen von Benutzer, SSH-Schlüsseln und sudo
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

