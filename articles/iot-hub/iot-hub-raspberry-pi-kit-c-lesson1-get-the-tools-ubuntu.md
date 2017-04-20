---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 1: Herunterladen der Tools (Ubuntu) | Microsoft-Dokumentation"
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für den Pi auf Ubuntu herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git auf Ubuntu installieren, Gulp ausführen, Node.js installieren Uubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 32cfea00-c254-4cef-8f6f-bbf807eca6b6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: aadeff581e6134144ddc43416d4d5f9daea1b9f4
ms.lasthandoff: 01/24/2017


---
# <a name="get-the-tools-ubuntu-1604"></a>Herunterladen der Tools (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 oder höher](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für Ihren Raspberry Pi 3 herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

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
* Einen Computer mit Ubuntu 16.04 oder höher

## <a name="install-git-nodejs-and-npm"></a>Installieren von Git, Node.js und NPM
Verwenden Sie die Tastenkombination `Ctrl + Alt + T`, um ein Terminal zu öffnen, und führen Sie die folgenden Befehle aus:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>Installieren zusätzlicher Node.js-Entwicklungstools
Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf Pi [gulp.js](http://gulpjs.com). Verwenden Sie [device-discovery-cli](https://github.com/Azure/device-discovery-cli), um Netzwerkinformationen über Ihre IoT-Geräte abzurufen.

Installieren Sie `gulp` und `device-discovery-cli`, indem Sie den folgenden Befehl im Terminal ausführen:

```bash
sudo npm install -g device-discovery-cli gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Entwicklungstools unter Ubuntu Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md) Lösungen für häufiger auftretende Probleme.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code
[Laden](https://code.visualstudio.com/docs/setup/linux) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

## <a name="summary"></a>Zusammenfassung
Sie haben die erforderlichen Entwicklungstools und die Software für die erste Beispielanwendung installiert. Die nächste Aufgabe besteht im Erstellen, Bereitstellen und Ausführen der Beispielanwendung auf Pi.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen und Bereitstellen der Blinkanwendung](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)


