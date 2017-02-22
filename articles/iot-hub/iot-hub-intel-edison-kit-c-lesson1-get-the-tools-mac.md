---
title: "Verbinden von Intel Edison (C) mit Azure IoT – Lektion 1: Abrufen der Tools (macOS) | Microsoft-Dokumentation"
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für Edison auf macOS herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Arduino-Entwicklungstools, IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git auf mac installieren, Node.js installieren mac
ms.assetid: 3f764f2e-25fa-4dde-9e8d-d278453fccfd
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: 6bfefbdb7728d375ea05a17978ece84aaf8e2821


---
# <a name="get-the-tools-macos-1010"></a>Herunterladen der Tools (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 oder höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für Ihren Intel Edison herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

> [!NOTE]
> Obwohl die Programmiersprache der Hauptlogik C ist, werden in den Lektionen Node.js-Tools zum Erstellen und Bereitstellen der Beispielanwendungen verwendet.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Installieren von Git und Node.js
  * [Git](https://git-scm.com) ist ein Open-Source-System zur verteilten Versionskontrolle. Die Beispielanwendung für diesen Artikel ist auf Git gespeichert.
  * [Node.js](https://nodejs.org/en/) ist eine JavaScript-Runtime mit einer umfangreichen Paketumgebung.
* Verwenden von NPM zum Installieren zusätzlicher Node.js-Entwicklungstools
  * Die erforderliche Minimalversion von Node.js ist 4.5 LTS.
  * [NPM](https://www.npmjs.com) ist einer der Paket-Manager für Node.js.

## <a name="what-you-need"></a>Erforderliches Element
Um diesen Vorgang abzuschließen, benötigen Sie Folgendes:
* Eine Internetverbindung zum Herunterladen der Entwicklungstools und der Software
* Einen Mac, auf dem macOS Yosemite (10.10) oder höher ausgeführt wird

## <a name="install-git-and-nodejs"></a>Installieren von Git und Node.js
Verwenden Sie zum Installieren von Git und Node.js das [Homebrew](http://brew.sh) Paketverwaltungs-Hilfsprogramm. Führen Sie dazu diese Schritte aus:

1. Installieren Sie Homebrew. Wenn Sie Homebrew bereits installiert haben, gehen Sie zu Schritt 2.

   1. Drücken Sie `Cmd + Space`, und geben Sie `Terminal` ein, um ein Terminal zu öffnen.
   2. Führen Sie den folgenden Befehl aus:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Installieren Sie Git und Node.js, indem Sie den folgenden Befehl ausführen:

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>Installieren zusätzlicher Node.js-Entwicklungstools
Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf Ihrem Edison [gulp.js](http://gulpjs.com).

Installieren Sie `gulp`, indem Sie den folgenden Befehl im Terminal ausführen:

```bash
sudo npm install -g gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Entwicklungstools unter macOS Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung][troubleshooting] Lösungen für häufiger auftretende Probleme.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code
[Laden](https://code.visualstudio.com/docs/setup/osx) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

## <a name="summary"></a>Zusammenfassung
Sie haben die erforderlichen Entwicklungstools und die Software für die erste Beispielanwendung installiert. Die nächste Aufgabe besteht im Erstellen, Bereitstellen und Ausführen der Beispielanwendung auf dem Edison.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen und Bereitstellen der Blinkanwendung][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md



<!--HONumber=Jan17_HO4-->


