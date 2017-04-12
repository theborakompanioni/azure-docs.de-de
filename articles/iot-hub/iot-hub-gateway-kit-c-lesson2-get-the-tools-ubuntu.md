---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 2: Abrufen der Tools (Ubuntu) | Microsoft-Dokumentation"
description: "Installieren Sie die Tools und die Software auf dem Hostcomputer unter Ubuntu, erstellen Sie eine IoT Hub-Instanz, und registrieren Sie Ihr Gerät bei IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IoT-Entwicklung, IoT-Software, IoT-Clouddienst, Internet der Dinge Software, Azure CLI, Git unter Ubuntu installieren, Gulp ausführen, Node.js installieren Ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0bac1412-385b-4255-a33f-9d44c35feb3e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 6414a4f37367d8acc0bab9356b8a09aadf13bfc9
ms.lasthandoff: 01/25/2017


---
# <a name="get-the-tools-ubuntu-1604"></a>Herunterladen der Tools (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 oder höher](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Aufgaben

- Installieren von Git, Node.js, Gulp und Python
- Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI). 

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).
## <a name="what-you-will-learn"></a>Sie lernen Folgendes

In dieser Lektion lernen Sie Folgendes:

- Installieren von Git und Node.js
  - Git ist ein Open-Source-System für die verteilte Versionskontrolle. Die Beispielanwendung für diese Lektion ist auf Git gespeichert.
  - Node.js ist eine JavaScript-Runtime mit einer umfangreichen Paketumgebung.
- Verwenden von NPM zum Installieren von Node.js-Entwicklungstools.
  - Die erforderliche Minimalversion von Node.js ist 4.5 LTS.
  - NPM ist einer der Paket-Manager für Node.js.
- Installieren von Visual Studio Code
  - Visual Studio Code ist ein einfacher, aber leistungsstarker, plattformübergreifender Quellcode-Editor für Windows, Linux und macOS. Er verfügt über hervorragende Unterstützung für das Debuggen, eingebettete Git-Steuerung, Syntaxhervorhebung, intelligente Codevervollständigung, Codeausschnitte und die Umgestaltung von Code.
- Gewusst wie: Installieren der Azure-Befehlszeilenschnittstelle
  - Die Azure-CLI bietet eine plattformübergreifende Befehlszeile für Azure. So können Sie Ressourcen direkt über die Befehlszeile bereitstellen und verwalten.
- Erstellen einer IoT Hub-Instanz mithilfe der Azure-Befehlszeilenschnittstelle

## <a name="what-you-need"></a>Erforderliches Element

- Eine Internetverbindung zum Herunterladen der Tools und der Software
- Einen Computer mit Ubuntu 16.04 oder höher

## <a name="install-git-and-nodejs"></a>Installieren von Git und Node.js

Gehen Sie zum Installieren von Git oder Node.js wie folgt vor:

1. Drücken Sie `Ctrl + Alt + T`, um ein Terminalfenster zu öffnen.
2. Führen Sie die folgenden Befehle aus:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Installieren von Node.js-Entwicklungstools

Verwenden Sie [gulp.js](http://gulpjs.com/), um die Bereitstellung und Ausführung von Skripts zu automatisieren.

Führen Sie den folgenden Befehl im Terminal aus, um Gulp zu installieren:

```bash
sudo npm install -g gulp
```

Wenn bei der Installation Probleme auftreten, helfen Ihnen die Lösungen für häufiger vorkommende Probleme in der [Anleitung zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md) weiter.

> [!Note]
> Node, NPM und Gulp sind für die Ausführung von Automatisierungsskripts, die mit Node.js entwickelt wurden, erforderlich.

## <a name="install-the-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle

Führen Sie zum Installieren der Azure-CLI die folgenden Schritte aus:

1. Führen Sie die folgenden Befehle im Terminal aus:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   Die Installation kann 5 Minuten dauern.

2. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

   ```bash
   az iot -h
   ```
Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.
![Überprüfen der Azure CLI-Installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Sie verwenden später in diesem Tutorial Visual Studio Code, um Konfigurationsdateien zu bearbeiten.

[Laden](https://code.visualstudio.com/docs/setup/linux) Sie Visual Studio Code herunter, und installieren Sie die Software.

## <a name="summary"></a>Zusammenfassung

Sie haben die erforderlichen Tools und Software auf Ihrem Hostcomputer installiert. Als Nächstes verwenden Sie die Azure-Befehlszeilenschnittstelle zum Erstellen einer IoT Hub-Instanz und zum Registrieren Ihres Geräts bei IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen eines IoT Hub und Registrieren Ihres Geräts](iot-hub-gateway-kit-c-lesson2-register-device.md)

