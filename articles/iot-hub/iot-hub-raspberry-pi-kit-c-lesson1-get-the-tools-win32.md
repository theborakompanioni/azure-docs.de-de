---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 1: Herunterladen der Tools (Windows) | Microsoft-Dokumentation"
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für Pi unter Windows 7 und höheren Versionen herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git unter Windows installieren, Node.js installieren Windows, npm installieren Windows
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: bd765ddd-65b7-4241-a391-dc77cb3af1c0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: d53efc7d25714427e25d4f88279f3b0d4f61c150
ms.lasthandoff: 01/24/2017


---
# <a name="get-the-tools-windows-7-or-later"></a>Herunterladen der Tools (Windows 7 oder höher)

> [!div class="op_single_selector"]
> * [Windows 7 oder höher](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für Raspberry Pi 3 herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> Obwohl die Programmiersprache der Hauptlogik C ist, werden in den Lektionen Node.js-Tools zum Ermitteln von Geräten und zum Erstellen und Bereitstellen der Beispielanwendungen verwendet.

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

Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf Pi [gulp.js](http://gulpjs.com). Verwenden Sie [device-discovery-cli](https://github.com/Azure/device-discovery-cli), um Netzwerkinformationen über Ihre IoT-Geräte abzurufen.

Starten Sie eine Eingabeaufforderung als Administrator. Installieren Sie `gulp` und `device-discovery-cli`, indem Sie den folgenden Befehl ausführen:

```bash
npm install -g device-discovery-cli gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Node.js-Entwicklungstools auf Ihrem Computer Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md) Lösungen für allgemeine Probleme.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

[Laden](https://code.visualstudio.com/docs/setup/windows) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

## <a name="summary"></a>Zusammenfassung

Sie haben die erforderlichen Entwicklungstools und die Software für die erste Beispielanwendung installiert. Die nächste Aufgabe besteht im Erstellen, Bereitstellen und Ausführen der Beispielanwendung auf Pi.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Bereitstellen der Blinkanwendung](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

