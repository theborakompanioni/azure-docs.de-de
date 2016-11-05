---
title: Verwenden der Benutzeroberfläche für die IoT Hub-Geräteverwaltung | Microsoft Docs
description: Exemplarische Vorgehensweise zur Verwendung der Benutzeroberfläche für die Azure IoT Hub-Geräteverwaltung
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: dobett

---
# Erkunden der Azure IoT Hub-Geräteverwaltung mithilfe der Beispielbenutzeroberfläche
Über die Interaktion mit der Beispielbenutzeroberfläche für die Geräteverwaltung können Sie sich mit den Konzepten und Möglichkeiten vertraut machen, die in den Artikeln zur Azure IoT Hub-Geräteverwaltung ([Übersicht][lnk-dm-overview] und [Erste Schritte][lnk-get-started]) behandelt werden. Dieser Artikel begleitet Sie durch die drei wesentlichen Geräteverwaltungskonzepte (*Gerätezwilling*, *Geräteabfragen* und *Geräteaufträge*) entsprechend der Darstellung der Beispiel-Webbenutzeroberfläche für die Geräteverwaltung.

Entwickler, die eine eigene interaktive Geräteverwaltungsumgebung einrichten möchten, können eine Verzweigung der Beispielcodebasis für die Benutzeroberfläche erstellen und als Grundlage eines benutzerdefinierten Projekts nutzen. Sie können den vollständigen Projektcode und die Infodateien durchgehen, in denen zusätzliche Entwicklerfunktionen im GitHub-Repository [Azure IoT device management UI][lnk-dm-github] detailliert vorgestellt werden.

## Voraussetzungen
Bevor Sie dieses Tutorial starten, sollten Sie die Schritte im Artikel [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung][lnk-get-started] ausführen. Falls noch nicht geschehen, kehren Sie zurück und führen alle Schritte in diesem Artikel aus, ehe Sie fortfahren.

Wenn Sie das Tutorial „Erste Schritte“ abgeschlossen haben, wird auf Ihrem Testsystem Folgendes ausgeführt:

* Sechs simulierte **Iotdm\_simple\_sample**-Geräte, die in Konsolen- bzw. Terminalfenstern ausgeführt werden und für die die Meldung „Erfolgreich registriert“ angezeigt wird.
* Die erstellte Geräteverwaltungsanwendung mit der Beispielbenutzeroberfläche, die unter <http://127.0.0.1:3003> ausgeführt wird.

## Standardansicht für Geräte
Die Standardstartseite der Beispielbenutzeroberfläche für die Geräteverwaltung ist die Ansicht **Geräte**, die die folgenden fünf Komponenten enthält:

![][1]

1. *Navigationsschaltflächen*: Ansicht **Geräte** (ausgewählt), Ansicht **Auftragsverlauf** und Ansicht **Gerät hinzufügen**.
2. *Gerätesuche*: Sie können ein einzelnes Gerät anhand der Geräte-ID suchen und bearbeiten.
3. *Geräteaktionen*: Aktion **Bearbeiten**, Aktion **Löschen** und Aktion **Exportieren**.
4. *Geräteaufträge*: **Gerät neu starten** **Firmware-Update** und **Zurücksetzung auf Werkseinstellungen**.
5. *Geräterasterfilter*: Filter-Editor zum Erstellen und Speichern benutzerdefinierter Ansichten des Geräterasters.
6. *Geräteraster*: Zeigt alle bei Ihrer IoT Hub-Instanz registrierten Geräte mit den Standardeigenschaften (**Geräte-ID**, **Status** und **Tags**).

Im Artikel [Überblick über die Azure IoT Hub-Geräteverwaltung][lnk-dm-overview] wurde das Konzept *Gerätezwilling* eingeführt, das ein physisches (oder simuliertes) Gerät in Azure IoT Hub darstellt. Im Geräteraster können Sie in der Geräteliste ein beliebiges registriertes Gerät auswählen, um den Gerätezwilling für dieses Gerät anzuzeigen und zu bearbeiten.

Um diese Detailansicht Ihres ersten simulierten Geräts (**Device11-7ce4a850**) anzuzeigen, wählen Sie die entsprechende Zeile mit dem Gerät aus. Klicken Sie dann auf die Schaltfläche **Bearbeiten** (Sie können auch auf die Zeile doppelklicken oder die Geräte-ID in das Suchfeld eingeben).

Sie sehen nun die vollständige Darstellung der Gerätezwillingskomponenten, in der Sie, wie nachstehend erläutert, Eigenschaften mit Schreibzugriff aktualisieren und andere Gerätevorgänge ausführen können:

![][2]

1. **Gerätezwilling bearbeiten**: Hier finden Sie die Umschaltfläche **Aktiviert/Deaktiviert** für das Gerät.
2. **Diensteigenschaften**: Diese Option schließt **Tags** von Geräten ein.
3. **Geräteeigenschaften**: Klicken Sie hierauf, um diesen Abschnitt aufzuklappen.
4. Schaltfläche **Aktualisieren**: Dient zum Abrufen der neuesten Gerätezwillingseigenschaften und -werte.

Klicken Sie zum Fortfahren rechts unten in dieser Ansicht auf **Abbrechen**, um zur Seite mit der Standardgeräteliste zurückzukehren.

## Filtern der Geräteansicht mithilfe von Geräteabfragen
Geräteabfragen sind eine geeignete Möglichkeit, ein Gerät oder eine Gruppe von Geräten mit einer bestimmten Eigenschaft (z.B. einem bestimmten Tag) schnell zu finden. Auf der Beispielbenutzeroberfläche werden Geräteabfragen verwendet, um die Geräteliste auf der Seite mit der Standardgeräteliste aufzufüllen. Die Beispielbenutzeroberfläche ermöglicht das Hinzufügen und Entfernen von Diensteigenschaften in der Tabelle und Filtern anhand einigen dieser Eigenschaften.

Führen Sie die folgenden Schritte aus, um einen Kundenfilter anhand des Diensteigenschaftstags „bacon“ zu erstellen:

1. Klicken Sie auf das Trichtersymbol, um die Bearbeitungsansicht für Geräteabfragen zu öffnen:
   
   ![][3]
2. Klicken Sie auf **+ Filter hinzufügen**, um den Editor aufzuklappen. Wählen Sie in der Einblendliste für Eigenschaften **Tags** aus, geben Sie **bacon** in das Textfeld ein, und klicken Sie auf **Übernehmen**. Die Geräteliste enthält nun nur die drei Geräte mit dem Tag „bacon“:
   
   ![][4]
3. Benennen Sie im Abfragetitelfeld (neben dem Trichtersymbol) die Abfrage mit **Only Bacon**, und klicken Sie auf **Speichern**:
   
   ![][5]
4. Klicken Sie auf das Trichtersymbol, um den Editor für Geräteabfragen zu beenden.

## Simulieren von Geräteneustarts mithilfe eines Geräteauftrags
Wie Sie im Artikel mit der Übersicht über die Geräteverwaltung erfahren haben, ermöglichen Geräteaufträge das Orchestrieren einfacher und komplexer Aktionen auf einem oder mehreren physischen Geräten. In diesem Abschnitt erstellen Sie auf der Beispielbenutzeroberfläche einen Geräteauftrag zum Ausführen eines Neustarts auf allen Simulationsgeräten mit dem Tag „bacon“:

1. Klicken Sie in der Geräteabfrageliste **Only Bacon** auf die einzelnen Gerätezeilen, um sie für den Neustartauftrag zu markieren:
   
   ![][6]
2. Klicken Sie auf der Symbolleiste für Geräteaufträge auf die Schaltfläche **Neu starten**, um den Neustartauftrag zu erstellen. Nach der Bestätigung durch Klicken auf **Ja** klicken Sie im eingeblendeten Dialogfeld **Auftrag für Gerät wurde gestartet** auf den Link **Auftragsverlauf**, um zum Fenster „Geräteaufträge“ zu wechseln.
   
   ![][7]

Sie haben soeben einen einzelnen übergeordneten Auftrag erstellt, der drei untergeordnete Aufträge erzeugt hat, von denen jeder einen Neustartvorgang auf die drei Geräte mit dem Tag „bacon“ anwendet:

![][8]

Aktualisieren Sie diesen Bildschirm nach einigen Augenblicken – der Status des übergeordneten Auftrags und der drei untergeordneten Aufträge hat sich in **Abgeschlossen** geändert. Die neuen Statuswerte zeigen an, dass die Neustartvorgänge erfolgreich durchgeführt und von den simulierten Geräten bestätigt wurden. Bestimmen Sie anhand der Spalte **Geräte-ID**, welche Aufträge welchen Geräten zugeordnet sind.

> [!NOTE]
> Wenn die untergeordneten Aufträge den Status „Fehler“ zurückgeben, prüfen Sie, ob die simulierten Geräte noch immer auf Ihrem Testsystem ausgeführt werden. Falls nicht, führen Sie das Skript „simulate.bat“ oder „simulate.sh“ erneut aus, und wiederholen Sie die im vorherigen Abschnitt beschriebenen Schritte des Auftrags zum Neustarten von Geräten.
> 
> 

## Nächste Schritte
Sie haben sich gerade anhand einer Beispielbenutzeroberfläche mit den Konzepten der Geräteverwaltung vertraut gemacht. Wenn Sie noch mehr über die Geräteverwaltungs-APIs wissen und mit einigen Codebeispielen experimentieren möchten, sehen Sie sich die folgenden Tutorials und Ressourcen für Entwickler an:

* [Verwenden des Gerätezwillings][lnk-tutorial-twin]
* [Suchen nach Gerätezwillingen mithilfe von Abfragen][lnk-tutorial-queries]
* [Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen][lnk-tutorial-jobs]
* [Aktivieren verwalteter Geräte hinter einem IoT-Gateway][lnk-dm-gateway]
* [Einführung in die Clientbibliothek der Azure IoT Hub-Geräteverwaltung][lnk-library-c]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwerfen Ihrer Lösung][lnk-design]
* [Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
* [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

[1]: media/iot-hub-device-management-ui-sample/image1.png
[2]: media/iot-hub-device-management-ui-sample/image2.png
[3]: media/iot-hub-device-management-ui-sample/image3.png
[4]: media/iot-hub-device-management-ui-sample/image4.png
[5]: media/iot-hub-device-management-ui-sample/image5.png
[6]: media/iot-hub-device-management-ui-sample/image6.png
[7]: media/iot-hub-device-management-ui-sample/image7.png
[8]: media/iot-hub-device-management-ui-sample/image8.png

[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management/

[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md
[lnk-dm-gateway]: iot-hub-gateway-device-management.md
[lnk-library-c]: iot-hub-device-management-library.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md
[lnk-securing]: iot-hub-security-ground-up.md

<!---HONumber=AcomDC_0831_2016-->