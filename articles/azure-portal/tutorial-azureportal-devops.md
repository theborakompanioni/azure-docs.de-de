<properties
	pageTitle="Tutorial: DevOps mit dem Azure-Portal | Microsoft Azure"
	description="Es wird beschrieben, welche DevOps-Workflows im Azure-Portal enthalten sind."
	services="azure-portal"
	documentationCenter=""
	authors="mlearned"
	manager="douge"
	editor="mlearned"/>

<tags
	ms.service="multiple"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="06/05/2016"
	ms.author="mlearned"/>

# Tutorial: DevOps mit dem Azure-Portal

Die Azure-Plattform enthält viele flexible DevOps-Workflows. In diesem Tutorial erfahren Sie, wie Sie die Funktionen des Azure-Portals nutzen, um ausgeführte Anwendungen zu entwickeln, zu testen, bereitzustellen, die Problembehandlung durchzuführen, zu überwachen und zu verwalten. In diesem Tutorial geht es vor allem um Folgendes:

1.  Erstellen einer Web-App und Aktivieren von Continuous Deployment

2.  Entwickeln und Testen einer App

3.  Überwachen und Durchführen der Problembehandlung für eine App

4.  Allgemeine Aufgaben der Anwendungsverwaltung

## Erstellen einer Web-App und Aktivieren von Continuous Deployment

Erstellen Sie eine Web-App mit [Azure App Service](https://azure.microsoft.com/services/app-service/), die Sie für dieses Tutorial verwenden können. Zuerst ermöglichen Sie den Continuous Deployment-Vorgang aus Ihrem Quellcode-Repository in die aktive Azure-Umgebung.

1.  Anmelden am Azure-Portal

2.  Wählen Sie **App Services** &gt; **Symbol hinzufügen**, und geben Sie einen Namen ein. Wählen Sie Ihr Abonnement aus, und erstellen Sie eine neue Ressourcengruppe, die als Container für den Dienst dienen kann.

    Mit Ressourcengruppen können Sie verschiedene Aspekte der Lösung, z.B. Abrechnung, Bereitstellungen und Überwachung, mit [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-overview/) als einzelne Gruppe verwalten.

	![Bild1][image1]


3.  Der App-Dienst wird nach kurzer Zeit erstellt. Sehen Sie sich die verschiedenen Menüoptionen für den Dienst im Portal an.

	![Bild2][image2]

4.  Klicken Sie auf die URL. Beachten Sie die vielen unterschiedlichen Optionen für Tools und Repositorys. Sie können auch die gewünschten Sprachen und Frameworks verwenden, z.B. .NET, Java und Ruby.

	![Bild3][image3]

3.  Mit dem Azure-Portal ist Continuous Deployment ein einfacher Prozess, der nur einige einfache Schritte umfasst. Wählen Sie im Azure-Portal über das Symbol für den gerade erstellten App-Dienst die Option „Einstellungen“.

    ![Bild4][image4]

    Scrollen Sie im Blatt, das auf der rechten Seite geöffnet wird, zum Abschnitt für die Veröffentlichung.

    ![Bild5][image5]

4.  Konfigurieren Sie anschließend einige Einstellungen, um Continuous Deployment für die App zu aktivieren. Klicken Sie auf „Bereitstellungsquelle“, und klicken Sie dann auf „Quelle auswählen“. Beachten Sie die vielen Optionen für Repositoryquellen.

	![Bild6][image6]

1.  Wählen Sie für dieses Beispiel „GitHub“. Optional können Sie auch das Repository Ihrer Wahl wählen und die Anmeldeinformationen für die Autorisierung einrichten.

    ![Bild7][image7]

2.  Nach der Autorisierung für Ihr Repository können Sie dann ein Projekt und eine Verzweigung für die Bereitstellung auswählen. Unten sind einige fiktive Beispiele aufgelistet.

    ![Bild8][image8]

3.  Klicken Sie auf „OK“, nachdem Sie das Projekt und die Verzweigung ausgewählt haben. Nun sollten die ersten Benachrichtigungen über eine Bereitstellung eintreffen.

    ![Bild9][image9]

4.  Navigieren Sie zurück zu GitHub, um den Webhook anzuzeigen, der zum Integrieren des Repositorys für die Quellcodeverwaltung in Azure erstellt wurde. Im Azure-Portal sind für die Integration in GitHub nur wenige einfache Schritte erforderlich.

    ![Bild10][image10]

5.  Sie fügen dem Repository schnell Inhalte hinzu, um den Continuous Deployment-Vorgang zu veranschaulichen. Ein einfaches Beispiel besteht darin, einem GitHub-Repository eine einfache Textdatei hinzuzufügen. Sie können .NET, Ruby, Python oder eine andere Art von Anwendung mit App Service verwenden. Fügen Sie dem Repository Ihrer Wahl ggf. eine Textdatei oder eine ASP.NET MVC-, Java- oder Ruby-Anwendung hinzu.

    ![Bild11][image11]

6.  Nachdem Sie die Änderungen für das Repository übernommen haben, sehen Sie im Benachrichtigungsbereich des Portals, dass eine neue Bereitstellung initiiert wird. Klicken Sie auf „Synchronisieren“, wenn nach der Übernahme für das Repository nicht schnell Änderungen zu erkennen sind.

    ![Bild12][image12]

7.  Wenn Sie an diesem Punkt versuchen, die Seite für den App-Dienst zu laden, erhalten Sie ggf. den Fehler 403. Bei diesem Beispiel liegt dies daran, dass kein typisches Standarddokument für die Seite eingerichtet wurde, z.B. eine Datei wie „index.htm“ oder „default.html“. Sie können den Fehler mit den Tools im Azure-Portal schnell beheben. Wählen Sie im Azure-Portal die Option „Einstellungen“ &gt; „Anwendungseinstellungen“.

	 ![Bild13][image13]

8.  Das Blatt mit den Anwendungseinstellungen wird geöffnet. Geben Sie den Namen der Seite „SamplePage.html“ ein, und klicken Sie auf „Speichern“. Nehmen Sie sich kurz Zeit, um sich die anderen Einstellungen anzusehen.

	![Bild14][image14]

9.  Optional können Sie die Browser-URL aktualisieren, um sicherzustellen, dass die erwarteten Änderungen angezeigt werden. In diesem Fall wird auf der Seite jetzt einfacher Text angezeigt. Jede weitere Änderung am Repository führt zu einer neuen automatischen Bereitstellung.

    ![Bild15][image15]

    Die Aktivierung von Continuous Deployment im Azure-Portal ist ein einfacher Vorgang. Sie können auch komplexere Releasepipelines erstellen und viele andere Verfahren mit vorhandenen Systemen für Quellcodeverwaltung und Continuous Integration für die Bereitstellung unter Azure verwenden, z.B. die Nutzung von automatisierten Build- und Releaseverwaltungssystemen.

## Entwickeln und Testen einer App

Nehmen Sie als Nächstes einige Änderungen an der Codebasis vor, und stellen Sie diese Änderungen in kurzer Zeit bereit. Außerdem richten Sie Leistungstests für die Web-App ein.

1.  Wählen Sie im Azure-Portal im Navigationsbereich die Option „App Services“, und suchen Sie nach Ihrem App-Dienst.

    ![Bild16][image16]

2.  Klicken Sie auf „Tools“.

    ![Bild17][image17]

3.  Beachten Sie unter „Tools“ die Kategorie „Entwickeln“. Hier befinden sich einige nützliche Tools, die die Arbeit mit Apps ermöglichen, ohne das Azure-Portal zu verlassen. Klicken Sie auf „Konsole“.

    ![Bild18][image18]

4.  Im Konsolenfenster können Sie Livebefehle für die App ausführen. Geben Sie den Befehl „dir“ ein, und drücken Sie die EINGABETASTE. Beachten Sie, dass Befehle, für die erhöhte Rechte erforderlich sind, nicht funktionieren.

    ![Bild19][image19]

5.  Wechseln Sie zurück zur Kategorie „Entwickeln“, und wählen Sie „Visual Studio Online“. Hinweis: Visual Studio Online heißt jetzt Visual Studio Team Services.

    ![Bild20][image20]

6.  Aktivieren Sie die browserinterne Bearbeitungsoberfläche für Ihre App.

    ![Bild21][image21]

7.  Für die App wird eine Weberweiterung installiert. Mit Erweiterungen können Apps in Azure schnell und einfach Funktionen hinzugefügt werden. Unten im Screenshot sind einige andere verfügbare Erweiterungstypen dargestellt.

    ![Bild22][image22]

8.  Klicken Sie nach der Installation der Visual Studio Online-Erweiterung auf „Los“.

    ![Bild23][image23]

9.  Eine Browserregisterkarte mit einer Entwicklungsumgebung direkt im Browser wird geöffnet. Die unten dargestellte Oberfläche befindet sich in Chrome.

    ![Bild24][image24]

10. Sie können mehrere Aktivitäten durchführen, z.B. Dateien bearbeiten, Dateien und Ordner hinzufügen und Inhalte von der Live-Website herunterladen. Nehmen Sie eine schnelle Änderung an der Datei „SamplePage.html“ vor.

    ![Bild25][image25]

11. Die Änderungen werden nach wenigen Augenblicken automatisch gespeichert. Wenn Sie zurück zur Seite navigieren, sehen Sie die Änderungen. Beachten Sie, dass Änderungen dieser Art im Livezustand meist nicht für Produktionsumgebungen geeignet sind. Mit den Tools ist es aber sehr einfach, in Entwicklungs- und Testumgebungen schnell Änderungen vorzunehmen.

    ![Bild26][image26]

    ![Bild27][image27]

12. Wechseln Sie zurück zum Blatt „Tools“, und klicken Sie unter der Kategorie „Entwickeln“ auf „Leistungstest“.

    ![Bild28][image28]

13. Sie müssen ein Team Services-Konto festlegen. Weitere Details finden Sie in diesem [Artikel](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) mit Informationen zum Erstellen eines Team Services-Kontos.

14. Klicken Sie auf „Neu“, um einen Leistungstest zu erstellen.

    ![Bild29][image29]

    Konfigurieren Sie die verschiedenen Werte, und klicken Sie unten im Dialogfeld auf „Test ausführen“, um einen Leistungstest zu initiieren.

    ![Bild30][image30]

	![Bild31][image31]

1.  Wenn der Test ausgeführt wird, können Sie den Status überwachen.

    ![Bild32][image32]

    Nach Abschluss des Tests können Sie auf das Ergebnis klicken, um weitere Details anzuzeigen.

    ![Bild33][image33]

2.  In diesem Beispiel haben Sie nur einen kleinen Testlauf erstellt, sodass nur eine eingeschränkte Menge an Daten analysiert werden kann. Es werden aber verschiedene Metriken angezeigt, und Sie können den Test über diese Ansicht erneut ausführen. Mit dem Azure-Portal ist das Erstellen, Ausführen und Analysieren von Webleistungstests ein einfacher Vorgang. In den folgenden Screenshots sind die Leistungsdaten dargestellt.

    ![Bild34][image34]

    ![Bild35][image35]

    ![Bild36][image36]

## Überwachen und Durchführen der Problembehandlung für eine App

Azure verfügt über viele Funktionen für die Überwachung und Problembehandlung laufender Anwendungen.

1.  Wählen Sie im Azure-Portal für unsere Web-App die Option „Tools“.

    ![Bild37][image37]

2.  Beachten Sie in der Kategorie „Problembehandlung“ die verschiedenen Auswahlmöglichkeiten zur Verwendung von Tools zum Durchführen der Problembehandlung für potenzielle Probleme mit einer laufenden App. Sie können beispielsweise HTTP-Livedatenverkehr überwachen, die Selbstreparatur aktivieren, Protokolle anzeigen und vieles mehr.

    ![Bild38][image38]

3.  Wählen Sie die Option „Websitemetriken“, um schnell eine Ansicht mit HTTP-Codes zu erhalten.

    ![Bild39][image39]

4.  Wählen Sie die Option „Diagnostics-as-a-Service“. Wählen Sie Ihren Anwendungstyp und dann die Option „Ausführen“.

    ![Bild40][image40]

    Eine Erfassung wird durchgeführt.

    ![Bild41][image41]

5.  Sie können das entsprechende Protokoll auswählen, um potenzielle Probleme zu diagnostizieren. Sie müssen die Protokollierung aktivieren, um alle verfügbaren Datenoptionen anzuzeigen, z.B. HTTP-Protokolle.

    ![Bild42][image42]

    Durch das Klicken auf die Speicherabbilddatei können Sie einen DebugDiag-Analysebericht herunterladen und analysieren, um die Ermittlung potenzieller Probleme zu vereinfachen.

    ![Bild43][image43]

6.  Um weitere Daten anzuzeigen, müssen Sie die zusätzliche Protokollierung aktivieren. Navigieren Sie im Azure-Portal zur Web-App, und wählen Sie die Option „Einstellungen“.

    ![Bild44][image44]

7.  Scrollen Sie nach unten zur Kategorie „Funktionen“, und wählen Sie „Diagnoseprotokolle“.

  	 ![Bild45][image45]

8.  Beachten Sie die verschiedenen Optionen für die Protokollierung. Aktivieren Sie die Webserverprotokollierung, und klicken Sie auf „Speichern“.

    ![Bild46][image46]

9.  Wechseln Sie zurück in den Toolbereich für die App, und wählen Sie „Diagnostics-as-a-Service“. Klicken Sie anschließend auf „Ausführen“, um die Datenerfassung erneut durchzuführen.

    ![Bild47][image47]

10. Bei aktivierter Einstellung für die HTTP-Protokollierung sehen Sie nun, dass Daten für HTTP-Protokolle erfasst werden.

    ![Bild48][image48]

11. Durch Klicken auf das HTML-Dateiprotokoll erzeugen Sie einen umfassenden browserbasierten Bericht für die weitere Untersuchung.

    ![Bild49][image49]

12. Wechseln Sie im Azure-Portal für die App zurück zum Abschnitt „Tools“. Scrollen Sie zum Abschnitt „Tools“, und wählen Sie „Prozess-Explorer“.

    ![Bild50][image50]

13. Durch die Auswahl von „Prozess-Explorer“ können Sie Details zu aktiven Prozessen anzeigen. Beachten Sie unten die Möglichkeit zum Aufrufen von Prozessdetails und Beenden von Prozessen – alles über das Azure-Portal.

    ![Bild51][image51]

    ![Bild52][image52]

14. Wechseln Sie zurück zum Blatt „Einstellungen“ auf der linken Seite. Klicken Sie auf „Neue Supportanfrage“.

    ![Bild53][image53]

15. Im Blatt auf der rechten Seite können Sie Details zu den Problemen angeben, Kontaktdaten eingeben und sogar Diagnosedaten hochladen. Durch das Azure-Portal wird die Zusammenarbeit mit dem Microsoft Support zu einem reibungslosen Vorgang.

    ![Bild54][image54]

    ![Bild55][image55]

    Mit dem Azure-Portal können leistungsstarke und vertraute Tools bereitgestellt werden, die Ihnen das Überwachen und das Durchführen der Problembehandlung für unsere laufenden Anwendungen erleichtern. Außerdem können Sie schnell Maßnahmen ergreifen, indem Sie Aufgaben wie das Recyceln von Prozessen, Aktivieren und Deaktivieren verschiedener Datenerfassungen und sogar das Integrieren in den professionellen Microsoft Support durchführen können.

## Allgemeine Anwendungsverwaltung

Beim Verwalten von Anwendungen müssen Sie häufig viele verschiedene Aktivitäten durchführen, z.B. das Konfigurieren von Sicherungsstrategien, Implementieren und Verwalten von Identitätsanbietern und Konfigurieren der rollenbasierten Zugriffssteuerung. Wie bei anderen DevOps-Oberflächen auch, werden diese Aufgaben mit der Azure-Plattform direkt in das Portal integriert.

1.  Sie müssen Sicherungen konfigurieren, um sicherzustellen, dass die Web-App vor Datenverlust geschützt ist. Navigieren Sie zum Bereich „Einstellungen“ für die Web-App.

    ![Bild56][image56]

2.  Scrollen Sie im Blatt auf der rechten Seite nach unten zur Kategorie „Funktionen“.

	 ![Bild57][image57]

1.  Wählen Sie die Option „Sicherungen“. Auf der rechten Seite wird ein Blatt geöffnet.

    ![Bild58][image58]

2.  Klicken Sie auf „Konfigurieren“, und wählen Sie im Blatt auf der rechten Seite ein Speicherkonto aus.

    ![Bild59][image59]

3.  Erstellen Sie nun einen Speichercontainer für die Sicherungen, und wählen Sie ihn aus. Klicken Sie unten auf dem Blatt auf „Erstellen“. Wählen Sie anschließend den Container aus.

    ![Bild60][image60]

4.  Nach dem Auswählen des Containers können Sie Zeitpläne konfigurieren und Sicherungen für Ihre Datenbanken einrichten. Klicken Sie für dieses Szenario auf das Symbol „Speichern“.

     ![Bild61][image61]

5.  Scrollen Sie nach dem Speichern zurück zum Blatt für Sicherungen auf der linken Seite. Klicken Sie auf „Jetzt sichern“, um die Anwendung zu sichern.

     ![Bild62][image62]

6.  Nach kurzer Zeit sehen Sie, dass eine Sicherung erstellt wurde. Beachten Sie unten im Screenshot die Option „Restore Now“ (Jetzt wiederherstellen).

     ![Bild63][image63]

7.  Klicken Sie auf „Restore Now“ (Jetzt wiederherstellen), und sehen Sie sich die Optionen auf dem Blatt auf der rechten Seite an. Sie können eine geeignete Sicherung auswählen und bei Bedarf leicht einen vorherigen Zustand wiederherstellen. Das Azure-Portal hat uns dabei unterstützt, für die App problemlos eine einfache Strategie für die Notfallwiederherstellung zu aktivieren.

     ![Bild64][image64]

8.  Wechseln Sie zurück zum Blatt „Einstellungen“ auf der linken Seite, und wählen Sie unter „Funktionen“ die Option „Authentifizierung/Autorisierung“.

     ![Bild65][image65]

9.  Wählen Sie im Blatt auf der rechten Seite die Option „App Service-Authentifizierung“. Beachten Sie die vielen verschiedenen Optionen, die Sie mit gängigen Anbietern konfigurieren können.

     ![Bild66][image66]

10. Wählen Sie den gewünschten Anbieter, und sehen Sie sich die Bereichsoptionen an. Sie können eine App-ID und einen geheimen App-Schlüssel angeben und die Facebook-Authentifizierung für die App schnell aktivieren. Der Azure-Portal ermöglicht die Authentifizierung als fertige Lösung für Apps.

     ![Bild67][image67]

11. Wechseln Sie zurück zum Blatt „Einstellungen“, und wählen Sie in der Kategorie „Ressourcenverwaltung“ die Option „Benutzer“.

     ![Bild68][image68]

12. Sehen Sie sich im Blatt auf der rechten Seite die verschiedenen Optionen zum Hinzufügen von Rollen und Benutzern an. Mit dem Azure-Portal können Sie die rollenbasierte Zugriffssteuerung für die Anwendung leicht steuern.

     ![Bild69][image69]


## Zusammenfassung

In diesem Tutorial wurde ein Teil der Leistungsstärke der Azure-Plattform veranschaulicht, indem für eine Web-App schnell Continuous Deployment aktiviert wurde, verschiedene Entwicklungs- und Testaktivitäten durchgeführt wurden, eine Live-App überwacht und die Problembehandlung dafür durchgeführt wurde und zuletzt die Verwaltung von wichtigen Strategien beschrieben wurde, z.B. Notfallwiederherstellung, Identität und rollenbasierte Zugriffssteuerung. Die Azure-Plattform ermöglicht eine integrierte Oberfläche für diese DevOps-Workflows, und Sie können effizient arbeiten, da Sie den jeweiligen Kontext nicht verlassen müssen.


## Nächste Schritte 

* Azure Resource Manager ist für die Aktivierung von DevOps auf der Azure-Plattform wichtig. Weitere Informationen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

* Weitere Informationen zur Azure App Service-Bereitstellung finden Sie unter [Bereitstellen der App in Azure App Service](../app-service-web/web-sites-deploy.md).


[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png

<!---HONumber=AcomDC_0615_2016--->