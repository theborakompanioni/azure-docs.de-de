---
title: "Verbinden von Intel Edison (Node) mit Azure IoT – Lektion 1: Abrufen der Tools (Windows) | Microsoft-Dokumentation"
description: "Laden Sie die erforderlichen Tools und die Software für die erste Beispielanwendung für Edison unter Windows 7 und höheren Versionen herunter, und installieren Sie sie."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Arduino-Entwicklungstools, IoT-Entwicklung, IoT-Software, Internet der Dinge Software, Git unter Windows installieren, Node.js installieren Windows
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 4164b5a1-5a42-4d8a-9ff6-441e79fcc936
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: adf5b10721a28432e6b37ef73c6a7e7ec9f93cdd
ms.openlocfilehash: 67e140e086d1deb17b7075133d969a93b859951c
ms.contentlocale: de-de
ms.lasthandoff: 01/25/2017

---
<a id="get-the-tools-windows-7-or-later" class="xliff"></a>

# Herunterladen der Tools (Windows 7 oder höher)
> [!div class="op_single_selector"]
> * [Windows 7 oder höher][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

<a id="what-you-will-do" class="xliff"></a>

## Aufgaben
Laden Sie die Entwicklungstools und die Software für die erste Beispielanwendung für den Intel Edison herunter. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

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
* Einen Computer mit Windows

<a id="install-git-and-nodejs" class="xliff"></a>

## Installieren von Git und Node.js

Klicken Sie auf die unten aufgeführten Links zum Herunterladen und Installieren von Git und Node.js LTS für Windows.

* [Abrufen von Git für Windows](https://git-scm.com/download/win/)
* [Abrufen von Node.js LTS für Windows](https://nodejs.org/en/)

<a id="install-additional-nodejs-development-tools" class="xliff"></a>

## Installieren zusätzlicher Node.js-Entwicklungstools

Verwenden Sie zum Automatisieren der Bereitstellung der Beispielanwendung auf dem Edison [gulp.js](http://gulpjs.com).

Starten Sie eine Eingabeaufforderung als Administrator. Installieren Sie `gulp`, indem Sie den folgenden Befehl ausführen:

```cmd
npm install -g gulp
```

Wenn bei der Installation von Node.js und diesen zusätzlichen Node.js-Entwicklungstools auf Ihrem Computer Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung][troubleshooting] Lösungen für häufiger auftretende Probleme.

<a id="install-visual-studio-code" class="xliff"></a>

## Installieren von Visual Studio Code

[Laden](https://code.visualstudio.com/docs/setup/windows) Sie Visual Studio Code herunter, und installieren Sie die Software. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor für Windows, Linux und macOS. Sie verwenden diesen Editor in diesem Tutorial, um den Beispielcode zu bearbeiten.

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

