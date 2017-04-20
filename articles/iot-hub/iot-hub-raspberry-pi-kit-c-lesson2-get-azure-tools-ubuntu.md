---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 2: Azure-Tools (Ubuntu) | Microsoft-Dokumentation"
description: Installieren Sie unter Ubuntu Python und die Azure-Befehlszeilenschnittstelle (Azure CLI).
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Clouddienst, Azure-CLI
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a03512f2-fabe-40c5-8505-b93eef8e5bec
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 4c31bb14f2b32d71f14a89576922b9701b226e66
ms.lasthandoff: 01/24/2017


---
# <a name="get-azure-tools-ubuntu-1604"></a>Abrufen der Azure-Tools (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 und höher](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Aufgaben
Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI). Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Installieren der Azure-Befehlszeilenschnittstelle
* Hinzufügen einer IoT-Untergruppe zur Azure-CLI

## <a name="what-you-need"></a>Erforderliches Element
* Ein Ubuntu-Computer mit Internetverbindung
* Ein aktives Azure-Abonnement. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.

## <a name="install-the-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle
Die Azure-CLI bietet eine plattformübergreifende Befehlszeile für Azure, sodass Sie Ressourcen direkt über die Befehlszeile bereitstellen und verwalten können.

Führen Sie zum Installieren der aktuellen Azure-CLI die folgenden Schritte aus:

1. Führen Sie die folgenden Befehle in einem Terminalfenster aus. Das Installieren der Azure-CLI kann fünf Minuten dauern.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

   ```bash
   az iot -h
   ```

Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.

![Ausgabe, die für eine erfolgreiche Installation steht](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-CLI installiert. Die nächste Aufgabe besteht in der Erstellung des Azure IoT-Hubs und der Geräteidentität mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen eines IoT-Hubs und Registrieren des Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)


