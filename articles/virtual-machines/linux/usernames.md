---
title: "Auswählen von Benutzernamen für Linux | Microsoft Docs"
description: "Erfahren Sie, wie Sie Benutzernamen für einen virtuellen Linux-Computer in Azure auswählen."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 1874d72e5f88816036667932371ff28704d186c8
ms.lasthandoff: 04/03/2017


---
# <a name="selecting-user-names-for-linux-on-azure"></a>Auswählen von Benutzernamen für Linux in Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Beim Bereitstellen eines virtuellen Linux-Computers in Azure müssen Sie den Namen eines Benutzers ohne Root-Rechte angeben, den Sie später bei der Anmeldung am virtuellen Computer benutzen können. Sie können den Namen des neuen Benutzers wählen, oder Sie können den Standardnamen „azureuser“ übernehmen, wenn Sie die Bereitstellung über das klassische Azure-Portal durchführen.

In den meisten Fällen ist dieser Benutzer im Basisimage nicht vorhanden und wird während des Bereitstellungsprozesses erstellt. Wenn der Benutzer im VM-Basisimage vorhanden ist, konfiguriert der Azure Linux-Agent in einigen Fällen einfach das Kennwort und/oder den SSH-Schlüssel für diesen Benutzer auf Grundlage der Informationen, die Sie beim Erstellen der VM festgelegt haben.

**Allerdings**definiert Linux einen Satz von Benutzernamen, die nicht verwendet werden sollten. Der Bereitstellungsprozess **schlägt fehl** , wenn Sie versuchen, einen virtuellen Linux-Computer mithilfe eines vorhandenen Systembenutzers bereitzustellen, der als Benutzer mit einer UID zwischen 0 und 99 definiert ist. Ein typisches Beispiel ist der Benutzer `root` , der die UID 0 hat.

* Siehe auch: [Linux-Standardbasis – Benutzer-ID-Bereiche](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Nachfolgend finden Sie eine Liste allgemeiner integrierter Systembenutzer für CentOS und Ubuntu, die Sie bei der Bereitstellung eines virtuellen Linux-Computers in Azure nicht verwenden sollten. Diese Liste ist nur ein Beispiel. Lesen Sie die Dokumentation für Ihre Verteilung, um sicherzustellen, dass der ausgewählte Benutzername nicht in Konflikt mit einem vorhandenen Systembenutzer steht.

## <a name="centos"></a>CentOS
* abrt
* adm
* audio
* bin
* cdrom
* cgred
* daemon
* dbus
* dialout
* dip
* disk
* floppy
* ftp
* ftp
* games
* gopher
* haldaemon
* halt
* kmem
* lock
* lp
* mail
* man
* mem
* nfsnobody
* nobody
* ntp
* operator
* oprofile
* postdrop
* postfix
* qpidd
* root
* rpc
* rpcuser
* saslauth
* shutdown
* slocate
* sshd
* stapdev
* stapusr
* sync
* sys
* tape
* test
* tcpdump
* tty
* users
* utempter
* utmp
* uucp
* vcsa
* video
* wheel

## <a name="ubuntu"></a>Ubuntu
* adm
* admin
* audio
* backup
* bin
* cdrom
* crontab
* daemon
* dialout
* dip
* disk
* fax
* floppy
* fuse
* games
* gnats
* irc
* kmem
* landscape
* libuuid
* list
* lp
* mail
* man
* messagebus
* mlocate
* netdev
* news
* nobody
* nogroup
* operator
* plugdev
* proxy
* root
* sasl
* shadow
* src
* ssh
* sshd
* staff
* sudo
* sync
* sys
* syslog
* tape
* tty
* users
* utmp
* uucp
* video
* voice
* whoopsie
* www-data


