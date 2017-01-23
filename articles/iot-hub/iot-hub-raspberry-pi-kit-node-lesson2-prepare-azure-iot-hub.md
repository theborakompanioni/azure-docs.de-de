---
title: Erstellen einer Azure IoT Hub-Instanz und Registrieren von Raspberry Pi 3 | Microsoft Docs
description: "Erstellen Sie eine Ressourcengruppe, erstellen Sie eine Azure IoT Hub-Instanz, und registrieren Sie Pi über die Azure-Befehlszeilenschnittstelle in der IoT Hub-Identitätsregistrierung."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Raspberry Pi Cloud, Pi Cloudverbindung
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: e223d0613cd48994315451da87e6b7066585bdb6
ms.openlocfilehash: 1f22ab6a97427f3be521c69e308386ad0b1b286b


---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Erstellen eines IoT-Hubs und Registrieren von Raspberry Pi 3
## <a name="what-you-will-do"></a>Aufgaben
* Erstellen Sie eine Ressourcengruppe.
* Erstellen Sie einen Azure IoT Hub in der Ressourcengruppe.
* Fügen Sie Raspberry Pi 3 mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) der Azure IoT Hub-Instanz hinzu.

Wenn Sie Pi mithilfe der Azure CLI Ihrem IoT-Hub hinzufügen, generiert der Dienst einen Schlüssel, mit dem sich Pi bei dem Dienst authentifizieren kann. Bei Problemen können Sie sich die Lösungen auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ansehen.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Erstellen eines IoT-Hubs mithilfe der Azure CLI
* Erstellen einer Geräteidentität für Pi in Ihrem IoT-Hub

## <a name="what-you-need"></a>Erforderliches Element
* Ein Azure-Konto
* Ein Mac oder ein Windows-Computer, auf dem die Azure-CLI installiert ist

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
> Der Name des IoT Hub muss global eindeutig sein. Sie können unter Ihrem Azure-Abonnement nur eine einzelne F1-Edition von Azure IoT Hub erstellen.

## <a name="register-pi-in-your-iot-hub"></a>Registrieren von Pi bei Ihrem IoT-Hub
Jedes Gerät, das Nachrichten an Ihren IoT-Hub sendet und Nachrichten von Ihrem IoT-Hub empfängt, muss mit einer eindeutigen ID registriert werden. Sie verwenden die Azure CLI, um Pi zu registrieren und ein selbstsigniertes X.509-Zertifikat für die Geräteauthentifizierung zu erstellen.

Führen Sie den folgenden Befehl aus:

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a>Zusammenfassung
Sie haben einen IoT-Hub erstellt und Pi mit einer Geräteidentität bei Ihrem IoT-Hub registriert. Informieren Sie sich im Anschluss darüber, wie Sie Nachrichten von Pi an Ihren IoT-Hub senden.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos zum Verarbeiten und Speichern von IoT Hub-Nachrichten](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)




<!--HONumber=Dec16_HO1-->


