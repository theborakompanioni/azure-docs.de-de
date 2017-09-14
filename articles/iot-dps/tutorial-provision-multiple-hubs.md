---
title: "Bereitstellen von Geräten für IoT Hubs mit Lastausgleich mithilfe des Azure IoT Hub Device Provisioning-Diensts | Microsoft-Dokumentation"
description: "Automatische DPS-Gerätebereitstellung für IoT Hubs mit Lastausgleich im Azure-Portal"
services: iot-dps
keywords: 
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 266b9b7eb228744075627e1e80710e63c27880cc
ms.openlocfilehash: 4842944cd0d980fb7e817165da23b9c3c4037e94
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---


# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Bereitstellen von Geräten für IoT Hubs mit Lastausgleich

In diesem Tutorial wird gezeigt, wie Sie anhand des Device Provisioning-Diensts (Device Provisioning Service, DPS) Geräte für mehrere IoT Hubs mit Lastausgleich bereitstellen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen eines zweiten Geräts für eine zweite IoT Hub-Instanz mit dem Azure-Portal 
> * Hinzufügen eines Registrierungslisteneintrags für das zweite Gerät
> * Festlegen der DPS-Zuordnungsrichtlinie zur **gleichmäßigen Verteilung**
> * Verknüpfen der neuen IoT Hub-Instanz mit dem DPS

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial baut auf dem vorherigen Tutorial [Bereitstellen eines Geräts für einen Hub](tutorial-provision-device-to-hub.md) auf.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Bereitstellen eines zweiten Geräts für eine zweite IoT Hub-Instanz mit dem Azure-Portal

Um ein zweites Gerät für eine andere IoT Hub-Instanz bereitzustellen, führen Sie die Schritte im Tutorial [Bereitstellen eines Geräts für einen Hub](tutorial-provision-device-to-hub.md) durch.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Hinzufügen eines Registrierungslisteneintrags für das zweite Gerät

Die Registrierungsliste weist den DPS an, welche Nachweismethode (die Methode zur Bestätigung einer Geräteidentität) beim Gerät verwendet wird. Der nächste Schritt besteht darin, einen Registrierungslisteneintrag für das zweite Gerät hinzuzufügen. 

1. Klicken Sie auf der Seite für Ihre DPS-Instanz auf **Registrierungen verwalten**. Die Seite **Registrierungslisteneintrag hinzufügen** wird angezeigt. 
2. Klicken Sie oben auf der Seite auf **Hinzufügen**.
2. Füllen Sie die Felder aus, und klicken Sie dann auf **Speichern**.

## <a name="set-the-dps-allocation-policy"></a>Festlegen der DPS-Zuordnungsrichtlinie

Bei der Zuordnungsrichtlinie handelt es sich um eine DPS-Einstellung, die festlegt, wie Geräte einer IoT Hub-Instanz zugewiesen werden. Es gibt drei unterstützte Zuordnungsrichtlinien: 

1. **Niedrigste Latenz**: Geräte werden basierend auf dem Hub mit der geringsten Latenz auf dem Gerät für eine IoT Hub-Instanz bereitgestellt.
2. **Gleichmäßig gewichtete Verteilung** (Standard): Bei verknüpften IoT Hubs ist die Wahrscheinlichkeit gleich hoch, dass ihnen Geräte bereitgestellt werden. Dies ist die Standardeinstellung. Wenn Sie nur für eine IoT Hub-Instanz Geräte bereitstellen, können Sie diese Einstellung beibehalten. 
3. **Statische Konfiguration über die Registrierungsliste**: Die Angabe der gewünschten IoT Hub-Instanz in der Registrierungsliste hat gegenüber der Zuordnungsrichtlinie auf DPS-Ebene Vorrang.

Führen Sie zum Festlegen der Zuordnungsrichtlinie folgende Schritte durch:

1. Klicken Sie zum Festlegen der Zuordnungsrichtlinie auf der DPS-Seite auf **Zuordnungsrichtlinie verwalten**.
2. Legen Sie für die Zuordnungsrichtlinie **Gleichmäßig gewichtete Verteilung** fest.
3. Klicken Sie auf **Speichern**.

## <a name="link-the-new-iot-hub-to-dps"></a>Verknüpfen der neuen IoT Hub-Instanz mit dem DPS

Verknüpfen Sie den DPS und IoT Hub, damit der DPS Geräte bei diesem Hub registrieren kann.

1. Klicken Sie auf der Seite **Alle Ressourcen** auf die zuvor erstellte DPS-Instanz.
2. Klicken Sie auf der DPS-Seite auf **Verknüpfte IoT Hubs**.
3. Klicken Sie auf **Hinzufügen**.
4. Aktivieren Sie auf der Seite **Verknüpfung zu IoT Hub hinzufügen** die Optionsfelder, um anzugeben, ob sich der verknüpfte IoT Hub im aktuellen Abonnement oder in einem anderen Abonnement befindet. Wählen Sie dann im Feld **IoT Hub** den Namen von IoT Hub.
5. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen eines zweiten Geräts für eine zweite IoT Hub-Instanz mit dem Azure-Portal 
> * Hinzufügen eines Registrierungslisteneintrags für das zweite Gerät
> * Festlegen der DPS-Zuordnungsrichtlinie zur **gleichmäßigen Verteilung**
> * Verknüpfen der neuen IoT Hub-Instanz mit dem DPS

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->

