---
title: Erstellen Ihres Azure IoT Hub und Registrieren von Adafruit Feather M0 WiFi | Microsoft-Dokumentation
description: Erstellen Sie eine Ressourcengruppe, erstellen Sie eine Azure IoT Hub-Instanz, und registrieren Sie Adafruit Feather M0 WiFi mithilfe der Azure-CLI im Azure IoT Hub.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Herstellen der Verbindung von Arduino mit der Cloud, Azure IoT Hub, Internet der Dinge-Cloud, Azure IoT Hub Gerät erstellen, Arduino-Cloud"
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 9e8084fe26229ef9ed1676c0a3c34e0ee7be88b1
ms.openlocfilehash: 232c391e61e7e7ec053d2e7170f9abe4c79d9528


---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a>Erstellen des IoT Hub und Registrieren des Adafruit Feather M0 WiFi-Arduino-Boards

## <a name="what-you-will-do"></a>Aufgaben
* Erstellen Sie eine Ressourcengruppe.
* Erstellen Sie einen Azure IoT Hub in der Ressourcengruppe.
* Fügen Sie Ihr Arduino-Board mithilfe der Azure-Befehlszeilenschnittstelle (Azure-CLI) dem Azure IoT Hub hinzu.

Wenn Sie dem IoT Hub das Arduino-Board mithilfe der Azure-CLI hinzufügen, generiert der Dienst einen Schlüssel zum Authentifizieren des Arduino-Boards beim Dienst. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshoot].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Erstellen einer IoT Hub-Instanz mithilfe der Azure-Befehlszeilenschnittstelle
* Erstellen einer Geräteidentität für das Arduino-Board im Azure IoT Hub

## <a name="what-you-need"></a>Erforderliche Komponenten
* Ein Azure-Konto
* Ein Computer mit installierter Azure-CLI

## <a name="create-your-iot-hub"></a>Erstellen Ihres IoT-Hubs
Mit einem Azure IoT Hub können Sie Millionen von IoT-Assets verbinden, überwachen und verwalten. Gehen Sie zum Erstellen Ihres IoT-Hubs wie folgt vor:

1. Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Konto an:

   ```bash
   az login
   ```

   Nach erfolgreicher Anmeldung werden alle Ihre verfügbaren Abonnements aufgeführt.

2. Legen Sie mit dem folgenden Befehl das gewünschte Standardabonnement fest:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name` finden Sie in der Ausgabe des Befehls `az login` oder `az account list`.

3. Registrieren Sie den Anbieter mithilfe des folgenden Befehls. Ressourcenanbieter sind Dienste, die Ressourcen für Ihre Anwendung bereitstellen. Der Anbieter muss registriert werden, damit die angebotene Azure-Ressource bereitgestellt werden kann.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Erstellen Sie mit dem folgenden Befehl eine Ressourcengruppe mit dem Namen „iot-sample“ in der Region „USA, Westen“:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus` ist der Ort, an dem Sie Ihre Ressourcengruppe erstellen. Wenn Sie einen anderen Ort verwenden möchten, können Sie `az account list-locations -o table` ausführen, um alle von Azure unterstützten Orte anzuzeigen.

5. Erstellen Sie mit dem folgenden Befehl einen IoT Hub in der Ressourcengruppe „iot-sample“:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

Standardmäßig erstellt das Tool eine IoT Hub-Instanz unter dem Tarif „Free“. Weitere Informationen finden Sie unter [Azure IoT Hub – Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Der Name des IoT Hub muss global eindeutig sein.
> Sie können unter Ihrem Azure-Abonnement nur eine einzelne F1-Edition von Azure IoT Hub erstellen.

## <a name="register-your-arduino-board-in-your-iot-hub"></a>Registrieren des Arduino-Boards in Ihrem IoT Hub
Jedes Gerät, das Nachrichten an Ihren IoT-Hub sendet und Nachrichten von Ihrem IoT-Hub empfängt, muss mit einer eindeutigen ID registriert werden.

Registrieren Sie das Arduino-Board mit dem folgenden Befehl bei der IoT Hub-Instanz:

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a>Zusammenfassung
Sie haben eine IoT Hub-Instanz erstellt und Ihr Arduino-Board mit einer Geräteidentität bei der IoT Hub-Instanz registriert. Informieren Sie sich im Anschluss darüber, wie Sie Nachrichten vom Arduino-Board an die IoT Hub-Instanz senden.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos zum Verarbeiten und Speichern von IoT Hub-Nachrichten][process-and-store-iot-hub-messages]


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md


<!--HONumber=Dec16_HO2-->


