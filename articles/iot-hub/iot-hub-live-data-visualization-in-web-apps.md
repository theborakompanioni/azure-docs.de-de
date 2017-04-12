---
title: Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Web-Apps | Microsoft-Dokumentation
description: Verwenden Sie Azure Web Apps, um Temperatur- und Luftfeuchtigkeitsdaten visuell darzustellen, die der Sensor gesammelt und an Ihren Azure IoT Hub gesendet hat.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
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
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 1d1eec1457917baef1acab27f7c60bdb048837a9
ms.lasthandoff: 04/12/2017


---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-azure-web-apps"></a>Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Web-Apps

![Verbindung zwischen Sensor, IoT-Gerät, IoT Hub und Azure-Web-App](media/iot-hub-live-data-visualization-in-web-apps/1_sensor-iot-device-azure-iot-hub-web-app-connection.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

In dieser Lektion erfahren Sie, wie Sie von Ihrem Azure IoT Hub empfangene Sensordaten in Echtzeit visualisieren, indem Sie eine Webanwendung ausführen, die in einer Azure-Web-App gehostet wird. Wenn Sie versuchen möchten, die Daten in Ihrem IoT Hub mit Power BI visuell darzustellen, siehe [Verwenden von Power BI zum Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Aufgaben

- Erstellen Sie im Azure-Portal eine Azure-Web-App.
- Bereiten Sie Ihren IoT Hub für den Datenzugriff vor, indem Sie eine Consumergruppe hinzufügen.
- Konfigurieren Sie die Web-App zum Lesen von Sensordaten in Ihrem IoT Hub.
- Laden Sie eine Webanwendung hoch, die von der Web-App gehostet wird.
- Öffnen Sie die Web-App, um Temperatur- und Luftfeuchtigkeitsdaten in Ihrem IoT Hub in Echtzeit anzuzeigen.

## <a name="what-you-need"></a>Voraussetzungen

- Sie müssen das Tutorial [Verbinden von ESP8266 mit Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md) abgeschlossen haben, in dem die folgenden Voraussetzungen behandelt werden:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.
- Git. ([Git herunterladen](https://www.git-scm.com/downloads)).

## <a name="create-an-azure-web-app"></a>Erstellen einer Azure-Web-App

1. Klicken Sie im [Azure-Portal](https://ms.portal.azure.com/) auf **Neu** > **Web und mobil** > **Web-App**.
1. Geben Sie einen eindeutigen Auftragsnamen an. Überprüfen Sie das Abonnement. Geben Sie eine Ressourcengruppe und einen Speicherort an. Wählen Sie **An Dashboard anheften** aus, und klicken Sie dann auf **Erstellen**.

   Es wird empfohlen, dass Sie den gleichen Speicherort auswählen, in dem sich Ihre Ressourcengruppe befindet. Dies beschleunigt die Verarbeitung und senkt die Datenübertragungskosten.

   ![Erstellen einer Azure-Web-App](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

## <a name="add-a-consumer-group-to-your-iot-hub"></a>Hinzufügen einer Consumergruppe zu Ihrem IoT Hub

Consumergruppen werden von Anwendungen verwendet, um Daten aus Azure IoT Hub abzurufen. In dieser Lektion erstellen Sie eine Consumergruppe, die von der Web-App zum Lesen von Daten in Ihrem IoT Hub verwendet wird.

Gehen Sie folgendermaßen vor, um Ihrem IoT Hub eine Consumergruppe hinzuzufügen:

1. Öffnen Sie im [Azure-Portal](https://ms.portal.azure.com/) Ihren IoT-Hub.
1. Klicken Sie im linken Bereich auf **Endpunkte**. Wählen Sie im mittleren Bereich **Ereignisse** aus. Geben Sie im rechten Bereich unter **Consumergruppen** einen Namen ein, und klicken Sie dann auf **Speichern**.

   ![Erstellen einer Consumergruppe in Azure IoT Hub](media/iot-hub-live-data-visualization-in-web-apps/3_add-consumer-group-iot-hub-azure.png)

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>Konfigurieren der Web-App zum Lesen von Daten in Ihrem IoT Hub

1. Öffnen Sie die Web-App, die Sie gerade bereitgestellt haben.
1. Klicken Sie auf **Anwendungseinstellungen**, und fügen Sie dann unter **Anwendungseinstellungen** die folgenden Schlüssel-Wert-Paare hinzu:

   | Schlüssel                                   | Wert                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Abgerufen aus IoT Hub-Explorer                               |
   | Azure.IoT.IoTHub.DeviceId             | Abgerufen aus IoT Hub-Explorer                               |
   | Azure.IoT.IoTHub.ConsumerGroup        | Der Name der Consumergruppe, die Sie Ihrem IoT Hub hinzufügen  |

   ![Hinzufügen von Einstellungen zur Azure-Web-App mit Schlüssel-Wert-Paaren](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Hochladen einer Webanwendung, die von der Web-App gehostet wird

Wir haben auf GitHub eine Webanwendung zur Verfügung gestellt, die Sensordaten in Ihrem IoT Hub in Echtzeit anzeigt. Sie müssen lediglich die Web-App für die Zusammenarbeit mit einem Git-Repository konfigurieren, die Webanwendung von GitHub herunterladen und Sie in Azure in die Web-App hochladen, die zum Hosten dient.

1. Klicken Sie in der Web-App auf **Bereitstellungsoptionen** > **Quelle auswählen** > **Lokales Git-Repository**.

   ![Konfigurieren Ihrer Azure-Web-App-Bereitstellung zur Verwendung eines lokalen Git-Repositorys](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

1. Klicken Sie auf **Verbindung einrichten**. Erstellen Sie einen Benutzernamen und ein Kennwort zum Herstellen der Verbindung mit dem Git-Repository in Azure, und klicken Sie dann auf **OK**.

   ![Festlegen des Benutzernamens und Kennworts für das Git-Repository in Azure für Ihre Web-App](media/iot-hub-live-data-visualization-in-web-apps/6_web-app-set-user-password-git-repo-azure.png)

1. Klicken Sie auf **OK**, um die Konfiguration abzuschließen.
1. Klicken Sie auf **Übersicht**, und notieren Sie den Wert von **Git-Klon-URL**.

   ![Abrufen der Git-Klon-URL Ihrer Azure-Web-App](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

1. Öffnen Sie auf Ihrem lokalen Computer ein Befehls- oder Terminalfenster.
1. Laden Sie die Webanwendung von GitHub herunter, und laden Sie sie in Azure in die Web-App hoch, die zum Hosten dient. Führen Sie zu diesem Zweck die folgenden Befehle aus:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!Note]
   > \<Git-Klon-URL\> ist die URL des Git-Repositorys, die auf der Seite **Übersicht** der Web-App zu finden ist.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Öffnen der Web-App, um Temperatur- und Luftfeuchtigkeitsdaten in Ihrem IoT Hub in Echtzeit anzuzeigen

Klicken Sie auf der Seite **Übersicht** der Web-App auf die URL, um die Web-App zu öffnen.

![Abrufen der URL Ihrer Azure-Web-App](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

Die Temperatur- und Luftfeuchtigkeitsdaten in Ihrem IoT Hub sollten in Echtzeit angezeigt werden.

![Azure-Web-App-Seite mit Echtzeitanzeige von Temperatur und Luftfeuchtigkeit](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

## <a name="next-steps"></a>Nächste Schritte
Sie haben eine Azure-Web-App erfolgreich eingesetzt, um Sensordaten in Ihrem Azure IoT Hub in Echtzeit zu visualisieren.

Es gibt eine alternative Möglichkeit zum Anzeigen von Daten in Ihrem Azure IoT Hub. Siehe [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Power BI](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
