---
title: "Gewusst wie: Verwalten der Geräteregistrierungen für Azure IoT Hub | Microsoft-Dokumentation"
description: "Gewusst wie: Verwalten der Geräteregistrierungen für Ihren DPS-Dienst im Azure-Portal"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 266b9b7eb228744075627e1e80710e63c27880cc
ms.openlocfilehash: c230e73f83d8acd0f142e037f70a80c9e0e4107e
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---

# <a name="how-to-manage-device-enrollments-in-the-iot-hub-device-provisioning-service"></a>Gewusst wie: Verwalten der Geräteregistrierungen im IoT Hub Device Provisioning-Dienst

Eine *Geräteregistrierung* erstellt einen Datensatz eines einzelnen Geräts oder einer Gruppe von Geräten, die sich zu einem beliebigen Zeitpunkt beim Azure IoT Hub Device Provisioning-Dienst registrieren können. Der Registrierungsdatensatz enthält die erste gewünschte Konfiguration für die Geräte im Rahmen dieser Registrierung, einschließlich des gewünschten IoT Hubs. Dieser Artikel zeigt Ihnen die Verwaltung von Geräteregistrierungen für Ihren Bereitstellungsdienst.


## <a name="create-a-device-enrollment"></a>Erstellen einer Geräteregistrierung

Sie haben zwei Möglichkeiten, Ihre Geräte beim Bereitstellungsdienst zu registrieren:

1. Eine **Registrierungsgruppe** ist ein Eintrag für eine Gruppe von Geräten mit einem gemeinsamen Nachweismechanismus von X. 509-Zertifikaten, signiert von der gleichen Stammzertifizierungsstelle. Es empfiehlt sich, eine Registrierungsgruppe für eine große Anzahl von Geräten, die eine gewünschte Erstkonfiguration gemeinsam nutzen, oder für Geräte zu verwenden, die alle demselben Mandanten zugeordnet sind. Beachten Sie, dass Sie nur Geräte registrieren können, die den X. 509-Nachweismechanismus als *Registrierungsgruppen* verwenden. 

    Sie können mit den folgenden Schritten im Portal eine Registrierungsgruppe für eine Gruppe von Geräten erstellen.

    1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
    2. Klicken Sie auf den Gerätebereitstellungsdienst, bei dem Sie Ihr Gerät aus der Liste der Ressourcen registrieren möchten.
    3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Registrierungsgruppen**.
    4. Klicken Sie ganz oben auf die Schaltfläche **Hinzufügen**, und geben Sie die erforderlichen Angaben für den Registrierungslisteneintrag ein. Laden Sie das Stammzertifikat für die Gruppe von Geräten hoch. 
    5. Klicken Sie auf **Speichern**. Bei der erfolgreichen Erstellung Ihrer Registrierungsgruppe wird der Gruppenname unter der Registerkarte **Registrierungsgruppen** angezeigt. 

        ![Registrierungsgruppe im Portal](./media/how-to-manage-enrollments/group-enrollment.png)

    
2. Eine **individuelle Registrierung** ist ein Eintrag für ein einzelnes Gerät, das registriert werden kann. Individuelle Registrierungen verwenden entweder X.509-Zertifikate oder SAS-Token (in einem physischen oder virtuellen TPM) als Nachweismechanismen. Individuelle Registrierungen sollten für Geräte, die besondere Erstkonfigurationen erfordern, oder Geräte verwendet werden, die nur SAS-Tokens über das TPM oder das virtuelle TPM als Nachweismechanismus verwenden können. Bei individuellen Registrierungen ist möglicherweise die gewünschte IoT Hub-Geräte-ID angegeben.

    Sie können eine individuelle Registrierung im Portal mithilfe der folgenden Schritte erstellen. 

    1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
    2. Klicken Sie auf den Gerätebereitstellungsdienst, bei dem Sie Ihr Gerät aus der Liste der Ressourcen registrieren möchten.
    3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Individuelle Registrierungen**.
    4. Klicken Sie ganz oben auf die Schaltfläche **Hinzufügen**. 
    5. Wählen Sie den Sicherheitsmechanismus für das Gerät, und geben Sie die erforderlichen Informationen für den Registrierungseintrag ein. Laden Sie ein signiertes Zertifikat hoch, wenn Ihr Gerät X.509 implementiert. 
    6. Klicken Sie auf **Speichern**. Bei der erfolgreichen Erstellung Ihrer Registrierungsgruppe sollte Ihr Gerät unter der Registerkarte **Individuelle Registrierungen** angezeigt werden. 

        ![Individuelle Registrierung im Portal](./media/how-to-manage-enrollments/individual-enrollment.png)


## <a name="update-an-enrollment-entry"></a>Aktualisieren eines Registrierungseintrags
Sie können einen vorhandenen Registrierungseintrag im Portal mit den folgenden Schritten aktualisieren.

1. Öffnen Sie Ihren Gerätebereitstellungsdienst im Azure-Portal, und klicken Sie auf **Registrierungen verwalten**. 
2. Navigieren Sie zu dem Registrierungseintrag, den Sie ändern möchten. Klicken Sie auf den Eintrag, worauf zusammenfassende Informationen zu Ihrer Geräteregistrierung geöffnet werden. 
3. Auf dieser Seite können Sie Elemente außer Sicherheitstyp und Anmeldeinformationen ändern, z.B. den IoT Hub, mit dem das Gerät verknüpft werden soll, sowie die Geräte-ID. Sie können auch den anfänglichen Gerätezwillingsstatus ändern. 
4. Klicken Sie bei Abschluss auf **Speichern**, um die Geräteregistrierung zu aktualisieren. 

    ![Aktualisieren der Registrierung im Portal](./media/how-to-manage-enrollments/update-enrollment.png)


## <a name="remove-a-device-enrollment"></a>Entfernen einer Geräteregistrierung
In Fällen, in denen Ihre Geräte nicht in einem IoT Hub bereitgestellt werden müssen, können Sie den zugehörigen Registrierungseintrag im Portal mithilfe der folgenden Schritte entfernen.

1. Öffnen Sie Ihren Gerätebereitstellungsdienst im Azure-Portal, und klicken Sie auf **Registrierungen verwalten**. 
2. Navigieren Sie zu dem Registrierungseintrag, den Sie entfernen möchten, und wählen Sie ihn aus. 
3. Klicken Sie ganz oben auf die Schaltfläche **Löschen**, und bestätigen Sie nach Aufforderung mit **Ja**. 
5. Sobald die Aktion abgeschlossen ist, sehen Sie, dass Ihr Eintrag aus der Geräteregistrierungsliste entfernt wurde. 
 
    ![Entfernen der Registrierung im Portal](./media/how-to-manage-enrollments/remove-enrollment.png)




