---
title: "Deaktivieren von SSH-Kennwörtern auf Ihrer Linux-VM durch Konfigurieren von SSHD | Microsoft Docs"
description: "Sichern Sie Ihre Linux-VM unter Azure durch Deaktivieren von Kennwortanmeldungen für SSH."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 46137640-a7d2-40e5-a1e9-9effef7eb190
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/26/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: dc45a1cdce29cef061acc5c7e5b15d9d89265cd9
ms.lasthandoff: 04/03/2017


---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Deaktivieren von SSH-Kennwörtern auf Ihrer Linux-VM durch Konfigurieren von SSHD
Dieser Artikel konzentriert sich auf das Festlegen der Anmeldesicherheit Ihrer Linux-VM.  Sobald der SSH-Port 22 geöffnet ist, beginnen Bots, Kennwörter zu erraten.  In diesem Artikel lernen Sie, Kennwortanmeldungen über SSH zu deaktivieren.  Indem wir die Möglichkeit, Kennwörter zu verwenden, vollständig unterbinden, schützen wir die Linux-VM vor dieser Form eines Brute-Force-Angriffs.  Zusätzlicher Bonus: Wir konfigurieren Linux-SSHD so, dass nur Anmeldungen über öffentliche und private SSH-Schlüssel möglich sind – mit Abstand die sicherste Möglichkeit für die Anmeldung bei Linux.  Aufgrund der Vielzahl möglicher Kombinationen erfordert das Erraten des privaten Schlüssels immensen Aufwand, und so werden Bots von vornherein entmutigt, auch nur zu versuchen, mit Brute-Force-Angriffen SSH-Schlüssel abzufragen.

## <a name="goals"></a>Ziele
* Konfigurieren von SSHD, um Folgendes zu unterbinden:
  * Anmeldungen mit Kennwort
  * Anmeldungen von Root-Benutzern
  * Abfrage/Rückmeldung-Authentifizierung
* Konfigurieren von SSHD, um Folgendes zu erlauben:
  * Nur SSH-Schlüsselanmeldungen
* Neustarten von SSHD, während Sie weiterhin angemeldet sind
* Testen der neuen SSHD-Konfiguration

## <a name="introduction"></a>Einführung
[SSH definiert](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD ist der SSH-Server, der auf der Linux-VM ausgeführt wird.  SSH ist ein Client, der von der Shell auf Ihrer MacBook- oder Linux-Arbeitsstation aus ausgeführt wird.  SSH ist auch das Protokoll, das zum Sichern und Verschlüsseln der Kommunikation zwischen Ihrer Arbeitsstation und der Linux-VM verwendet wird.

Für diesen Artikel ist es sehr wichtig, dass Sie eine Anmeldung bei Ihrer Linux-VM für die gesamte Vorgehensweise offen halten.  Aus diesem Grund werden wir zwei Terminals und eine SSH-Verbindung mit der Linux-VM für beide öffnen.  Wir verwenden das erste Terminal, um die Konfigurationsdatei der SSHDs zu ändern und den SSHD-Dienst neu zu starten.  Wir verwenden das zweite Terminal, um diese Änderungen zu testen, sobald der Dienst neu gestartet wurde.  Da wir SSH-Kennwörter deaktivieren und uns strikt auf SSH-Schlüssel verlassen, wird die VM dauerhaft gesperrt, wenn Ihre SSH-Schlüssel nicht korrekt sind, und Sie die Verbindung mit der VM schließen, und niemand wird in der Lage sein, sich bei ihr anzumelden. Sie muss dann also gelöscht und neu erstellt werden.

## <a name="prerequisites"></a>Voraussetzungen
* [Erstellen von SSH-Schlüsseln unter Linux und Mac für Linux-VMs in Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Azure-Konto
  * [Anmeldung für kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)
  * [Azure-Portal](http://portal.azure.com)
* Linux-VM unter Azure
* Öffentliches und privates SSH-Schlüsselpaar in `~/.ssh/`
* Öffentlicher SSH-Schlüssel in `~/.ssh/authorized_keys` auf Linux-VM
* Sudo-Rechte auf der VM
* Port 22 offen

## <a name="quick-commands"></a>Schnellbefehle
*Erfahrene Linux-Administratoren, die nur die TLDR-Version wünschen, beginnen hier.  Alle anderen Benutzer, die ausführliche Erläuterungen und eine exemplarische Vorgehensweise wünschen, überspringen diesen Abschnitt.*

```bash
sudo vim /etc/ssh/sshd_config
```

Bearbeiten Sie die config-Datei wie folgt:

```sh
# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

Starten Sie den SSHD-Dienst neu. In Debian-basierten Distributionen:

```bash
sudo service ssh restart
```

In Red Hat-basierten Distributionen:

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Ausführliche exemplarische Vorgehensweise
Melden Sie sich auf Terminal 1 (T1) bei der Linux-VM an.  Melden Sie sich auf Terminal 2 (T2) bei der Linux-VM an.

Auf T2 werden wir die SSHD-Konfigurationsdatei bearbeiten.  

```bash
sudo vim /etc/ssh/sshd_config
```

Von hier aus bearbeiten wir nur die Einstellungen zum Deaktivieren von Kennwörtern und Aktivieren von SSH-Schlüsselanmeldungen.  Es gibt viele Einstellungen in der Datei, die Sie untersuchen sollten, und ändern, um Linux und SSH so sicher wie nötig zu machen.

#### <a name="disable-password-logins"></a>Deaktivieren von Anmeldungen mit Kennwort

```sh
# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Aktivieren der Authentifizierung mit dem öffentlichen Schlüssel

```sh
# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Deaktivieren der Root-Anmeldung

```sh
# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Deaktivieren der Abfrage/Rückmeldung-Authentifizierung
```sh
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Neustarten von SSHD
Überprüfen Sie von der T1-Shell aus, ob Sie immer noch angemeldet sind.  Dies ist wichtig, damit Sie nicht von Ihrer VM ausgesperrt werden, wenn Ihre SSH-Schlüssel nicht korrekt sind, da Kennwörter jetzt deaktiviert sind.  Wenn eine Einstellung auf Ihrer Linux-VM falsch ist, können Sie „sshd_config“ mit T1 korrigieren, da Sie immer noch angemeldet sind, und SSH die Verbindung aufrechterhält, während der SSHD-Dienst neu startet.

Von T2 ausführen:

##### <a name="on-the-debian-family"></a>Auf der Debian-Familie
```bash
sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Auf der RedHat-Familie
```bash
sudo service sshd restart
```

Kennwörter sind jetzt auf Ihrer VM deaktiviert, sodass sie vor Brute-Force-Kennwortanmeldeversuchen geschützt ist.  Da nur SSH-Schlüssel zulässig sind, können Sie sich schneller und viel sicherer anmelden.


