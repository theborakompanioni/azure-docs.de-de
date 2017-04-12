---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 2: Abrufen der Tools (Windows) | Microsoft-Dokumentation"
description: "Installieren Sie die Tools und die Software auf dem Hostcomputer unter Windows, erstellen Sie eine IoT Hub-Instanz, und registrieren Sie Ihr Gerät bei IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IoT-Entwicklung, IoT-Software, IoT-Clouddienst, Internet der Dinge Software, Azure CLI, Git unter Windows installieren, Gulp ausführen, Node.js installieren Windows, npm unter Windows installieren, Python unter Windows installieren"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 18ae6ee4-574a-4d5f-9838-ca2a78165628
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 4a854222be9aaf6733f1c256dd75195927299099
ms.lasthandoff: 01/25/2017


---
# <a name="get-the-tools-windows-7-and-later"></a>Abrufen der Tools (Windows 7 und höher)
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
- Ein Windows-Computer

## <a name="install-git-and-nodejs"></a>Installieren von Git und Node.js

Klicken Sie auf die folgenden Links zum Herunterladen und Installieren von Git und Node.js LTS für Windows.

- [Abrufen von Git für Windows](https://git-scm.com/download/win/)
- [Abrufen von Node.js LTS für Windows](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Installieren von Node.js-Entwicklungstools

Verwenden Sie [gulp.js](http://gulpjs.com/), um die Bereitstellung und Ausführung von Skripts zu automatisieren.

Drücken Sie `Windows + R`, geben Sie `cmd` ein, und drücken Sie `Enter`, um ein Eingabeaufforderungsfenster zu öffnen. Führen Sie dann den folgenden Befehl aus:

```cmd
npm install -g gulp
```

Wenn bei der Installation Probleme auftreten, finden Sie in der [Anleitung zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md) Lösungen für häufiger auftretende Probleme.

> [!Note]
> Node, NPM und Gulp sind für die Ausführung von Automatisierungsskripts, die mit Node.js entwickelt wurden, erforderlich.

## <a name="install-python"></a>Installieren von Python

Sie können zwischen Python 2.7, 3.4 oder 3.5 wählen. In diesem Tutorial wird Python 2.7 verwendet. Wenn Sie Python bereits installiert haben, fahren Sie mit dem nächsten Abschnitt fort.

[Python für Windows herunterladen](https://www.python.org/downloads/)

Sie müssen außerdem der `PATH`-Systemumgebungsvariablen den Pfad der Ordner hinzufügen, in denen „python.exe“ und „pip.exe“ installiert sind. Standardmäßig wird „python.exe“ in `C:\Python27` und „pip.exe“ in `C:\Python27\Scripts` installiert.

## <a name="install-the-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle

Führen Sie zum Installieren der Azure-CLI die folgenden Schritte aus:

1. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.

2. Installieren Sie die Azure-Befehlszeilenschnittstelle, indem Sie die folgenden Befehle ausführen:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   Die Installation kann 5 Minuten dauern.

3. Führen Sie den folgenden Befehl aus, um die Installation zu überprüfen:

   ```cmd
   az iot -h
   ```

   Wenn die Installation erfolgreich war, wird die folgende Ausgabe angezeigt.

   ![Überprüfen der Azure CLI-Installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Sie verwenden später in diesem Tutorial Visual Studio Code, um Konfigurationsdateien zu bearbeiten.

[Laden](https://code.visualstudio.com/docs/setup/windows) Sie Visual Studio Code herunter, und installieren Sie die Software.

## <a name="summary"></a>Zusammenfassung

Sie haben die erforderlichen Tools und Software auf Ihrem Hostcomputer installiert. Als Nächstes verwenden Sie die Azure-Befehlszeilenschnittstelle zum Erstellen einer IoT Hub-Instanz und zum Registrieren Ihres Geräts bei IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen eines IoT Hub und Registrieren Ihres Geräts](iot-hub-gateway-kit-c-lesson2-register-device.md)

