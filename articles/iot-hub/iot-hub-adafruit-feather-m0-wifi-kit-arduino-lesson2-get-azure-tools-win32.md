---
title: "Verbinden von Arduino mit Azure IoT – Lektion 2: Azure-Tools (Windows) | Microsoft-Dokumentation"
description: "Installieren Sie unter Windows 7 und höheren Versionen Python und die Azure-Befehlszeilenschnittstelle (Azure-CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure CLI, IoT-Clouddienst, Arduino Cloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 70dfff14-4be1-468d-9919-e40f4bead308
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 74f4448cbd5348988c185c86cadad5337da3a0da
ms.lasthandoff: 03/10/2017


---
# <a name="get-azure-tools-windows-7-and-later"></a>Abrufen der Azure-Tools (Windows 7 und höher)

> [!div class="op_single_selector"]
> * [Windows 7 oder höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Aufgaben

Installieren von Python und der Azure-Befehlszeilenschnittstelle (Azure-CLI) Wenn Probleme auftreten, suchen Sie auf der [Problembehandlungsseite](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) nach Lösungen für Ihr Arduino-Board Adafruit Feather M0 WiFi.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Installieren von Python
* Installieren der Azure-Befehlszeilenschnittstelle

## <a name="what-you-need"></a>Erforderliches Element
* Ein Windows-Computer mit Internetverbindung
* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.

## <a name="install-python"></a>Installieren von Python
[Installieren Sie Python](https://www.python.org/downloads/) auf dem Windows-Computer. Sie können Python 2.7, 3.4 oder 3.5 installieren. Dieses Tutorial basiert auf Python 2.7. Wenn Sie Python bereits installiert haben, gehen Sie zum nächsten Abschnitt, und installieren Sie die Azure-CLI.

Sie müssen außerdem der `PATH`-Umgebungsvariablen den Pfad der Ordner hinzufügen, in denen „python.exe“ und „pip.exe“ auf dem System installiert sind. Standardmäßig wird „python.exe“ in `C:\Python27` und „pip.exe“ in `C:\Python27\Scripts` installiert.

## <a name="install-the-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle
Die Azure-CLI bietet eine plattformübergreifende Befehlszeile für Azure. So können Sie Ressourcen direkt über die Befehlszeile bereitstellen und verwalten.

Führen Sie zum Installieren der Azure-CLI die folgenden Schritte aus:

1. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.
2. Führen Sie die folgenden Befehle aus:

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

   ```bash
   az iot -h
   ```

Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.

![Ausgabe, die für eine erfolgreiche Installation steht][output]

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-CLI installiert. Die nächste Aufgabe besteht in der Erstellung des Azure IoT-Hubs und der Geräteidentität mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen der IoT Hub-Instanz und Registrieren des Arduino-Boards][create-your-iot-hub-and-register-your-arduino-board]


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_win.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
