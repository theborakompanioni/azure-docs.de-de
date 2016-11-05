---
title: Erstellen einer IoT-Lösung mithilfe von Stream Analytics | Microsoft Docs
description: Enthält ein Tutorial zu den ersten Schritten für die Stream Analytics-IoT-Lösung für ein Mauthäuschen-Szenario.
keywords: IoT-Lösung, Fensterfunktionen
documentationcenter: ''
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun

ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 09/26/2016
ms.author: jeffstok

---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Erstellen einer IoT-Lösung mithilfe von Stream Analytics
## <a name="introduction"></a>Einführung
In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Stream Analytics in Echtzeit Einblicke in Ihre Daten erhalten. Entwickler können Datenströme, z.B. Klickstreams, Protokolle und von Geräten generierte Ereignisse, leicht mit Verlaufsdatensätzen oder Referenzdaten kombinieren, um geschäftliche Erkenntnisse zu gewinnen. Da Azure Stream Analytics ein vollständig verwalteter Dienst für die Datenstromberechnung in Echtzeit ist, der in Microsoft Azure gehostet wird, bietet er integrierte Resilienz, Skalierbarkeit sowie geringe Latenz und ist innerhalb von Minuten einsatzbereit.

Nach Abschluss dieses Tutorials, werden Sie in der Lage sein, folgende Aufgaben auszuführen:

* Kennenlernen des Azure Stream Analytics-Portals
* Konfigurieren und Bereitstellen eines Streamingauftrags
* Formulieren von realen Problemen und Durchführen der Behebung mithilfe der Stream Analytics-Abfragesprache
* Problemloses Entwickeln von Streaminglösungen für Ihre Kunden mit Stream Analytics
* Verwenden der Überwachung und Protokollierung beim Beheben von Problemen

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Aktuelle Version von [Azure PowerShell](../powershell-install-configure.md)
* Visual Studio 2015 oder die kostenlose [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* [Ein Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
* Administratorrechte auf dem Computer
* Download von [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) aus dem Microsoft Download Center
* Optional: Quellcode für den TollApp-Ereignisgenerator in [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction:-“hello,-toll!”"></a>Einführung in das Szenario: „Hallo, Maut!“
Eine Mautstation ist eine bekannte Einrichtung. Es gibt sie auf vielen Autobahnen und an Brücken und Tunneln auf der ganzen Welt. Jede Mautstation umfasst mehrere Mauthäuschen. An Häuschen mit manueller Bezahlung halten Sie an und entrichten den Mautbetrag bei der zuständigen Person. Bei Häuschen mit automatisierter Bezahlung wird mit einem Sensor oben auf dem Häuschen eine RFID-Karte abgetastet, die an der Windschutzscheibe Ihres Fahrzeugs angebracht ist, während Sie das Mauthäuschen passieren. Es ist einfach, die Durchfahrt von Fahrzeugen durch diese Mautstellen als einen Strom von Ereignissen zu visualisieren, über den interessante Vorgänge ausgeführt werden können.

![Bild mit Fahrzeugen an Mauthäuschen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Eingehende Daten
In diesem Tutorial werden zwei Datenströme verwendet. Mit Sensoren, die am Eingang und Ausgang der Mautstationen installiert sind, wird der erste Datenstrom produziert. Der zweite Datenstrom ist ein statisches Suchdataset mit Daten zur Fahrzeugregistrierung.

### <a name="entry-data-stream"></a>Eingangsdatenstrom
Der Eingangsdatenstrom enthält Informationen zu den Fahrzeugen, die in Mautstationen einfahren.

| TollId | EntryTime | LicensePlate | Zustand | Make | Modell | VehicleType | VehicleWeight | Toll | Tag |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

Nachfolgend finden Sie eine kurze Beschreibung der Spalten:

| Column | Beschreibung |
| --- | --- |
| TollId |Mauthäuschen-ID zur eindeutigen Identifizierung des Mauthäuschens |
| EntryTime |Datum und Uhrzeit der Einfahrt des Fahrzeugs in das Mauthäuschen in UTC |
| LicensePlate |Nummernschild des Fahrzeugs |
| Zustand |Bundesstaat (USA) |
| Make |Der Fahrzeughersteller |
| Modell |Modellnummer des Fahrzeugs |
| VehicleType |1 für Privatfahrzeuge oder 2 für Nutzfahrzeuge |
| WeightType |Fahrzeuggewicht in Tonnen; „0“ für Pkws |
| Toll |Die Mautgebühr in US-Dollar |
| Tag |Nummer des elektronischen Geräts („ETag“) im Fahrzeug, das die automatische Zahlung ermöglicht. Bei nicht-automatischer Zahlung bleibt diese Spalte leer. |

### <a name="exit-data-stream"></a>Ausgangsdatenstrom
Der Ausgangsdatenstrom enthält Informationen zu den Fahrzeugen, die aus der Mautstelle ausfahren.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

Nachfolgend finden Sie eine kurze Beschreibung der Spalten:

| Column | Beschreibung |
| --- | --- |
| TollId |Mauthäuschen-ID zur eindeutigen Identifizierung des Mauthäuschens |
| ExitTime |Datum und Uhrzeit der Ausfahrt des Fahrzeugs aus einem Mauthäuschen in UTC |
| LicensePlate |Nummernschild des Fahrzeugs |

### <a name="commercial-vehicle-registration-data"></a>Registrierungsdaten von Nutzfahrzeugen
Im Tutorial wird eine statische Momentaufnahme der Registrierungsdatenbank für Nutzfahrzeuge verwendet.

| LicensePlate | RegistrationId | Abgelaufen |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| BAC 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Nachfolgend finden Sie eine kurze Beschreibung der Spalten:

| Column | Beschreibung |
| --- | --- |
| LicensePlate |Nummernschild des Fahrzeugs |
| RegistrationId |ID der Fahrzeugregistrierung |
| Abgelaufen |Der Registrierungsstatus des Fahrzeugs: „0“, wenn das Fahrzeug registriert ist, und „1“, wenn die Registrierung abgelaufen ist |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Einrichten der Umgebung für Azure Stream Analytics
Sie benötigen ein Microsoft Azure-Abonnement, um dieses Tutorial auszuführen. Microsoft hat kostenlose Testversionen von Microsoft Azure-Diensten im Angebot.

Wenn Sie nicht über ein Azure-Konto verfügen, können Sie eine [kostenlose Testversion anfordern](http://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Um sich für eine kostenlose Testversion zu registrieren, sind ein Mobilgerät, das Textnachrichten (SMS) empfangen kann, sowie eine gültige Kreditkarte erforderlich.
> 
> 

Stellen Sie sicher, dass Sie am Ende dieses Artikels die Schritte der Anleitung zum „Bereinigen Ihres Azure-Kontos“ befolgen, sodass Sie Ihre Azure-Gutschrift in Höhe von 200 US-Dollar bestmöglich nutzen können.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Bereitstellen von Azure-Ressourcen, die für das Tutorial nötig sind
Für dieses Tutorial sind zwei Event Hubs zum Empfangen von Eingabe- und Ausgabedatenströmen (*entry* und *exit*) erforderlich. Azure SQL-Datenbank gibt die Ergebnisse der Stream Analytics-Aufträge aus. In Azure Storage werden Referenzdaten zu Fahrzeugregistrierungen gespeichert.

Sie können das Skript „Setup.ps1“ im Ordner TollApp auf GitHub verwenden, um alle erforderlichen Ressourcen zu erstellen. Aus Zeitgründen empfehlen wir Ihnen, dieses Skript auszuführen. Weitere Informationen zur Konfiguration dieser Ressourcen im Azure-Portal finden Sie im Anhang „Konfigurieren von Tutorial-Ressourcen im Azure-Portal“.

Laden Sie die unterstützenden [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) -Ordner und -Dateien herunter, und speichern Sie sie.

Öffnen Sie ein **Microsoft Azure PowerShell** -Fenster *als Administrator*. Falls Sie Azure PowerShell noch nicht besitzen, können Sie die Installationsanleitung unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) befolgen.

Da Windows PS1-, DLL- und EXE-Dateien automatisch blockiert, müssen Sie die Ausführungsrichtlinie festlegen, bevor Sie das Skript ausführen. Stellen Sie sicher, dass das Azure PowerShell-Fenster _als Administrator_ausgeführt wird. Führen Sie **Set-ExecutionPolicy unrestricted**aus. Geben Sie **Y**ein, wenn Sie dazu aufgefordert werden.

![Screenshot der Ausführung von „Set-ExecutionPolicy unrestricted“ im Azure PowerShell-Fenster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Führen Sie **Get-ExecutionPolicy** aus, um sicherzustellen, dass der Befehl erfolgreich ausgeführt wurde.

![Screenshot der Ausführung von „Get-ExecutionPolicy“ im Azure PowerShell-Fenster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Wechseln Sie in das Verzeichnis mit den Skripts und der Generatoranwendung.

![Screenshot der Ausführung von „cd .\TollApp\TollApp“ im Azure PowerShell-Fenster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Geben Sie **.\\\Setup.ps1** ein, um Ihr Azure-Konto einzurichten, alle benötigten Ressourcen zu erstellen und zu konfigurieren und mit dem Generieren von Ereignissen zu beginnen. Das Skript wählt nach dem Zufallsprinzip eine Region aus, um Ihre Ressourcen zu erstellen. Um explizit eine Region anzugeben, können Sie den Parameter **-location** wie im folgenden Beispiel übergeben:

**.\\\Setup.ps1-location "Central US"**

![Screenshot der Azure-Anmeldeseite](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Das Skript öffnet die Seite **Anmelden** für Microsoft Azure. Geben Sie Ihre Anmeldeinformationen ein.

> [!NOTE]
> Wenn Ihr Konto auf mehrere Abonnements zugreifen kann, werden Sie aufgefordert, den Abonnementnamen einzugeben, den Sie für das Tutorial verwenden möchten.
> 
> 

Die Ausführung des Skripts kann mehrere Minuten dauern. Nach Beendigung des Vorgangs sollte die Ausgabe wie in der folgenden Momentaufnahme aussehen.

![Screenshot der Ausgabe des Skripts im Azure PowerShell-Fenster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Sie sehen ein anderes Fenster, das etwa wie im folgenden Screenshot aussieht. Diese Anwendung sendet Ereignisse an Azure Event Hubs, um die Ausführung des Tutorials zu ermöglichen. Sie sollten die Anwendung also vor dem Ende des Tutorials nicht anhalten und dieses Fenster nicht schließen.

![Screenshot zum „Senden von Event Hub-Daten“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Nun sollten alle erstellten Ressourcen im Azure-Portal angezeigt werden. Navigieren Sie zu <https://manage.windowsazure.com>, und melden Sie sich mit den Anmeldeinformationen für Ihr Microsoft-Konto an.

### <a name="azure-event-hubs"></a>Azure Event Hubs
Klicken Sie links im Azure-Portal auf **SERVICE BUS** , um Event Hubs anzuzeigen, die vom Skript im vorherigen Abschnitt erstellt wurden.

![SERVICE BUS](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Sie sehen alle verfügbaren Namespaces in Ihrem Abonnement. Klicken Sie auf den Eintrag, der mit *tolldata* beginnt (in unserem Beispiel „tolldata4637388511“). Klicken Sie auf die Registerkarte **EVENT HUBS** .

![Registerkarte „Event Hubs“ im Azure-Portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Sie sehen zwei Event Hubs namens *entry* und *exit*, die in diesem Namespace erstellt wurden.

![Screenshot von Event Hubs („entry“ und „exit“) im Azure-Portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure Storage-Container
1. Klicken Sie links im Azure-Portal auf **SPEICHER** , um den Azure Storage-Container anzuzeigen, der im Tutorial verwendet wird.
   
    ![Menüelement „Speicher“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)
2. Klicken Sie auf den Eintrag, der mit *tolldata* beginnt (in unserem Beispiel „tolldata4637388511“). Klicken Sie auf die Registerkarte **CONTAINER** , um den erstellten Container anzuzeigen.
   
    ![Registerkarte „Container“ im Azure-Portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)
3. Klicken Sie auf den Container **tolldata** , um die hochgeladene JSON-Datei mit den Daten zur Fahrzeugregistrierung anzuzeigen.
   
    ![Screenshot der Datei „registration.json“ im Container](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Azure SQL-Datenbank
1. Klicken Sie links im Azure-Portal auf **SQL-DATENBANKEN** , um die SQL-Datenbank anzuzeigen, die im Tutorial verwendet wird.
   
    ![SQL-DATENBANKEN](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)
2. Klicken Sie auf **tolldatadb**.
   
    ![Screenshot der erstellten SQL-Datenbank](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
3. Kopieren Sie den Servernamen ohne die Portnummer (z.B. *servername*.database.windows.net).

## <a name="connect-to-the-database-from-visual-studio"></a>Herstellen einer Verbindung mit der Datenbank über Visual Studio
Verwenden Sie Visual Studio, um auf Abfrageergebnisse in der Ausgabedatenbank zuzugreifen.

Gehen Sie wie folgt vor, um über Visual Studio eine Verbindung mit der SQL-Datenbank (dem Ziel) herzustellen:

1. Öffnen Sie Visual Studio, und klicken Sie dann auf **Tools** > **Mit Datenbank verbinden**.
2. Klicken Sie auf **Microsoft SQL Server** , wenn Sie nach der Datenquelle gefragt werden.
   
    ![Dialogfeld „Datenquelle ändern“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. Fügen Sie in das Feld **Servername** den Namen ein, den Sie im vorherigen Abschnitt aus dem Azure-Portal kopiert haben (also „ *servername*.database.windows.net“).
4. Klicken Sie auf **SQL Server-Authentifizierung verwenden**.
5. Geben Sie im Feld **Benutzername** den Text **tolladmin** und **123toll!** in the **123toll!** ein.
6. Klicken Sie auf **Datenbanknamen eingeben oder auswählen**, und wählen Sie **TollDataDB** als Datenbank aus.
   
    ![Dialogfeld „Verbindung hinzufügen“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. Klicken Sie auf **OK**.
8. Öffnen Sie Server-Explorer.
   
    ![Server-Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. In der Datenbank „TollDataDB“ werden vier Tabellen angezeigt.
   
    ![Tabellen in der Datenbank „TollDataDB“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator:-tollapp-sample-project"></a>Ereignisgenerator: TollApp-Beispielprojekt
Das PowerShell-Skript beginnt automatisch damit, Ereignisse zu senden, indem es die TollApp-Beispielanwendung verwendet. Sie müssen keine zusätzlichen Schritte ausführen.

Falls Sie an Details zur Implementierung interessiert sind, finden Sie den Quellcode der TollApp-Anwendung in GitHub unter [samples/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Screenshot des Beispielcodes in Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
1. Öffnen Sie Stream Analytics im Azure-Portal, und klicken Sie in der unteren linken Ecke der Seite auf **NEU** , um einen neuen Analytics-Auftrag zu erstellen.
   
    ![Schaltfläche "Neu"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. Klicken Sie auf **SCHNELLERFASSUNG**. Wählen Sie die Region aus, in der das Skript Ihre anderen Ressourcen erstellt.
3. Wählen Sie für die Einstellung **SPEICHERKONTO FÜR REGIONALE ÜBERWACHUNG** die Option **NEUES SPEICHERKONTO ERSTELLEN** aus, und versehen Sie das Konto mit einem eindeutigen Namen. Azure Stream Analytics verwendet dieses Konto, um Überwachungsinformationen für alle Ihre zukünftigen Aufträge zu speichern.
4. Klicken Sie am Ende der Seite auf **STREAM ANALYTICS-AUFTRAG ERSTELLEN** .
   
    ![Option „Stream Analytics-Auftrag erstellen“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Definieren von Eingabequellen
1. Klicken Sie im Portal auf den erstellten Analytics-Auftrag.
   
    ![Screenshot des Analytics-Auftrags im Portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)
2. Öffnen Sie die Registerkarte **EINGABEN** , um die Quelldaten zu definieren.
   
    ![Registerkarte „Eingaben“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)
3. Klicken Sie auf **EINGABE HINZUFÜGEN**.
   
    ![Option „Eingabe hinzufügen“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. Klicken Sie auf der ersten Seite auf die Option **DATENSTROM** .
   
    ![Option „Datenstrom“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)
5. Wählen Sie auf der zweiten Seite des Assistenten die Option **EVENT HUB** .
   
    ![Option „Event Hub“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)
6. Geben Sie **EntryStream** als **EINGABEALIAS** ein.
7. Klicken Sie auf die Dropdownliste **Event Hub**, und wählen Sie den Eintrag aus, der mit „TollData“ beginnt (z.B. TollData9518658221).
8. Wählen Sie **entry** als Name des Event Hub und **all** als Name der Event Hubrichtlinie aus.
   
    Ihre Einstellungen sehen nun folgendermaßen aus:
   
    ![Event Hub-Einstellungen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)
9. Wählen Sie auf der nächsten Seite **JSON** für **EREIGNISSERIALISIERUNGSFORMAT** und **UTF8** für **CODIERUNG** aus.
   
    ![Serialisierungseinstellungen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)
10. Klicken Sie am unteren Rand der Seite auf **OK** , um den Assistenten zu beenden.
    
    ![Screenshot der EntryStream-Eingabe im Azure-Portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)
    
    Nachdem Sie nun den Eingabedatenstrom erstellt haben, führen Sie die gleichen Schritte zum Erstellen des Ausgabedatenstroms aus. Achten Sie darauf, dass Sie die Werte auf der dritten Seite des Assistenten wie im folgenden Screenshot eingeben:
    
    ![Einstellungen für Ausgabedatenstrom](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    Sie haben zwei Eingabedatenströme definiert:
    
    ![Definierte Eingabedatenströme im Azure-Portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)
    
    Als Nächstes fügen Sie die Referenzdateneingabe für die Blobdatei hinzu, in der die Fahrzeugregistrierungsdaten enthalten sind.
11. Klicken Sie auf **EINGABE HINZUFÜGEN** und dann auf **REFERENZDATEN**.
    
    ![Option „Eingabe hinzufügen“ mit ausgewählten Referenzdaten](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)
12. Wählen Sie auf der nächsten Seite das Speicherkonto, das mit **tolldata**beginnt. Der Name des Containers sollte **tolldata** und der Blobname unter **PFADMUSTER** sollte **registration.json** lauten. Für diesen Dateinamen wird die Groß-/Kleinschreibung berücksichtigt, und er sollte nur Kleinbuchstaben enthalten.
    
    ![Blogspeichereinstellungen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. Wählen Sie auf der nächsten Seite die Werte wie im folgenden Screenshot aus, und klicken Sie auf **OK** , um den Assistenten zu beenden.
    
    ![Auswahl von „JSON“ für „Ereignisserialisierungsformat“ und „UTF8“ für „Codierung“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Jetzt sind alle Eingaben definiert.

![Screenshot der drei definierten Eingaben](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Definieren der Ausgabe
1. Klicken Sie auf die Registerkarte **AUSGABE** und dann auf **AUSGABE HINZUFÜGEN**.
   
    ![Registerkarte „Ausgabe“ und Option „Ausgabe hinzufügen“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)
2. Klicken Sie auf **SQL Database**.
3. Wählen Sie den Namen des Servers, der im Abschnitt „Herstellen einer Verbindung mit der Datenbank über Visual Studio“ des Artikels verwendet wurde. Der Datenbankname ist **TollDataDB**.
4. Geben Sie im Feld **BENUTZERNAME** den Text **tolladmin**, **123toll!** im Feld **KENNWORT** den Text und im Feld **TABELLE** **TollDataRefJoinein**.
   
    ![SQL-Datenbankeinstellungen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure Stream Analytics-Abfragen
Die Registerkarte **ABFRAGE** enthält eine SQL-Abfrage, mit der die eingehenden Daten transformiert werden.

![Hinzugefügte Abfrage auf der Registerkarte „Abfrage“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

In diesem Tutorial versuchen wir, einige geschäftliche Fragen zu beantworten, die im Zusammenhang mit Mautdaten stehen. Außerdem werden Stream Analytics-Abfragen erstellt, die in Azure Stream Analytics verwendet werden können, um eine relevante Antwort zu liefern.

Bevor wir mit dem ersten Stream Analytics-Auftrag beginnen, sehen wir uns einige Szenarien und die Abfragesyntax an.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Einführung in die Azure Stream Analytics-Abfragesprache
- - -
Angenommen, Sie müssen die Fahrzeuge zählen, die ein Mauthäuschen passieren. Da es sich um einen kontinuierlichen Datenstrom von Ereignissen handelt, müssen Sie einen bestimmten Zeitraum definieren. Wir formulieren die Frage also neu: „Wie viele Fahrzeuge passieren ein Mauthäuschen jeweils innerhalb von drei Minuten?“ Dies wird für gewöhnlich als „Rollierende Anzahl“ bezeichnet.

Sehen wir uns die Beantwortung dieser Frage durch die Azure Stream Analytics-Abfrage an:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Es ist erkennbar, dass in Azure Stream Analytics eine Abfragesprache verwendet wird, die wie SQL aufgebaut ist und über einige Erweiterungen verfügt, damit die zeitbezogenen Aspekte der Abfrage angegeben werden können.

Weitere Informationen finden Sie in den Artikeln zu [Zeitmanagement](https://msdn.microsoft.com/library/azure/mt582045.aspx)- und [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx)-Konstrukten, die in der Abfrage von MSDN verwendet werden.

## <a name="testing-azure-stream-analytics-queries"></a>Testen von Azure Stream Analytics-Abfragen
Da Sie nun Ihre erste Azure Stream Analytics-Abfrage geschrieben haben, können Sie sie mithilfe der Beispieldatendateien testen, die in Ihrem TollApp-Ordner unter dem folgendem Pfad vorhanden sind:

**..\\TollApp\\TollApp\\Data**

Dieser Ordner enthält die folgenden Dateien:

* Entry.json
* Exit.json
* registration.json

## <a name="question-1:-number-of-vehicles-entering-a-toll-booth"></a>Frage 1: Wie viele Fahrzeuge passieren ein Mauthäuschen?
1. Öffnen Sie das Azure-Portal, und navigieren Sie zum erstellten Azure Stream Analytics-Auftrag. Klicken Sie auf die Registerkarte **ABFRAGE** , und fügen Sie die Abfrage aus dem vorherigen Abschnitt ein.
   
    ![Auf Registerkarte „Abfrage“ eingefügte Abfrage](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)
2. Um diese Abfrage mit Beispieldaten zu überprüfen, klicken Sie auf die Schaltfläche **Test**. Navigieren Sie im sich öffnenden Dialogfeld zu „Entry.json“. Diese Datei enthält Beispieldaten aus dem **EntryTime**-Ereignisstrom.
   
    ![Screenshot der Datei „Entry.json“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. Überprüfen Sie, ob die Abfrage wie erwartet ausgegeben wird.
   
    ![Ergebnisse des Tests](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2:-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Frage 2: Wie lange benötigt ein einzelnes Auto, um das Mauthäuschen zu passieren?
Die durchschnittliche Zeit, die ein Fahrzeug zum Passieren des Mauthäuschens benötigt, trägt zur Bewertung der Effizienz des Prozesses und der Benutzerfreundlichkeit für Kunden bei.

Zur Ermittlung der Gesamtzeit müssen Sie den EntryTime-Datenstrom mit dem ExitTime-Datenstrom verknüpfen. Sie fügen die Datenströme in den Spalten „TollId“ und „LicencePlate“ zusammen. Beim Operator **JOIN** müssen Sie einen zeitlichen Spielraum angeben, um die akzeptable Zeitdifferenz zwischen den verknüpften Ereignissen zu beschreiben. Sie verwenden die Funktion **DATEDIFF** , um anzugeben, dass Ereignisse nicht mehr als 15 Minuten auseinander liegen sollen. Sie wenden außerdem die Funktion **DATEDIFF** auf Ausfahrts- und Einfahrtszeiten an, um die genaue Zeit zu berechnen, die ein Auto in der Mautstation verbringt. Beachten Sie den Unterschied bei der Verwendung der Funktion **DATEDIFF** in einer **SELECT**-Anweisung im Vergleich zu einer **JOIN**-Bedingung.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Aktualisieren Sie die Abfrage auf der Registerkarte **ABFRAGE** Ihres Auftrags, um sie zu testen:
   
    ![Aktualisierte Abfrage auf der Registerkarte „Abfrage“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)
2. Klicken Sie auf **Test** , und geben Sie Beispieleingabedateien für die Einfahrts- und Ausfahrtszeit (EntryTime und ExitTime) an.
   
    ![Screenshot von ausgewählten Eingabedateien](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)
3. Aktivieren Sie das Kontrollkästchen, um die Abfrage zu testen und die Ausgabe anzuzeigen:
   
    ![Ausgabe des Tests](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3:-report-all-commercial-vehicles-with-expired-registration"></a>Frage 3: Welche Nutzfahrzeuge sind nicht mehr registriert?
Azure Stream Analytics kann statische Momentaufnahmen von Daten mit temporären Datenströmen verknüpfen. Um diese Funktion zu demonstrieren, verwenden wir die folgende Beispielfrage.

Wenn ein Nutzfahrzeug bei einer Mautfirma registriert ist, kann es das Mauthäuschen passieren, ohne für eine Kontrolle angehalten zu werden. Sie verwenden die Nachschlagetabellen für die Nutzfahrzeugregistrierung, um alle Nutzfahrzeuge mit abgelaufener Registrierung zu identifizieren.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Um eine Abfrage mit Referenzdaten zu testen, müssen Sie eine Eingabequelle für die Referenzdaten definieren. Dies haben Sie bereits durchgeführt.

1. Um diese Abfrage zu testen, fügen Sie die Abfrage in die Registerkarte **ABFRAGE** ein, klicken Sie auf **Test**, und geben Sie zwei Eingabequellen an:
   
    ![Screenshot von ausgewählten Eingabedateien](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)
2. Betrachten Sie die Ausgabe der Abfrage.
   
    ![Screenshot der Abfrageausgabe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Starten des Stream Analytics-Auftrags
Jetzt beenden Sie die Konfiguration und starten den Auftrag. Speichern Sie die Abfrage aus Frage 3, um eine Ausgabe gemäß dem Schema der Ausgabetabelle **TollDataRefJoin** zu erzeugen.

Navigieren Sie zum **DASHBOARD** des Auftrags, und klicken Sie auf **START**.

![Screenshot der Schaltfläche „Start“ im Dashboard des Auftrags](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

Ändern Sie im sich öffnenden Dialogfeld die Zeitangabe für **AUSGABE STARTEN** in **BENUTZERDEFINIERTE UHRZEIT**. Ändern Sie die Stunde in eine Stunde vor der aktuellen Zeit. Mit dieser Änderung wird sichergestellt, dass alle Ereignisse aus dem Event Hub seit dem Beginn der Generierung von Ereignissen am Anfang des Tutorials verarbeitet werden. Klicken Sie auf das Häkchen, um den Auftrag zu starten.

![Auswahl der benutzerdefinierten Zeit](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Das Starten des Auftrags kann einige Minuten dauern. Der Status wird auf der obersten Seite von Stream Analytics angezeigt.

![Screenshot des Auftragsstatus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Überprüfen Sie die Ergebnisse in Visual Studio
1. Öffnen Sie Visual Studio Server-Explorer, und klicken Sie mit der rechten Maustaste auf die Tabelle **TollDataRefJoin** .
2. Klicken Sie auf **Tabellendaten anzeigen** , um die Ausgabe des Auftrags anzuzeigen.
   
    ![Auswahl von „Tabellendaten anzeigen“ im Server-Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Horizontales Hochskalieren von Azure Stream Analytics-Aufträgen
Azure Stream Analytics ist für die elastische Skalierung ausgelegt, damit große Datenmengen verarbeitet werden können. Die Azure Stream Analytics-Abfrage kann eine **PARTITION BY** -Klausel verwenden, um das System darauf hinzuweisen, dass dieser Schritt horizontal hochskaliert wird. **PartitionId** ist eine spezielle Spalte, die vom System hinzugefügt wurde und mit der Partitions-ID der Eingabe (Event Hub) übereinstimmt.

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Beenden Sie den aktuellen Auftrag, aktualisieren Sie die Abfrage auf der Registerkarte **ABFRAGE**, und öffnen Sie die Registerkarte **SKALIEREN**.
   
    **STREAMINGEINHEITEN** definieren die Menge an Rechenleistung, die der Auftrag erhalten kann.
2. Verschieben Sie den Schieberegler auf 6.
   
    ![Screenshot der Auswahl von sechs Streamingeinheiten](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)
3. Wechseln Sie zur Registerkarte **AUSGABE**, und ändern Sie den Namen der SQL-Tabelle in **TollDataTumblingCountPartitioned**.

Wenn Sie den Auftrag jetzt starten, kann Azure Stream Analytics die Arbeit auf mehrere Computeressourcen verteilen und einen besseren Durchsatz erzielen. Bitte beachten Sie, dass die TollApp-Anwendung auch Ereignisse sendet, die nach der TollId partitioniert sind.

## <a name="monitor"></a>Monitor
Die Registerkarte **MONITOR** (ÜBERWACHEN) enthält Statistiken zum laufenden Auftrag.

![Screenshot der Statistiken zu ausgeführten Aufträgen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

Sie können auf die **Vorgangsprotokolle** über die Registerkarte **DASHBOARD** zugreifen.

![Option „Vorgangsprotokolle“](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Screenshot mit Vorgangsprotokollen und dem Status von Aufträgen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Um weitere Informationen zu einem bestimmten Ereignis anzuzeigen, klicken Sie auf das Ereignis und dann auf die Schaltfläche **DETAILS** .

![Screenshot mit Details zu einem ausgewählten Ereignis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Zusammenfassung
In diesem Tutorial haben Sie eine Einführung in den Azure Stream Analytics-Dienst erhalten. Es wurde gezeigt, wie Sie Eingaben und Ausgaben für den Stream Analytics-Auftrag konfigurieren. Anhand des Mautdatenszenarios wurden die häufigsten Probleme erläutert, die im Bereich der Daten in Bewegung vorkommen, und aufgezeigt, wie diese mithilfe von einfachen SQL-ähnlichen Abfragen in Azure Stream Analytics behoben werden können. Im Tutorial wurden SQL-Erweiterungskonstrukte für die Arbeit mit temporären Datenströmen beschrieben. Es wurde veranschaulicht, wie Sie Datenströme verknüpfen, den Datenstrom um statische Referenzdaten erweitern und eine Abfrage horizontal hochskalieren, um einen höheren Durchsatz zu erzielen.

Das Tutorial ist zwar eine gute Einführung, aber es ist bei Weitem nicht erschöpfend. Weitere Abfragemuster mit der SAQL-Sprache finden Sie unter [Abfragebeispiele für gängige Stream Analytics-Verwendungsmuster](stream-analytics-stream-analytics-query-patterns.md).
Weitere Informationen zu Azure Stream Analytics finden Sie in der [Onlinedokumentation](https://azure.microsoft.com/documentation/services/stream-analytics/) .

## <a name="clean-up-your-azure-account"></a>Bereinigen Ihres Azure-Kontos
1. Beenden Sie den Stream Analytics-Auftrag im Azure-Portal.
   
    Mit dem Skript „Setup.ps1“ werden zwei Event Hubs und eine SQL-Datenbank erstellt. Die folgenden Anleitungen helfen Ihnen dabei, Ressourcen am Ende des Tutorials zu bereinigen.
2. Geben Sie in einem PowerShell-Fenster **.\\\Cleanup.ps1** ein, um das Skript zu starten, mit dem die im Tutorial verwendeten Ressourcen gelöscht werden.
   
   > [!NOTE]
   > Ressourcen werden anhand des Namens identifiziert. Stellen Sie sicher, dass Sie jedes Element sorgfältig überprüfen, bevor Sie das Entfernen bestätigen.
   > 
   > 
   
    ![Screenshot des Bereinigungsprozesses](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)

<!--HONumber=Oct16_HO2-->


