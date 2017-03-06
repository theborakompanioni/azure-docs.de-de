---
title: "Erste Schritte mit vorkonfigurierten Lösungen | Microsoft Docs"
description: "In diesem Tutorial erlernen Sie das Bereitstellen einer vorkonfigurierten Azure IoT Suite-Lösung."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 9e1bcba086a9f70c689a5d7d7713a8ecdc764492
ms.openlocfilehash: 8248e0a02cb0775a87f0c8130e53b98f8bcfe581
ms.lasthandoff: 02/27/2017


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Tutorial: Erste Schritte mit den vorkonfigurierten Lösungen
## <a name="introduction"></a>Einführung
[Vorkonfigurierte Lösungen][lnk-preconfigured-solutions] von Azure IoT Suite kombinieren mehrere Azure IoT-Dienste, um durchgängige Lösungen bereitzustellen, die allgemeine IoT-Unternehmensszenarien implementieren. Die vorkonfigurierte Lösung für die *Remoteüberwachung* stellt eine Verbindung mit Ihren Geräten her und überwacht die Geräte. Sie können die Lösung zum Analysieren des Datenstroms von Ihren Geräten und Verbessern der Geschäftsergebnisse verwenden, indem Sie Prozesse so einrichten, dass sie automatisch auf diesen Datenstrom reagieren.

Dieses Tutorial zeigt, wie Sie eine vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen. Außerdem lernen Sie die grundlegenden Funktionen der vorkonfigurierten Lösung kennen. Sie können auf einen Großteil dieser Funktionen über das Lösungsdashboard zugreifen, das zusammen mit der vorkonfigurierten Lösung bereitgestellt wird:

![Lösungsdashboard der vorkonfigurierten Lösung für die Remoteüberwachung][img-dashboard]

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].
> 
> 

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Anzeigen des Lösungsdashboards
Im Lösungsdashboard können Sie die bereitgestellte Lösung verwalten. Sie können z. B. Telemetriedaten anzeigen, Geräte hinzufügen und Regeln konfigurieren.

1. Wenn die Bereitstellung abgeschlossen ist und auf der Kachel für die vorkonfigurierte Lösung **Bereit** angezeigt wird, klicken Sie auf **Starten**, um das Portal der Remoteüberwachungslösung auf einer neuen Registerkarte zu öffnen.
   
   ![Vorkonfigurierte Lösung starten][img-launch-solution]
2. Standardmäßig zeigt das Lösungsportal das *Lösungsdashboard*an. Sie können im linken Menü andere Ansichten auswählen.
   
   ![Lösungsdashboard der vorkonfigurierten Lösung für die Remoteüberwachung][img-dashboard]

Im Dashboard werden die folgenden Informationen angezeigt:

* Auf der Karte werden die Standorte der einzelnen Geräte angezeigt, die mit der Lösung verbunden sind. Beim ersten Ausführen der Lösung sind vier simulierte Geräte vorhanden. Die simulierten Geräte werden als Azure WebJobs implementiert, und für die Lösung wird die Bing Maps-API verwendet, um Informationen zu Karte darzustellen.
* Im Bereich **Telemetrieverlauf** werden Telemetriedaten zur Luftfeuchtigkeit und Temperatur von einem ausgewählten Gerät nahezu in Echtzeit dargestellt und zusammengefasste Daten angezeigt, z. B. die maximale, minimale und durchschnittliche Luftfeuchtigkeit.
* Im Bereich **Alarmverlauf** werden die letzten Alarmereignisse für die Fälle angezeigt, in denen ein Telemetriewert einen Schwellenwert überschritten hat. Sie können zusätzlich zu den Beispielen, die von der vorkonfigurierten Lösung erstellt werden, auch eigene Alarme definieren.
* Im Bereich der **Aufträge** werden Informationen zu geplanten Aufträgen angezeigt. Auf der Seite für **Verwaltungsaufträge** können Sie Ihre eigenen Aufträge planen.

## <a name="view-the-device-list"></a>Anzeigen der Geräteliste
Die *Geräteliste* zeigt alle registrierten Geräte in der Lösung an. In der Geräteliste können Sie Gerätemetadaten anzeigen und bearbeiten, Geräte hinzufügen oder entfernen und Methoden auf Geräten aufrufen.

1. Klicken Sie im linken Menü auf **Geräte**, um die Geräteliste für diese Lösung anzuzeigen.
   
   ![Geräteliste im Dashboard][img-devicelist]
2. Die Geräteliste zeigt anfänglich vier simulierte Geräte an, die im Rahmen des Bereitstellungsprozesses erstellt werden. Sie können der Lösung zusätzliche simulierte und physische Geräte hinzufügen.
3. Sie können anpassen, welche Informationen in der Geräteliste angezeigt werden, indem Sie auf den **-Spalten-Editor** klicken. Sie können Spalten hinzufügen und entfernen, die gemeldete Eigenschaften und Tagwerte enthalten. Sie können Spalten auch neu anordnen und umbenennen:
   
   ![Spalten-Editor im Dashboard][img-columneditor]
4. Klicken Sie auf ein Gerät in der Geräteliste, um die Gerätedetails anzuzeigen.
   
   ![Gerätedetails im Dashboard][img-devicedetails]

Der Bereich mit den **Gerätedetails** enthält sechs Abschnitte:

* Eine Auflistung von Links, mit denen Sie das Gerätesymbol anpassen, das Gerät deaktivieren, eine Regel hinzufügen, eine Methode aufrufen oder einen Befehl senden können. Einen Vergleich dieser Befehle (C2D-Nachrichten) und Methoden (direkte Methoden) finden Sie im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].
* Im Abschnitt zu **Gerätezwilling – Tags** können Sie Tagwerte für das Gerät bearbeiten. Sie können die Tagwerte in der Geräteliste anzeigen und Tagwerte zum Filtern der Geräteliste verwenden.
* Im Abschnitt zu **Gerätezwilling – Gewünschte Eigenschaften** legen Sie die Eigenschaftswerte fest, die an das Gerät gesendet werden.
* Im Abschnitt zu **Gerätezwilling – Gemeldete Eigenschaften** können Sie die Eigenschaftswerte anzeigen, die vom Gerät gesendet werden.
* Im Abschnitt mit den **Geräteeigenschaften** werden Informationen aus der Identitätsregistrierung angezeigt, z. B. Geräte-ID und Authentifizierungsschlüssel.
* Der Bereich über **kürzlich ausgeführte Aufträge** zeigt Informationen zu allen Aufträgen an, die vor Kurzem von diesem Gerät ausgeführt wurden.

## <a name="customize-the-device-icon"></a>Anpassen des Gerätesymbols

Sie können das Gerätesymbol in der Geräteliste im Bereich mit den **Gerätedetails** wie folgt anpassen:

1. Klicken Sie auf das Stiftsymbol, um den Bereich **Bild bearbeiten** für ein Gerät zu öffnen:
   
   ![Editor zum Öffnen des Gerätebilds][img-startimageedit]
2. Laden Sie ein neues Bild hoch oder verwenden Sie eines der vorhandenen Bilder, und klicken Sie auf **Speichern**:
   
   ![Editor zum Bearbeiten des Gerätebilds][img-imageedit]
3. Das ausgewählte Bild wird jetzt in der **Symbol**-Spalte für das Gerät angezeigt.

> [!NOTE]
> Das Bild wird im Blob-Speicher gespeichert. Ein Tag im Gerätezwilling enthält einen Link zu dem Bild im Blob-Speicher.
> 
> 

## <a name="invoke-a-method-on-a-device"></a>Aufrufen einer Methode auf einem Gerät
Über den Bereich mit den **Gerätedetails** können Sie Methoden auf dem Gerät aufrufen. Wenn ein Gerät zum ersten Mal gestartet wird, sendet es Informationen zu den unterstützten Methoden an die Lösung.

1. Klicken Sie im Bereich mit den **Gerätedetails** für das ausgewählte Gerät auf **Methoden**:
   
   ![Gerätemethoden im Dashboard][img-devicemethods]
2. Wählen Sie in der Methodenliste **Neu starten**.
3. Klicken Sie auf die **Aufrufmethode**.
4. Im Methodenverlauf können Sie den Status des Methodenaufrufs prüfen.
   
   ![Methodenstatus im Dashboard][img-pingmethod]

Die Lösung verfolgt den Status jeder einzelnen Methode, die gesendet wird. Wenn das Gerät die Methode abgeschlossen hat, wird ein neuer Eintrag in der Tabelle zum Methodenverlauf angezeigt.

Manche Methoden starten asynchrone Aufträge auf dem Gerät. So startet z. B. die **InitiateFirmwareUpdate**-Methode eine asynchrone Aufgabe, um das Update auszuführen. Das Gerät verwendet gemeldete Eigenschaften, um über den Status des Firmwareupdates zu berichten.

## <a name="send-a-command-to-a-device"></a>Senden eines Befehls an ein Gerät
Über den Bereich mit den **Gerätedetails** können Sie Befehle an das Gerät senden. Wenn ein Gerät zum ersten Mal gestartet wird, sendet es Informationen zu den unterstützten Befehlen an die Lösung.

1. Klicken Sie im Bereich mit den **Gerätedetails** für das ausgewählte Gerät auf **Befehle**:
   
   ![Gerätebefehle im Dashboard][img-devicecommands]
2. Wählen Sie in der Befehlsliste **PingDevice** aus.
3. Klicken Sie auf **Befehl senden**.
4. Sie sehen den Status des Befehls im Befehlsverlauf.
   
   ![Befehlsstatus im Dashboard][img-pingcommand]

Die Lösung verfolgt den Status jedes einzelnen Befehls, der gesendet wird. Zunächst lautet das Ergebnis **Ausstehend**. Wenn das Gerät meldet, dass es den Befehl ausgeführt hat, wird der Status in **Erfolgreich**geändert.

## <a name="add-a-new-simulated-device"></a>Hinzufügen eines neuen simulierten Geräts
Bei der Bereitstellung der vorkonfigurierten Lösung stellen Sie automatisch vier Beispielgeräte bereit, die in der Geräteliste angezeigt werden. Dies sind *simulierte Geräte* , die in einem Azure WebJob ausgeführt werden. Simulierte Geräte erleichtern Ihnen das Experimentieren mit der vorkonfigurierten Lösung, ohne dass Sie echte physische Geräte bereitstellen müssen. Falls Sie für ein echtes Gerät eine Verbindung mit der Lösung herstellen möchten, hilft Ihnen das Tutorial [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung][lnk-connect-rm] weiter.

Die folgenden Schritte verdeutlichen, wie Sie der Lösung ein simuliertes Gerät hinzufügen:

1. Navigieren Sie zurück zur Geräteliste.
2. Klicken Sie in der unteren linken Ecke auf **+ Gerät hinzufügen**, um ein Gerät hinzuzufügen.
   
   ![Gerät der vorkonfigurierten Lösung hinzufügen][img-adddevice]
3. Klicken Sie auf der Kachel **Simuliertes Gerät** auf **Neues hinzufügen**.
   
   ![Neue Gerätedetails im Dashboard festlegen][img-addnew]
   
   Zusätzlich zum Erstellen eines neuen simulierten Geräts können Sie auch ein physisches Gerät hinzufügen, wenn Sie sich für die Erstellung eines **benutzerdefinierten Geräts**entscheiden. Weitere Informationen zum Verbinden von physischen Geräten mit der Lösung finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung von IoT Suite][lnk-connect-rm].
4. Wählen Sie **Eigene Geräte-ID definieren** aus, und geben Sie einen eindeutigen Geräte-ID-Namen wie **mydevice_01** ein.
5. Klicken Sie auf **Erstellen**.
   
   ![Neues Gerät speichern][img-definedevice]
6. Klicken Sie in Schritt 3 von **Simuliertes Gerät hinzufügen** auf **Fertig**, um zur Geräteliste zurückzukehren.
7. Ihr Gerät wird mit **Wird ausgeführt** in der Geräteliste angezeigt.
   
    ![Neues Gerät in der Geräteliste anzeigen][img-runningnew]
8. Sie können auch die simulierten Telemetriedaten des neuen Geräts im Dashboard anzeigen:
   
    ![Telemetrie von neuem Gerät anzeigen][img-runningnew-2]

## <a name="device-properties"></a>Geräteeigenschaften
Die per Remoteüberwachung vorkonfigurierte Lösung verwendet [Gerätezwillinge][lnk-device-twin] zum Synchronisieren von Gerätemetadaten zwischen Geräten und dem Lösungs-Back-End. Ein Gerätezwilling ist ein in IoT Hub gespeichertes JSON-Dokument, das Eigenschaftswerte für ein einzelnes Gerät speichert. Geräte senden regelmäßig Metadaten als *gemeldete Eigenschaften* zum Speichern im Gerätezwilling an das Lösungs-Back-End. Das Lösungs-Back-End kann *gewünschte Eigenschaften* im Gerätezwilling festlegen, um Aktualisierungen von Metadaten an Geräte zu senden. Die gemeldeten Eigenschaften zeigen die neuesten vom Gerät gesendeten Metadatenwerte an. Weitere Informationen finden Sie im Abschnitt zu [Geräteidentitätsregistrierung, Gerätezwilling und DocumentDB][lnk-devicemetadata].

> [!NOTE]
> Die Lösung verwendet außerdem eine DocumentDB-Datenbank zum Speichern von gerätespezifischen Daten zu Befehlen und Methoden.
> 
> 

1. Navigieren Sie zurück zur Geräteliste.
2. Wählen Sie das neue Gerät in der **Geräteliste** aus, und klicken Sie dann auf **Bearbeiten**, um **Gerätezwilling – Gewünschte Eigenschaften** zu bearbeiten:
   
   ![Bearbeiten gewünschter Geräteeigenschaften][img-editdevice]
3. Wählen Sie für den **Namen der gewünschten Eigenschaft** die Option **Breitengrad** aus und legen Sie den Wert **47.639521** fest. Klicken Sie anschließend auf **Änderungen in Geräteregistrierung speichern**:
   
    ![Bearbeiten gewünschter Geräteeigenschaft][img-editdevice2]
4. Im Bereich **Gerätedetails** wird der neue Breitengrad anfänglich als gewünschte Eigenschaft, und der alte Breitengrad als gemeldete Eigenschaft angezeigt:
   
    ![Anzeigen einer gemeldeten Eigenschaft][img-editdevice3]
5. Derzeit verarbeiten simulierte Geräte in der vorkonfigurierten Lösung nur die gewünschten Eigenschaften **Desired.Config.TemperatureMeanValue** und **Desired.Config.TelemetryInterval**. Ein echtes Gerät sollte alle Eigenschaften aus dem IoT Hub lesen, die Änderungen an der Konfiguration vornehmen und die neuen Werte als gemeldete Eigenschaften an den Hub berichten.

Im Bereich **Gerätedetails** können Sie **Gerätezwilling – Tags** ebenso bearbeiten wie **Gerätezwilling – Gewünschte Eigenschaften**. Im Gegensatz zu gewünschten Eigenschaften werden Tags jedoch nicht mit dem Gerät synchronisiert. Tags sind nur im Gerätezwilling und im IoT Hub vorhanden. Tags sind nützlich beim Erstellen von benutzerdefinierten Filtern in der Geräteliste.

## <a name="sort-the-device-list"></a>Sortieren der Geräteliste

Sie können die Geräteliste sortieren, indem Sie auf eine Spaltenüberschrift klicken. Beim ersten Klick sortieren Sie die Liste in aufsteigender Reihenfolge, beim zweiten Klick in absteigender Reihenfolge:

![Sortieren der Geräteliste][img-sortdevices]

## <a name="filter-the-device-list"></a>Filtern der Geräteliste

In der Geräteliste können Sie Filter erstellen, speichern und erneut laden, um eine benutzerdefinierte Liste der Geräte anzuzeigen, die mit Ihrem Hub verbunden sind. So erstellen Sie einen Filter:

1. Klicken Sie über der Geräteliste auf das Symbol zum Bearbeiten des Filters:
   
   ![Öffnen des Filtereditors][img-editfiltericon]
2. Fügen Sie im **Filtereditor**die Felder, Operatoren und Werte zum Filtern der Geräteliste hinzu. Sie können mehrere Klauseln hinzufügen, um Ihren Filter zu verfeinern. Klicken Sie auf **Filter**, um den Filter anzuwenden:
   
   ![Erstellen eines Filters][img-filtereditor]
3. In diesem Beispiel wird die Liste nach Hersteller und Modellnummer gefiltert:
   
   ![Gefilterte Liste][img-filterelist]
4. Um den Filter mit einem benutzerdefinierten Namen zu speichern, klicken Sie auf das Symbol **Speichern als**:
   
   ![Speichern eines Filters][img-savefilter]
5. Um einen Filter zuvor gespeicherten Filter erneut anzuwenden, klicken Sie auf das Symbol **Gespeicherten Filter öffnen**:
   
   ![Öffnen eines Filters][img-openfilter]

Sie können Filter auf Grundlage von Geräte-ID, Gerätestatus, gewünschten Eigenschaften, gemeldeten Eigenschaften und Tags erstellen.

> [!NOTE]
> Im **Filtereditor** können Sie die **Erweiterte Ansicht** zum direkten Bearbeiten des Abfragetexts verwenden.
> 
> 

## <a name="add-a-rule-for-the-new-device"></a>Hinzufügen einer Regel für das neue Gerät
Es sind keine Regeln für das neue Gerät vorhanden, das Sie gerade hinzugefügt haben. In diesem Abschnitt fügen Sie eine Regel hinzu, mit der ein Alarm ausgelöst wird, wenn die vom neuen Gerät gemeldete Temperatur 47 Grad übersteigt. Beachten Sie vor dem Beginn, dass im Dashboard unter dem Telemetriedatenverlauf für das neue Gerät zu sehen ist, dass die Gerätetemperatur 45 Grad nicht überschritten hat.

1. Navigieren Sie zurück zur Geräteliste.
2. Wählen Sie das neue Gerät in der **Geräteliste** aus, und klicken Sie dann auf **Regel hinzufügen**, um eine Regel für das Gerät hinzuzufügen.
3. Erstellen Sie eine Regel, bei der **Temperature** als Datenfeld und **AlarmTemp** als Ausgabe verwendet wird, wenn die Temperatur 47 Grad überschreitet:
   
    ![Geräteregel hinzufügen][img-adddevicerule]
4. Klicken Sie auf **Regeln speichern und anzeigen**, um die Änderungen zu speichern.
5. Klicken Sie im Bereich mit den Gerätedetails für das neue Gerät auf **Befehle** .
   
   ![Geräteregel hinzufügen][img-adddevicerule2]
6. Wählen Sie in der Befehlsliste den Befehl **ChangeSetPointTemp** aus, und legen Sie **SetPointTemp** auf 45 fest. Klicken Sie anschließend auf **Befehl senden**:
   
   ![Geräteregel hinzufügen][img-adddevicerule3]
7. Navigieren Sie zurück zum Dashboard der Lösung. Nach kurzer Zeit wird im Bereich **Alarmverlauf** ein neuer Eintrag angezeigt, wenn die vom neuen Gerät gemeldete Temperatur den Schwellenwert von 47 Grad überschreitet:
   
   ![Geräteregel hinzufügen][img-adddevicerule4]
8. Sie können alle Regeln im Dashboard auf der Seite **Regeln** prüfen und bearbeiten:
   
    ![Geräteregeln auflisten][img-rules]
9. Sie können alle Aktionen, die als Reaktion auf eine Regel durchgeführt werden können, im Dashboard auf der Seite **Aktionen** prüfen und bearbeiten:
   
    ![Geräteaktionen auflisten][img-actions]

> [!NOTE]
> Sie können Aktionen definieren, bei denen als Reaktion auf eine Regel eine E-Mail oder SMS gesendet werden kann oder die per [Logik-App][lnk-logic-apps] in ein Branchensystem integriert werden können. Weitere Informationen finden Sie im Tutorial [Verbinden einer Logik-App mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite][lnk-logicapptutorial].
> 
> 

## <a name="disable-and-remove-devices"></a>Deaktivieren und Entfernen von Geräten
Sie können ein Gerät deaktivieren, und nachdem es deaktiviert wurde, können Sie es entfernen:

![Gerät deaktivieren und entfernen][img-disable]

## <a name="run-jobs"></a>Ausführen von Aufträgen
Sie können Aufträge planen, um Massenvorgänge auf Ihren Geräten auszuführen. Sie erstellen einen Auftrag für eine Geräteliste. Diese Liste kann all Ihre Geräte enthalten oder eine gefilterte Liste sein, die Sie mit den [Filtertools](#filter-the-device-list) in der Geräteliste erstellt haben. Ein Auftrag kann eine Methode auf jedem Gerät in der Liste ausführen oder den Gerätezwilling für jedes Gerät in der Geräteliste aktualisieren.

### <a name="create-a-job-to-invoke-a-method"></a>Erstellen eines Auftrags zum Aufrufen einer Methode

Die folgenden Schritte veranschaulichen, wie Sie einen Auftrag erstellen, der auf jedem Gerät in einer Liste die Firmware-Update-Methode aufruft. Die Methode wird nur auf Geräten aufgerufen, die die Methode unterstützen:

1. Verwenden Sie die Filtertools in der Geräteliste, um eine Geräteliste für den Empfang des Firmwareupdates zu erstellen:
   
   ![Öffnen des Filtereditors][img-editfiltericon]
2. Klicken Sie in der gefilterten Liste auf **Auftragsplaner**:
   
   ![Öffnen des Auftragsplaners][img-clickjobscheduler]
3. Klicken Sie im Bereich **Auftragsplaner** auf die **Aufrufmethode**.
4. Geben Sie auf der Seite zur **Aufrufmethode** dann die Details der aufzurufenden Methode ein, und klicken Sie dann auf **Zeitplan**:
   
   ![Konfigurieren des Methodenauftrags][img-invokemethodjob]

Die **InitiateFirmwareUpdate**-Methode startet eine asynchrone Aufgabe auf dem Gerät und wird dann sofort zurückgegeben. Das Firmwareupgrade verwendet dann die gemeldeten Eigenschaften, um während dessen Ausführung Berichte über den Updatevorgang zu erstellen.

### <a name="create-a-job-to-edit-the-device-twin"></a>Erstellen einen Auftrags zum Bearbeiten des Gerätezwillings

Die folgenden Schritte veranschaulichen, wie Sie einen Auftrag erstellen, der auf jedem Gerät in einer Liste den Gerätezwilling bearbeitet:

1. Verwenden Sie die Filtertools in der Geräteliste, um eine Geräteliste für den Empfang der Änderungen am Gerätezwilling zu erstellen:
   
   ![Öffnen des Filtereditors][img-editfiltericon]
2. Klicken Sie in der gefilterten Liste auf **Auftragsplaner**:
   
   ![Öffnen des Auftragsplaners][img-clickjobscheduler]
3. Klicken Sie im Bereich **Auftrag planen** auf die Option zum **Bearbeiten des Gerätezwillings**.
4. Geben Sie auf der Seite zum **Bearbeiten des Gerätezwillings** die Details der **gewünschten Eigenschaften** und **Tags**, die Sie bearbeiten möchten, ein und klicken Sie dann auf **Zeitplan**:
   
   ![Konfigurieren des Methodenauftrags][img-edittwinjob]

### <a name="monitor-the-job"></a>Überwachen des Auftrags
Sie können den Status der geplanten Aufträge auf der Seite für **Verwaltungsaufträge** überwachen. In den **Auftragsdetails** werden Informationen zum ausgewählten Auftrag angezeigt:
   
   ![Anzeigen des Auftragsstatus][img-jobstatus]

Sie können Informationen zu Aufträgen auch auf dem **Dashboard** anzeigen:
   
   ![Anzeigen von Aufträgen auf dem Dashboard][img-jobdashboard]


## <a name="behind-the-scenes"></a>Abläufe im Hintergrund
Wenn Sie eine vorkonfigurierte Lösung bereitstellen, werden vom Bereitstellungsprozess mehrere Ressourcen im gewählten Azure-Abonnement erstellt. Sie können diese Ressourcen im Azure-[Portal][lnk-portal] anzeigen. Der Bereitstellungsprozess erstellt eine **Ressourcengruppe** mit einem Namen basierend auf dem Namen, den Sie für Ihre vorkonfigurierte Lösung wählen:

![Vorkonfigurierte Lösung im Azure-Portal][img-portal]

Sie können die Einstellungen der einzelnen Ressourcen anzeigen, indem Sie sie in der Ressourcengruppe in der Liste mit den Ressourcen auswählen.

Sie können auch den Quellcode für die vorkonfigurierte Lösung anzeigen. Den Quellcode zur vorkonfigurierten Lösung für die Remoteüberwachung finden Sie im GitHub-Repository [azure-iot-remote-monitoring][lnk-rmgithub]:

* Der Ordner **DeviceAdministration** enthält den Quellcode für das Dashboard.
* Der Ordner **Simulator** enthält den Quellcode für das simulierte Gerät.
* Der Ordner **EventProcessor** enthält den Quellcode für den Back-End-Prozess, mit dem die eingehenden Telemetriedaten behandelt werden.

Wenn Sie fertig sind, können Sie die vorkonfigurierte Lösung auf der Website [azureiotsuite.com][lnk-azureiotsuite] aus Ihrem Azure-Abonnement löschen. Über diese Website können Sie problemlos alle Ressourcen löschen, die bei der Erstellung der vorkonfigurierten Lösung bereitgestellt wurden.

> [!NOTE]
> Um sicherzustellen, dass Sie alle zur vorkonfigurierten Lösung gehörenden Daten löschen, sollten Sie sie über die Website [azureiotsuite.com][lnk-azureiotsuite] löschen und nicht nur einfach die Ressourcengruppe im Portal löschen.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Sie haben eine funktionierende vorkonfigurierte Lösung bereitgestellt und können nun mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung][lnk-rm-walkthrough]
* [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)][lnk-connect-rm]
* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-devicemethods]: media/iot-suite-getstarted-preconfigured-solutions/devicemethods.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-pingmethod]: media/iot-suite-getstarted-preconfigured-solutions/pingmethod.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-sortdevices]: media/iot-suite-getstarted-preconfigured-solutions/sortdevices.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-clickjobscheduler]: media/iot-suite-getstarted-preconfigured-solutions/clickscheduler.png
[img-invokemethodjob]: media/iot-suite-getstarted-preconfigured-solutions/invokemethodjob.png
[img-edittwinjob]: media/iot-suite-getstarted-preconfigured-solutions/edittwinjob.png
[img-jobstatus]: media/iot-suite-getstarted-preconfigured-solutions/jobstatus.png
[img-jobdashboard]: media/iot-suite-getstarted-preconfigured-solutions/jobdashboard.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-device-twin-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
