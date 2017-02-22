---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 2: Registrieren des Geräts | Microsoft-Dokumentation"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure IoT Hub, Internet der Dinge Cloud, Azure IoT Hub Gerät erstellen, TI SensorTag, TI BLE"
ms.assetid: 2c18f5ae-e39a-48ae-a9fe-04bb595740a0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: a895f18b4cefc01f12613e701c71bd471bb369c3


---

# <a name="create-your-azure-iot-hub-and-register-your-device"></a>Erstellen einer Azure IoT Hub-Instanz und Registrieren Ihres Geräts

## <a name="what-you-will-do"></a>Aufgaben

- Erstellen einer Ressourcengruppe
- Erstellen Ihrer ersten IoT Hub-Instanz
- Registrieren Ihres Geräts bei der IoT Hub-Instanz mithilfe der Azure-Befehlszeilenschnittstelle 

Wenn Sie Ihr Gerät bei der IoT Hub-Instanz registrieren, generiert der Azure IoT Hub-Dienst einen Schlüssel zum Authentifizieren Ihres Geräts bei dem Dienst. 

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

In dieser Lektion lernen Sie Folgendes:

- Erstellen einer IoT Hub-Instanz mithilfe der Azure-Befehlszeilenschnittstelle
- Registrieren eines Geräts bei einer IoT Hub-Instanz.

## <a name="what-you-need"></a>Erforderliches Element

- Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.
- Die Azure-Befehlszeilenschnittstelle muss installiert sein.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Gehen Sie zum Erstellen einer IoT Hub-Instanz wie folgt vor:

1. Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Konto an:

   ```bash
   az login
   ```

   Nach erfolgreicher Anmeldung werden alle Ihre verfügbaren Abonnements aufgeführt.

2. Legen Sie mit dem folgenden Befehl das Azure-Standardabonnement fest, das Sie verwenden möchten:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name` finden Sie in der Ausgabe des Befehls `az login` oder `az account list`.

3. Registrieren Sie den Anbieter mithilfe des folgenden Befehls. Ressourcenanbieter sind Dienste, die Ressourcen für Ihre Anwendung bereitstellen. Der Anbieter muss registriert werden, damit die angebotene Azure-Ressource bereitgestellt werden kann.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. Erstellen Sie mit dem folgenden Befehl eine Ressourcengruppe mit dem Namen `iot-gateway` in der Region „USA, Westen“:

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   `westus` ist der Ort, an dem Sie Ihre Ressourcengruppe erstellen. Wenn Sie einen anderen Ort verwenden möchten, können Sie `az account list-locations -o table` ausführen, um alle von Azure unterstützten Orte anzuzeigen.

5. Erstellen Sie mit dem folgenden Befehl eine IoT Hub-Instanz in der Ressourcengruppe `iot-gateway`:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

Standardmäßig erstellt das Tool eine IoT Hub-Instanz unter dem Tarif „Free“. Weitere Informationen finden Sie unter [Azure IoT Hub – Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Der Name des IoT Hub muss global eindeutig sein. Sie können unter Ihrem Azure-Abonnement nur eine einzelne F1-Edition von Azure IoT Hub erstellen.

## <a name="register-your-device-in-your-iot-hub"></a>Registrieren Ihres Geräts bei der IoT Hub-Instanz

Jedes Gerät, das Nachrichten an Ihren IoT-Hub sendet und Nachrichten von Ihrem IoT-Hub empfängt, muss mit einer eindeutigen ID registriert werden.
Registrieren Sie Ihr Gerät mit dem folgenden Befehl bei der IoT Hub-Instanz:

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a>Zusammenfassung

Sie haben eine IoT Hub-Instanz erstellt und Ihr logisches Gerät mit einer Geräteidentität bei der IoT Hub-Instanz registriert. Als Nächstes können Sie sich nun damit vertraut machen, wie Sie eine Gatewaybeispielanwendung konfigurieren und ausführen, um Daten von Ihrem physischen Gerät an die IoT Hub-Instanz in der Cloud zu senden.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren und Ausführen einer BLE-Beispielanwendung](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)


<!--HONumber=Jan17_HO4-->


