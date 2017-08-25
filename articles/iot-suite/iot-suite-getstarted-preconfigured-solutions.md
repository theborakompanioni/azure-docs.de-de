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
ms.date: 08/24/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 2364ab79ec4091d41e4d340e13033181df05b507
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="get-started-with-the-preconfigured-solutions"></a>Erste Schritte mit den vorkonfigurierten Lösungen

[Vorkonfigurierte Lösungen][lnk-preconfigured-solutions] von Azure IoT Suite kombinieren mehrere Azure IoT-Dienste, um durchgängige Lösungen bereitzustellen, die allgemeine IoT-Unternehmensszenarien implementieren. Die vorkonfigurierte Lösung für die *Remoteüberwachung* stellt eine Verbindung mit Ihren Geräten her und überwacht die Geräte. Sie können die Lösung zum Analysieren des Datenstroms von Ihren Geräten und Verbessern der Geschäftsergebnisse verwenden, indem Sie Prozesse so einrichten, dass sie automatisch auf diesen Datenstrom reagieren.

Dieses Tutorial zeigt, wie Sie eine vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen. Außerdem lernen Sie die grundlegenden Funktionen der vorkonfigurierten Lösung kennen. Sie können auf einen Großteil dieser Features über das *Lösungsdashboard* zugreifen, das zusammen mit der vorkonfigurierten Lösung bereitgestellt wird:

![Lösungsdashboard der vorkonfigurierten Lösung für die Remoteüberwachung][img-dashboard]

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Übersicht über das Szenario

Wenn Sie die vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen, verfügt diese bereits über Ressourcen, mit denen Sie ein allgemeines Remoteüberwachungsszenario durchlaufen können. In diesem Szenario melden mehrere mit der Lösung verbundene Geräte unerwartete Temperaturwerte. Im weiteren Verlauf wird Folgendes vermittelt:

* Ermitteln der Geräte, die unerwartete Temperaturwerte senden
* Konfigurieren der betroffenen Geräte, um ausführlichere Telemetriedaten zu erhalten
* Beheben des Problems durch Aktualisieren der Firmware auf den betroffenen Geräten
* Vergewissern, dass das Problem durch die Aktion behoben wurde

Ein wichtiges Merkmal dieses Szenarios: Die Aktionen können alle per Remotezugriff über das Lösungsdashboard ausgeführt werden. Sie benötigen also keinen physischen Zugriff auf die Geräte.

## <a name="view-the-solution-dashboard"></a>Anzeigen des Lösungsdashboards

Im Lösungsdashboard können Sie die bereitgestellte Lösung verwalten. Sie können z. B. Telemetriedaten anzeigen, Geräte hinzufügen und Regeln konfigurieren.

1. Wenn die Bereitstellung abgeschlossen ist und auf der Kachel für die vorkonfigurierte Lösung **Bereit** angezeigt wird, wählen Sie **Starten** aus, um eine neue Registerkarte mit dem Lösungsportal für die Remoteüberwachung zu öffnen.

    ![Vorkonfigurierte Lösung starten][img-launch-solution]

1. Standardmäßig zeigt das Lösungsportal das *Dashboard* an. Über das Menü auf der linken Seite gelangen Sie zu anderen Bereichen des Lösungsportals.

    ![Lösungsdashboard der vorkonfigurierten Lösung für die Remoteüberwachung][img-menu]

Im Dashboard werden die folgenden Informationen angezeigt:

* Eine Karte mit den Standorten der einzelnen Geräte, die mit der Lösung verbunden sind. Wenn Sie die Lösung zum ersten Mal ausführen, sind 25 simulierte Geräte vorhanden. Die simulierten Geräte werden als Azure WebJobs implementiert, und für die Lösung wird die Bing Maps-API verwendet, um Informationen zu Karte darzustellen. Informationen zur Verwendung einer dynamischen Karte finden Sie in den [häufig gestellten Fragen][lnk-faq].
* Der Bereich **Telemetrieverlauf** mit nahezu in Echtzeit dargestellten Telemetriedaten zur Luftfeuchtigkeit und Temperatur eines ausgewählten Geräts sowie mit aggregierten Daten wie etwa der maximalen, minimalen und durchschnittlichen Luftfeuchtigkeit.
* Der Bereich **Alarmverlauf** mit aktuellen Alarmereignissen für den Fall, dass ein Telemetriewert einen Schwellenwert überschritten hat. Sie können zusätzlich zu den Beispielen, die von der vorkonfigurierten Lösung erstellt werden, auch eigene Alarme definieren.
* Der Bereich **Aufträge** mit Informationen zu geplanten Aufträgen. Auf der Seite für **Verwaltungsaufträge** können Sie Ihre eigenen Aufträge planen.

## <a name="view-alarms"></a>Anzeigen von Alarmen

Im Bereich mit dem Alarmverlauf sehen Sie, dass fünf Geräte erhöhte Telemetriewerte melden.

![TODO Alarmverlauf auf dem Lösungsdashboard][img-alarms]

> [!NOTE]
> Diese Alarme werden durch eine Regel generiert, die in der vorkonfigurierten Lösung enthalten ist. Diese Regel generiert einen Alarm, wenn die von einem Gerät gesendete Temperatur den Wert 60 überschreitet. Sie können eigene Regeln und Aktionen definieren, indem Sie im Menü auf der linken Seite [Regeln](#add-a-rule) und [Aktionen](#add-an-action) auswählen.

## <a name="view-devices"></a>Anzeigen von Geräten

Die Liste *Geräte* enthält alle registrierten Geräte in der Lösung. In der Geräteliste können Sie Gerätemetadaten anzeigen und bearbeiten, Geräte hinzufügen oder entfernen und Methoden auf Geräten aufrufen. Die Geräteliste kann gefiltert und sortiert werden. Außerdem können Sie die Spalten der Geräteliste anpassen.

1. Wählen Sie **Geräte** aus, um die Geräteliste für diese Lösung anzuzeigen.

   ![Anzeigen der Geräteliste im Lösungsportal][img-devicelist]

1. Die Geräteliste enthält zunächst 25 simulierte Geräte, die im Rahmen des Bereitstellungsprozesses erstellt wurden. Sie können der Lösung zusätzliche simulierte und physische Geräte hinzufügen.

1. Klicken Sie in der Geräteliste auf ein Gerät, um Details zu einem Gerät anzuzeigen.

   ![Anzeigen der Gerätedetails im Lösungsportal][img-devicedetails]

Der Bereich mit den **Gerätedetails** enthält sechs Abschnitte:

* Eine Auflistung von Links, mit denen Sie das Gerätesymbol anpassen, das Gerät deaktivieren, eine Regel hinzufügen, eine Methode aufrufen oder einen Befehl senden können. Einen Vergleich dieser Befehle (C2D-Nachrichten) und Methoden (direkte Methoden) finden Sie im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].
* Im Abschnitt zu **Gerätezwilling – Tags** können Sie Tagwerte für das Gerät bearbeiten. Sie können die Tagwerte in der Geräteliste anzeigen und Tagwerte zum Filtern der Geräteliste verwenden.
* Im Abschnitt zu **Gerätezwilling – Gewünschte Eigenschaften** legen Sie die Eigenschaftswerte fest, die an das Gerät gesendet werden.
* Im Abschnitt zu **Gerätezwilling – Gemeldete Eigenschaften** können Sie die Eigenschaftswerte anzeigen, die vom Gerät gesendet werden.
* Im Abschnitt mit den **Geräteeigenschaften** werden Informationen aus der Identitätsregistrierung angezeigt, z. B. Geräte-ID und Authentifizierungsschlüssel.
* Der Bereich über **kürzlich ausgeführte Aufträge** zeigt Informationen zu allen Aufträgen an, die vor Kurzem von diesem Gerät ausgeführt wurden.

## <a name="filter-the-device-list"></a>Filtern der Geräteliste

Sie können einen Filter verwenden, um nur die Geräte anzuzeigen, die unerwartete Temperaturwerte senden. Die vorkonfigurierte Lösung für die Remoteüberwachung enthält den Filter **Unhealthy devices** (Fehlerhafte Geräte), um die Anzeige auf Geräte zu beschränken, deren Temperaturmittelwert größer 60 ist. Sie können auch [eigene Filter erstellen](#add-a-filter).

1. Wählen Sie **Gespeicherten Filter öffnen** aus, um eine Liste mit verfügbaren Filtern anzuzeigen. Wählen Sie anschließend **Unhealthy devices** (Fehlerhafte Geräte) aus, um den Filter anzuwenden:

    ![Anzeigen der Filterliste][img-unhealthy-filter]

1. In der Geräteliste werden nun nur noch Geräte angezeigt, deren Temperaturmittelwert größer 60 ist.

    ![Anzeigen der nach fehlerhaften Geräten gefilterten Geräteliste][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>Aktualisieren gewünschter Eigenschaften

Sie haben nun eine Gruppe von Geräten ermittelt, bei denen unter Umständen ein Problem vorliegt. Sie beschließen jedoch, dass die Datenfrequenz von 15 Sekunden für eine eindeutige Diagnose des Problems nicht ausreicht. Legen Sie die Telemetriefrequenz auf fünf Sekunden fest, um mehr Datenpunkte zu erhalten und das Problem besser diagnostizieren zu können. Diese Konfigurationsänderung kann über das Lösungsportal mittels Push an die Remotegeräte weitergegeben werden. Sie können die Änderung einmal vornehmen, die Auswirkungen untersuchen und die Ergebnisse als Handlungsgrundlage heranziehen.

Gehen Sie wie folgt vor, um einen Auftrag auszuführen, der die gewünschte Eigenschaft **TelemetryInterval** für die betroffenen Geräte ändert. Wenn die Geräte den neuen Wert für die Eigenschaft **TelemetryInterval** erhalten, ändern sie ihre Konfiguration, um das Intervall für die Übermittlung von Telemetriedaten von 15 Sekunden auf fünf Sekunden zu verringern:

1. Wählen Sie in der nach fehlerhaften Geräten gefilterten Geräteliste die Option **Auftragsplaner** und anschließend **Gerätezwilling bearbeiten** aus.

1. Nennen Sie den Auftrag **Change telemetry interval** (Telemetrieintervall ändern).

1. Legen Sie unter **Gewünschte Eigenschaften** den Wert der gewünschten Eigenschaft namens **desired.Config.TelemetryInterval** auf fünf Sekunden fest.

1. Wählen Sie **Zeitplan** aus.

    ![Festlegen der TelemetryInterval-Eigenschaft auf fünf Sekunden][img-change-interval]

1. Den Status des Auftrags können Sie im Portal auf der Seite **Verwaltungsaufträge** überwachen.

> [!NOTE]
> Wenn Sie einen gewünschten Eigenschaftswert für ein einzelnes Gerät ändern möchten, verwenden Sie den Abschnitt **Gewünschte Eigenschaften** des Bereichs **Gerätedetails**, anstatt einen Auftrag auszuführen.

Dieser Auftrag legt den Wert der gewünschten Eigenschaft **TelemetryInterval** auf dem Gerätezwilling für alle Geräte fest, die durch den Filter ausgewählt wurden. Die Geräte rufen diesen Wert von dem Gerätezwilling ab und aktualisieren ihr Verhalten. Wenn ein Gerät eine gewünschte Eigenschaft von einem Gerätezwilling abruft und verarbeitet, legt es die entsprechende Eigenschaft für den gemeldeten Wert fest.

## <a name="invoke-methods"></a>Aufrufen von Methoden

Während der Ausführung des Auftrags stellen Sie in der Liste mit den fehlerhaften Geräten fest, dass alle darin enthaltenen Geräte eine alte Firmwareversion (vor Version 1.6) besitzen.

![Anzeigen der gemeldeten Firmwareversion für die fehlerhaften Geräte][img-old-firmware]

Da andere fehlerfreie Geräte kürzlich auf Version 2.0 aktualisiert wurden, sind die unerwarteten Temperaturwerte unter Umständen auf die alte Firmwareversion zurückzuführen. Verwenden Sie den integrierten Filter **Old firmware devices** (Geräte mit alter Firmware), um alle Geräte mit alter Firmwareversion zu ermitteln. Über das Portal können Sie dann remote alle Geräte aktualisieren, die noch über eine alte Firmwareversion verfügen:

1. Wählen Sie **Gespeicherten Filter öffnen** aus, um eine Liste mit verfügbaren Filtern anzuzeigen. Wählen Sie anschließend **Old firmware devices** Geräte mit alter Firmware) aus, um den Filter anzuwenden:

    ![Anzeigen der Filterliste][img-old-filter]

1. In der Geräteliste werden nun nur noch Geräte mit alter Firmwareversion angezeigt. Neben den fünf Geräten, die durch den Filter **Unhealthy devices** (Fehlerhafte Geräte) ermittelt wurden, enthält die Liste noch drei weitere Geräte:

    ![Anzeigen der nach alten Geräten gefilterten Geräteliste][img-filtered-old-list]

1. Wählen Sie **Auftragsplaner** und anschließend **Methode aufrufen** aus.

1. Geben Sie unter **Auftragsname** den Namen **Firmware update to version 2.0** (Firmwareaktualisierung auf Version 2.0) an.

1. Wählen Sie unter **Methode** die Option **InitiateFirmwareUpdate** aus.

1. Legen Sie den Parameter **FwPackageUri** auf **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin** fest.

1. Wählen Sie **Zeitplan** aus. Der Auftrag wird standardmäßig sofort ausgeführt.

    ![Erstellen eines Auftrags zum Aktualisieren der Firmware der ausgewählten Geräte][img-method-update]

> [!NOTE]
> Wenn Sie eine Methode für ein einzelnes Gerät aufrufen möchten, wählen Sie im Bereich **Gerätedetails** die Option **Methoden** aus, anstatt einen Auftrag auszuführen.

Dieser Auftrag ruft die direkte Methode **InitiateFirmwareUpdate** für alle Geräte auf, die durch den Filter ausgewählt sind. Geräte reagieren umgehend auf IoT Hub und initiieren die Firmwareaktualisierung dann asynchron. Die Geräte liefern Statusinformationen zur Firmwareaktualisierung über gemeldete Eigenschaftswerte, wie in den folgenden Screenshots zu sehen. Wählen Sie das Symbol **Aktualisieren** aus, um die Informationen in der Geräte- und in der Auftragsliste zu aktualisieren:

![Auftragsliste mit laufender Firmwareaktualisierung][img-update-1]
![Geräteliste mit dem Status der Firmwareaktualisierung][img-update-2]
![Auftragsliste mit abgeschlossener Firmwareaktualisierung][img-update-3]

> [!NOTE]
> In einer Produktionsumgebung können Sie Aufträge für ein bestimmtes Wartungsfenster planen.

## <a name="scenario-review"></a>Szenariorückblick

In diesem Szenario haben Sie mithilfe des Alarmverlaufs auf dem Dashboard sowie mithilfe eines Filters ein mögliches Problem mit einigen Ihrer Remotegeräte identifiziert. Anschließend haben Sie die Geräte unter Verwendung des Filters und eines Auftrags remote konfiguriert, um weitere Informationen zu erhalten und das Problem besser diagnostizieren zu können. Und schließlich haben Sie unter Verwendung eines Filters und eines Auftrags die Wartung der betroffenen Geräte geplant. Auf dem Dashboard können Sie sich vergewissern, dass die Geräte in Ihrer Lösung keine weiteren Alarme generieren. Mithilfe eines Filters können Sie sich vergewissern, dass die Firmware auf allen Geräten in Ihrer Lösung auf dem neuesten Stand ist und dass keine fehlerhaften Geräte mehr vorhanden sind:

![Filter, der zeigt, dass die Firmware aller Geräte auf dem neuesten Stand ist][img-updated]

![Filter, der zeigt, dass alle Geräte fehlerfrei sind][img-healthy]

## <a name="other-features"></a>Andere Funktionen

In den folgenden Abschnitten werden einige Zusatzfeatures der vorkonfigurierten Lösung für die Remoteüberwachung beschrieben, die im vorherigen Szenario nicht behandelt wurden.

### <a name="customize-columns"></a>Anpassen von Spalten

Durch Auswählen von **Spalten-Editor** können Sie anpassen, welche Informationen in der Geräteliste angezeigt werden sollen. Sie können Spalten hinzufügen und entfernen, die gemeldete Eigenschaften und Tagwerte enthalten. Sie können Spalten auch neu anordnen und umbenennen:

   ![Spalten-Editor in der Geräteliste][img-columneditor]

### <a name="customize-the-device-icon"></a>Anpassen des Gerätesymbols

Sie können das Gerätesymbol in der Geräteliste im Bereich mit den **Gerätedetails** wie folgt anpassen:

1. Wählen Sie das Stiftsymbol aus, um den Bereich **Bild bearbeiten** für ein Gerät zu öffnen:

   ![Editor zum Öffnen des Gerätebilds][img-startimageedit]

1. Laden Sie ein neues Bild hoch, oder verwenden Sie eines der vorhandenen Bilder, und wählen Sie anschließend **Speichern** aus:

   ![Editor zum Bearbeiten des Gerätebilds][img-imageedit]

1. Das ausgewählte Bild wird jetzt in der **Symbol**-Spalte für das Gerät angezeigt.

> [!NOTE]
> Das Bild wird im Blob-Speicher gespeichert. Ein Tag im Gerätezwilling enthält einen Link zu dem Bild im Blob-Speicher.

### <a name="add-a-device"></a>Hinzufügen eines Geräts

Im Zuge der Bereitstellung der vorkonfigurierten Lösung werden automatisch 25 Beispielgeräte bereitgestellt. Diese werden in der Geräteliste angezeigt. Dies sind *simulierte Geräte* , die in einem Azure WebJob ausgeführt werden. Simulierte Geräte erleichtern Ihnen das Experimentieren mit der vorkonfigurierten Lösung, ohne dass Sie echte physische Geräte bereitstellen müssen. Falls Sie für ein echtes Gerät eine Verbindung mit der Lösung herstellen möchten, hilft Ihnen das Tutorial [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung][lnk-connect-rm] weiter.

Die folgenden Schritte verdeutlichen, wie Sie der Lösung ein simuliertes Gerät hinzufügen:

1. Navigieren Sie zurück zur Geräteliste.

1. Wählen Sie links unten **+ Gerät hinzufügen** aus, um ein Gerät hinzuzufügen.

   ![Gerät der vorkonfigurierten Lösung hinzufügen][img-adddevice]

1. Wählen Sie auf der Kachel **Simuliertes Gerät** die Option **Neu hinzufügen** aus.

   ![Neue Gerätedetails im Dashboard festlegen][img-addnew]

   Zusätzlich zum Erstellen eines neuen simulierten Geräts können Sie auch ein physisches Gerät hinzufügen, wenn Sie sich für die Erstellung eines **benutzerdefinierten Geräts**entscheiden. Weitere Informationen zum Verbinden von physischen Geräten mit der Lösung finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung von IoT Suite][lnk-connect-rm].

1. Wählen Sie **Eigene Geräte-ID definieren** aus, und geben Sie einen eindeutigen Geräte-ID-Namen wie **mydevice_01** ein.

1. Wählen Sie **Erstellen**.

   ![Neues Gerät speichern][img-definedevice]

1. Wählen Sie in Schritt 3 von **Simuliertes Gerät hinzufügen** die Option **Fertig** aus, um zur Geräteliste zurückzukehren.

1. Ihr Gerät wird mit **Wird ausgeführt** in der Geräteliste angezeigt.

    ![Neues Gerät in der Geräteliste anzeigen][img-runningnew]

1. Sie können auch die simulierten Telemetriedaten des neuen Geräts im Dashboard anzeigen:

    ![Telemetrie von neuem Gerät anzeigen][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Deaktivieren und Löschen eines Geräts

Sie können ein Gerät deaktivieren, und nachdem es deaktiviert wurde, können Sie es entfernen:

![Gerät deaktivieren und entfernen][img-disable]

### <a name="add-a-rule"></a>Hinzufügen einer Regel

Es sind keine Regeln für das neue Gerät vorhanden, das Sie gerade hinzugefügt haben. In diesem Abschnitt fügen Sie eine Regel hinzu, mit der ein Alarm ausgelöst wird, wenn die vom neuen Gerät gemeldete Temperatur 47 Grad übersteigt. Beachten Sie vor dem Beginn, dass im Dashboard unter dem Telemetriedatenverlauf für das neue Gerät zu sehen ist, dass die Gerätetemperatur 45 Grad nicht überschritten hat.

1. Navigieren Sie zurück zur Geräteliste.

1. Wählen Sie in der **Geräteliste** das neue Gerät und anschließend **Regel hinzufügen** aus, um eine Regel für das Gerät hinzuzufügen.

1. Erstellen Sie eine Regel, bei der **Temperature** als Datenfeld und **AlarmTemp** als Ausgabe verwendet wird, wenn die Temperatur 47 Grad überschreitet:

    ![Geräteregel hinzufügen][img-adddevicerule]

1. Wählen Sie **Speichern und Regeln anzeigen** aus, um die Änderungen zu speichern.

1. Wählen Sie im Bereich mit den Gerätedetails für das neue Gerät die Option **Befehle** aus.

    ![Geräteregel hinzufügen][img-adddevicerule2]

1. Wählen Sie in der Befehlsliste den Befehl **ChangeSetPointTemp** aus, und legen Sie **SetPointTemp** auf 45 fest. Wählen Sie dann **Befehl senden** aus:

    ![Geräteregel hinzufügen][img-adddevicerule3]

1. Kehren Sie zum Dashboard zurück. Nach kurzer Zeit wird im Bereich **Alarmverlauf** ein neuer Eintrag angezeigt, wenn die vom neuen Gerät gemeldete Temperatur den Schwellenwert von 47 Grad überschreitet:

    ![Geräteregel hinzufügen][img-adddevicerule4]

1. Sie können alle Regeln im Dashboard auf der Seite **Regeln** prüfen und bearbeiten:

    ![Geräteregeln auflisten][img-rules]

1. Sie können alle Aktionen, die als Reaktion auf eine Regel durchgeführt werden können, im Dashboard auf der Seite **Aktionen** prüfen und bearbeiten:

    ![Geräteaktionen auflisten][img-actions]

> [!NOTE]
> Sie können Aktionen definieren, bei denen als Reaktion auf eine Regel eine E-Mail oder SMS gesendet werden kann oder die per [Logik-App][lnk-logic-apps] in ein Branchensystem integriert werden können. Weitere Informationen finden Sie im Tutorial [Verbinden einer Logik-App mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite][lnk-logicapptutorial].

### <a name="manage-filters"></a>Verwalten von Filtern

In der Geräteliste können Sie Filter erstellen, speichern und erneut laden, um eine benutzerdefinierte Liste der Geräte anzuzeigen, die mit Ihrem Hub verbunden sind. So erstellen Sie einen Filter:

1. Wählen Sie über der Geräteliste das Filterbearbeitungssymbol aus:

    ![Öffnen des Filtereditors][img-editfiltericon]

1. Fügen Sie im **Filtereditor**die Felder, Operatoren und Werte zum Filtern der Geräteliste hinzu. Sie können mehrere Klauseln hinzufügen, um Ihren Filter zu verfeinern. Wählen Sie **Filter** aus, um den Filter anzuwenden:

    ![Erstellen eines Filters][img-filtereditor]

1. In diesem Beispiel wird die Liste nach Hersteller und Modellnummer gefiltert:

    ![Gefilterte Liste][img-filterelist]

1. Wählen Sie das Symbol **Speichern unter** aus, um den Filter mit einem benutzerdefinierten Namen zu speichern:

    ![Speichern eines Filters][img-savefilter]

1. Wählen Sie das Symbol **Gespeicherten Filter öffnen** aus, um einen zuvor gespeicherten Filter erneut anzuwenden:

    ![Öffnen eines Filters][img-openfilter]

Sie können Filter auf Grundlage von Geräte-ID, Gerätestatus, gewünschten Eigenschaften, gemeldeten Eigenschaften und Tags erstellen. Eigene benutzerdefinierte Tags können einem Gerät entweder im Abschnitt **Tags** des Bereichs **Gerätedetails** oder durch Ausführen eines Auftrags hinzugefügt werden, der Tags für mehrere Geräte aktualisiert.

> [!NOTE]
> Im **Filtereditor** können Sie die **Erweiterte Ansicht** zum direkten Bearbeiten des Abfragetexts verwenden.

### <a name="commands"></a>Befehle

Über den Bereich mit den **Gerätedetails** können Sie Befehle an das Gerät senden. Wenn ein Gerät zum ersten Mal gestartet wird, sendet es Informationen zu den unterstützten Befehlen an die Lösung. Eine Besprechung der Unterschiede zwischen Befehlen und Methoden finden Sie im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].

1. Wählen Sie im Bereich **Gerätedetails** des ausgewählten Geräts die Option **Befehle** aus:

   ![Gerätebefehle im Dashboard][img-devicecommands]

1. Wählen Sie in der Befehlsliste **PingDevice** aus.

1. Wählen Sie **Befehl senden** aus.

1. Sie sehen den Status des Befehls im Befehlsverlauf.

   ![Befehlsstatus im Dashboard][img-pingcommand]

Die Lösung verfolgt den Status jedes einzelnen Befehls, der gesendet wird. Zunächst lautet das Ergebnis **Ausstehend**. Wenn das Gerät meldet, dass es den Befehl ausgeführt hat, wird der Status in **Erfolgreich**geändert.

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

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine funktionierende vorkonfigurierte Lösung bereitgestellt und können nun mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung][lnk-rm-walkthrough]
* [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)][lnk-connect-rm]
* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
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
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md
