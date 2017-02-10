---
title: Erstellen einer Azure IoT Hub-Instanz und Registrieren von Intel Edison | Microsoft-Dokumentation
description: Erstellen Sie eine Ressourcengruppe, erstellen Sie eine Azure IoT Hub-Instanz, und registrieren Sie den Edison mithilfe der Azure CLI bei Azure IoT Hub.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ms.assetid: c1465cc2-d0d9-4326-967c-64d95b61dd54
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: f45b3bf00d619376ac07418f0c02eca5f3241939
ms.openlocfilehash: cc3b0295e73b3cf85eb8c5468d56f175a873bddc


---
# <a name="create-your-iot-hub-and-register-intel-edison"></a>Erstellen der IoT Hub-Instanz und Registrieren von Intel Edison
## <a name="what-you-will-do"></a>Aufgaben
* Erstellen Sie eine Ressourcengruppe.
* Erstellen Sie einen Azure IoT Hub in der Ressourcengruppe.
* Fügen Sie Intel Edison mithilfe der Azure-Befehlszeilenschnittstelle (Azure-CLI) der Azure IoT Hub-Instanz hinzu.

Wenn Sie den Edison mithilfe der Azure CLI dem IoT Hub hinzufügen, generiert der Dienst einen Schlüssel zum Authentifizieren des Edison bei dem Dienst. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:
* Erstellen einer IoT Hub-Instanz mithilfe der Azure-Befehlszeilenschnittstelle
* Erstellen einer Geräteidentität für Edison in Ihrer IoT Hub-Instanz

## <a name="what-you-need"></a>Erforderliches Element
* Ein Azure-Konto. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](http://azure.microsoft.com/pricing/free-trial/) erstellen.
* Die Azure-Befehlszeilenschnittstelle muss installiert sein.

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


## <a name="register-edison-in-your-iot-hub"></a>Registrieren von Edison bei Ihrer IoT Hub-Instanz
Jedes Gerät, das Nachrichten an Ihren IoT-Hub sendet und Nachrichten von Ihrem IoT-Hub empfängt, muss mit einer eindeutigen ID registriert werden.

Registrieren Sie Edison mit folgendem Befehl bei der Azure IoT Hub-Instanz:

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a>Zusammenfassung
Sie haben eine IoT Hub-Instanz erstellt und Edison mit einer Geräteidentität bei Ihrer IoT Hub-Instanz registriert. Informieren Sie sich im Anschluss darüber, wie Sie Nachrichten von Edison an Ihre IoT Hub-Instanz senden.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos zum Verarbeiten und Speichern von IoT Hub-Nachrichten][process-and-store-iot-hub-messages]


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md


<!--HONumber=Dec16_HO2-->


