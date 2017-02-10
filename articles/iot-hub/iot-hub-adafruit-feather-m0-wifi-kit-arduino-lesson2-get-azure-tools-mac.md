---
title: "Abrufen der Azure-Tools für Ihr Azure IoT Starter Kit (macOS 10.10) | Microsoft Docs"
description: Installieren Sie unter macOS Python und die Azure-Befehlszeilenschnittstelle (Azure CLI).
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure CLI, IoT-Clouddienst, Arduino Cloud
ms.assetid: 9b719293-01d2-4a2d-9c49-476e67f4816d
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 9e8084fe26229ef9ed1676c0a3c34e0ee7be88b1
ms.openlocfilehash: de7d031a05110ccbd2d5ab18a7735a91df2bd05e


---
# <a name="get-azure-tools-macos-1010"></a>Abrufen der Azure-Tools (macOS 10.10)

> [!div class="op_single_selector"]
> * [Windows 7 oder höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Aufgaben

Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI). Wenn Probleme auftreten, suchen Sie auf der [Problembehandlungsseite](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) nach Lösungen für Ihr Arduino-Board Adafruit Feather M0 WiFi.

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

![Ausgabe, die für eine erfolgreiche Installation steht][output]

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-CLI installiert. Die nächste Aufgabe besteht in der Erstellung des Azure IoT-Hubs und der Geräteidentität mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen der IoT Hub-Instanz und Registrieren des Arduino-Boards][create-your-iot-hub-and-register-your-arduino-board]


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_osx.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md


<!--HONumber=Dec16_HO2-->


