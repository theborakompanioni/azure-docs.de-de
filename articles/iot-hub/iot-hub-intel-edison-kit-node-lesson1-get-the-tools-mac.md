---
title: "Verbinden von Intel Edison (Node) mit Azure IoT – Lektion 1: Abrufen der Tools (macOS) | Microsoft-Dokumentation"
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für Edison auf macOS herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Arduino-Entwicklungstools, IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git auf mac installieren, Node.js installieren mac
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: fb6742be-2825-4524-89f7-8ccb7e7f1de1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: adf5b10721a28432e6b37ef73c6a7e7ec9f93cdd
ms.openlocfilehash: 7d8c4ef5cf055930178408ee9751058392a2f894
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---
<a id="get-the-tools-macos-1010" class="xliff"></a>

# Herunterladen der Tools (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 oder höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für Ihren Intel Edison herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

<a id="what-you-will-learn" class="xliff"></a>

## Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Installieren von Git und Node.js
  * [Git](https://git-scm.com) ist ein Open-Source-System zur verteilten Versionskontrolle. Die Beispielanwendung für diesen Artikel ist auf Git gespeichert.
  * [Node.js](https://nodejs.org/en/) ist eine JavaScript-Runtime mit einer umfangreichen Paketumgebung.
* Verwenden von NPM zum Installieren zusätzlicher Node.js-Entwicklungstools
  * Die erforderliche Minimalversion von Node.js ist 4.5 LTS.
  * [NPM](https://www.npmjs.com) ist einer der Paket-Manager für Node.js.

<a id="what-you-need" class="xliff"></a>

## Erforderliches Element
Um diesen Vorgang abzuschließen, benötigen Sie Folgendes:
* Eine Internetverbindung zum Herunterladen der Entwicklungstools und der Software
* Einen Mac, auf dem macOS Yosemite (10.10) oder höher ausgeführt wird

<a id="install-git-and-nodejs" class="xliff"></a>

## Installieren von Git und Node.js
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

<a id="install-additional-nodejs-development-tools" class="xliff"></a>

## Installieren zusätzlicher Node.js-Entwicklungstools
Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf Ihrem Edison [gulp.js](http://gulpjs.com).

Installieren Sie `gulp`, indem Sie den folgenden Befehl im Terminal ausführen:

```bash
sudo npm install -g gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Entwicklungstools unter macOS Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung][troubleshooting] Lösungen für häufiger auftretende Probleme.

<a id="install-visual-studio-code" class="xliff"></a>

## Installieren von Visual Studio Code
[Laden](https://code.visualstudio.com/docs/setup/osx) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

<a id="summary" class="xliff"></a>

## Zusammenfassung
Sie haben die erforderlichen Entwicklungstools und die Software für die erste Beispielanwendung installiert. Die nächste Aufgabe besteht im Erstellen, Bereitstellen und Ausführen der Beispielanwendung auf dem Edison.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Erstellen und Bereitstellen der Blinkanwendung][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md

