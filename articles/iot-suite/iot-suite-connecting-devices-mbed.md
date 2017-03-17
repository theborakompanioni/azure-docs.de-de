---
title: "Verbinden eines Geräts mit C unter mbed | Microsoft Docs"
description: "Beschreibt, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung auf einem mbed-Gerät ausführen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: df9772796796f7383aafc583b01f299a53679d88
ms.openlocfilehash: 12535cbb6fa63c24dd63903380d697f8f38db6f9
ms.lasthandoff: 02/27/2017


---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (mbed)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Erstellen und Ausführen der C-Beispielprojektmappe

Im Folgenden werden die Schritte zum Verbinden eines [mbed-fähigen Freescale FRDM-K64F-Geräts][lnk-mbed-home] mit der Remoteüberwachungslösung beschrieben.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Verbinden des mbed-Geräts mit Ihrem Netzwerk und Desktopcomputer

1. Verbinden Sie das mbed-Gerät mithilfe eines Ethernet-Kabels mit Ihrem Netzwerk. Dieser Schritt ist erforderlich, da die Beispielanwendung Zugriff auf das Internet benötigt.

1. Informationen zum Verbinden des mbed-Geräts mit Ihrem Desktop-PC finden Sie unter [Erste Schritte mit mbed][lnk-mbed-getstarted].

1. Wenn auf Ihrem Desktop-PC Windows ausgeführt wird, finden Sie unter [PC-Konfiguration][lnk-mbed-pcconnect] weitere Informationen zum Konfigurieren des mbed-Gerätezugriffs über den seriellen Anschluss.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Erstellen eines mbed-Projekts und Importieren des Beispielcodes

Führen Sie diese Schritte aus, um einem mbed-Projekt Beispielcode hinzuzufügen. Sie importieren das Remoteüberwachungs-Startprojekt und ändern dann das Projekt so, dass das MQTT-Protokoll anstelle des AMQP-Protokolls verwendet wird. Derzeit müssen Sie das MQTT-Protokoll verwenden, um die Geräteverwaltungsfeatures von IoT Hub einzusetzen.

1. Wechseln Sie in Ihrem Webbrowser zur [Entwicklerwebsite](https://developer.mbed.org/)von mbed.org. Wenn Sie sich noch nicht registriert haben, können Sie die Option zum Erstellen eines Kontos verwenden (kostenlos). Melden Sie sich andernfalls mit Ihren Anmeldeinformationen an. Klicken Sie anschließend rechts oben auf der Seite auf **Compiler** . Dadurch gelangen Sie zur Oberfläche *Arbeitsbereich*.

1. Stellen Sie sicher, dass die von Ihnen verwendete Hardwareplattform in der oberen rechten Ecke des Fensters angezeigt wird, oder klicken Sie auf das Symbol in der rechten Ecke, um Ihre Hardwareplattform auszuwählen.

1. Klicken Sie im Hauptmenü auf **Importieren** . Klicken Sie dann auf **Hier klicken, um von URL zu importieren**.
   
    ![Starten des Imports in den mbed-Arbeitsbereich][6]

1. Geben Sie im Popupfenster den Link zum Beispielcode „https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/“ ein, und klicken Sie dann auf **Importieren**.
   
    ![Importieren von Beispielcode in mbed-Arbeitsbereich][7]

1. Im Fenster des mbed-Compilers sehen Sie, dass während des Imports dieses Projekts auch verschiedene Bibliotheken importiert werden. Einige werden vom Azure IoT-Team bereitgestellt und verwaltet ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), bei anderen handelt es sich um Bibliotheken von Drittanbietern, die im Katalog mit den mbed-Bibliotheken verfügbar sind.
   
    ![Anzeigen des mbed-Projekts][8]

1. Klicken Sie mit der rechten Maustaste im **Programmarbeitsbereich** auf die **iothub\_amqp\_transport**-Bibliothek, klicken Sie auf **Löschen** und dann zum Bestätigen auf **OK**.

1. Klicken Sie mit der rechten Maustaste im **Programmarbeitsbereich** auf die **azure\_amqp\_c**-Bibliothek, klicken Sie auf **Löschen** und dann zum Bestätigen auf **OK**.

1. Klicken Sie mit der rechten Maustaste im **Programmarbeitsbereich** auf das **remote_monitoring**-Projekt, wählen Sie **Bibliothek importieren** und dann **Von URL**.
   
    ![Starten des Bibliotheksimports in den mbed-Arbeitsbereich][6]

1. Geben Sie im Popupfenster den Link „https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/“ für die MQTT-Transportbibliothek ein, und klicken Sie dann auf **Importieren**.
   
    ![Importieren der Bibliothek in den mbed-Arbeitsbereich][12]

1. Wiederholen Sie den vorherigen Schritt, um die MQTT-Bibliothek aus „https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/“ hinzuzufügen.

1. Ihr Arbeitsbereich sieht nun folgendermaßen aus:

    ![Anzeigen des mbed-Arbeitsbereichs][13]

1. Öffnen Sie die Datei remote\_monitoring\remote_monitoring.c, und ersetzen Sie die vorhandenen `#include`-Anweisungen durch folgenden Code:

    ```
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. Löschen Sie den gesamten übrigen Code in der Datei remote\_monitoring\remote\_monitoring.c.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Fügen Sie Code zum Aufrufen der Funktion **remote\_monitoring\_run** hinzu, erstellen Sie anschließend die Geräteanwendung, und führen Sie sie aus.

1. Fügen Sie eine **main**-Funktion mit folgendem Code am Ende der Datei remote\_monitoring.c hinzu, um die Funktion **remote\_monitoring\_run** aufzurufen:
   
    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klicken Sie auf **Kompilieren** , um das Programm zu erstellen. Sie können etwaige Warnungen ignorieren, aber wenn während der Erstellung Fehler auftreten, sollten Sie diese vor dem Fortfahren beheben.

1. Wenn die Erstellung erfolgreich ist, generiert die mbed-Compilerwebsite eine BIN-Datei mit dem Namen Ihres Projekts und lädt sie auf Ihren lokalen Computer herunter. Kopieren Sie die BIN-Datei auf das Gerät. Durch das Speichern der BIN-Datei auf dem Gerät wird dieses neu gestartet, und das in der BIN-Datei enthaltene Programm wird ausgeführt. Sie können das Programm zu einem beliebigen Zeitpunkt starten, indem Sie die Schaltfläche zum Zurücksetzen am mbed-Gerät drücken.

1. Stellen Sie mit einer SSH-Clientanwendung eine Verbindung mit dem Gerät her, z. B. PuTTY. Sie können ermitteln, welcher serielle Port von Ihrem Gerät verwendet wird, indem Sie dies im Windows-Geräte-Manager überprüfen.
   
    ![][11]

1. Klicken Sie in PuTTY auf den Verbindungstyp **Seriell**. Das Gerät stellt in der Regel eine Verbindung mit 9600 Baud her. Geben Sie also in das Feld **Geschwindigkeit** den Wert „9600“ ein. Klicken Sie dann auf **Öffnen**.

1. Das Programm wird gestartet. Unter Umständen müssen Sie das Board zurücksetzen (drücken Sie STRG+UNTBR oder die Reset-Taste des Boards), falls das Programm nach dem Herstellen der Verbindung nicht automatisch gestartet wird.
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration

