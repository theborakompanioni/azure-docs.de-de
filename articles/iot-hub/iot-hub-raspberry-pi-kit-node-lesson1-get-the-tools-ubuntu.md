---
title: Abrufen der Tools (Ubuntu 16.04) | Microsoft-Dokumentation
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für den Pi auf Ubuntu herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git auf Ubuntu installieren, Gulp ausführen, Node.js installieren Uubuntu"
ms.assetid: 4d5e45c0-1db9-4662-a039-99ba26333085
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: ffcb9214b8fa645a8a2378c5e7054b9f984addbb
ms.openlocfilehash: 31b5ac4e60eaafcc134ee702a4d47c4ea735df39


---
# <a name="get-the-tools-ubuntu-1604"></a>Herunterladen der Tools (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 oder höher](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a>Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für Raspberry Pi 3 herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Installieren von Git und Node.js
  * [Git](https://git-scm.com) ist ein Open-Source-System zur verteilten Versionskontrolle. Die Beispielanwendung für diesen Artikel ist auf Git gespeichert.
  * [Node.js](https://nodejs.org/en/) ist eine JavaScript-Runtime mit einer umfangreichen Paketumgebung.
* Verwenden von NPM zum Installieren zusätzlicher Node.js-Entwicklungstools
  * Die erforderliche Minimalversion von Node.js ist 4.5 LTS.
  * [NPM](https://www.npmjs.com) ist einer der Paket-Manager für Node.js.

## <a name="what-do-you-need"></a>Voraussetzungen
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
Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf Pi [gulp.js](http://gulpjs.com). Sie verwenden auch [device-discovery-cli](https://github.com/Azure/device-discovery-cli), um Netzwerkinformationen über Ihre IoT-Geräte abzurufen.

Installieren Sie `gulp` und `device-discovery-cli`, indem Sie den folgenden Befehl im Terminal ausführen:

```bash
sudo npm install -g device-discovery-cli gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Entwicklungstools unter Ubuntu Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) Lösungen für häufiger auftretende Probleme.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code
[Laden](https://code.visualstudio.com/docs/setup/linux) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

## <a name="summary"></a>Zusammenfassung
Sie haben die erforderlichen Entwicklungstools und die Software für die erste Beispielanwendung installiert. Die nächste Aufgabe besteht im Erstellen, Bereitstellen und Ausführen der Beispielanwendung auf Pi.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen und Bereitstellen der Beispielanwendung blink](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)




<!--HONumber=Nov16_HO5-->


