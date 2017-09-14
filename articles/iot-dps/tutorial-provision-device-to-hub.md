---
title: "Bereitstellen eines Geräts mithilfe des Azure IoT Hub Device Provisioning-Diensts | Microsoft-Dokumentation"
description: "Bereitstellen Ihres Geräts für eine einzelne IoT Hub-Instanz mithilfe des Azure IoT Hub Device Provisioning-Diensts"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 266b9b7eb228744075627e1e80710e63c27880cc
ms.openlocfilehash: bf50699d2dc67294d554ba15713254a8b88d8ade
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---

# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Bereitstellen des Geräts für eine IoT Hub-Instanz mithilfe des Azure IoT Hub Device Provisioning-Diensts

Im vorherigen Tutorial haben Sie erfahren, wie ein Gerät eingerichtet wird, um eine Verbindung mit Ihrem Device Provisioning-Dienst herzustellen. In diesem Tutorial erfahren Sie, wie Sie mithilfe dieses Diensts Ihr Gerät durch **_Registrierungslisten_** für eine einzelne IoT Hub-Instanz bereitstellen. Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, konfigurieren Sie auf jeden Fall Ihr Gerät und das zugehörige *Hardwaresicherheitsmodul* (HSM), wie in dem Tutorial [Einrichten eines bereitzustellenden Geräts mithilfe des Azure IoT Hub Device Provisioning-Diensts](./tutorial-set-up-device.md) erläutert wird.


<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Registrieren des Geräts

Bei diesem Schritt werden die einzigartigen Sicherheitsartefakte des Geräts zum Device Provisioning-Dienst hinzugefügt. Zu diesen Sicherheitsartefakten gehören Folgende:

- Für TPM-basierte Geräte:
    - Der *Endorsement Key*, der für jeden TPM-Chip oder jede TPM-Simulation eindeutig ist. Weitere Informationen finden Sie im Artikel [Grundlegendes zum TPM Endorsement Key](https://technet.microsoft.com/library/cc770443.aspx).
    - Die *Registrierungs-ID*, die zur eindeutigen Identifizierung eines Geräts im Namespace/Bereich verwendet wird. Diese ist eventuell mit der Geräte-ID identisch. Die ID ist für jedes Gerät erforderlich. Bei TPM-basierten Geräten kann die Registrierungs-ID vom TMP selbst abgeleitet werden, z.B. ein SHA-256-Hash des TPM Endorsement Key.

    ![Registrierungsinformationen für das TPM im Portal](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- Für X.509-basierte Geräte:
    - Das [für den X.509-Chip oder die X.509-Simulation ausgestellte Zertifikat](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) in Form einer *PEM*- oder *CER*-Datei. Für die individuelle Registrierung müssen Sie für Ihr X.509-System das *Signaturzertifikat* verwenden, für Registrierungsgruppen dagegen das *Stammzertifikat*.

    ![Registrierungsinformationen für X.509 im Portal](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)


Es gibt zwei Möglichkeiten zum Registrieren des Geräts beim Device Provisioning-Dienst:

- **Registrierungsgruppen** Diese stellen Gruppen von Geräten dar, die einen bestimmten Nachweismechanismus gemeinsam nutzen. Es wird empfohlen, eine Registrierungsgruppe für eine große Anzahl von Geräten, die eine gewünschte Erstkonfiguration gemeinsam nutzen, oder für Geräte zu verwenden, die alle demselben Mandanten zugeordnet sind.

    ![Registrierungsgruppen für X.509 im Portal](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Individuelle Registrierungen** Diese stellen Einträge für ein einzelnes Gerät dar, das beim Device Provisioning-Dienst registriert werden kann. Individuelle Registrierungen verwenden entweder X.509-Zertifikate oder SAS-Tokens (in einem physischen oder virtuellen TPM) als Nachweismechanismen. Individuelle Registrierungen sollten für Geräte, die besondere Erstkonfigurationen erfordern, oder Geräte verwendet werden, die nur SAS-Tokens über das TPM oder das virtuelle TPM als Nachweismechanismus verwenden können. Bei individuellen Registrierungen ist möglicherweise die gewünschte IoT Hub-Geräte-ID angegeben.

Im Folgenden werden die Schritte zum Registrieren des Geräts im Portal beschrieben:

1. Beachten Sie die Sicherheitsartefakte für das HSM auf Ihrem Gerät. Möglicherweise müssen Sie in einer Entwicklungsumgebung die APIs verwenden, die im Abschnitt [Extrahieren von Sicherheitsartefakten](./tutorial-set-up-device.md#extractsecurity) des vorherigen Tutorials erwähnt werden.

1. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Device Provisioning-Dienst.

1. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Registrierungen verwalten** aus. Wählen Sie je nach Ihrer Geräteinstallation entweder die Registerkarte **Individuelle Registrierungen** oder **Registrierungsgruppen**. Klicken Sie oben auf die Schaltfläche **Hinzufügen**. Wählen Sie **TPM** oder **X.509** als *Mechanismus* zum Identitätsnachweis aus, und geben Sie wie zuvor erläutert die entsprechenden Sicherheitsartefakte ein. Sie können eine neue **IoT Hub-Geräte-ID** eingeben. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

1. Wenn das Gerät erfolgreich registriert ist, sollte es wie folgt im Portal angezeigt werden:

    ![Erfolgreiche TPM-Registrierung im Portal](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)


## <a name="start-the-device"></a>Starten des Geräts

An dieser Stelle ist Folgendes für die Geräteregistrierung eingerichtet:

1. Ihr Gerät oder eine Gruppe von Geräten ist bei Ihrem Device Provisioning-Dienst registriert, und 
2. Ihr Gerät ist mit dem HSM-Chip konfiguriert und kann durch die Anwendung mithilfe des Client-SDK des Device Provisioning-Diensts aufgerufen werden.

Starten Sie das Gerät, damit Ihre Clientanwendung die Registrierung bei Ihrem Device Provisioning-Dienst starten kann.  


## <a name="verify-the-device-is-registered"></a>Sicherstellen, dass das Gerät registriert ist

Nachdem Ihr Gerät gestartet wurde, sollten folgende Aktionen durchgeführt werden. Weitere Einzelheiten finden Sie in der TPM-Simulatorbeispielanwendung [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c). 

1. Das Gerät sendet eine Registrierungsanforderung an Ihren Device Provisioning-Dienst.
2. Bei TPM-Geräten sendet der Device Provisioning-Dienst eine Registrierungsherausforderung zurück, auf die Ihr Gerät antwortet. 
3. Nach erfolgreicher Registrierung sendet der Device Provisioning-Dienst den IoT Hub-URI, die Geräte-ID und den verschlüsselten Schlüssel an das Gerät zurück. 
4. Die IoT Hub-Clientanwendung auf dem Gerät stellt dann eine Verbindung mit Ihrem Hub her. 
5. Nachdem erfolgreich eine Verbindung mit dem Hub hergestellt wurde, sollte das Gerät im **Device Explorer** von IoT Hub angezeigt werden. 

    ![Erfolgreiche Herstellung einer Verbindung mit dem Hub im Portal](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie mehrere Geräte für Hubs mit Lastausgleich bereitgestellt werden. 

> [!div class="nextstepaction"]
> [Bereitstellen von Geräten für IoT Hubs mit Lastausgleich](./tutorial-provision-multiple-hubs.md)

