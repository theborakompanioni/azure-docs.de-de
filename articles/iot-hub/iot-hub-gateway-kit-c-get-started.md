---
title: "SensorTag-Gerät und Azure IoT Gateway – Erste Schritte | Microsoft-Dokumentation"
description: "Enthält die ersten Schritte mit dem IoT Gateway Starter Kit und Informationen zum Erstellen des Azure IoT Hub und Herstellen der Verbindung für den SensorTag und das Gateway mit dem IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure IoT Hub, IoT Gateway, Erste Schritte mit dem Internet der Dinge, IoT Toolkit
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: dd14d137c23fbccb3616b60db8a63e668ae7ea80
ms.lasthandoff: 01/25/2017


---

# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a>Erste Schritte mit dem IoT Gateway Starter Kit mit einem SensorTag

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simuliertes Gerät](iot-hub-gateway-kit-c-sim-get-started.md)

In diesem Tutorial machen Sie sich zuerst mit den Grundlagen des [IoT Gateway Starter Kit](https://aka.ms/gateway-kit) vertraut. Sie verwenden ein Intel NUC-Gerät mit Ausführung von Wind River Linux und [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main). Es wird beschrieben, wie Sie Ihre Geräte mithilfe von Azure IoT Hub nahtlos mit der Cloud verbinden.

***
**Wenn Sie noch nicht über das Kit verfügen:** Klicken Sie [hier](https://aka.ms/gateway-kit). **Sie verfügen noch nicht über einen SensorTag?:** [Beginnen Sie mit einem simulierten Gerät](iot-hub-gateway-kit-c-sim-get-started.md), oder [erwerben Sie einen SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag).
***

## <a name="lesson-1-configure-your-nuc"></a>Lektion 1: Konfigurieren Ihres NUC-Geräts
![Lektion&1;: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

In dieser Lektion richten Sie Intel NUC (Next Unit of Computing) im Kit als Azure IoT Gateway ein, installieren das Azure IoT Gateway SDK-Paket auf dem NUC und führen eine Beispiel-App aus, um die Gatewayfunktionalität zu überprüfen.

*Geschätzter Zeitaufwand*: 15 Minuten.

[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) (Einrichten von Intel NUC als IoT Gateway)

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen des IoT Hub
![Lektion&2;: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

In dieser Lektion installieren Sie die Tools und Software auf dem Hostcomputer. Anschließend erstellen Sie ein kostenloses Azure-Konto, stellen Ihren Azure IoT Hub bereit und erstellen Ihr erstes Gerät im IoT Hub.

Schließen Sie Lektion 1 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="get-the-tools"></a>Tools herunterladen
Installieren Sie die Tools und Software auf dem Hostcomputer.

*Geschätzter Zeitaufwand: 20 Minuten*

Wechseln Sie zu [Get the tools](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md) (Herunterladen der Tools).

### <a name="create-an-iot-hub-and-register-your-device"></a>Erstellen eines IoT Hub und Registrieren Ihres Geräts
Erstellen Sie Ihre Ressourcengruppe, stellen Sie Ihren ersten Azure IoT Hub bereit, und fügen Sie dem IoT Hub mithilfe der Azure-CLI Ihr erstes Gerät hinzu.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen eines IoT Hub und Registrieren Ihres Geräts](iot-hub-gateway-kit-c-lesson2-register-device.md).

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a>Lektion 3: Empfangen von Nachrichten von SensorTag und Lesen von Nachrichten von Ihrem IoT Hub
In dieser Lektion verwenden Sie Skripts zum Automatisieren der Konfiguration und der Ausführung einer BLE-Beispielanwendung in Ihrem Gateway. Für solche Anwendungen wird eine Modulsammlung zum Aggregieren und Transformieren von Daten, Verarbeiten von Befehlen oder Durchführen einer beliebigen Zahl von verwandten Aufgaben verwendet. Die Module kommunizieren miteinander über einen Nachrichtenbroker. Die Beispielanwendung verfügt über ein BLE-Modul und ein IoT Hub-Modul. Das BLE-Modul empfängt Daten vom BLE SensorTag. Das IoT Hub-Modul packt die empfangenen Daten und sendet sie über das Gatewayframework, das über das Azure IoT Gateway SDK bereitgestellt wird, an Ihren IoT Hub.

![Lektion 3: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-the-ble-sample-app"></a>Konfigurieren und Ausführen der BLE-Beispielanwendung
Richten Sie die Verbindung zwischen SensorTag und Ihrem Gateway ein. Schließen Sie anschließend die Konfiguration ab, und führen Sie die BLE-Beispielanwendung aus.

*Geschätzter Zeitaufwand*: 15 Minuten.

[Konfigurieren und Ausführen der BLE-Beispielanwendung](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Lesen von Nachrichten von Ihrem IoT Hub
Führen Sie Beispielcode auf Ihrem Hostcomputer aus, um Nachrichten vom IoT Hub zu lesen.

*Geschätzter Zeitaufwand*: 15 Minuten.

Wechseln Sie zu [Lesen von Nachrichten von Ihrem IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).

## <a name="lesson-4-save-messages-to-azure-table-storage"></a>Lektion 4: Speichern von Nachrichten in Azure Table Storage
Erstellen Sie eine Azure-Funktionen-App, die eingehende Nachrichten von Ihrem IoT-Hub erhält und sie in Azure Table Storage (Tabellenspeicher) schreibt.

![Lektion 4: Komplettes Diagramm](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).

### <a name="read-messages-persisted-in-azure-table-storage"></a>Lesen von Nachrichten in Azure Table Storage
Überwachen Sie die Nachrichten vom Gateway an die Cloud, wenn sie in den Azure-Tabellenspeicher geschrieben werden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Lesen von Nachrichten in Azure Table Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Problembehandlung
Falls Sie bei den Lektionen Probleme haben, helfen Ihnen die Lösungen im Artikel zur [Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md) weiter.

## <a name="explore-more"></a>Mehr erfahren
Weitere Informationen finden Sie unter [Intel IoT Gateway Kit Developer Zone](http://software.intel.com/iot/microsoft-azure).
