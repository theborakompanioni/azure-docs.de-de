---
title: Verwenden von Stammberechtigungen auf virtuellen Linux-Computern | Microsoft Docs
description: Erfahren Sie, wie Sie Stammberechtigungen auf einem virtuellen Linux-Computer in Azure verwenden.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: a2c106a2-dceb-43a3-9dd1-50ed77685952
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: dc39db1f5fecffb60499a5420bfe72850e2fffd9
ms.lasthandoff: 04/03/2017


---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Verwenden von Stammberechtigungen auf virtuellen Linux-Computern in Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Standardmäßig ist der Benutzer `root` auf virtuellen Linux-Computern in Azure deaktiviert. Benutzer können Befehle mit dem Befehl `sudo` mit erweiterten Rechten ausführen. Das Verhalten kann je nachdem, wie das System bereitgestellt wurde, unterschiedlich sein.

1. **SSH-Schlüssel und Kennwort ODER nur Kennwort** – Der virtuelle Computer wurde mit einem Zertifikat (`.CER`-Datei) oder einem SSH-Schlüssel sowie einem Kennwort oder nur mit einem Benutzernamen und einem Kennwort bereitgestellt. In diesem Fall fordert `sudo` den Benutzer zur Eingabe des Kennworts auf, bevor der Befehl ausgeführt wird.
2. **Nur SSH-Schlüssel** – Der virtuelle Computer wurde mit einem Zertifikat (`.cer`-, `.pem`- oder `.pub`-Datei) oder einem SSH-Schlüssel bereitgestellt, jedoch ohne Kennwort.  In diesem Fall fordert `sudo` **nicht** zur Eingabe des Kennworts auf, bevor der Befehl ausgeführt wird.

## <a name="ssh-key-and-password-or-password-only"></a>SSH-Schlüssel und Kennwort oder nur Kennwort
Melden Sie sich beim virtuellen Linux-Computer mit der SSH-Schlüssel- oder Kennwortauthentifizierung an, und führen Sie dann mit `sudo` Befehle aus. Beispiel:

    # sudo <command>
    [sudo] password for azureuser:

In diesem Fall wird der Benutzer zur Eingabe eines Kennworts aufgefordert. Nachdem das Kennwort eingegeben wurde, führt `sudo` den Befehl mit `root`-Berechtigungen aus.

Sie können sudo auch ohne Kennwort aktivieren, indem Sie die Datei `/etc/sudoers.d/waagent` bearbeiten, zum Beispiel:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Diese Änderung ermöglicht das Ausführen von sudo ohne Kennwort durch den Benutzer "azureuser".

## <a name="ssh-key-only"></a>Nur SSH-Schlüssel
Melden Sie sich beim virtuellen Linux-Computer mit dem SSH-Schlüssel an, und führen Sie dann mit `sudo` Befehle aus. Beispiel:

    # sudo <command>

In diesem Fall wird der Benutzer **nicht** zur Eingabe eines Kennworts aufgefordert. Nachdem Sie `<enter>` gedrückt haben, wird der Befehl von `sudo` mit `root`-Berechtigungen ausgeführt.


