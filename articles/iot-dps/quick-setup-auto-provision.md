---
title: Einrichten von Device Provisioning im Azure-Portal | Microsoft-Dokumentation
description: "Azure-Schnellstart – Einrichten des Azure IoT Hub Device Provisioning-Diensts im Azure-Portal"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 519ea43bfaa9d61de6ff962d838c197eb9e4edc0
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="set-up-the-iot-hub-device-provisioning-service-preview-with-the-azure-portal"></a>Einrichten des IoT Hub Device Provisioning-Diensts (Vorschauversion) mit dem Azure-Portal

Diese Schritte veranschaulichen, wie Sie die Azure-Cloudressourcen im Portal für die Bereitstellung Ihrer Geräte einrichten. Dies umfasst die Erstellung Ihres IoT Hub, die Erstellung eines neuen IoT Hub Device Provisioning-Diensts und die Verknüpfung dieser beiden Dienste. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie **Internet der Dinge** und **IoT Hub**, und klicken Sie auf die Schaltfläche **Erstellen**. 

3. Geben Sie Ihrem IoT Hub einen **Namen**. Wählen Sie unter den verfügbaren Preisoptionen, geben Sie die [IoT Hub-Einheiten](https://azure.microsoft.com/pricing/details/iot-hub/) ein, und wählen Sie die Anzahl von Partitionen für D2C-Nachrichten sowie das Abonnement aus, das für diese Ressource verwendet werden soll. Geben Sie den Namen einer neuen oder vorhandenen Ressourcengruppe ein, und wählen Sie den Standort aus. Klicken Sie auf **Erstellen**, wenn Sie fertig sind.

    ![Eingeben grundlegender Informationen zu Ihrem IoT Hub auf dem Portalblatt](./media/quick-setup-auto-provision/create-iot-hub-portal.png)  

4. Nach der erfolgreichen Bereitstellung des IoT Hub wird automatisch das Blatt mit der Hub-Zusammenfassung angezeigt.


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>Erstellen einer neuen Instanz für den IoT Hub Device Provisioning-Dienst

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Verwenden Sie die Option *Marketplace durchsuchen*, um den Marketplace nach dem **Device Provisioning-Dienst** zu durchsuchen. Wählen Sie den **IoT Device Provisioning-Dienst (Vorschauversion)** aus, und klicken Sie auf die Schaltfläche **Erstellen**. 

3. Geben Sie Ihrer Instanz des Device Provisioning-Diensts einen **Namen**. Wählen Sie das Abonnement aus, das für diese Instanz verwendet werden soll, und geben Sie eine neue oder vorhandene Ressourcengruppe an. Wählen Sie den Standort aus. Klicken Sie auf **Erstellen**, wenn Sie fertig sind.

    ![Eingeben grundlegender Informationen zu Ihrer Instanz des Device Provisioning-Diensts auf dem Portalblatt](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Nachdem der Dienst erfolgreich bereitgestellt wurde, wird automatisch das Blatt mit der Zusammenfassung geöffnet.


## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>Verknüpfen von IoT Hub und Ihrem Device Provisioning-Dienst

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**. Wählen Sie die Instanz des Device Provisioning-Diensts aus, die Sie im vorherigen Abschnitt erstellt haben.  

2. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Linked IoT hubs** (Verknüpfte IoT Hubs). Klicken Sie oben auf die Schaltfläche **+ Hinzufügen**. 

3. Wählen Sie auf dem Portalblatt **Add link to IoT hub or pool** (Verknüpfung mit IoT Hub oder Pool hinzufügen) entweder das aktuelle Abonnement aus, oder geben Sie den Namen und die Verbindungszeichenfolge für ein anderes Abonnement ein. Geben Sie *IoT Hub* als **Ressourcentyp** an, und wählen Sie den Namen des Hub in der Dropdownliste aus. Klicken Sie auf **Speichern**, wenn Sie fertig sind. 

    ![Verknüpfen des Hubnamens mit der Instanz des Device Provisioning-Diensts auf dem Portalblatt](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Der ausgewählte Hub sollte jetzt unter dem Blatt **Linked IoT hubs** (Verknüpfte IoT Hubs) angezeigt werden. 



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und wählen Sie Ihren Device Provisioning-Dienst aus. Klicken Sie im oberen Bereich des Blatts **Alle Ressourcen** auf **Löschen**.  
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub-Instanz aus. Klicken Sie im oberen Bereich des Blatts **Alle Ressourcen** auf **Löschen**.  

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen IoT Hub und eine Instanz des Device Provisioning-Diensts bereitgestellt und die beiden Ressourcen verknüpft. Sie können mit dem Schnellstart zur Erstellung eines simulierten Geräts fortfahren, um zu erfahren, wie Sie diese Einrichtung zum Bereitstellen eines simulierten Geräts verwenden.

> [!div class="nextstepaction"]
> [Schnellstart zum Erstellen eines simulierten Geräts](./quick-create-simulated-device.md)

