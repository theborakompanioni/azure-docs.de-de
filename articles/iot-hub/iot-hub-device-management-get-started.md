<properties
	pageTitle="Erste Schritte mit der IoT Hub-Geräteverwaltung | Microsoft Azure"
	description="Tutorial für den Einstieg in die Azure IoT Hub-Geräteverwaltung mit C#. Implementieren Sie eine Geräteverwaltung mithilfe von Azure IoT Hub, C# und den Microsoft Azure IoT SDKs."
	services="iot-hub"
	documentationCenter=".net"
	authors="ellenfosborne"
	manager="timlt"
	editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Erste Schritte mit der Azure IoT Hub-Geräteverwaltung (Vorschau)

## Einführung
Um die Azure IoT Hub-Geräteverwaltung verwenden zu können, müssen Sie zunächst einen Azure IoT Hub erstellen, Geräte im IoT Hub bereitstellen und mehrere simulierte Geräten starten. In diesem Tutorial werden die entsprechenden Schritte erläutert.

> [AZURE.NOTE]  Die Geräteverwaltungsfunktionen stehen erst nach Erstellung eines neuen IoT Hubs zur Verfügung. Dies gilt auch, wenn bereits ein IoT Hub vorhanden ist, da dieser noch nicht über Geräteverwaltungsfunktionen verfügt. Sobald die Geräteverwaltung allgemein verfügbar ist, werden alle bereits vorhandenen IoT Hubs per Upgrade ebenfalls mit Geräteverwaltungsfunktionen ausgestattet.

## Voraussetzungen

Für die Schritte muss Folgendes installiert sein:

- Microsoft Visual Studio 2015
- Git
- CMake (ab Version 2.8). CMake können Sie hier herunterladen: <https://cmake.org/download/>. Wählen Sie für einen PC unter Windows die Windows Installer-Option (MSI-Datei). Vergewissern Sie sich, dass das Kontrollkästchen zum Hinzufügen von CMake zur PATH-Variablen des aktuellen Benutzers aktiviert ist.
- Ein aktives Azure-Abonnement.

	Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

## Erstellen eines IoT Hubs mit Geräteverwaltungsfunktionen

Sie müssen einen IoT Hub mit Geräteverwaltungsfunktionen erstellen, mit dem Ihre simulierten Geräte eine Verbindung herstellen können. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1.  Melden Sie sich beim [Azure-Portal] an.
2.  Klicken Sie in der Navigationsleiste auf **Neu**, klicken Sie auf **Internet der Dinge** und dann auf **Azure IoT Hub**.

	![][img-new-hub]

3.  Wählen Sie auf dem Blatt **IoT Hub** die gewünschte Konfiguration für Ihren IoT Hub aus.

	![][img-configure-hub]

  -   Geben Sie im Feld **Name** einen Namen für Ihren IoT Hub ein. Wenn der **Name** gültig und verfügbar ist, wird im Feld **Name** ein grünes Häkchen angezeigt.
  -   Wählen Sie eine **Preis- und Skalierungsstufe** aus. Für dieses Tutorial ist keine bestimmte Stufe erforderlich.
  -   Erstellen Sie in **Ressourcengruppe** eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen].
  -   Aktivieren Sie das Kontrollkästchen zum Aktivieren der Geräteverwaltung.
  -   Wählen Sie unter **Standort** den Standort aus, an dem Ihr IoT Hub gehostet werden soll. Die IoT Hub-Geräteverwaltung ist nur in den Regionen „USA, Osten“, „Europa, Norden“ und „Asien, Osten“ verfügbar.

4.  Wenn Sie die Konfigurationsoptionen für Ihren IoT Hub ausgewählt haben, klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann einige Minuten dauern. Den Status können Sie im Startmenü oder im Benachrichtigungsbereich überwachen.

	![][img-monitor]

5.  Öffnen Sie nach erfolgreicher Erstellung des IoT Hubs das Blatt für den neuen IoT Hub, notieren Sie sich den Hostnamen, und klicken Sie dann auf das Schlüsselsymbol.

	![][img-keys]

6.  Klicken Sie auf die Richtlinie **iothubowner**, und kopieren Sie oder notieren Sie die Verbindungszeichenfolge im Blatt **iothubowner**. Kopieren Sie sie an einen Speicherort, auf den Sie später zugreifen können. Sie wird im weiteren Verlauf des Tutorials benötigt.

 	> [AZURE.NOTE] In Produktionsszenarien dürfen keine Anmeldeinformationen vom Typ **iothubowner** verwendet werden.

	![][img-connection]

Damit haben Sie einen IoT Hub mit Geräteverwaltungsfunktionen erstellt. Die Verbindungszeichenfolge benötigen Sie im weiteren Verlauf des Tutorials.

## Erstellen der Beispiele und Bereitstellen von Geräten in Ihrem IoT Hub

In diesem Abschnitt wird ein Skript ausgeführt, das das simulierte Gerät und die Beispiele erstellt und in der Geräteregistrierung Ihres IoT Hubs einen Satz neuer Geräteidentitäten bereitstellt. Ein Gerät kann nur dann eine Verbindung mit dem IoT Hub herstellen, wenn in der Geräteregistrierung ein Eintrag für das Gerät vorhanden ist.

Gehen Sie wie folgt vor, um die Beispiele zu erstellen und Geräte in Ihrem IoT Hub bereitzustellen:

1.  Öffnen Sie die Developer-Eingabeaufforderung für VS2015.

2.  Klonen Sie das GitHub-Repository. **Das Zielverzeichnis für den Klonvorgang darf keine Leerzeichen enthalten.**

  ```
  git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
  ```

3.  Navigieren Sie in dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Ordner **\\azure-iot-sdks\\csharp\\service\\samples**, und führen Sie den folgenden Befehl aus. Ersetzen Sie dabei den Platzhalterwert durch die Verbindungszeichenfolge aus dem vorherigen Abschnitt:

  ```
  setup.bat <IoT Hub Connection String>
  ```

Das Skript bewirkt Folgendes:

1.  Es führt **cmake** aus, um eine Visual Studio 2015-Projektmappe für das simulierte Gerät zu erstellen. Projektdatei: **azure-iot-sdks\\csharp\\service\\samples\\cmake\\iotdm\_client\\samples\\iotdm\_simple\_sample\\iotdm\_simple\_sample.vcxproj**. Die Quelldateien befinden sich im Ordner ****azure-iot-sdks\\c\\iotdm\_client\\samples\\iotdm\_simple\_sample**.

2.  Es erstellt das Projekt **iotdm\_simple\_sample.vcxproj** für das simulierte Gerät.

3.  Es erstellt Geräteverwaltungsbeispiele: **azure-iot-sdks\\csharp\\service\\samples\\GetStartedWithIoTDM\\GetStartedWithIoTDM.sln**.

4.  Es führt **GenerateDevices.exe** aus, um Geräteidentitäten in Ihrem IoT Hub bereitzustellen. Die Geräte werden in **sampledevices.json** (im Ordner **azure-iot-sdks\\node\\service\\samples**) beschrieben. Nach der Bereitstellung der Geräte werden die Anmeldeinformationen in der Datei **devicecreds.txt** (im Ordner **azure-iot-sdks\\csharp\\service\\samples\\bin** Ordner) gespeichert.

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

Achten Sie darauf, dass die simulierten Geräte weiter ausgeführt werden, wenn Sie die Tutorials unter „Nächste Schritte“ ausführen.

## Nächste Schritte

Weitere Informationen zu den Geräteverwaltungsfeatures von Azure IoT Hub erhalten Sie in den folgenden Tutorials:

- [Gewusst wie: Verwenden des Gerätezwillings][lnk-tutorial-twin]

- [Gewusst wie: Suchen nach Gerätezwillingen mithilfe von Abfragen][lnk-tutorial-queries]

- [Gewusst wie: Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen][lnk-tutorial-jobs]

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started/image2.png
[img-monitor]: media/iot-hub-device-management-get-started/image3.png
[img-keys]: media/iot-hub-device-management-get-started/image4.png
[img-connection]: media/iot-hub-device-management-get-started/image5.png
[img-output]: media/iot-hub-device-management-get-started/image6.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure-Portal]: https://portal.azure.com/
[Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen]: ../azure-portal/resource-group-portal.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md

<!---HONumber=AcomDC_0504_2016-->