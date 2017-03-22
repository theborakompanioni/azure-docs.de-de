---
title: "Verbinden von Intel Edison (Node) mit Azure IoT – Lektion 2: Azure-Tools (Windows) | Microsoft-Dokumentation"
description: "Installieren Sie unter Windows 7 und höheren Versionen Python und die Azure-Befehlszeilenschnittstelle (Azure-CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure CLI, IoT-Clouddienst, Arduino Cloud
ms.assetid: 60631b54-6d2e-4e8a-88bf-7c2f8e7e1f29
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: 181d7113bda6e9d39bc24c1892940803fc03fd6b
ms.lasthandoff: 01/25/2017


---
# <a name="get-azure-tools-windows-7-and-later"></a>Abrufen der Azure-Tools (Windows 7 und höher)
> [!div class="op_single_selector"]
> * [Windows 7 und höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Aufgaben
Installieren von Python und der Azure-Befehlszeilenschnittstelle (Azure-CLI) Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

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

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

   ```cmd
   az iot -h
   ```

Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.

![Ausgabe, die für eine erfolgreiche Installation steht](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-CLI installiert. Die nächste Aufgabe besteht in der Erstellung des Azure IoT-Hubs und der Geräteidentität mithilfe der Azure-CLI.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen der IoT Hub-Instanz und Registrieren des Intel Edison][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md

