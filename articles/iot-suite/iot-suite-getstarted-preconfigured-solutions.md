<properties
	pageTitle="Erste Schritte mit vorkonfigurierten Lösungen | Microsoft Azure"
	description="In diesem Tutorial erlernen Sie das Bereitstellen einer vorkonfigurierten Azure IoT Suite-Lösung."
	services=""
    suite="iot-suite"
	documentationCenter=""
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="03/02/2016"
     ms.author="dobett"/>

# Tutorial: Erste Schritte mit den vorkonfigurierten Lösungen

## Einführung

[Vorkonfigurierte Lösungen][lnk-preconfigured-solutions] der Azure IoT- Suite kombinieren mehrere Azure IoT-Dienste, um durchgängige Lösungen bereitzustellen, die allgemeine IoT-Unternehmensszenarien implementieren.

Dieses Tutorial zeigt, wie Sie eine vorkonfigurierte Lösung für die *Remoteüberwachung* bereitstellen. Außerdem lernen Sie die grundlegenden Funktionen der vorkonfigurierten Lösung für die Remoteüberwachung kennen.

Sie benötigen ein aktives Azure-Abonnement, um dieses Lernprogramm auszuführen.

> [AZURE.NOTE]  Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].

## Bereitstellen einer vorkonfigurierten Lösung für die Remoteüberwachung

1.  Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto bei [azureiotsuite.com][lnk-azureiotsuite] an, und klicken Sie auf **+**, um eine neue Lösung zu erstellen.

    > [AZURE.NOTE] Wenn Sie Probleme mit den Berechtigungen für die Bereitstellung einer Lösung haben, finden Sie unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions] weitere Informationen.

2.  Klicken Sie auf der Kachel **Remoteüberwachung** auf **Auswählen**.

3.  Geben Sie einen **Lösungsnamen** für Ihre vorkonfigurierte Remoteüberwachungslösung ein.

4.  Wählen Sie die **Region** und das **Abonnement**, die Sie zum Bereitstellen der Lösung verwenden möchten.

5.  Klicken Sie auf **Lösung erstellen**, um den Bereitstellungsprozess zu beginnen. Dies dauert normalerweise einige Minuten.

## Warten auf den Abschluss des Bereitstellungsvorgangs

1. Klicken Sie auf die Kachel für Ihre Lösung mit dem Status **Bereitstellung**.
 
2. Beachten Sie die **Bereitstellungsstatus** während der Bereitstellung von Azure-Dienste im Azure-Abonnement.

3. Nach Abschluss der Bereitstellung ändert sich der Status **Bereit**.

4. Klicken Sie auf die Kachel, um im rechten Bereich Details zur Lösung anzuzeigen.

> [AZURE.NOTE] Falls beim Bereitstellen der vorkonfigurierten Lösung Probleme auftreten, helfen Ihnen die Informationen unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions] und die [häufig gestellten Fragen][lnk-faq] weiter. Wenn die Probleme weiterhin auftreten, können Sie im [Portal][lnk-portal] ein Dienstticket erstellen.

Erwarten Sie Informationen zu Ihrer Lösung, die nicht aufgeführt werden? Senden Sie uns Vorschläge für neue Funktionen über den [Feedbackbereich](https://feedback.azure.com/forums/321918-azure-iot) zu.

## Anzeigen des Dashboards der Remoteüberwachungslösung

Im Lösungsdashboard können Sie die bereitgestellte Lösung verwalten. Sie können z. B. Telemetriedaten anzeigen, Geräte hinzufügen und Regeln konfigurieren.

1.  Wenn die Bereitstellung abgeschlossen ist und auf der Kachel für die vorkonfigurierte Lösung **Bereit** angezeigt wird, klicken Sie auf **Starten**, um das Portal der Remoteüberwachungslösung auf einer neuen Registerkarte zu öffnen.

    ![][img-launch-solution]

2.  Standardmäßig zeigt das Lösungsportal das *Lösungsdashboard* an. Sie können im linken Menü andere Ansichten auswählen.

    ![][img-dashboard]

Im Dashboard werden die folgenden Informationen angezeigt:

- Auf der Karte werden die Standorte der einzelnen Geräte angezeigt, die mit der Lösung verbunden sind. Beim ersten Ausführen der Lösung sind vier simulierte Geräte vorhanden. Die simulierten Geräte werden als Azure WebJobs implementiert, und für die Lösung wird die Bing Maps-API verwendet, um Informationen zu Karte darzustellen.
- Im Bereich **Telemetrieverlauf** werden Telemetriedaten zur Luftfeuchtigkeit und Temperatur von einem ausgewählten Gerät nahezu in Echtzeit dargestellt und zusammengefasste Daten angezeigt, z.B. die maximale, minimale und durchschnittliche Luftfeuchtigkeit.
- Im Bereich **Alarmverlauf** werden die letzten Alarmereignisse für die Fälle angezeigt, in denen ein Telemetriewert einen Schwellenwert überschritten hat. Sie können zusätzlich zu den Beispielen, die von der vorkonfigurierten Lösung erstellt werden, auch eigene Alarme definieren.

## Anzeigen der Lösungsgeräteliste

Die Geräteliste enthält alle registrierten Geräte in der Lösung. Sie können Gerätemetadaten anzeigen und bearbeiten, Geräte hinzufügen oder entfernen und Befehle an Geräte übermitteln.

1.  Klicken Sie im linken Menü auf **Geräte**, um die *Geräteliste* für diese Lösung anzuzeigen.

    ![][img-devicelist]

2.  Die Geräteliste zeigt, dass es vier simulierte Geräte gibt, die im Rahmen des Bereitstellungsprozesses erstellt werden.

3.  Klicken Sie auf ein Gerät in der Geräteliste, um die Gerätedetails anzuzeigen.

    ![][img-devicedetails]

Der Bereich **Gerätedetails** enthält drei Abschnitte:

- Im Abschnitt **Aktionen** sind die Aktionen aufgeführt, die Sie auf dem Gerät durchführen können. Wenn Sie das Gerät deaktivieren, ist es nicht mehr zulässig, dass Telemetriedaten gesendet oder Befehle empfangen werden. Wenn Sie ein Gerät deaktivieren, können Sie es wieder aktivieren. Sie können eine Regel hinzufügen, die dem Gerät zugeordnet ist und einen Alarm auslöst, wenn ein Telemetriewert einen Schwellenwert überschreitet. Sie können auch einen Befehl an ein Gerät senden. Wenn ein Gerät zum ersten Mal die Verbindung herstellt, teilt es der Lösung die Befehle mit, auf die es reagieren kann.
- Im Abschnitt **Geräteeigenschaften** sind die Metadaten des Geräts aufgeführt. Einige dieser Metadaten stammen vom Gerät selbst (z.B. der Hersteller), und einige werden von der Lösung generiert (z.B. der Erstellungszeitpunkt). Sie können die Metadaten des Geräts hier bearbeiten.
- Im Abschnitt **Authentifizierungsschlüssel** sind die Schlüssel aufgeführt, die vom Gerät für die Authentifizierung beim Gerät verwendet werden können.

## Senden eines Befehls an ein Gerät

Im Detailbereich zu einem Gerät werden alle Befehle angezeigt, die von einem bestimmten Gerät unterstützt werden. Außerdem können Sie hier Befehle an ein Gerät senden. Wenn ein Gerät zum ersten Mal gestartet wird, sendet es Informationen zu den unterstützten Befehlen an die Lösung.

1.  Klicken Sie im Bereich mit den Gerätedetails für das ausgewählte Gerät auf **Befehle**.

    ![][img-devicecommands]

2.  Wählen Sie in der Befehlsliste **PingDevice** aus.

3.  Klicken Sie auf **Befehl senden**.

4.  Sie sehen den Status des Befehls im Befehlsverlauf.

    ![][img-pingcommand]

Die Lösung verfolgt den Status jedes einzelnen Befehls, der gesendet wird. Zunächst lautet das Ergebnis **Ausstehend**. Wenn das Gerät meldet, dass es den Befehl ausgeführt hat, wird der Status in **Erfolgreich** geändert.

## Hinzufügen eines neuen simulierten Geräts

1.  Navigieren Sie zurück zur Geräteliste.

2.  Klicken Sie in der unteren linken Ecke auf **+ Gerät hinzufügen**, um ein neues Gerät hinzuzufügen.

    ![][img-adddevice]

3.  Klicken Sie auf der Kachel **Simuliertes Gerät** auf **Neues hinzufügen**.

    ![][img-addnew]
    
    Zusätzlich zum Erstellen eines neuen simulierten Geräts können Sie auch ein physisches Gerät hinzufügen, wenn Sie sich für die Erstellung eines **benutzerdefinierten Geräts** entscheiden. Weitere Informationen finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung von IoT Suite][lnk-connecting-devices].

4.  Wählen Sie **Eigene Geräte-ID definieren** aus, und geben Sie einen eindeutigen Geräte-ID-Namen wie **mydevice\_01** ein.

5.  Klicken Sie auf **Erstellen**.

    ![][img-definedevice]

6. Klicken Sie in Schritt 3 von **Simuliertes Gerät hinzufügen** auf **Fertig**, um zur Geräteliste zurückzukehren.

7. Ihr Gerät wird mit **Wird ausgeführt** in der Geräteliste angezeigt.

    ![][img-runningnew]

8. Sie können auch die simulierten Telemetriedaten des neuen Geräts im Dashboard anzeigen:

    ![][img-runningnew-2]

## Bearbeiten der Metadaten des Geräts

1.  Navigieren Sie zurück zur Geräteliste.

2.  Wählen Sie das neue Gerät in der **Geräteliste** aus, und klicken Sie dann auf **Bearbeiten**, um die **Geräteeigenschaften** zu bearbeiten:

    ![][img-editdevice]

3. Scrollen Sie nach unten, und nehmen Sie eine Änderung an den Breiten- und Längengradwerten vor. Klicken Sie anschließend auf **Änderungen in Geräteregistrierung speichern**.

    ![][img-editdevice2]

4. Navigieren Sie zurück zum Dashboard. Sie sehen, dass sich die Position des Geräts in der Karte geändert hat:

    ![][img-editdevice3]

## Hinzufügen einer Regel für das neue Gerät

Es sind keine Regeln für das neue Gerät vorhanden, das Sie gerade hinzugefügt haben. In diesem Abschnitt fügen Sie eine Regel hinzu, mit der ein Alarm ausgelöst wird, wenn die vom neuen Gerät gemeldete Temperatur 47 Grad übersteigt. Beachten Sie vor dem Beginn, dass im Dashboard unter dem Telemetriedatenverlauf für das neue Gerät zu sehen ist, dass die Gerätetemperatur 45 Grad nicht überschritten hat.

1.  Navigieren Sie zurück zur Geräteliste.

2.  Wählen Sie das neue Gerät in der **Geräteliste** aus, und klicken Sie dann auf **Regel hinzufügen**, um eine neue Regel für das Gerät hinzuzufügen.

3. Erstellen Sie eine Regel, bei der **Temperature** als Datenfeld und **AlarmTemp** als Ausgabe verwendet wird, wenn die Temperatur 47 Grad überschreitet:

    ![][img-adddevicerule]

4. Klicken Sie auf **Regeln speichern und anzeigen**, um die Änderungen zu speichern.

5.  Klicken Sie im Bereich mit den Gerätedetails für das neue Gerät auf **Befehle**.

    ![][img-adddevicerule2]

6.  Wählen Sie in der Befehlsliste den Befehl **ChangeSetPointTemp** aus, und legen Sie **SetPointTemp** auf 45 fest. Klicken Sie anschließend auf **Befehl senden**:

    ![][img-adddevicerule3]

7.  Navigieren Sie zurück zum Dashboard der Lösung. Nach kurzer Zeit wird im Bereich **Alarmverlauf** ein neuer Eintrag angezeigt, wenn die vom neuen Gerät gemeldete Temperatur den Schwellenwert von 47 Grad überschreitet:

    ![][img-adddevicerule4]

8. Sie können alle Regeln im Dashboard auf der Seite **Regeln** prüfen und bearbeiten:

    ![][img-rules]

9. Sie können alle Aktionen, die als Reaktion auf eine Regel durchgeführt werden können, im Dashboard auf der Seite **Aktionen** prüfen und bearbeiten:

    ![][img-actions]

> [AZURE.NOTE] Sie können Aktionen definieren, bei denen als Reaktion auf eine Regel eine E-Mail oder SMS gesendet werden kann oder die per [Logik-App][lnk-logic-apps] in ein Branchensystem integriert werden können.

## Abläufe im Hintergrund

Wenn Sie eine vorkonfigurierte Lösung bereitstellen, werden vom Bereitstellungsprozess mehrere Ressourcen im gewählten Azure-Abonnement erstellt. Sie können diese Ressourcen im Azure-[Portal][lnk-portal] anzeigen. Der Bereitstellungsprozess erstellt eine **Ressourcengruppe** mit einem Namen basierend auf dem Namen, den Sie für Ihre vorkonfigurierte Lösung gewählt haben:

![][img-portal]

Sie können die Einstellungen der einzelnen Ressourcen anzeigen, indem Sie sie in der Ressourcengruppe in der Liste mit den Ressourcen auswählen. Im obigen Screenshot sind die Einstellungen für den IoT Hub in der vorkonfigurierten Lösung dargestellt.

Sie können auch den Quellcode für die vorkonfigurierte Lösung anzeigen. Den Quellcode zur vorkonfigurierten Lösung für die Remoteüberwachung finden Sie unter [azure-iot-remote-monitoring][lnk-rmgithub]\:

- Der Ordner **DeviceAdministration** enthält den Quellcode für das Dashboard.
- Der Ordner **Simulator** enthält den Quellcode für das simulierte Gerät.
- Der Ordner **EventProcessor** enthält den Quellcode für den Back-End-Prozess, mit dem die eingehenden Telemetriedaten behandelt werden.

Wenn Sie fertig sind, können Sie die vorkonfigurierte Lösung auf der Website [azureiotsuite.com][lnk-azureiotsuite] aus Ihrem Azure-Abonnement löschen. Dadurch können Sie problemlos alle Ressourcen löschen, die bei der Erstellung der vorkonfigurierten Lösung bereitgestellt wurden.

> [AZURE.NOTE] Um sicherzustellen, dass Sie alle zur vorkonfigurierten Lösung gehörenden Daten löschen, sollten Sie sie aus [azureiotsuite.com][lnk-azureiotsuite] löschen und nicht nur einfach die Ressourcengruppe im Portal löschen.

## Nächste Schritte

Nachdem Sie jetzt eine funktionsfähige vorkonfigurierte Lösung erstellt haben, können Sie mit den folgenden exemplarischen Vorgehensweisen fortfahren:

-   [Anleitung zum Anpassen vorkonfigurierter Lösungen][lnk-customize]
-   [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf][lnk-predictive]

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

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-predictive]: iot-suite-predictive-overview.md
[lnk-connecting-devices]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-faq]: iot-suite-faq.md

<!---HONumber=AcomDC_0427_2016-->