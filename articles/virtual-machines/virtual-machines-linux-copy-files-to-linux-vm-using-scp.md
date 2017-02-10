---
title: "Verschieben von Dateien für eine Linux-VM per SCP | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Dateien mit SCP und einem SSH-Schlüsselpaar auf sichere Weise auf eine bzw. von einer Linux-VM verschieben."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 5ca865475b82b73e5c7e1ab21f098a87548605d9
ms.openlocfilehash: fe07ea13f202f19e399a8ede163fcba932490418


---

# <a name="moving-files-to-and-from-a-linux-vm-using-scp"></a>Verschieben von Dateien für eine Linux-VM per SCP

In diesem Artikel wird gezeigt, wie Sie Dateien per Secure Copy (SCP) von Ihrer Arbeitsstation auf eine Azure Linux-VM oder von einer Azure Linux-VM auf die Arbeitsstation verschieben.  Als Beispiel verschieben wir Azure-Konfigurationsdateien auf eine Linux-VM und ein Protokolldateiverzeichnis von der Linux-VM – jeweils mithilfe von SCP und SSH-Schlüsseln.   

Für diesen Artikel benötigen Sie Folgendes:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Schnellbefehle

Kopieren einer Datei auf die Linux-VM

```bash
scp file user@host:directory/targetfile
```

Kopieren einer Datei von der Linux-VM

```bash
scp user@host:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Das schnelle und sichere Verschieben von Dateien zwischen Ihrer Arbeitsstation und einer Linux-VM ist ein wichtiger Teil der Verwaltung Ihrer Azure-Infrastruktur.  In diesem Artikel wird die Verwendung von SCP beschrieben. Das Tool basiert auf SSH und ist in der Bash-Standardshell von Linux, Mac und Windows enthalten.

## <a name="ssh-key-pair-authentication"></a>Authentifizierung per SSH-Schlüsselpaar

SCP nutzt SSH für die Transportschicht.  Indem SSH für den Transport verwendet wird, übernimmt SSH die Authentifizierung auf dem Zielhost, und gleichzeitig wird die Datei über einen verschlüsselten Tunnel verschoben, der für SSH standardmäßig bereitgestellt wird.  Für die SSH-Authentifizierung können Benutzernamen und Kennwörter verwendet werden, aber als bewährte Sicherheitsmethode wird dringend die SSH-Authentifizierung mit öffentlichem und privatem Schlüssel empfohlen. Nachdem die Verbindung von SSH authentifiziert wurde, beginnt SCP mit dem Kopieren der Datei.  Indem eine richtig konfigurierte Datei `~/.ssh/config` und öffentliche und private SSH-Schlüssel verwendet werden, kann die SCP-Verbindung nur mit einem Servernamen hergestellt werden (also ohne Benutzername).  Falls Sie nur über einen SSH-Schlüssel verfügen, sucht SCP im Verzeichnis `~/.ssh/` danach und verwendet ihn standardmäßig zum Anmelden an der VM.

Weitere Informationen zum Konfigurieren der Datei `~/.ssh/config` und zu öffentlichen und privaten SSH-Schlüsseln finden Sie im Artikel [Erstellen von SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-to-a-linux-vm"></a>Verschieben einer Datei per SCP auf eine Linux-VM

Für das erste Beispiel kopieren wir eine Datei mit Azure-Anmeldeinformationen auf eine Linux-VM, die zum Bereitstellen von Automation verwendet wird.  Da diese Datei Azure API-Anmeldeinformationen enthält, zu denen auch Geheimnisse gehören, ist die Sicherheit wichtig. Mit dem bereitgestellten verschlüsselten Tunnel von SSH wird der Inhalt der Datei geschützt.

```bash
scp ~/.azure/credentials myserver:/home/ahmet/.azure/credentials
```

## <a name="scp-a-directory-from-a-linux-vm"></a>Verschieben eines Verzeichnisses von einer Linux-VM per SCP

In diesem Beispiel kopieren wir ein Verzeichnis mit Protokolldateien von der Linux-VM auf Ihre Arbeitsstation.  Eine Protokolldatei kann unter Umständen sensible Daten enthalten, und durch die Verwendung von SCP wird sichergestellt, dass der Inhalt der Protokolldateien verschlüsselt ist.  Die Verwendung von SCP zum geschützten Übertragen der Dateien ist der einfachste und gleichzeitig ein sicherer Weg zur Übertragung des Protokollverzeichnisses und der Dateien auf Ihre Arbeitsstation.

```bash
scp -r myserver:/home/ahmet/logs/ /tmp/.
```

Mit dem CLI-Flag `-r` wird SCP angewiesen, die Dateien und Verzeichnisse rekursiv von dem Punkt des Verzeichnisses zu kopieren, der im Befehl angegeben ist.  Beachten Sie auch, dass die Befehlszeilensyntax dem Kopierbefehl `cp` ähnelt.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf Azure-Linux-VMs mit der VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Deaktivieren von SSH-Kennwörtern auf Ihrer Linux-VM durch Konfigurieren von SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Dec16_HO3-->


