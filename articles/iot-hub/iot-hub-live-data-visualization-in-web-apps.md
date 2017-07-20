---
title: Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Web-Apps | Microsoft-Dokumentation
description: Verwenden Sie das Web-Apps-Feature von Microsoft Azure App Service, um Temperatur- und Luftfeuchtigkeitsdaten zu visualisieren, die der Sensor gesammelt und an IoT Hub gesendet hat.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Datenvisualisierung in Echtzeit, Visualisierung von Livedaten, Visualisierung von Sensordaten
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 74f95d571da2e3066958cf3c5ab1938a5c631f7d
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Visualisieren von Echtzeit-Sensordaten über Azure IoT Hub mithilfe des Web-Apps-Features von Azure App Service

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

In diesem Tutorial erfahren Sie, wie Sie von IoT Hub empfangene Echtzeit-Sensordaten visualisieren, indem Sie eine in einer Web-App gehostete Webanwendung ausführen. Wenn Sie versuchen möchten, die Daten in IoT Hub mit Power BI zu visualisieren, lesen Sie den Artikel [Verwenden von Power BI zum Visualisieren von Echtzeit-Sensordaten in Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Aufgaben

- Erstellen Sie eine Web-App im Azure-Portal.
- Bereiten Sie Ihren IoT Hub für den Datenzugriff vor, indem Sie eine Consumergruppe hinzufügen.
- Konfigurieren Sie die Web-App zum Lesen von Sensordaten in Ihrem IoT Hub.
- Laden Sie eine Webanwendung hoch, die von der Web-App gehostet wird.
- Öffnen Sie die Web-App, um Temperatur- und Luftfeuchtigkeitsdaten in Ihrem IoT Hub in Echtzeit anzuzeigen.

## <a name="what-you-need"></a>Erforderliches Element

- [Einrichten Ihres Geräts](iot-hub-raspberry-pi-kit-node-get-started.md), das folgende Anforderungen beinhaltet:
  - Ein aktives Azure-Abonnement
  - IoT Hub in Ihrem Abonnement
  - Eine Clientanwendung, die Nachrichten an IoT Hub sendet
- [Herunterladen von Git](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Erstellen einer Web-App

1. Klicken Sie im [Azure-Portal](https://ms.portal.azure.com/) auf **Neu** > **Web und mobil** > **Web-App**.
2. Geben Sie einen eindeutigen Auftragsnamen an. Überprüfen Sie das Abonnement. Geben Sie eine Ressourcengruppe und einen Speicherort an. Wählen Sie **An Dashboard anheften** aus, und klicken Sie dann auf **Erstellen**.

   Es wird empfohlen, denselben Standort wie den Ihrer Ressourcengruppe auszuwählen. Dies beschleunigt die Verarbeitung und senkt die Datenübertragungskosten.

   ![Erstellen einer Web-App](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>Konfigurieren der Web-App zum Lesen von Daten in Ihrem IoT Hub

1. Öffnen Sie die Web-App, die Sie gerade bereitgestellt haben.
2. Klicken Sie auf **Anwendungseinstellungen**, und fügen Sie dann unter **App-Einstellungen** die folgenden Schlüssel/Wert-Paare hinzu:

   | Schlüssel                                   | Wert                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Aus IoT Hub-Explorer abgerufen                                |
   | Azure.IoT.IoTHub.ConsumerGroup        | Der Name der Consumergruppe, die Sie Ihrem IoT Hub hinzufügen  |

   ![Hinzufügen von Einstellungen zur Web-App mit Schlüssel/Wert-Paaren](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. Klicken Sie unter **Allgemeine Einstellungen** auf **Anwendungseinstellungen**, schalten Sie die Option **Websockets** um, und klicken Sie dann auf **Speichern**.

   ![Umschalten der Option „Websockets“](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Hochladen einer Webanwendung, die von der Web-App gehostet wird

Wir haben auf GitHub eine Webanwendung zur Verfügung gestellt, die Echtzeit-Sensordaten in IoT Hub anzeigt. Sie müssen lediglich die Web-App für die Zusammenarbeit mit einem Git-Repository konfigurieren, die Webanwendung von GitHub herunterladen und sie anschließend für die zu hostende Web-App in Azure hochladen.

1. Klicken Sie in der Web-App auf **Bereitstellungsoptionen** > **Quelle auswählen** > **Lokales Git-Repository**, und klicken Sie dann auf **OK**.

   ![Konfigurieren Ihrer Web-App-Bereitstellung zur Verwendung des lokalen Git-Repositorys](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Klicken Sie auf **Anmeldeinformationen für die Bereitstellung**. Erstellen Sie einen Benutzernamen und ein Kennwort zum Herstellen der Verbindung mit dem Git-Repository in Azure, und klicken Sie dann auf **OK**.

3. Klicken Sie auf **Übersicht**, und notieren Sie den Wert von **Git-Klon-URL**.

   ![Abrufen der Git-Klon-URL Ihrer Web-App](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. Öffnen Sie auf Ihrem lokalen Computer ein Befehls- oder Terminalfenster.

5. Laden Sie die Web-App von GitHub herunter, und laden Sie sie für die zu hostende Web-App in Azure hoch. Führen Sie zu diesem Zweck die folgenden Befehle aus:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<Git-Klon-URL\> ist die URL des Git-Repositorys, die auf der Seite **Übersicht** der Web-App zu finden ist.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Öffnen der Web-App, um Temperatur- und Luftfeuchtigkeitsdaten in Ihrem IoT Hub in Echtzeit anzuzeigen

Klicken Sie auf der Seite **Übersicht** der Web-App auf die URL, um die Web-App zu öffnen.

![Abrufen der URL der Web-App](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

Die Temperatur- und Luftfeuchtigkeitsdaten in Ihrem IoT Hub sollten in Echtzeit angezeigt werden.

![Web-App-Seite mit Anzeige von Temperatur und Luftfeuchtigkeit in Echtzeit](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

## <a name="next-steps"></a>Nächste Schritte
Sie haben mithilfe Ihrer Web-App erfolgreich Sensordaten in Echtzeit in IoT Hub visualisiert.

Eine alternative Möglichkeit zur Visualisierung von Daten in Azure IoT Hub finden Sie im Artikel [Verwenden von Power BI zum Visualisieren von Echtzeit-Sensordaten in IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

