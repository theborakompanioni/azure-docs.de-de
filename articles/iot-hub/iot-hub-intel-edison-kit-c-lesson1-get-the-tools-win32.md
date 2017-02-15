---
title: "Abrufen der Tools für Ihr Azure IoT Starter Kit (ab Windows 7) | Microsoft Docs"
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für Edison unter Windows 7 und höheren Versionen herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Arduino-Entwicklungstools, IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git unter Windows installieren, Node.js installieren Windows
ms.assetid: 7d29a358-544d-4657-a504-5ed9b79c2925
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: f45b3bf00d619376ac07418f0c02eca5f3241939
ms.openlocfilehash: 111018da51823f928e7f2b815db8621ecda65025


---
# <a name="get-the-tools-windows-7-or-later"></a>Herunterladen der Tools (Windows 7 oder höher)
> [!div class="op_single_selector"]
> * [Windows 7 oder höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für den Intel Edison herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

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
* Einen Computer mit Windows

## <a name="install-git-and-nodejs"></a>Installieren von Git und Node.js

Klicken Sie auf die unten aufgeführten Links zum Herunterladen und Installieren von Git und Node.js LTS für Windows.

* [Abrufen von Git für Windows](https://git-scm.com/download/win/)
* [Abrufen von Node.js LTS für Windows](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>Installieren zusätzlicher Node.js-Entwicklungstools

Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf dem Edison [gulp.js](http://gulpjs.com).

Starten Sie eine Eingabeaufforderung als Administrator. Installieren Sie `gulp`, indem Sie den folgenden Befehl ausführen:

```cmd
npm install -g gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Node.js-Entwicklungstools auf Ihrem Computer Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung][troubleshooting] Lösungen für häufiger auftretende Probleme.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

[Laden](https://code.visualstudio.com/docs/setup/windows) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

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



<!--HONumber=Dec16_HO2-->


