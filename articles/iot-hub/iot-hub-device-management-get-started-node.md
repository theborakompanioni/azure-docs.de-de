---
title: Erste Schritte mit der IoT Hub-Geräteverwaltung | Microsoft Docs
description: Tutorial für den Einstieg in die Azure IoT Hub-Geräteverwaltung mit C#. Implementieren Sie eine Geräteverwaltung mithilfe von Azure IoT Hub, C# und den Microsoft Azure IoT SDKs.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: dotnet
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2016
ms.author: juanpere

---
# Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mithilfe von Node.js (Vorschau)
[!INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Einführung
Um die Azure IoT Hub-Geräteverwaltung verwenden zu können, müssen Sie zunächst einen Azure IoT Hub erstellen, Geräte im IoT Hub bereitstellen, mehrere simulierte Geräte starten und diese Geräte in der Beispiel-UI für die Geräteverwaltung anzeigen. In diesem Tutorial werden die entsprechenden Schritte erläutert.

> [!NOTE]
> Die Geräteverwaltungsfunktionen stehen erst nach Erstellung eines neuen IoT Hubs zur Verfügung. Dies gilt auch, wenn bereits ein IoT Hub vorhanden ist, da diese IoT Hubs noch nicht über diese Funktionen verfügen. Sobald die Geräteverwaltung allgemein verfügbar ist, werden alle bereits vorhandenen IoT Hubs per Upgrade ebenfalls mit Geräteverwaltungsfunktionen ausgestattet.
> 
> 

## Voraussetzungen
In diesem Tutorial wird davon ausgegangen, dass Sie einen Ubuntu Linux-Entwicklungscomputer verwenden.

Für die Schritte muss die folgende Software installiert sein:

* Git
* gcc (Version 4.9 oder höher). Sie können prüfen, welche aktuelle Version in Ihrer Umgebung installiert ist, indem Sie den Befehl `gcc --version` verwenden. Informationen zur Aktualisierung Ihrer Version von gcc unter Ubuntu 14.04 finden Sie unter <http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04>.
* [CMake](https://cmake.org/download/) (Version 2.8 oder höher). Sie können prüfen, welche aktuelle Version in Ihrer Umgebung installiert ist, indem Sie den Befehl `cmake --version` verwenden.
* Node.js 6.1.0 oder höher. Installieren Sie Node.js für Ihre Plattform unter <https://nodejs.org/>.
* Ein aktives Azure-Abonnement. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

## Erstellen eines IoT Hubs mit Geräteverwaltungsfunktionen
Sie müssen einen IoT Hub mit Geräteverwaltungsfunktionen erstellen, mit dem Ihre simulierten Geräte eine Verbindung herstellen können. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1. Melden Sie sich auf dem [Azure-Portal] an.
2. Klicken Sie in der Navigationsleiste auf **Neu**, klicken Sie auf **Internet der Dinge** und dann auf **Azure IoT Hub**.
   
   ![][img-new-hub]
3. Wählen Sie im Blatt **IoT Hub** die gewünschte Konfiguration für Ihren IoT Hub.
   
   ![][img-configure-hub]
   
   * Geben Sie im Feld **Name** einen Namen für Ihren IoT Hub ein. Wenn der **Name** gültig und verfügbar ist, wird im Feld **Name** ein grünes Häkchen angezeigt.
   * Wählen Sie eine **Preis- und Skalierungsstufe** aus. Für dieses Tutorial ist keine bestimmte Stufe erforderlich.
   * Erstellen Sie in **Ressourcengruppe** eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen].
   * Aktivieren Sie das Kontrollkästchen zum Aktivieren der Geräteverwaltung. Wenn Sie das Kontrollkästchen **Geräteverwaltung aktivieren** nicht aktivieren, können die Beispiele nicht ausgeführt werden. Durch das Aktivieren von **Geräteverwaltung aktivieren** erstellen Sie eine IoT Hub-Vorschauversion, die nur in den Regionen „USA, Osten“, „Europa, Norden“ und „Asien, Osten“ unterstützt wird und nicht für Produktionsszenarien bestimmt ist. Es ist nicht möglich, Geräte zu und aus Hubs zu migrieren, für die die Geräteverwaltung aktiviert ist.
   * Wählen Sie unter **Standort** den Standort aus, an dem Ihr IoT Hub gehostet werden soll. Die IoT Hub-Geräteverwaltung ist nur in den Regionen „USA, Osten“, „Europa, Norden“ und „Asien, Osten“ verfügbar. Künftig wird sie in allen Regionen verfügbar sein.
4. Wenn Sie die Konfigurationsoptionen für Ihren IoT Hub ausgewählt haben, klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann einige Minuten dauern. Im **Startmenü** oder im Bereich **Benachrichtigungen** können Sie den Fortschritt überwachen und den Status überprüfen.
   
   ![][img-monitor]
5. Öffnen Sie nach erfolgreicher Erstellung des IoT Hub das Blatt für den neuen IoT Hub, notieren Sie sich den **Hostnamen**, und klicken Sie dann auf **Freigegebene Zugriffsrichtlinien**.
   
   ![][img-keys]
6. Klicken Sie auf die Richtlinie **iothubowner**, und kopieren Sie oder notieren Sie die Verbindungszeichenfolge im Blatt **iothubowner**. Kopieren Sie sie an einen Speicherort, auf den Sie später zugreifen können. Sie wird im weiteren Verlauf des Tutorials noch benötigt.
   
   > [!NOTE]
   > In Produktionsszenarien dürfen keine Anmeldeinformationen vom Typ **iothubowner** verwendet werden.
   > 
   > 
   
   ![][img-connection]

Damit haben Sie einen IoT Hub mit Geräteverwaltungsfunktionen erstellt. Die Verbindungszeichenfolge benötigen Sie im weiteren Verlauf des Tutorials.

## Erstellen der Beispiele und Bereitstellen von Geräten in Ihrem IoT Hub
In diesem Abschnitt wird ein Skript ausgeführt, mit dem das simulierte Gerät und die Beispiele erstellt werden und in der Geräteregistrierung Ihres IoT Hub ein Satz neuer Geräteidentitäten bereitgestellt wird. Ein Gerät kann nur dann eine Verbindung mit dem IoT Hub herstellen, wenn in der Geräteregistrierung ein Eintrag für das Gerät vorhanden ist.

Gehen Sie wie folgt vor, um die Beispiele zu erstellen und Geräte in Ihrem IoT Hub bereitzustellen:

1. Öffnen Sie eine Shell.
2. Klonen Sie das GitHub-Repository. **Das Zielverzeichnis für den Klonvorgang darf keine Leerzeichen enthalten.**
   
     ```
     git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
     ```
3. Navigieren Sie aus dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Verzeichnis **azure-iot-sdks/c/build\_all/linux**, und führen Sie den folgenden Befehl zum Installieren der Pakete mit den erforderlichen Komponenten und der abhängigen Bibliotheken aus:
   
     ```
     ./setup.sh
     ```
4. Navigieren Sie in dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Verzeichnis **azure-iot-sdks/node/service/samples**, und führen Sie den folgenden Befehl aus. Ersetzen Sie dabei den Platzhalterwert durch die Verbindungszeichenfolge aus dem vorherigen Abschnitt:
   
     ```
     ./setup.sh <IoT Hub Connection String>
     ```

Das Skript bewirkt Folgendes:

1. Es führt **cmake** aus, um die erforderlichen Dateien für die Erstellung des simulierten Geräts zu erstellen. Die ausführbare Datei befindet sich unter **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample**. Die Quelldateien befinden sich im Ordner **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample**.
2. Es erstellt die ausführbare Datei **iotdm\_simple\_sample** für das simulierte Gerät.
3. Es führt `npm install` aus, um die erforderlichen Pakete zu installieren.
4. Es führt `node generate_devices.js` aus, um Geräteidentitäten in Ihrem IoT Hub bereitzustellen. Die Geräte werden in **sampledevices.json** beschrieben. Nachdem die Geräte bereitgestellt wurden, werden die Anmeldeinformationen in der Datei **devicecreds.txt** (im Verzeichnis **azure-iot-sdks/node/service/samples**) gespeichert.

## Starten der simulierten Geräte
Nachdem die Geräte der Geräteregistrierung hinzugefügt wurden, können Sie nun die simulierten verwalteten Geräte starten. Sie müssen für jede im Azure IoT Hub bereitgestellte Geräteidentität ein simuliertes Gerät starten.

Navigieren Sie mit einer Shell zum Verzeichnis **azure-iot-sdks/node/service/samples**, und führen Sie Folgendes aus:

  ```
  ./simulate.sh
  ```

Dieses Skript gibt die Befehle aus, die Sie ausführen müssen, um **iotdm\_simple\_sample** für jedes in der Datei **devicecreds.txt** aufgeführte Gerät zu starten. Führen Sie die Befehle für jedes simulierte Gerät einzeln in einem separaten Terminalfenster aus. Das simulierte Gerät wird ausgeführt, bis Sie das Befehlsfenster schließen.

Die Anwendung **iotdm\_simple\_sample** basiert auf der Clientbibliothek für C der Azure IoT Hub-Geräteverwaltung. Dies ermöglicht die Erstellung von IoT-Geräten, die von Azure IoT Hub verwaltet werden können. Mithilfe dieser Bibliothek können Gerätehersteller Geräteeigenschaften melden und die für Geräteaufträge erforderlichen Ausführungsaktionen implementieren. Die Bibliothek wird im Rahmen des quelloffenen Azure IoT Hub-SDKs als Komponente bereitgestellt.

Beim Ausführen von **simulate.sh** wird im Ausgabefenster ein Datenstrom angezeigt. Diese Ausgabe zeigt den eingehenden und ausgehenden Datenverkehr sowie Anweisungen vom Typ **printf** in den anwendungsspezifischen Rückruffunktionen. Anhand dieser Ausgabe sehen Sie neben dem eingehenden und ausgehenden Datenverkehr auch, wie die Beispielanwendung mit den decodierten Paketen verfährt. Wenn das Gerät eine Verbindung mit dem IoT Hub herstellt, beginnt der Dienst automatisch mit der Überwachung der Geräteressourcen. Daraufhin initiiert die Clientbibliothek der IoT Hub-Geräteverwaltung die Geräterückrufe, um vom Gerät die neuesten Werte abzurufen.

Unten ist die Ausgabe der Beispielanwendung **iotdm\_simple\_sample** angegeben. Am Anfang sehen Sie die Erfolgsmeldung **REGISTERED**, die angibt, dass das Gerät mit der ID **Device11-7ce4a850** eine Verbindung mit dem IoT Hub herstellt.

> [!NOTE]
> Wenn Sie eine weniger ausführliche Ausgabe vorziehen, erstellen Sie die Retail-Konfiguration, und führen Sie sie aus.
> 
> 

![][img-output]

Achten Sie darauf, dass alle simulierten Geräte weiter ausgeführt werden, während Sie die folgenden Abschnitte durcharbeiten.

## Ausführen der Beispiel-UI für die Geräteverwaltung
Nachdem Sie nun einen IoT Hub bereitgestellt haben und mehrere simulierte Geräte ausgeführt werden und für die Verwaltung registriert sind, können Sie die Beispiel-UI für die Geräteverwaltung bereitstellen. Die Beispiel-UI für die Geräteverwaltung enthält ein Arbeitsbeispiel für die Nutzung der Geräteverwaltungs-APIs zum Erstellen einer interaktiven Benutzeroberfläche. Weitere Informationen zur Beispiel-UI für die Geräteverwaltung, z.B. [bekannte Probleme](https://github.com/Azure/azure-iot-device-management#knownissues), finden Sie im GitHub-Repository zur [Azure IoT-UI für die Geräteverwaltung][lnk-dm-github].

Führen Sie diese Schritte aus, um die Beispiel-UI für die Geräteverwaltung abzurufen, zu erstellen und auszuführen:

1. Öffnen Sie eine Shell.
2. Vergewissern Sie sich, dass Sie Node.js 6.1.0 oder höher gemäß dem Abschnitt „Voraussetzungen“ installiert haben, indem Sie `node --version` eingeben.
3. Klonen Sie das GitHub-Repository der Azure IoT-UI für die Gerätebereitstellung, indem Sie in der Shell den folgenden Befehl ausführen:
   
    ```
    git clone https://github.com/Azure/azure-iot-device-management.git
    ```
4. Führen Sie im Stammordner Ihrer geklonten Kopie des Repositorys der Azure IoT-UI für die Gerätebereitstellung den folgenden Befehl aus, um die abhängigen Pakete abzurufen:
   
    ```
    npm install
    ```
5. Führen Sie nach Abschluss des Installationsbefehls „npm“ den folgenden Befehl in der Shell aus, um den Code zu erstellen:
   
    ```
    npm run build
    ```
6. Verwenden Sie einen Text-Editor, um die Datei „user-config.json“ im Stamm des geklonten Ordners zu öffnen. Ersetzen Sie den Text „&lt;YOUR CONNECTION STRING HERE&gt;“ durch die IoT Hub-Verbindungszeichenfolge aus dem vorherigen Abschnitt, und speichern Sie die Datei.
7. Führen Sie in der Shell den folgenden Befehl aus, um die UX-App für die Geräteverwaltung zu starten:
   
    ```
    npm run start
    ```
8. Wenn an der Eingabeaufforderung die Meldung „Services have started“ angezeigt wird, können Sie einen Webbrowser öffnen und unter der folgenden URL zur Geräteverwaltungs-App navigieren, um die simulierten Geräte anzuzeigen: <http://127.0.0.1:3003>.
   
    ![][img-dm-ui]

Lassen Sie die simulierten Geräte und die Geräteverwaltungs-App laufen, während Sie mit dem nächsten Tutorial für die Geräteverwaltung fortfahren.

## Nächste Schritte
Informationen zu den weiteren ersten Schritten mit IoT Hub finden Sie unter [Erste Schritte mit dem Gateway-SDK][lnk-gateway-SDK].

Weitere Informationen zu den Features der Azure IoT Hub-Geräteverwaltung finden Sie im Tutorial [Erkunden der Azure IoT Hub-Geräteverwaltung mit der Beispiel-UI][lnk-sample-ui].

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started-node/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started-node/image2.png
[img-monitor]: media/iot-hub-device-management-get-started-node/image3.png
[img-keys]: media/iot-hub-device-management-get-started-node/image4.png
[img-connection]: media/iot-hub-device-management-get-started-node/image5.png
[img-output]: media/iot-hub-device-management-get-started-node/image6.png
[img-dm-ui]: media/iot-hub-device-management-get-started-node/dmui.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure-Portal]: https://portal.azure.com/
[Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-sample-ui]: iot-hub-device-management-ui-sample.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

<!---HONumber=AcomDC_0817_2016-->