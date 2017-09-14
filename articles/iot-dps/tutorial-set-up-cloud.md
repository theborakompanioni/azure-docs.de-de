---
title: "Einrichten einer Cloud für den Azure IoT Hub Device Provisioning-Dienst im Portal | Microsoft-Dokumentation"
description: "Automatische IoT Hub-Gerätebereitstellung im Azure-Portal"
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
ms.openlocfilehash: 088d127521ce89d3a82e30ad8797fe5746ae7e03
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---

# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Konfigurieren von Cloudressourcen für die Gerätebereitstellung mit dem IoT Hub Device Provisioning-Dienst

In diesem Tutorial wird gezeigt, wie die Cloud für die automatische Gerätebereitstellung mithilfe des IoT Hub Device Provisioning-Diensts eingerichtet wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines IoT Hub Device Provisioning-Diensts und Abrufen des ID-Bereichs mit dem Azure-Portal
> * Erstellen eines IoT Hubs
> * Verknüpfen von IoT Hub mit dem Device Provisioning-Dienst
> * Festlegen der Zuordnungsrichtlinie im Device Provisioning-Dienst

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Erstellen einer Device Provisioning-Dienstinstanz und Abrufen des ID-Bereichs

Führen Sie zum Erstellen einer neuen Device Provisioning-Dienstinstanz folgende Schritte durch:

1. Klicken Sie im Azure-Portal oben links auf **Neu**.
2. Geben Sie in das Suchfeld **Gerätebereitstellung** ein. 
3. Klicken Sie auf **IoT Hub Device Provisioning-Dienst**.
4. Geben Sie in das Formular **IoT Hub Device Provisioning-Dienst** folgende Informationen ein:
    
   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Name** | Ein beliebiger eindeutiger Name | -- | 
   | **Abonnement** | Ihr Abonnement  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |   

   ![Eingeben grundlegender Informationen zum DPS im Portal](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Klicken Sie auf **Erstellen**.
6. Der *ID-Bereich* dient zum Identifizieren von Registrierungs-IDs und gewährleistet, dass die Registrierungs-ID eindeutig ist. Klicken Sie zum Abrufen dieses Werts auf **Übersicht**, um die Seite **Zusammenfassung** für den Device Provisioning-Dienst zu öffnen. Kopieren Sie den Wert von **ID-Bereich** zur späteren Verwendung an einem temporären Speicherort.
7. Notieren Sie sich auch den Wert von **Dienstendpunkt**, oder kopieren Sie ihn zur späteren Verwendung an einem temporären Speicherort. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben nun Ihren IoT-Hub erstellt und verfügen über den Hostnamen und die IoT Hub-Verbindungszeichenfolge, die Sie für die weiteren Schritte in diesem Tutorial benötigen.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Verknüpfen des Device Provisioning-Diensts mit einer IoT Hub-Instanz

Der nächste Schritt besteht darin, den Device Provisioning-Dienst und IoT Hub zu verknüpfen, damit der IoT Hub Device Provisioning-Dienst bei diesem Hub Geräte registrieren kann. Der Dienst kann nur Geräte mit IoT Hubs bereitstellen, die mit dem Device Provisioning-Dienst verknüpft wurden. Führen Sie folgende Schritte durch:

1. Klicken Sie auf der Seite **Alle Ressourcen** auf die Device Provisioning-Dienstinstanz, die Sie zuvor erstellt haben.
2. Klicken Sie auf der Seite „Device Provisioning-Dienst“ auf **Verknüpfte IoT Hubs**.
3. Klicken Sie auf **Hinzufügen**.
4. Aktivieren Sie auf der Seite **Verknüpfung zu IoT Hub hinzufügen** die Optionsfelder, um anzugeben, ob sich der verknüpfte IoT Hub im aktuellen Abonnement oder in einem anderen Abonnement befindet. Wählen Sie dann im Feld **IoT Hub** den Namen von IoT Hub.
5. Klicken Sie auf **Speichern**.

   ![Verknüpfen des Hubnamens mit dem DPS im Portal](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Festlegen der Zuordnungsrichtlinie im Device Provisioning-Dienst

Die Zuordnungsrichtlinie ist eine Einstellung des IoT Hub Device Provisioning-Diensts, die festlegt, wie Geräte einer IoT Hub-Instanz zugewiesen werden. Es gibt drei unterstützte Zuordnungsrichtlinien: 

1. **Niedrigste Latenz**: Geräte werden basierend auf dem Hub mit der geringsten Latenz auf dem Gerät für eine IoT Hub-Instanz bereitgestellt.
2. **Gleichmäßig gewichtete Verteilung** (Standard): Bei verknüpften IoT Hubs ist die Wahrscheinlichkeit gleich hoch, dass ihnen Geräte bereitgestellt werden. Dies ist die Standardeinstellung. Wenn Sie nur für eine IoT Hub-Instanz Geräte bereitstellen, können Sie diese Einstellung beibehalten. 
3. **Statische Konfiguration über die Registrierungsliste**: Die Angabe der gewünschten IoT Hub-Instanz in der Registrierungsliste hat gegenüber der Zuordnungsrichtlinie auf Ebene des Device Provisioning-Diensts Vorrang.

Klicken Sie zum Festlegen der Zuordnungsrichtlinie auf der Seite „Device Provisioning-Dienst“ auf **Zuordnungsrichtlinie verwalten**. Stellen Sie sicher, dass die Zuordnungsrichtlinie auf **Gleichmäßig gewichtete Verteilung** (Standard) festgelegt ist. Wenn Sie Änderungen vornehmen, klicken Sie danach auf **Speichern**.

![Verwalten von Zuordnungsrichtlinien](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Tutorials in dieser Sammlung bauen auf diesem Tutorial auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Tutorial erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte durchführen, um alle in diesem Tutorial erstellten Ressourcen im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub Device Provisioning-Dienstinstanz aus. Klicken Sie im oberen Bereich der Seite **Alle Ressourcen** auf **Löschen**.  
2. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Alle Ressourcen**, und wählen Sie Ihre IoT Hub-Instanz aus. Klicken Sie im oberen Bereich der Seite **Alle Ressourcen** auf **Löschen**.
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines IoT Hub Device Provisioning-Diensts und Abrufen des ID-Bereichs mit dem Azure-Portal
> * Erstellen eines IoT Hubs
> * Verknüpfen von IoT Hub mit dem Device Provisioning-Dienst
> * Festlegen der Zuordnungsrichtlinie im Device Provisioning-Dienst

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihr Gerät für die Bereitstellung konfigurieren.

> [!div class="nextstepaction"]
> [Einrichten von Geräten für die Bereitstellung](tutorial-set-up-device.md)

