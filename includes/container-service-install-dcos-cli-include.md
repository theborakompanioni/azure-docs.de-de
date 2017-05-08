---
title: Installieren der DC/OS-CLI | Microsoft Docs
description: Installieren Sie die DC/OS-CLI.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Container, Microservices, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: e664ce9426a2852a35dfdade5d41a9ce8b37a3b7
ms.openlocfilehash: a8ea47f158c0d666340815d2e039995c7483257f


---
> [!NOTE]
> Diese Anleitung ist für die Verwendung von DC/OS-basierten ACS-Clustern bestimmt. Sie ist für Swarm-basierte ACS-Cluster nicht erforderlich.
> 
> 

[Stellen Sie zuerst eine Verbindung mit Ihrem DC/OS-basierten ACS-Cluster her](../articles/container-service/container-service-connect.md). Anschließend können Sie die DC/OS-CLI mit den folgenden Befehlen auf Ihrem Clientcomputer installieren:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Wenn Sie eine alte Version von Python verwenden, kann es sein, dass Warnungen vom Typ „InsecurePlatformWarnings“ auftreten. Sie können dies ignorieren.

Führen Sie Folgendes aus, um zu beginnen, ohne die Shell neu zu starten:

```bash
source ~/.bashrc
```

Dieser Schritt ist nicht erforderlich, wenn Sie neue Shells starten.

Jetzt können Sie überprüfen, ob die CLI installiert wurde:

```bash
dcos --help
```
