---
title: "Simuliertes Gerät und Azure IoT Gateway – Lektion 2: Abrufen der Tools (macOS) | Microsoft-Dokumentation"
description: "Installieren Sie die Tools auf Ihrem Mac-Computer, erstellen Sie eine IoT Hub-Instanz, und registrieren Sie Ihr Gerät bei IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IoT-Entwicklung, IoT-Software, IoT-Clouddienst, Internet der Dinge Software, Azure-CLI, Python installieren Mac, Git auf Mac installieren, Gulp ausführen, Node.js installieren Mac"
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 8b79b166fb16d6454fde46996e715779bf19eac7


---
# <a name="get-the-tools-macos"></a>Herunterladen der Tools (macOS)
> [!div class="op_single_selector"]
> * [Windows 7 oder höher](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Aufgaben

- Installieren von Git, Node.js, Gulp und Python
- Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI). 

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

In dieser Lektion lernen Sie Folgendes:

- Installieren von [Git](https://git-scm.com/) und [Node.js](https://nodejs.org/en/).
  - Git ist ein Open-Source-System zur verteilten Versionskontrolle. Die Beispielanwendung für diese Lektion ist auf Git gespeichert.
  - Node.js ist eine JavaScript-Runtime mit einer umfangreichen Paketumgebung.
- Verwenden von [NPM](https://www.npmjs.com/) zum Installieren von Node.js-Entwicklungstools.
  - Die erforderliche Minimalversion von Node.js ist 4.5 LTS.
  - NPM ist einer der Paket-Manager für Node.js.
- Installieren von Visual Studio Code
  - Visual Studio Code ist ein einfacher, aber leistungsstarker, plattformübergreifender Quellcode-Editor für Windows, Linux und macOS. Er verfügt über hervorragende Unterstützung für das Debuggen, eingebettete Git-Steuerung, Syntaxhervorhebung, intelligente Codevervollständigung, Codeausschnitte und die Umgestaltung von Code.
- Installieren von Python
  - Python ist eine weit verbreitete, interpretierte und dynamische Programmiersprache für allgemeine Aufgaben auf hoher Ebene.
- Installieren der Azure-Befehlszeilenschnittstelle
  - Die Azure-CLI bietet eine plattformübergreifende Befehlszeile für Azure. So können Sie Ressourcen direkt über die Befehlszeile bereitstellen und verwalten.
- Erstellen einer IoT Hub-Instanz mithilfe der Azure-Befehlszeilenschnittstelle

## <a name="what-you-need"></a>Erforderliches Element

- Eine Internetverbindung zum Herunterladen der Tools und der Software
- Ein Mac-Computer, auf dem OS X Yosemite (10.10) oder höher ausgeführt wird

## <a name="install-git-and-nodejs"></a>Installieren von Git und Node.js

Verwenden Sie zum Installieren von Git und Node.js das Paketverwaltungs-Hilfsprogramm Homebrew. Führen Sie dazu diese Schritte aus:

1. [Laden](http://brew.sh/) Sie Homebrew herunter, und installieren Sie es. Wenn Sie Homebrew bereits installiert haben, fahren Sie mit Schritt 2 fort.
   1. Drücken Sie `Cmd + Space`, und geben Sie `Terminal` ein, um ein Terminal zu öffnen.
   2. Führen Sie den folgenden Befehl aus:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. Installieren Sie Git und Node.js, indem Sie den folgenden Befehl ausführen:

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a>Installieren von Node.js-Entwicklungstools

Verwenden Sie [gulp.js](http://gulpjs.com/), um die Bereitstellung und Ausführung von Skripts zu automatisieren.

Führen Sie den folgenden Befehl im Terminal aus, um Gulp zu installieren:

```bash
npm install -g gulp
```

Wenn bei der Installation Probleme auftreten, helfen Ihnen die Lösungen für häufiger vorkommende Probleme in der [Anleitung zur Problembehandlung](iot-hub-gateway-kit-c-sim-troubleshooting.md) weiter.

> [!Note]
> Node, NPM und Gulp sind für die Ausführung von Automatisierungsskripts, die mit Node.js entwickelt wurden, erforderlich.

## <a name="install-python"></a>Installieren von Python

Obwohl Python 2.7 bereits in Mac OS X enthalten ist, wird empfohlen, Python über Homebrew zu installieren. Siehe [Installieren von Python unter Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).

Installieren Sie Python und pip, indem Sie den folgenden Befehl ausführen:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle

Führen Sie zum Installieren der Azure-CLI die folgenden Schritte aus:

1. Führen Sie die folgenden Befehle im Terminal aus:
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   Die Installation kann 5 Minuten dauern.

2. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:
   ```bash
   az iot -h
   ```
   Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.

   ![Überprüfen der Azure CLI-Installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Sie verwenden später in diesem Tutorial Visual Studio Code, um Konfigurationsdateien zu bearbeiten.

[Laden](https://code.visualstudio.com/docs/setup/osx) Sie Visual Studio Code herunter, und installieren Sie die Software.

## <a name="summary"></a>Zusammenfassung

Sie haben die erforderlichen Tools und Software auf Ihrem Mac-Computer installiert. Als Nächstes verwenden Sie die Azure-Befehlszeilenschnittstelle zum Erstellen einer IoT Hub-Instanz und zum Registrieren Ihres Geräts bei IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer IoT Hub-Instanz und Registrieren des Geräts](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)



<!--HONumber=Jan17_HO4-->


