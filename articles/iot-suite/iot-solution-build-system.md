<properties 
	pageTitle="MyDriving-Beispiel für Azure IoT – Buildvorgang | Microsoft Azure" 
	description="Erfahren Sie, wie Sie eine App erstellen, mit der umfassend demonstriert wird, wie Sie ein IoT-System mit Microsoft Azure zusammenstellen, einschließlich Stream Analytics, Machine Learning und Event Hubs." 
	services="" 
    documentationCenter=""
    suite="iot-suite"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="iot-suite" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/25/2016" 
	ms.author="awills"/>


# Erstellen und Bereitstellen der MyDriving-Lösung für Ihre Umgebung

MyDriving ist eine IoT-Lösung (Internet of Things), mit der Daten aus Ihrem Auto erfasst, per Machine Learning verarbeitet und auf Ihrem Smartphone bereitgestellt werden. Das Back-End besteht aus einer Vielzahl von Diensten, die von Microsoft Azure bereitgestellt werden, und bei den Clients kann es sich um Android-, iOS- oder Windows 10-Smartphones handeln.

Wir haben die MyDriving-Lösung als Starthilfe für die Erstellung Ihres eigenen IoT-Systems erstellt. Sie können Azure Resource Manager-Skripts aus dem Repository abrufen, um die Back-End-Architektur in Ihrem eigenen Azure-Konto bereitzustellen. Ab diesem Punkt können Sie die verschiedenen Dienste neu konfigurieren, die Abfragen ändern, um sie an Ihre eigenen Daten anzupassen, usw. Diese Skripts und den Code für die mobile App, das Azure App Service-API-Projekt und mehr finden Sie unter <https://github.com/Azure-Samples/MyDriving>.

Falls Sie die App noch nicht ausprobiert haben, können Sie sich den [Leitfaden zu den ersten Schritten](iot-solution-get-started.md) ansehen.

## Was müssen Sie erstellen?

Der [MyDriving-Referenzleitfaden](http://aka.ms/mydrivingdocs) enthält ausführliche Informationen zur Architektur. Zusammenfassung: Wir haben mehrere Komponenten eingerichtet, die Sie beim Einrichten eines ähnlichen Projekts auch erstellen würden:

* **Client-App**: Sie wird auf Android-, iOS- und Windows 10-Smartphones ausgeführt. Wir verwenden die Xamarin-Plattform, um einen Großteil des Codes freizugeben, der bei GitHub unter `src/MobileApp` gespeichert ist. Mit der App werden eigentlich zwei separate Funktionen erfüllt:
 * **Weiterleiten von Telemetriedaten** vom OBD-Gerät und von dessen eigenem Standortdienst an das Cloud-Back-End des Systems.
 * **Benutzeroberfläche**, über die Benutzer Abfragen für die aufgezeichneten Fahrten durchführen können.
* **Clouddienst**, mit dem die Daten der Fahrten in Echtzeit erfasst und verarbeitet werden. Die eigentliche Arbeit zum Erstellen dieses Diensts besteht darin, verschiedene Azure-Dienste auszuwählen, zu parametrisieren und zu verknüpfen. Für einige Teile sind Skripts zum Filtern und Verarbeiten der eingehenden Daten erforderlich. Wir verwenden eine **Azure Resource Management**-Vorlage (ARM), um alle Teile zu konfigurieren.
* **Mobile Services-App**: Dies ist der Webdienst hinter der Benutzeroberfläche, die Teil der Geräte-App ist. Die Hauptaufgabe ist das Abfragen der Datenbank mit den gespeicherten verarbeiteten Daten. Der Code hierfür befindet sich bei GitHub unter `src/MobileAppService`.
* **Visual Studio mit Xamarin** ist unsere Entwicklungsumgebung. Xamarin – sowohl als Komponente von Visual Studio als auch als eigenständige IDE verfügbar – wird verwendet, um den plattformübergreifenden Gerätecode zu erstellen. Zum Erstellen des iOS-Codes muss eine Instanz von Xamarin auf einem OSX-Computer ausgeführt werden. Falls erforderlich, ist aber auch die Ausführung als Agent möglich, der über Visual Studio verwaltet wird.
* **Komponententests** der Geräte-Apps werden in der Xamarin Test Cloud ausgeführt.
* **GitHub** ist das Repository, in dem wir den gesamten Code, Skripts und Vorlagen speichern.
* **Visual Studio Team Services** ist ein Clouddienst, der zum Verwalten der kontinuierlichen Erstellung und des Testens des Webdiensts und der Geräte-Apps verwendet wird.
* **HockeyApp** wird genutzt, um Versionen des Gerätecodes zu verteilen. Zusätzlich zur Verwaltung der Verteilung werden Daten für Absturz- und Nutzungsberichte sowie Benutzerfeedback gesammelt.
* Mit **Application Insights** wird der mobile Webdienst überwacht.


Wir sehen uns nun an, wie diese Komponenten eingerichtet werden. Beachten Sie, dass viele Schritte optional sind.

## Anmelden für Konten

-   [**Visual Studio Dev Essentials**](https://www.visualstudio.com/products/visual-studio-dev-essentials-vs.aspx). Mit diesem kostenlosen Programm ist der einfache Zugriff auf viele Entwicklertools und Dienste möglich, z.B. Visual Studio, Visual Studio Team Services und Azure, und Sie erhalten für Azure zwölf Monate lang eine Gutschrift von 25 USD pro Monat. Außerdem sind darin Abonnements für Pluralsight-Training und Xamarin University enthalten. Sie können sich auch separat für Free-Tarife von [Azure](https://azure.com) und [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx) registrieren, aber dies ist dann nicht mit dem Erhalt einer Azure-Gutschrift verbunden.

-   [**HockeyApp**](https://rink.hockeyapp.net/) (optional) zum Verwalten der Testverteilung von mobilen Apps und zum Erfassen von Telemetriedaten.

-   [**Xamarin**](https://xamarin.com/) (erforderlich) zum Erstellen der mobilen App und Ausführen von Debugvorgängen und Tests für die [Xamarin Test Cloud](https://xamarin.com/test-cloud).

-   [**GitHub**](https://github.com/Azure-Samples/MyDriving/) (optional) zum Erstellen kostenloser öffentlicher Repositorys für Ihren eigenen Code (private Repositorys sind kostenpflichtig). Sie können auch den Basic-Plan in Visual Studio Team Services für private Repositorys verwenden.

-   [**Power BI**](https://powerbi.microsoft.com/) (optional) zum Erstellen umfassender Visualisierungen von Daten für das gesamte System.

> [AZURE.NOTE] Sie benötigen kein GitHub-Konto, um auf den Code von MyDriving unter [https://github.com/Azure-Samples/MyDriving](https://github.com/Azure-Samples/MyDriving) zuzugreifen.

## Installieren von Entwicklungstools

Das folgende Setup gilt für die Entwicklung der vollständigen Lösung: plattformübergreifende iOS-, Android- und Windows Phone 10-App mit einem Azure-Back-End.

Alternativ dazu können Sie Xamarin Studio unter Mac oder Windows zum Entwickeln der mobilen Apps verwenden, wenn Sie nicht am Azure-Back-End arbeiten.

[Hier finden Sie eine längere Beschreibung dieses Setups.](https://msdn.microsoft.com/library/mt613162.aspx)

### Windows-Entwicklungscomputer

Das wichtigste Tool unter Windows ist Visual Studio. Es wird genutzt, um mit der MyDriving-App für Android und Windows, dem App Service-API-Projekt und Microserviceerweiterungen zu arbeiten.

Xamarin, Git, Emulatoren und andere nützliche Komponenten sind in Visual Studio integriert.

Installation:

-   [**Visual Studio 2015 mit Xamarin**](https://www.visualstudio.com/products/visual-studio-community-vs) (beliebige Edition – Community Edition ist kostenlos)

-   [**SQLite for Universal Windows Platform**](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936) (SQLite für universelle Windows-Plattform). Wird benötigt, um den Windows Phone 10-Code zu erstellen.

-   [Azure SDK für VS2015](https://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409): Dies ist das SDK zum Ausführen von Apps in Azure, und Sie erhalten Befehlszeilentools für die Verwaltung in Azure.

-   [Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric). Wird zum Erstellen der [Microservice](../service-fabric/service-fabric-get-started.md)erweiterung benötigt.

Visual Studio-Erweiterungen: Überprüfen Sie, ob unter „Extras“ Android, iOS, Xamarin usw. angezeigt werden. Falls nicht, öffnen Sie **Systemsteuerung** > **Programme und Funktionen** > **Microsoft** **Visual Studio 2015** > **Ändern**. Wählen Sie unter **Plattformübergreifende Entwicklung** die Option **C#/.NET (Xamarin)**. Überprüfen Sie an dieser Stelle auch gleich, ob **GitHub** installiert ist.

### Macintosh-Entwicklungscomputer

Ein Mac-Computer (Yosemite oder höher) ist erforderlich, wenn Sie für iOS entwickeln möchten. Wir verwenden zwar Visual Studio mit Xamarin unter Windows, um den gesamten Code zu entwickeln und zu verwalten, aber für Xamarin wird ein auf einem Mac installierter Agent genutzt, um den iOS-Code zu erstellen und zu signieren.

![](./media/iot-solution-build-system/image1.png)

(Als Alternative können Sie Xamarin Studio auch direkt auf dem Mac verwenden, um plattformübergreifende Apps zu entwickeln.)

Sie benötigen den Mac-Computer nicht, wenn Sie iOS nicht als Zielplattform einbeziehen möchten.

Installation:

-   [**Xamarin Studio** für iOS](https://developer.xamarin.com/guides/ios/getting_started/installation/mac/). Sie können Visual Studio und Xamarin auch auf einem Mac-Computer einrichten, auf dem ein virtueller Windows-Computer ausgeführt wird. Informationen hierzu finden Sie unter [Setup, Installation und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) auf der MSDN-Website.

-   (Optional) [Azure-Entwicklungstools](https://azure.microsoft.com/downloads/).

Aktivieren Sie die Remoteanmeldung auf dem Macintosh. Öffnen Sie „Systemeinstellungen“ und „Freigabe“, und aktivieren Sie die Option „Remoteanmeldung“.

Wenn Sie ein iOS-Projekt in Visual Studio unter Windows öffnen, werden Sie vom Xamarin-Plug-In zum Eingeben der ID des Macintosh aufgefordert.

## Abrufen des GitHub-Repositorys

Rufen Sie eine lokale Kopie von <https://github.com/Azure-Samples/MyDriving> ab, indem Sie bei GitHub, in Visual Studio oder in einem anderen Git-Client die Schaltfläche **Download ZIP** verwenden

Entpacken Sie die Datei in einen Ordner mit einem kurzen Pfadnamen, z.B. „C:\\code“.

Wenn Sie auf dem Laufenden bleiben oder einen Beitrag zu unserem Code leisten möchten, können Sie das Repository wie folgt klonen:

> git clone <https://github.com/Azure-Samples/MyDriving.git>

## Bing Maps-Entwicklercode

Registrieren Sie sich unter [Getting a Bing Maps Key](https://msdn.microsoft.com/library/ff428642.aspx) (Beschaffen eines Bing Maps-Schlüssels) für einen Bing Maps-API-Schlüssel.

Sie müssen diese Angabe in Zeile 22 in `src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs` ersetzen.



## Erstellen der Demo-App

Öffnen Sie in Visual Studio die folgenden Lösungen:

-   src\\MobileApps\\MyDriving.sln
-   src\\MobileAppService\\MyDrivingService.sln
-   src\\Extensions\\ServiceFabric\\VINLookUpApplication\\VINLookUpApplication.sln

Sie erhalten einige Aufforderungen zu folgenden Punkten:

-   Vertrauen einiger potenziell nicht vertrauenswürdiger Projekte. Geben Sie an, dass Sie sie öffnen möchten, um den Vorgang fortzusetzen.

-   Festlegen des Entwicklermodus, wenn Sie an einem neu eingerichteten Windows 10-Computer arbeiten.

-   Angeben Ihrer Xamarin-Anmeldeinformationen.

-   Verbinden mit dem Xamarin-Macintosh-Computer. Wenn Sie keinen Mac verwenden, können Sie in Visual Studio mit der rechten Maustaste auf das iOS-Projekt klicken und die Option **Projekt entfernen** wählen.

Erstellen Sie die Projektmappe neu.

### Haben Sie Probleme bei der Erstellung?

Hier sind einige Eigenheiten aufgeführt, die ggf. auftreten können:

-   Das VINLookupApplication-Projekt kann nicht geladen werden: Stellen Sie sicher, dass Sie das [Azure SDK für VS2015](https://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) installiert haben.

-   Das Service Fabric-Projekt kann nicht erstellt werden: Erstellen Sie zuerst die Schnittstellenprojekte, und stellen Sie sicher, dass Sie das Service Fabric SDK installiert haben.

-   Android-App wird nicht erstellt:

    -   Öffnen Sie „Extras“, „Android“, „Android SDK Manager“, und vergewissern Sie sich, dass die Android 6 (API 23)/SDK-Plattform installiert ist.

    -   Löschen Sie dieses Verzeichnis, und führen Sie dann eine Neuerstellung durch:<br/> `%LocalAppData%\Xamarin\zips`

## Einführung in den Code

Folgendes ist in der Lösung enthalten:

-   Azure-Erweiterungen

    -   Service Fabric

-   HDInsight – Skripts für die Verarbeitung von Fahrtdaten in Azure

-   Mobile Apps – Geräte-Apps

-   MobileAppsService/MyDrivingService – Web-Back-End

-   Power BI – Power BI-Berichtsdefinition

-   Skripts

    -   ARM – Vorlagen zum Erstellen der Azure-Ressourcen

    -   PowerShell – Skripts zum Ausführen der ARM-Vorlagen

    -   SQL-Datenbank – Debuggen von Datenbanken

-   SQLDatabase

    -   CreateTables – Schemadefinitionen

-   StreamingAnalytics – Abfragen, mit denen der eingehende Datenstrom transformiert wird

## Ausführen der Apps im Entwicklungsmodus

### Back-End

Legen Sie MyDrivingService als Startprojekt fest, und drücken Sie F5, um den Back-End-Webdienst auszuführen. Eine Browseransicht der API-Liste wird geöffnet.

### Mobile Clients

Die [mobilen Apps werden in Xamarin entwickelt](https://developer.xamarin.com/guides/cross-platform/deployment,_testing,_and_metrics/debugging_with_xamarin/).

-   [Debuggen von Android in Xamarin](http://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debugging_with_xamarin_android/)

### iOS

-   [Debuggen in iOS](http://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/debugging_in_xamarin_ios/)

### Windows Phone

-   [Xamarin + Windows Phone](https://developer.xamarin.com/guides/cross-platform/windows/phone/)


## HockeyApp

Mit HockeyApp wird die Verteilung Ihrer Android-, iOS- oder Windows-Geräte-App an Testbenutzer verwaltet, und Benutzer werden über neue Versionen informiert. Außerdem werden nützliche Absturzberichte, Benutzerfeedback mit Screenshots und Nutzungsmetriken erfasst.

[Starten Sie, indem Sie Ihre Build-App hochladen](http://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app). Melden Sie sich dann mit Ihrem Entwicklungscomputer bei HockeyApp (<https://rink.hockeyapp.net>) an. Klicken Sie auf dem Entwickler-Dashboard auf **Neue App**, und ziehen Sie die erstellten Dateien in das Fenster. (Später können Sie hierfür Ihren Builddienst automatisieren.)

Sie befinden sich nun im App-Dashboard.

![](./media/iot-solution-build-system/image2.png)

Wiederholen Sie den Prozess für jede Plattform, auf der Ihre App ausgeführt wird.

- Verwenden Sie die [App-ID](http://support.hockeyapp.net/kb/app-management-2/how-to-find-the-app-id) aus dem Dashboard, um Absturzdaten und Feedback aus Ihrer App zu senden. Aktualisieren Sie die IDs in MyDriving in der Datei „src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs“. 
-   [**Laden Sie Testbenutzer ein**](http://support.hockeyapp.net/kb/app-management-2/how-to-invite-beta-testers). Sie erhalten eine URL zum Rekrutieren von Testbenutzern. Diese können sich für Ihr Team registrieren, die App herunterladen und Feedback an Sie senden.

-   Falls Sie die Verwendung einer offeneren Betaversion vorziehen, können Sie die Verteilung auf „Öffentlich“ festlegen: Klicken Sie auf **App verwalten, Verteilung, Download = Öffentlich**. Jetzt können alle Benutzer Ihre App herunterladen und Feedback senden, und sie erhalten eine Benachrichtigung, wenn Sie eine neue Version bereitstellen. Unter Umständen erhalten Sie von diesen Benutzern auch Absturzberichte.
![](./media/iot-solution-build-system/image3.png)

-   [**Verknüpfen von Absturzberichten mit Visual Studio Team Services**](http://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs): Klicken Sie auf **App verwalten, Visual Studio Team Services**. Für HockeyApp können automatisch Arbeitsaufgaben in Team Services erstellt werden, wenn Absturzberichte vorhanden sind oder wenn Feedback eintrifft.

> Weitere Informationen finden Sie unter <https://hockeyapp.net>.

## Xamarin Test Cloud

Mit der [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud/introduction-to-test-cloud/) werden UI-Tests auf echten Geräten in der Cloud automatisiert. Mit dem NUnit-Framework schreiben Sie Tests, bei denen Ihre App über die Benutzeroberfläche ausgeführt wird.

Zum Verwenden von Xamarin binden Sie in Ihre App das [Xamarin.UITests](https://developer.xamarin.com/guides/testcloud/uitest/intro-to-uitest/)-SDK ein, das als NuGet-Paket verfügbar ist. Sie finden es in der Demo-App, und es wird eingebunden, wenn Sie mit den Xamarin-Vorlagen neue Testprojekte erstellen.

![](./media/iot-solution-build-system/image4.png)

Ein Beispieltestprojekt ist in der App im Repository enthalten: Suchen Sie in [MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService) unter [src](https://github.com/Azure-Samples/MyDriving/tree/master/src)/MobileApps/[MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileApps/MyDriving)/MyDriving.UITests/ danach.

Wenn Sie einen Visual Studio Team Services-Build verwenden, ist es einfach, Komponententests für die Xamarin-UI zu schreiben und im Rahmen des Builds auszuführen.

## Bereitstellen von Azure-Diensten

Sehen Sie in der ausführlichen Anleitung in **scripts/README.md** nach, um eine automatische Bereitstellung von Azure-Diensten und Team Services-Builddiensten durchzuführen.

### Wozu dient das Bereitstellungsskript?

Microsoft Azure verfügt über viele verschiedene Dienste, die Sie zum Erstellen von Cloudanwendungen verwenden können. Ein Großteil davon kann auch einzeln genutzt werden (z.B. App Service/Web-Apps), aber die beste Leistung wird erzielt, wenn sie zusammen ein integriertes System bilden, z.B. für MyDriving.

Es ist möglich, Azure-Dienste manuell zu erstellen und zu verknüpfen, aber viel schneller und zuverlässiger ist die Verwendung von Azure Resource Manager-Vorlagen (ARM). Per [ARM](../resource-group-overview.md) wird die Bereitstellung der Ressourcen einer Lösung und die entsprechende Verknüpfung automatisiert.

Sie finden die Vorlage für das MyDriving-System im GitHub-Repository unter [Scripts/ARM](https://github.com/Azure-Samples/MyDriving/tree/master/scripts/ARM). Hier wird umfassend und gleichzeitig in möglichst kurzer Form beschrieben, wie die unterschiedlichen Dienste in unserer Architektur miteinander verbunden sind. Noch ausführlicher wird hierauf im [MyDriving-Referenzleitfaden](http://aka.ms/mydrivingdocs) eingegangen, aber Sie können bereits viel erfahren, indem Sie sich die Vorlage durchlesen.

> [AZURE.NOTE] Für die meisten Azure-Dienste fallen je nach Tarif Kosten an. Falls Sie mit Azure noch nicht vertraut sind, können Sie die [Anwendung kostenlos ausprobieren](https://azure.microsoft.com/free/). Falls Sie aber planen, bestimmte Komponenten im MyDriving-System nicht zu verwenden, sollten Sie diese entfernen, um dafür anfallende Kosten zu verhindern. Eine Zusammenfassung der typischen Kosten für Dienste finden Sie unten unter *Geschätzte Betriebskosten*.

#### Bearbeiten der Vorlage

Beim Anpassen der Bereitstellung, z.B. zum Entfernen nicht benötigter Komponenten oder zum Hinzufügen von Komponenten, sollten Sie zuerst Kopien der Dateien „scenario\_complete.params.json“ und „scenario\_complete.json“ erstellen, in denen die Änderungen vorgenommen werden sollen.

Mit der Datei „scenario\_complete.params.json“ können Sie verschiedene Standardwerte außer Kraft setzen, z.B. die Dienst-SKU oder den Speicherreplikationstyp. Dies ist in der folgenden Tabelle beschrieben. Bei den Standardwerten werden die Optionen mit den niedrigsten Kosten gewählt.

| **Parameter** | **Beschreibung** | **Standardwert** |
|--------|---------|-------|
| IoT Hub-SKU | Tarif für Azure IoT Hub-Dienst | F1 |
| Speicherkontotyp | Speicherreplikationstyp | Standardmäßiger LRS |
| SQL-Dienstziel | Verbrauch von Parallelitätsslots | DW100 |
| Hostingplan-SKU | Dienstplan für App Service | F1 |

In „scenario\_complete.json“:

-   Suchen Sie nach „baseName“, und ändern Sie die Angabe in den von Ihnen gewünschten Text.

-   Suchen Sie nach „Create“. Mit jedem dieser Abschnitte wird eine Ressource erstellt.

-   Legen Sie „sqlServerAdminLogin“ und „sqlServerAdminPassword“ auf geeignete Werte fest.

-   Überprüfen Sie vor dem Löschen eines Abschnitts, mit dem eine Ressource erstellt wird, ob dafür abhängige Elemente vorhanden sind. Suchen Sie hierfür im Rest der Datei nach dem Namen. Beachten Sie, dass jeder Abschnitt zum Erstellen eines Diensts den Abschnitt *dependsOn* enthält, in dem die Abhängigkeiten aufgeführt sind.

Hier ist angegeben, was mit der Vorlage konfiguriert wird. Ausführliche Informationen finden Sie im [Referenzleitfaden](http://aka.ms/mydrivingdocs):

| **Service** | **Beschreibung und Details**  
|---|----
| Speicherkonten | Mit der Vorlage werden drei Konten erstellt:                                                                                                                                                                       
||- Eine SQL-Datenbank, über die aggregierte Telemetriedaten aus Stream Analytics empfangen werden und die als Sicherungsspeicher für Azure App Service-Tabellen dient, in denen diese Daten über API-Endpunkte verfügbar gemacht werden.                      
| | - Blobspeicher, mit dem Verlaufsdaten eines anderen Stream Analytics-Auftrags für die Verarbeitung mit HDInsight erfasst werden.                                                                                         
| | - Eine SQL-Datenbank, die Ergebnisse für die von HDInsight verarbeiteten Daten erhält, damit diese für Power BI verwendet werden können.                                                                                                                 
| IoT Hub | Stellt eine bidirektionale Verbindung mit jedem verbundenen Gerät her. Bei der MyDriving-Lösung fungiert die mobile App als Bereichsgateway zum Senden von Daten an den IoT Hub. Der IoT Hub dient dann als Eingabe für Stream Analytics. |
| Event Hub | Eine Ausgabe für einen Stream Analytics-Auftrag, bei dem die Ausgabe für Erweiterungen, die mit Azure Service Fabric erstellt wurden, in die Warteschlange eingereiht wird.                                                                                               
| SQL Data Warehouse |                                                                                                                                                                                                            
| Stream Analytics-Aufträge (ASA) | Verbinden Sie die Eingaben und Ausgaben mit einer Abfrage, die zum Aggregieren von Echtzeit- und Verlaufsdaten für die App Service-APIs, Machine Learning, Erweiterungen und Power BI verwendet wird.                               
| Machine Learning-Arbeitsbereich | Umfasst Experimente, R-Code und einen API-Dienst.                                                                                                                                                              
| Data Factory | Geplantes erneutes Trainieren von Machine Learning                                                                                                                                                                     
| Service Fabric-Hostingplan | Für Erweiterungen bestimmt.                                                                                                                                                                                            
| App Service („Mobile App“) | Hostet das Mobile Apps-API-Projekt, mit dem Endpunkte für die mobile App bereitgestellt werden. Der API-Code muss für den App Service aus Visual Studio bereitgestellt werden.                                                         
| Warnungsregeln | Sie erhalten E-Mails, wenn bei den App-Antworten auf Fehler hingewiesen wird.                                                                                                                                            
| Application Insights | Dient zum Überwachen der Leistung von APIs in App Service. Sie müssen die Verbindung in Visual Studio konfigurieren.                                                                                          
| Schlüsseltresor | Dient zum Speichern des Webdienst-Clusterzertifikats.                                                                                                                                                                

#### Ausführen der Vorlage

**Eine ausführliche Anleitung finden Sie unter „scripts/README.md“**.

Wählen Sie eine der folgenden Vorgehensweisen, um alle Dienste per Skript unter Ihrem eigenen Azure-Konto bereitzustellen:

-   Verwenden von PowerShell:

    ```

    cd scripts/PowerShell;
    deploy.ps1 *location* *resourceGroupName*
    ```

 -   *location* ist der [Azure-Standort](https://azure.microsoft.com/regions/), z.B. „Europa, Norden“ oder „USA, Westen“. Verwenden Sie `Get-AzureLocation`, um eine Liste mit verfügbaren Standorten zu erhalten.

 -   *resourceGroupName* ist der Name, mit dem Sie die Gruppe benennen, der alle Ressourcen angehören. Wenn Sie die Arbeit an den Ressourcen abgeschlossen haben, können Sie sie alle auf einmal löschen, indem Sie diese Gruppe löschen.

-   Führen Sie „DeploymentScripts/Bash/deploy.sh“ mit Bash aus.

-   Öffnen Sie die Visual Studio-Projektmappe „DeploymentScripts/VS/DeployARM.sln“, und führen Sie den Buildvorgang dafür durch.

Beachten Sie, dass bei jeder Ausführung der Vorlage ein neuer Ressourcensatz mit neuen Namen erstellt wird. Wechseln Sie zum Löschen der Ressourcen zum Portal, und löschen Sie die Ressourcengruppe.

Falls bei der Ausführung des Skripts ein Fehler auftritt, können Sie es erneut ausführen.

Mit dem Skript haben Sie die Möglichkeit, die fortlaufende Integration in Visual Studio Team Services zu konfigurieren. Wenn Sie ein Team Services-Projekt eingerichtet haben, verfügen Sie über die URL https://yourAccountName.visualstudio.com. Geben Sie bei Aufforderung die vollständige URL ein. Sie können einen neuen oder einen vorhandenen Namen für ein Team Services-Projekt vergeben.



## Visual Studio Team Services

Wir verwenden Team Services bei diesem Projekt zumeist für die Build- und Testfeatures. Die Anwendung verfügt aber auch über eine hervorragende Unterstützung der Zusammenarbeit, z.B. Aufgabenverwaltung mit Kanban-Boards, in Aufgaben und Quellcodeverwaltung integrierte Codeüberprüfung, abgegrenzte Builds und vieles mehr. Außerdem ist die Integration in andere Tools problemlos möglich, z.B. GitHub, Xamarin, HockeyApp und natürlich Visual Studio. Sie können darauf über die Weboberfläche oder über Visual Studio zugreifen. Dies richtet sich danach, was für Sie jeweils am einfachsten ist.

Für die Schritte in den Build- und Versionsdefinitionen werden verschiedene Plug-In-Dienste verwendet, die über den [Marketplace](https://marketplace.visualstudio.com/VSTS) von Team Services verfügbar sind. Zusätzlich zu den grundlegenden Hilfsprogrammen zum Ausführen von Befehlszeilen oder Kopieren von Dateien gibt es Dienste, mit denen Builds von Xamarin, Android und anderen Anbietern aufgerufen werden und die als Schnittstelle für HockeyApp dienen.

![](./media/iot-solution-build-system/image5.png)

### Builddefinitionen

Es sind Builddefinitionen für alle Hauptziele und Varianten für Funktions- und Regressionstests vorhanden. Damit erhalten wir Folgendes:

-   MyDriving.Services – Back-End-Web-App für die mobile App

-   MyDriving.Xamarin.Android

    -   MyDriving.Xamarin.Android-Feature

    -   MyDriving.Xamarin.Android-Regression

-   MyDriving.Xamarin.iOS

    -   MyDriving.Xamarin.iOS-Feature

    -   MyDriving.Xamarin.iOS-Regression

-   MyDriving.Xamarin.UWP

    -   MyDriving.Xamarin.UWP-Feature

    -   MyDriving.Xamarin.UWP-Regression

Sie können alle Details zu unserer Konfiguration anzeigen, indem Sie Abschnitt 4.7 *Build and Release Configuration* (Build- und Versionskonfiguration) des [MyDriving-Referenzleitfadens](http://aka.ms/mydrivingdocs) aufrufen. Es wird jeweils das gleiche allgemeine Muster verwendet:

1.  NuGet-Paketwiederherstellung: Im Repository halten wir keinen kompilierten Code vor. Der erste Schritt jedes Buildvorgangs umfasst also die Wiederherstellung der erforderlichen NuGet-Pakete.

2.  Lizenzaktivierung: Der Buildvorgang wird in der Cloud durchgeführt. Wenn wir also eine Lizenz benötigen – vor allem für den Xamarin-Builddienst –, müssen wir unsere Lizenz auf dem aktuellen Buildcomputer aktivieren. Anschließend deaktivieren wir sie sofort wieder, damit sie auf einem anderen Computer verwendet werden kann.

3.  Buildvorgang mit dem richtigen Dienst: Wir verwenden Xamarin-Builds für die mobilen Apps und den Visual Studio-Build für den Back-End-Webdienst.

4.  Buildtests

5.  Ausführung von Tests: Wir führen die Tests der mobilen Apps in der Xamarin Test Cloud durch.

6.  Veröffentlichung des Buildergebnisses am Ablagespeicherort

Der Trigger für die Hauptbuilds ist auf „Continuous Integration“ festgelegt. Der Buildvorgang wird also jedes Mal ausgeführt, wenn Code in die Masterverzweigung eingecheckt wird.

![](./media/iot-solution-build-system/image6.png)

### Versionsdefinitionen

Versionsdefinitionen werden auf ähnliche Weise eingerichtet.

Für den Webdienst richten wir die Bereitstellung als Azure-Web-App ein:

![](./media/iot-solution-build-system/image7.png)

Legen Sie außerdem den Releasetrigger auf „Continuous Deployment“ fest. Jeder Eincheckvorgang, auf den ein erfolgreicher Buildvorgang folgt, führt dann zu einer Aktualisierung der Web-App:

![](./media/iot-solution-build-system/image8.png)

Für mobile Apps führen wir die Bereitstellung für HockeyApp durch:
![](./media/iot-solution-build-system/image9.png)

## Application Insights

Mit [Application Insights](../application-insights/app-insights-overview.md) werden Telemetriedaten zur Leistung und Nutzung von Webdiensten erfasst. Das Application Insights SDK sendet Telemetriedaten aus dem Dienst an die Application Insights-Ressource in Azure.

Navigieren Sie zur Application Insights-Ressource, die von der Vorlage eingerichtet wurde. Darin können Sie sich Diagramme zur Leistung des [Mobile App Service-Projekts](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService) ansehen. Es werden Serveranforderungen und Reaktionszeiten, Fehler und die Anzahl von Ausnahmen angezeigt. Es sind auch Diagramme mit Reaktionszeiten von Abhängigkeiten vorhanden, also Aufrufe der Datenbank und von REST-APIs, z.B. Machine Learning. Falls Leistungsprobleme auftreten, können Sie sehen, welche Komponente des Systems die Ursache dafür ist.

![Beispiel-Leistungsdiagramm](./media/iot-solution-build-system/image11.png)

Wenn Sie einen Webdienst manuell eingerichtet haben, ist es einfach, die gleichen Diagramme zu erhalten:

![Wählen Sie auf dem Webdienstblatt die Optionen „Extras“ > „Erweiterungen“ > „Hinzufügen“. Wählen Sie „Application Insights“.](./media/iot-solution-build-system/image12.png)

Bei diesem Feature wird Ihre Anwendung mit dem Application Insights SDK instrumentiert.

Sie können benutzerdefinierte Telemetrie hinzufügen (oder eine Anwendung instrumentieren, die außerhalb von Azure ausgeführt wird), indem Sie das [Application Insights SDK zur Entwicklungszeit hinzufügen](../application-insights/app-insights-asp-net.md). Dies ist nützlich, um Metriken zu protokollieren, die von der Anwendung abhängig sind, z.B. die durchschnittliche Fahrtstrecke oder Gesamtkilometer des Benutzers.

![Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie die Option „Application Insights hinzufügen“.](./media/iot-solution-build-system/image10.png)

Application Insights sendet Warnungs-E-Mails, wenn eine ungewöhnliche Anzahl von Fehlerantworten erkannt wird, und Sie können auch eigene Warnungen für verschiedene Metriken einrichten, z.B. Reaktionszeiten.

Um sicherzugehen, dass Ihr Webdienst immer betriebsbereit ist, können Sie [Verfügbarkeitstests](../application-insights/app-insights-monitor-web-app-availability.md) einrichten, bei denen Ihr Standort alle 15 Minuten von unterschiedlichen Orten der Welt per Ping kontaktiert wird. Auch hier erhalten Sie wieder eine E-Mail, wenn ein Problem vorzuliegen scheint.



## Geschätzte Betriebskosten

Es ist erstaunlich kostengünstig, eine App wie diese in kleinerem Umfang auszuführen. Viele Dienste verfügen als Einstiegshilfe über Free-Tarife, sodass nur geringe Kosten für die Entwicklung und den anfänglichen Betrieb anfallen. Und für Ihre eigenen Apps müssen Sie natürlich auch nicht alle Features nutzen, die im Rahmen von MyDriving demonstriert werden.

Unten ist eine grobe Schätzung der Kosten für die Einrichtung der Entwicklungskonfiguration für MyDriving angegeben.

Es sind auch einige Alternativen aufgeführt, die wir *nicht* genutzt haben. #

Wir treffen folgende Annahmen:

-   Team mit maximal fünf Mitgliedern (zzgl. relevante Stakeholder)

-   Laufzeit von einem Monat

-   100 Benutzer mit vier Fahrten pro Tag

>[AZURE.NOTE] Falls Azure neu für Sie ist, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/free/) nutzen.

| **Dienst/Komponente** | **Hinweise** | **Kosten pro Monat** |
|--------|--------|----------------|
| [Visual Studio 2015 Community](https://www.visualstudio.com/products/visual-studio-community-vs) mit [Xamarin](https://visualstudiogallery.msdn.microsoft.com/dcd5b7bd-48f0-4245-80b6-002d22ea6eee) <br/>Plattformübergreifende Entwicklungsumgebung| Visual Studio Community <br/>(# [VS Pro](https://www.visualstudio.com/vs-2015-product-editions) ist erforderlich für [Xamarin.Forms](https://xamarin.com/forms), um plattformübergreifend über eine zentrale Codebasis zu entwerfen.) | 0 USD |
| [Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) <br/>Bidirektionale Datenverbindung mit Geräten | 8\.000 Nachrichten + 0,5 KB/Nachricht kostenlos | 0 USD |
| [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics/) <br/> Verarbeitung eines hohen Datenstromvolumens | 0,031 USD pro Streamingeinheit pro Stunde bei Aktivierung. Sie wählen die gewünschte Anzahl von Streamingeinheiten aus, zentrales Hochskalieren möglich. | 23 USD |
| [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)<br/> Adaptive Antworten | 10 USD/Arbeitsplatz/Monat <br/> + 3 h Versuch * 1 USD/Versuchsstunde <br/> + 3,5 h API-CPU * 2 USD/CPU-Produktionsstunde <br/> Bei der API-CPU-Zeit werden 5 Min. erneutes Trainieren pro Tag angenommen, Wert steigt bei mehr Eingabedaten an <br/> + 2 Min./Tag Bewertung zur Verarbeitung von 400 Fahrten/Tag | 20 USD |
| [Azure App Service](https://azure.microsoft.com/pricing/details/app-service/) <br/> Hosten des mobilen Back-Ends | Tarif B1 – Web-Apps für Produktion | 56 USD |
| [Visual Studio Team Services ](https://azure.microsoft.com/pricing/details/visual-studio-team-services/) <br/> Buildvorgang, Komponententest und Releaseverwaltung, Aufgabenverwaltung | Private Agents, 5 Benutzer| 0 USD |
| [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) <br/>Überwachung der Leistung und Nutzung von Webdiensten und Standorten| Kostenlose Version | 0 USD |
| [HockeyApp](http://hockeyapp.net/pricing/) <br/> Verteilung von Beta-Apps, Erfassung von Feedback, Nutzungs- und Absturzdaten | Zwei kostenlose Apps für neue Benutzer.<br/> Danach 30 USD/Monat. | 0 USD |
| [Xamarin](https://store.xamarin.com/)<br/> Codeerstellung auf einer einheitlichen Plattform für mehrere Geräte | Kostenlose Testversion. <br/>Danach 25 USD/Monat.| 0 USD |
| [SQL-Instanz](https://azure.microsoft.com/pricing/details/sql-database/) für Azure App Service| Basic-Tarif, Modell mit Einzeldatenbank | 5 USD |
| [Service Fabric](../service-fabric/services/service-fabric.md) (optional) | Ausführung eines lokalen Clusters | 0 USD |
| [Power BI](https://powerbi.microsoft.com/pricing/)<br/> Vielseitige Anzeigemöglichkeiten und Untersuchung von Datenstrom- und statischen Daten| Free-Tarif: 1 GB, 10.000 Zeilen/Stunde, tägliche Aktualisierung <br/> # 10 USD/Benutzer/Monat für [höhere Limits](https://powerbi.microsoft.com/documentation/powerbi-power-bi-pro-content-what-is-it/), mehr Verbindungsoptionen, Zusammenarbeit | 0 USD |
| [Speicher](https://azure.microsoft.com/pricing/details/storage/) | L (lokal redundant) &lt; 100G 0,024 USD/GB | 3 USD |
| [Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) | 0,60 USD pro Aktivität * (8 - 5 FOC)| 2 USD |
| [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) (bedarfsgesteuerter Cluster) <br/> Für tägliches erneutes Trainieren | 3 A3-Knoten zu 0,32 USD/h für 1 Stunde täglich x 31 Tage | 30 USD |
| [Event Hub](https://azure.microsoft.com/pricing/details/event-hubs/) | Basic mit 11 USD/Monat Durchsatzeinheit + 0,028 USD Eingang | 11 USD |
| OBD-Dongle || 12 USD |
| **Gesamt**| | **157 USD** |

Weitere Informationen:

-   Zusammenfassung der [Kontingente und Limits für Azure-Dienste](../azure-subscription-service-limits/#iot-hub-limits)

-   Azure-[Preisrechner](https://azure.microsoft.com/pricing/calculator/)



## Referenzleitfaden

Wir empfehlen Ihnen den [MyDriving-Referenzleitfaden](http://aka.ms/mydrivingdocs), bei dem es sich um eine umfassende Beschreibung des Systemdesigns und der dazugehörigen Komponenten handelt.


## Feedback 

Da wir MyDriving als Starthilfe für Ihre eigenen IoT-Systeme erstellt haben, würden wir uns freuen, wenn Sie uns Feedback zu Ihren Ergebnissen senden. Teilen Sie uns mit, falls bei Ihnen Schwierigkeiten oder Probleme auftreten, falls eine Erweiterungsmöglichkeit besteht, um die Eignung für Ihr Szenario zu verbessern, falls Sie einen effizienteren Weg zur Erreichung bestimmter Ziele finden oder falls Sie andere Vorschläge zur Verbesserung von MyDriving oder dieser Dokumentation haben.

Um Feedback zu senden, können Sie ein [Problem bei GitHub] beschreiben oder unten einen Kommentar hinterlassen (de-DE-Version).

Wir freuen uns darauf, von Ihnen zu hören!

<!-----HONumber=AcomDC_0406_2016-->