---
title: "Verbinden von Intel Edison (C) mit Azure IoT – Lektion 2: Azure-Tools (macOS) | Microsoft-Dokumentation"
description: Installieren Sie unter macOS Python und die Azure-Befehlszeilenschnittstelle (Azure CLI).
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure CLI, IoT-Clouddienst, Arduino Cloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: d561680f-69cc-427a-820d-24f710ba05a8
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: c79f1595c0e1359044c8c3ec3b6e8bc87f4aa4fa
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---
<a id="get-azure-tools-macos-1010" class="xliff"></a>

# Abrufen der Azure-Tools (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 und höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI). Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Installieren der Azure-Befehlszeilenschnittstelle
* Hinzufügen einer IoT-Untergruppe zur Azure-CLI

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element
* Ein Mac mit Internetverbindung
* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.

<a id="install-python" class="xliff"></a>

## Installieren von Python
Obwohl Python 2.7 bereits in macOS enthalten ist, wird empfohlen, Python über Homebrew zu installieren. Siehe [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/) (Installieren von Python unter macOS, in englischer Sprache).

Installieren Sie Python und pip, indem Sie den folgenden Befehl ausführen:

```bash
brew install python
```

<a id="install-the-azure-cli" class="xliff"></a>

## Installieren der Azure-Befehlszeilenschnittstelle
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

![Ausgabe, die für eine erfolgreiche Installation steht](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

<a id="summary" class="xliff"></a>

## Zusammenfassung
Sie haben die Azure-CLI installiert. Die nächste Aufgabe besteht in der Erstellung des Azure IoT-Hubs und der Geräteidentität mithilfe der Azure-CLI.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Erstellen der IoT Hub-Instanz und Registrieren des Intel Edison][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md

