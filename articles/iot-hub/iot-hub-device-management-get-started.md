<properties
	pageTitle="Erste Schritte mit der IoT Hub-Geräteverwaltung | Microsoft Azure"
	description="Tutorial für den Einstieg in die Azure IoT Hub-Geräteverwaltung mit C#. Implementieren Sie eine Geräteverwaltung mithilfe von Azure IoT Hub, C# und den Microsoft Azure IoT SDKs."
	services="iot-hub"
	documentationCenter=".net"
	authors="juanjperez"
	manager="timlt"
	editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mithilfe von C# (Vorschau)

[AZURE.INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Einführung
Um die Azure IoT Hub-Geräteverwaltung verwenden zu können, müssen Sie zunächst einen Azure IoT Hub erstellen, Geräte im IoT Hub bereitstellen, mehrere simulierte Geräte starten und diese Geräte in der Beispiel-UI für die Geräteverwaltung anzeigen. In diesem Tutorial werden die entsprechenden Schritte erläutert.

> [AZURE.NOTE]  Die Geräteverwaltungsfunktionen stehen erst nach Erstellung eines neuen IoT Hubs zur Verfügung. Dies gilt auch, wenn bereits ein IoT Hub vorhanden ist, da dieser noch nicht über Geräteverwaltungsfunktionen verfügt. Sobald die Geräteverwaltung allgemein verfügbar ist, werden alle bereits vorhandenen IoT Hubs per Upgrade ebenfalls mit Geräteverwaltungsfunktionen ausgestattet.

## Voraussetzungen

In diesem Tutorial wird davon ausgegangen, dass Sie einen Windows-Entwicklungscomputer verwenden.

Für die Schritte muss Folgendes installiert sein:

- Microsoft Visual Studio 2015

- Git

- CMake (ab Version 2.8). CMake können Sie hier herunterladen: <https://cmake.org/download/>. Wählen Sie für einen PC unter Windows die Windows Installer-Option (MSI-Datei). Vergewissern Sie sich, dass das Kontrollkästchen zum Hinzufügen von CMake zur PATH-Variablen des aktuellen Benutzers aktiviert ist.

- Node.js 6.1.0 oder höher. Installieren Sie Node.js für Ihre Plattform unter <https://nodejs.org/>.

- Ein aktives Azure-Abonnement. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

## Erstellen eines IoT Hubs mit Geräteverwaltungsfunktionen

Sie müssen einen IoT Hub mit Geräteverwaltungsfunktionen erstellen, mit dem Ihre simulierten Geräte eine Verbindung herstellen können. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1.  Melden Sie sich beim [Azure-Portal] an.
2.  Klicken Sie in der Navigationsleiste auf **Neu**, klicken Sie auf **Internet der Dinge** und dann auf **Azure IoT Hub**.

	![][img-new-hub]

3.  Wählen Sie auf dem Blatt **IoT Hub** die gewünschte Konfiguration für Ihren IoT Hub.

	![][img-configure-hub]

  -   Geben Sie im Feld **Name** einen Namen für Ihren IoT Hub ein. Wenn der **Name** gültig und verfügbar ist, wird im Feld **Name** ein grünes Häkchen angezeigt.
  -   Wählen Sie eine **Preis- und Skalierungsstufe** aus. Für dieses Tutorial ist keine bestimmte Stufe erforderlich.
  -   Erstellen Sie in **Ressourcengruppe** eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen].
  -   Aktivieren Sie das Kontrollkästchen zum Aktivieren der Geräteverwaltung.
  -   Wählen Sie unter **Standort** den Standort aus, an dem Ihr IoT Hub gehostet werden soll. Die IoT Hub-Geräteverwaltung ist nur in den Regionen „USA, Osten“, „Europa, Norden“ und „Asien, Osten“ verfügbar. Künftig wird sie in allen Regionen verfügbar sein.

    > [AZURE.NOTE]  Wenn Sie das Kontrollkästchen zum Aktivieren der Geräteverwaltung nicht aktivieren, können die Beispiele nicht ausgeführt werden.

4.  Wenn Sie die Konfigurationsoptionen für Ihren IoT Hub ausgewählt haben, klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann einige Minuten dauern. Im **Startmenü** oder im Bereich **Benachrichtigungen** können Sie den Fortschritt überwachen und den Status überprüfen.

	![][img-monitor]

5.  Öffnen Sie nach erfolgreicher Erstellung des IoT Hub das Blatt für den neuen IoT Hub, notieren Sie sich den Hostnamen, und klicken Sie dann auf das Schlüsselsymbol.

	![][img-keys]

6.  Klicken Sie auf die Richtlinie **iothubowner**, und kopieren Sie oder notieren Sie die Verbindungszeichenfolge im Blatt **iothubowner**. Kopieren Sie sie an einen Speicherort, auf den Sie später zugreifen können. Sie wird im weiteren Verlauf des Tutorials benötigt.

 	> [AZURE.NOTE] In Produktionsszenarien dürfen keine Anmeldeinformationen vom Typ **iothubowner** verwendet werden.

	![][img-connection]

Damit haben Sie einen IoT Hub mit Geräteverwaltungsfunktionen erstellt. Die Verbindungszeichenfolge benötigen Sie im weiteren Verlauf des Tutorials.

## Erstellen der Beispiele und Bereitstellen von Geräten in Ihrem IoT Hub

In diesem Abschnitt wird ein Skript ausgeführt, das das simulierte Gerät und die Beispiele erstellt und in der Geräteregistrierung Ihres IoT Hubs einen Satz neuer Geräteidentitäten bereitstellt. Ein Gerät kann nur dann eine Verbindung mit dem IoT Hub herstellen, wenn in der Geräteregistrierung ein Eintrag für das Gerät vorhanden ist.

Gehen Sie wie folgt vor, um die Beispiele zu erstellen und Geräte in Ihrem IoT Hub bereitzustellen:

1.  Öffnen Sie die **Developer-Eingabeaufforderung für VS2015**.

2.  Klonen Sie das GitHub-Repository. **Das Zielverzeichnis für den Klonvorgang darf keine Leerzeichen enthalten.**

	  ```
	  git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
	  ```

3.  Navigieren Sie in dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Ordner **\\azure-iot-sdks\\csharp\\service\\samples**, und führen Sie den folgenden Befehl aus. Ersetzen Sie dabei den Platzhalterwert durch die Verbindungszeichenfolge aus dem vorherigen Abschnitt:

	  ```
	  setup.bat <IoT Hub Connection String>
	  ```

Das Skript bewirkt Folgendes:

1.  Es führt **cmake** aus, um eine Visual Studio 2015-Projektmappe für das simulierte Gerät zu erstellen. Die Projektdatei ist **azure-iot-sdks\\csharp\\service\\samples\\cmake\\iotdm\_client\\samples\\iotdm\_simple\_sample\\iotdm\_simple\_sample.vcxproj**. Die Quelldateien befinden sich im Ordner ***azure-iot-sdks\\c\\iotdm\_client\\samples\\iotdm\_simple\_sample**.

2.  Es erstellt das Projekt **iotdm\_simple\_sample.vcxproj** für das simulierte Gerät.

3.  Es erstellt die Geräteverwaltungsbeispiele **azure-iot-sdks\\csharp\\service\\samples\\GetStartedWithIoTDM\\GetStartedWithIoTDM.sln**.

4.  Es führt **GenerateDevices.exe** aus, um Geräteidentitäten in Ihrem IoT Hub bereitzustellen. Die Geräte werden in **sampledevices.json** (im Ordner **azure-iot-sdks\\node\\service\\samples**) beschrieben. Nach der Bereitstellung der Geräte werden die Anmeldeinformationen in der Datei **devicecreds.txt** (im Ordner **azure-iot-sdks\\csharp\\service\\samples\\bin**) gespeichert.

## Starten der simulierten Geräte

Nachdem die Geräte der Geräteregistrierung hinzugefügt wurden, können Sie nun die simulierten verwalteten Geräte starten. Für jede im Azure IoT Hub bereitgestellte Geräteidentität wird jeweils ein simuliertes Gerät gestartet.

Führen Sie über die Developer-Eingabeaufforderung im Ordner **\\azure-iot-sdks\\csharp\\service\\samples\\bin** Folgendes aus:

  ```
  simulate.bat
  ```

Dieses Skript führt für jedes in der Datei **devicecreds.txt** angegebene Gerät eine Instanz von **iotdm\_simple\_sample.exe** aus. Das simulierte Gerät wird ausgeführt, bis Sie das Befehlsfenster schließen.

Die Beispielanwendung **iotdm\_simple\_sample** basiert auf der Clientbibliothek für C der Azure IoT Hub-Geräteverwaltung. Dies ermöglicht die Erstellung von IoT-Geräten, die von Azure IoT Hub verwaltet werden können. Mithilfe dieser Bibliothek können Gerätehersteller Geräteeigenschaften melden und die für Geräteaufträge erforderlichen Ausführungsaktionen implementieren. Die Bibliothek wird im Rahmen des quelloffenen Azure IoT Hub-SDKs als Komponente bereitgestellt.

Wenn Sie **simulate.bat** ausführen, wird im Ausgabefenster ein Datenstrom angezeigt. Diese Ausgabe zeigt den eingehenden und ausgehenden Datenverkehr sowie Anweisungen vom Typ **printf** in den anwendungsspezifischen Rückruffunktionen. Dadurch sehen Sie neben dem eingehenden und ausgehenden Datenverkehr auch, wie die Beispielanwendung mit den decodierten Paketen verfährt. Wenn das Gerät eine Verbindung mit dem IoT Hub herstellt, beginnt der Dienst automatisch mit der Überwachung der Geräteressourcen. Daraufhin initiiert die Clientbibliothek der IoT Hub-Geräteverwaltung die Geräterückrufe, um vom Gerät die neuesten Werte abzurufen.

Im Anschluss folgt eine Ausgabe der Beispielanwendung **iotdm\_simple\_sample**. Am Anfang sehen Sie die Erfolgsmeldung **REGISTERED**, die angibt, dass das Gerät mit der ID **Device11-7ce4a850** eine Verbindung mit dem IoT Hub herstellt.

> [AZURE.NOTE]  Wenn Sie eine weniger ausführliche Ausgabe vorziehen, erstellen Sie die Retail-Konfiguration, und führen Sie sie aus.

![][img-output]

Achten Sie darauf, dass alle simulierten Geräte weiter ausgeführt werden, während Sie die folgenden Abschnitte durcharbeiten.

## Ausführen der Beispiel-UI für die Geräteverwaltung

Nachdem Sie nun einen IoT Hub bereitgestellt haben und mehrere simulierte Geräte ausgeführt werden und für die Verwaltung registriert sind, können Sie die Beispiel-UI für die Geräteverwaltung bereitstellen. Die Beispiel-UI für die Geräteverwaltung enthält ein Arbeitsbeispiel für die Nutzung der Geräteverwaltungs-APIs zum Erstellen einer interaktiven Benutzeroberfläche. Weitere Informationen zur Beispiel-UI für die Geräteverwaltung, z.B. [bekannte Probleme](https://github.com/Azure/azure-iot-device-management#knownissues), finden Sie im GitHub-Repository zur [Azure IoT-UI für die Geräteverwaltung][lnk-dm-github].

Führen Sie die folgenden Schritte aus, um die Beispiel-UI für die Geräteverwaltung abzurufen, zu erstellen und auszuführen:

1. Öffnen Sie eine **Eingabeaufforderung**.

2. Vergewissern Sie sich, dass Sie Node.js 6.1.0 oder höher gemäß dem Abschnitt „Voraussetzungen“ installiert haben, indem Sie `node --version` eingeben.

3. Klonen Sie das GitHub-Repository zur Azure IoT-UI für die Gerätebereitstellung, indem Sie den folgenden Befehl ausführen:

	```
	git clone https://github.com/Azure/azure-iot-device-management.git
	```
	
4. Führen Sie im Stammordner Ihrer geklonten Kopie des Repositorys der Azure IoT-UI für die Gerätebereitstellung den folgenden Befehl aus, um die abhängigen Pakete abzurufen:

	```
	npm install
	```

5. Führen Sie nach Abschluss des Installationsbefehls „npm“ den folgenden Befehl aus, um den Code zu erstellen:

	```
	npm run build
	```

6. Verwenden Sie einen Text-Editor, um die Datei „user-config.json“ im Stamm des geklonten Ordners zu öffnen. Ersetzen Sie den Text „&lt;YOUR CONNECTION STRING HERE&gt;“ durch die IoT Hub-Verbindungszeichenfolge aus dem vorherigen Abschnitt, und speichern Sie die Datei.

7. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um die UX-App für die Geräteverwaltung zu starten:

	```
	npm run start
	```

8. Wenn in der Eingabeaufforderung die Meldung „Services have started“ angezeigt wird, können Sie einen Webbrowser öffnen (Edge/IE 11+/Safari/Chrome werden derzeit unterstützt) und unter der folgenden URL zur Geräteverwaltungs-App navigieren, um die simulierten Geräte anzuzeigen: <http://127.0.0.1:3003>.

	![][img-dm-ui]

Lassen Sie die simulierten Geräte und die Geräteverwaltungs-App laufen, während Sie mit dem nächsten Tutorial für die Geräteverwaltung fortfahren.


## Nächste Schritte

Informationen zu den weiteren ersten Schritten mit IoT Hub finden Sie unter [Erste Schritte mit dem Gateway-SDK][lnk-gateway-SDK].

Weitere Informationen zu den Features der Azure IoT Hub-Geräteverwaltung finden Sie im Tutorial [Erkunden der Azure IoT Hub-Geräteverwaltung mit der Beispiel-UI][lnk-sample-ui].

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started/image2.png
[img-monitor]: media/iot-hub-device-management-get-started/image3.png
[img-keys]: media/iot-hub-device-management-get-started/image4.png
[img-connection]: media/iot-hub-device-management-get-started/image5.png
[img-output]: media/iot-hub-device-management-get-started/image6.png
[img-dm-ui]: media/iot-hub-device-management-get-started/dmui.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure-Portal]: https://portal.azure.com/
[Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-sample-ui]: iot-hub-device-management-ui-sample.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

<!---HONumber=AcomDC_0720_2016-->