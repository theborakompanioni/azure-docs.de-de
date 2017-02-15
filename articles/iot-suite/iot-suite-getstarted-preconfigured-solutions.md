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
ms.date: 11/16/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f86a70a5207f19063e9992325c8f8d696ca7823e


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Tutorial: Erste Schritte mit den vorkonfigurierten Lösungen
## <a name="introduction"></a>Einführung
[Vorkonfigurierte Lösungen][lnk-preconfigured-solutions] von Azure IoT Suite kombinieren mehrere Azure IoT-Dienste, um durchgängige Lösungen bereitzustellen, die allgemeine IoT-Unternehmensszenarien implementieren. Die vorkonfigurierte Lösung für die *Remoteüberwachung* stellt eine Verbindung mit Ihren Geräten her und überwacht die Geräte. Sie können die Lösung zum Analysieren des Datenstroms von Ihren Geräten und Verbessern der Geschäftsergebnisse verwenden, indem Sie Prozesse so einrichten, dass sie automatisch auf diesen Datenstrom reagieren.

Dieses Tutorial zeigt, wie Sie eine vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen. Außerdem lernen Sie die grundlegenden Funktionen der Lösung für die Remoteüberwachung kennen. Sie können auf einen Großteil dieser Features über das Lösungsdashboard zugreifen, das zusammen mit der vorkonfigurierten Lösung bereitgestellt wird:

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

## <a name="view-the-device-list"></a>Anzeigen der Geräteliste
Die Geräteliste enthält alle registrierten Geräte in der Lösung. Sie können Gerätemetadaten anzeigen und bearbeiten, Geräte hinzufügen oder entfernen und Befehle an Geräte übermitteln.

1. Klicken Sie im linken Menü auf **Geräte** , um die *Geräteliste* für diese Lösung anzuzeigen.
   
   ![Geräteliste im Dashboard][img-devicelist]
2. Die Geräteliste zeigt, dass es vier simulierte Geräte gibt, die im Rahmen des Bereitstellungsprozesses erstellt werden.
3. Klicken Sie auf ein Gerät in der Geräteliste, um die Gerätedetails anzuzeigen.
   
   ![Gerätedetails im Dashboard][img-devicedetails]

Der Bereich **Gerätedetails** enthält drei Abschnitte:

* Im Abschnitt **Aktionen** sind die Aktionen aufgeführt, die Sie auf dem Gerät durchführen können. Wenn Sie das Gerät deaktivieren, ist es nicht mehr zulässig, dass Telemetriedaten gesendet oder Befehle empfangen werden. Wenn Sie ein Gerät deaktivieren, können Sie es wieder aktivieren. Sie können eine Regel hinzufügen, die dem Gerät zugeordnet ist und einen Alarm auslöst, wenn ein Telemetriewert einen Schwellenwert überschreitet. Sie können auch einen Befehl an ein Gerät senden. Wenn ein Gerät zum ersten Mal die Verbindung herstellt, teilt es der Lösung die Befehle mit, auf die es reagieren kann.
* Im Abschnitt **Geräteeigenschaften** sind die Metadaten des Geräts aufgeführt. Einige dieser Metadaten stammen vom Gerät selbst (z.B. der Hersteller), und einige werden von der Lösung generiert (z.B. der Erstellungszeitpunkt). Sie können die Metadaten des Geräts hier bearbeiten.
* Im Abschnitt **Authentifizierungsschlüssel** sind die Schlüssel aufgeführt, die vom Gerät für die Authentifizierung beim Gerät verwendet werden können.

## <a name="send-a-command-to-a-device"></a>Senden eines Befehls an ein Gerät
Im Detailbereich zu einem Gerät werden alle Befehle angezeigt, die von einem bestimmten Gerät unterstützt werden. Außerdem können Sie hier Befehle an ein Gerät senden. Wenn ein Gerät zum ersten Mal gestartet wird, sendet es Informationen zu den unterstützten Befehlen an die Lösung.

1. Klicken Sie im Bereich mit den Gerätedetails für das ausgewählte Gerät auf **Befehle** .
   
   ![Gerätebefehle im Dashboard][img-devicecommands]
2. Wählen Sie in der Befehlsliste **PingDevice** aus.
3. Klicken Sie auf **Befehl senden**.
4. Sie sehen den Status des Befehls im Befehlsverlauf.
   
   ![Befehlsstatus im Dashboard][img-pingcommand]

Die Lösung verfolgt den Status jedes einzelnen Befehls, der gesendet wird. Zunächst lautet das Ergebnis **Ausstehend**. Wenn das Gerät meldet, dass es den Befehl ausgeführt hat, wird der Status in **Erfolgreich**geändert.

## <a name="add-a-new-simulated-device"></a>Hinzufügen eines neuen simulierten Geräts
Bei der Bereitstellung der vorkonfigurierten Lösung stellen Sie automatisch die vier Beispielgeräte bereit, die in der Geräteliste angezeigt werden. Dies sind *simulierte Geräte* , die in einem Azure WebJob ausgeführt werden. Simulierte Geräte erleichtern Ihnen das Experimentieren mit der vorkonfigurierten Lösung, ohne dass Sie echte physische Geräte bereitstellen müssen. Falls Sie für ein echtes Gerät eine Verbindung mit der Lösung herstellen möchten, hilft Ihnen das Tutorial [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung][lnk-connect-rm] weiter.

Die folgenden Schritte verdeutlichen, wie Sie der Lösung ein simuliertes Gerät hinzufügen:

1. Navigieren Sie zurück zur Geräteliste.
2. Klicken Sie in der unteren linken Ecke auf **+ Gerät hinzufügen** , um ein Gerät hinzuzufügen.
   
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

## <a name="edit-the-device-metadata"></a>Bearbeiten der Metadaten des Geräts
Wenn für ein Gerät zum ersten Mal eine Verbindung mit der Lösung hergestellt wird, werden seine Metadaten an die Lösung gesendet. Wenn Sie die Gerätemetadaten im Lösungsdashboard bearbeiten, werden die neuen Metadatenwerte an das Gerät gesendet, und die neuen Werte werden in der DocumentDB-Datenbank der Lösung gespeichert. Weitere Informationen finden Sie unter [Geräteidentitätsregistrierung und DocumentDB][lnk-devicemetadata].

1. Navigieren Sie zurück zur Geräteliste.
2. Wählen Sie das neue Gerät in der **Geräteliste** aus, und klicken Sie dann auf **Bearbeiten**, um die **Geräteeigenschaften** zu bearbeiten:
   
   ![Gerätemetadaten bearbeiten][img-editdevice]
3. Scrollen Sie nach unten, und nehmen Sie eine Änderung an den Breiten- und Längengradwerten vor. Klicken Sie anschließend auf **Änderungen in Geräteregistrierung speichern**.
   
    ![Gerätemetadaten bearbeiten][img-editdevice2]
4. Navigieren Sie zurück zum Dashboard. Sie sehen, dass sich die Position des Geräts in der Karte geändert hat:
   
    ![Gerätemetadaten bearbeiten][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Hinzufügen einer Regel für das neue Gerät
Es sind keine Regeln für das neue Gerät vorhanden, das Sie gerade hinzugefügt haben. In diesem Abschnitt fügen Sie eine Regel hinzu, mit der ein Alarm ausgelöst wird, wenn die vom neuen Gerät gemeldete Temperatur 47 Grad übersteigt. Beachten Sie vor dem Beginn, dass im Dashboard unter dem Telemetriedatenverlauf für das neue Gerät zu sehen ist, dass die Gerätetemperatur 45 Grad nicht überschritten hat.

1. Navigieren Sie zurück zur Geräteliste.
2. Wählen Sie das neue Gerät in der **Geräteliste** aus, und klicken Sie dann auf **Regel hinzufügen**, um eine Regel für das Gerät hinzuzufügen.
3. Erstellen Sie eine Regel, bei der **Temperature** als Datenfeld und **AlarmTemp** als Ausgabe verwendet wird, wenn die Temperatur 47 Grad überschreitet:
   
    ![Geräteregel hinzufügen][img-adddevicerule]
4. Klicken Sie auf **Regeln speichern und anzeigen** , um die Änderungen zu speichern.
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

## <a name="other-features"></a>Andere Funktionen
Mit dem Lösungsportal können Sie nach Geräten mit bestimmten Merkmalen wie z. B. einer Modellnummer suchen:

![Nach Gerät suchen][img-search]

Sie können ein Gerät deaktivieren, und nachdem es deaktiviert wurde, können Sie es entfernen:

![Gerät deaktivieren und entfernen][img-disable]

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
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
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
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Feb17_HO2-->


