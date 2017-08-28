---
title: "IoT-Remoteüberwachung und -Benachrichtigungen mit Azure Logic Apps | Microsoft-Dokumentation"
description: "Verwenden Sie Azure Logic Apps für die IoT-Temperaturüberwachung auf Ihrem IoT Hub und zum automatischen Senden von E-Mail-Benachrichtigungen an Ihr Postfach, sobald Anomalien gefunden werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IoT-Überwachung, IoT-Benachrichtigungen, IoT-Temperaturüberwachung"
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: ca27c55f97d1b1d9694bf79282b7714283927ee7
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IoT-Remoteüberwachung und -Benachrichtigungen mit Azure Logic Apps zum Herstellen einer Verbindung zwischen Ihrem IoT Hub und Postfach

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Azure Logic Apps bietet eine Möglichkeit zum Automatisieren von Prozessen als eine Reihe von Schritten. Eine Logik-App kann verschiedene Dienste und Protokolle miteinander verbinden. Den Anfang macht ein Auslöser wie „Wenn ein Konto hinzugefügt wird“, auf den eine Reihe von Aktionen folgt, wie z.B. „Senden einer Pushbenachrichtigung“. Hierdurch wird Azure Logic Apps zu einer perfekten IoT-Lösung für die IoT-Überwachung, u.a. zur Erkennung von Anomalien.

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie eine Logik-App erstellen, die Ihren IoT Hub und Ihr Postfach für die Temperaturüberwachung und Benachrichtigungen verbindet. Wenn die Temperatur mehr als 30°C beträgt, gibt die Clientanwendung `temperatureAlert = "true"` in der Nachricht an, die an Ihren IoT Hub gesendet wird. Die Nachricht löst die Logik-App so aus, dass eine E-Mail-Benachrichtigung an Sie gesendet wird.

## <a name="what-you-do"></a>Aufgaben

* Erstellen Sie einen Service Bus-Namespace, und fügen Sie diesem eine Warteschlange hinzu.
* Fügen Sie mit Ihrem IoT Hub einen Endpunkt und eine Routingregel hinzu.
* Erstellen, konfigurieren und testen Sie eine Logik-App.

## <a name="what-you-need"></a>Erforderliches Element

* Abschluss des Tutorials [Einrichten Ihres Geräts](iot-hub-raspberry-pi-kit-node-get-started.md), in dem die folgenden Anforderungen behandelt werden:
  * Ein aktives Azure-Abonnement.
  * Ein Azure IoT Hub in Ihrem Abonnement.
  * Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Erstellen eines Service Bus-Namespaces, dem eine Warteschlange hinzugefügt wird

### <a name="create-a-service-bus-namespace"></a>Erstellen eines Service Bus-Namespaces

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu** > **Unternehmensintegration** > **Service Bus**.
1. Geben Sie die folgenden Informationen ein:

   **Name**: Der Name des Service Bus.

   **Tarif**: Klicken Sie auf **Basic** > **Auswählen**. Der Tarif „Basic“ ist für dieses Tutorial ausreichend.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Speicherort**: Verwenden Sie denselben Speicherort, den Ihr IoT Hub verwendet.
1. Klicken Sie auf **Erstellen**.

   ![Erstellen eines Service Bus-Namespaces im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Hinzufügen einer Service Bus-Warteschlange

1. Öffnen Sie den Servicebus-Namespace, und klicken Sie dann auf **+ Warteschlange**.
1. Geben Sie einen Namen für die Warteschlange ein, und klicken Sie dann auf **Erstellen**.
1. Öffnen Sie die Service Bus-Warteschlange, und klicken Sie dann auf **Freigegebene Zugriffsrichtlinien** > **+ Hinzufügen**.
1. Geben Sie einen Namen für die Richtlinie ein, aktivieren Sie **Verwalten**, und klicken Sie dann auf **Erstellen**.

   ![Hinzufügen einer Service Bus-Warteschlange im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a>Hinzufügen eines Endpunkts und einer Routingregel zu Ihrem IoT Hub

### <a name="add-an-endpoint"></a>Hinzufügen eines Endpunkts

1. Öffnen Sie Ihren IoT Hub, klicken Sie auf „Endpunkte > + Hinzufügen“.
1. Geben Sie Folgendes ein:

   **Name**: Der Name des Endpunkts.

   **Endpunkttyp**: Wählen Sie **Service Bus-Warteschlange** aus.

   **Service Bus-Namespace**: Wählen Sie den Namespace aus, den Sie erstellt haben.

   **Service Bus-Warteschlange**: Wählen Sie die Warteschlange aus, die Sie erstellt haben.
1. Klicken Sie auf **OK**.

   ![Hinzufügen eines Endpunkts zu Ihren IoT Hub im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Hinzufügen einer Routingregel

1. Klicken Sie in Ihrem IoT-Hub auf **Routen** > **+Hinzufügen**.
1. Geben Sie Folgendes ein:

   **Name**: Der Name der Routingregel.

   **Datenquelle**: Wählen Sie **DeviceMessages** aus.

   **Endpunkt**: Wählen Sie den Endpunkt aus, den Sie erstellt haben.

   **Abfragezeichenfolge**: Geben Sie `temperatureAlert = "true"` ein.
1. Klicken Sie auf **Speichern**.

   ![Hinzufügen einer Routingregel im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Erstellen und Konfigurieren einer Logik-App

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu** > **Unternehmensintegration** > **Logik-App**.
1. Geben Sie Folgendes ein:

   **Name**: Der Name der Logik-App.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Speicherort**: Verwenden Sie denselben Speicherort, den Ihr IoT Hub verwendet.
1. Klicken Sie auf **Erstellen**.

### <a name="configure-the-logic-app"></a>Konfigurieren der Logik-App

1. Öffnen Sie die Logik-App, die im Designer für Logik-Apps geöffnet wird.
1. Klicken Sie im Designer für Logik-Apps auf **Leere Logik-App**.

   ![Beginnen Sie im Azure-Portal mit einer leeren Logik-App.](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Klicken Sie auf **Service Bus**.

   ![Wählen Sie „Service Bus“ aus, um mit der Erstellung Ihrer Logik-App im Azure-Portal zu beginnen.](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Klicken Sie auf **Service Bus – Bei Empfang mindestens einer Nachricht in der Warteschlange (autom. abschließen)**.
1. Erstellen Sie eine Service Bus-Verbindung.
   1. Geben Sie einen Verbindungsnamen ein.
   1. Klicken Sie auf den Service Bus-Namespace, dann auf die Service Bus-Richtlinie und danach auf **Erstellen**.

      ![Erstellen einer Service Bus-Verbindung für Ihre Logik-App im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Klicken Sie nach dem Erstellen der Service Bus-Verbindung auf **Fortfahren**.
   1. Wählen Sie die Warteschlange aus, die Sie erstellt haben, und geben Sie `175` für **Maximale Nachrichtenanzahl** ein.

      ![Angeben der maximalen Nachrichtenanzahl für die Service Bus-Verbindung in der Logik-App](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Klicken Sie auf die Schaltfläche „Speichern“, um die Änderungen zu speichern.

1. Erstellen Sie eine SMTP-Dienstverbindung.
   1. Klicken Sie auf **Nächster Schritt** > **Aktion hinzufügen**.
   1. Geben Sie `SMTP` ein. Klicken Sie in den Suchergebnissen auf den Dienst **SMTP** und dann auf **SMTP – E-Mail senden**.

      ![Erstellen einer SMTP-Verbindung in Ihrer Logik-App im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Geben Sie die SMTP-Informationen Ihres Postfachs ein, und klicken Sie dann auf **Erstellen**.

      ![Eingeben der Informationen für die SMTP-Verbindung in Ihrer Logik-App im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Rufen Sie die SMTP-Informationen für [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en) und [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html) ab.
   1. Geben Sie für **Von** und **An** Ihre E-Mail-Adresse und `High temperature detected` für **Betreff** und **Text** ein.
   1. Klicken Sie auf **Speichern**.

Die Logik-App befindet sich in funktionsfähigem Zustand, wenn Sie sie speichern.

## <a name="test-the-logic-app"></a>Testen der Logik-App

1. Starten Sie die Clientanwendung, die Sie unter [Verbinden von ESP8266 mit Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md) auf Ihrem Gerät bereitgestellt haben.
1. Erhöhen Sie die Umgebungstemperatur um das SensorTag auf über 30°C. Entzünden Sie beispielsweise eine Kerze in der Nähe des SensorTag.
1. Sie sollten eine von der Logik-App gesendete E-Mail-Benachrichtigung erhalten.

   > [!NOTE]
   > Ihr E-Mail-Dienstanbieter muss ggf. die Identität des Absenders überprüfen, um sicherzustellen, dass die gesendete E-Mail von Ihnen stammt.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich eine Logik-App erstellt, die Ihren IoT Hub und Ihr Postfach für die Temperaturüberwachung und Benachrichtigungen verbindet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

