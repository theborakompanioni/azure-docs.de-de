---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 2: Azure-Tools (macOS) | Microsoft-Dokumentation"
description: Installieren Sie unter macOS Python und die Azure-Befehlszeilenschnittstelle (Azure CLI).
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Clouddienst, Azure-CLI
ms.assetid: f2d7d584-7734-401c-976c-81788a7282a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: f0615f10adaf3bd03bc9a3b446830f2906875a89
ms.lasthandoff: 01/24/2017


---
# <a name="get-azure-tools-macos-1010"></a>Abrufen der Azure-Tools (macOS 10.10)
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
* Ein Mac mit Internetverbindung
* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.

## <a name="install-python"></a>Installieren von Python
Obwohl Python 2.7 bereits in macOS enthalten ist, wird empfohlen, Python über Homebrew zu installieren. Siehe [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/) (Installieren von Python unter macOS, in englischer Sprache).

Installieren Sie Python und pip, indem Sie den folgenden Befehl ausführen:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle
Die Azure-CLI bietet eine plattformübergreifende Befehlszeile für Azure. So können Sie Ressourcen direkt über die Befehlszeile bereitstellen und verwalten. 

Führen Sie zum Installieren der aktuellen Azure-CLI die folgenden Schritte aus:

1. Führen Sie die folgenden Befehle in einem Terminalfenster aus. Das Installieren der Azure-CLI kann fünf Minuten dauern.

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

   ```bash
   az iot -h
   ```

Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.

![Ausgabe, die für eine erfolgreiche Installation steht](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-CLI installiert. Die nächste Aufgabe besteht in der Erstellung des Azure IoT-Hubs und der Geräteidentität mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen eines IoT-Hubs und Registrieren des Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)


