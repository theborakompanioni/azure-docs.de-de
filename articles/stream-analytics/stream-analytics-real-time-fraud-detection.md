# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit

Dieses Tutorial enthält detaillierte Abbildungen zur Verwendung von Azure Stream Analytics. Folgendes wird vermittelt: 

* Weiterleiten von Datenstromereignissen an eine Instanz von Azure Event Hubs. In diesem Tutorial verwenden Sie eine App, die wir zum Simulieren eines Datenstroms von Mobiltelefon-Metadatensätze bereitstellen.

* Schreiben von SQL-ähnlichen Stream Analytics-Abfragen zum Transformieren von Daten, Aggregieren von Informationen oder Suchen nach Mustern. Sie erfahren, wie Sie über eine Abfrage den eingehenden Datenstrom überprüfen und nach Anrufen mit Verdacht auf betrügerischem Beweggrund suchen.

* Senden der Ergebnisse an eine Ausgabesenke (Speicher), die Sie zur Gewinnung weiterer Erkenntnisse analysieren können. In diesem Fall senden Sie die Daten verdächtiger Anrufe an den Azure Blob Storage.

In diesem Tutorial verwenden wir als Beispiel Telefonanrufdaten im Rahmen einer Betrugserkennung in Echtzeit. Die von uns dargelegten Verfahren lassen sich jedoch auch bei anderen Arten von Betrugserkennungsszenarien einsetzen, z.B. bei Kreditkartenbetrug oder Identitätsdiebstahl. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Szenario: Telekommunikation und SIM-Betrugserkennung in Echtzeit

Ein Telekommunikationsunternehmen hat eine große Datenmenge durch eingehende Anrufe. Das Unternehmen möchte betrügerische Anrufe in Echtzeit erkennen, um Kunden hierüber benachrichtigen zu können oder den Dienst einer bestimmten Nummer zu sperren. Eine Form von SIM-Kartenbetrug sind mehrmalige Anrufe von derselben Identität, die ungefähr zur selben Zeit, jedoch von geografisch unterschiedlichen Standorten aus getätigt werden. Zur Erkennung dieser Art von Betrug muss das Unternehmen die Datensätze eingehender Anrufe überprüfen und nach bestimmten Mustern suchen – in diesem Fall nach Anrufen, die ungefähr zur selben Zeit von verschiedenen Ländern aus getätigt werden. Telefondatensätze, die in diese Kategorie fallen, werden zur anschließenden Analyse in den Speicher geschrieben.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial simulieren Sie Telefonanrufdaten mithilfe einer Client-App, die beispielhafte Telefonanrufmetadaten generiert. Einige der Datensätze, die die App erzeugt, ähneln betrügerischen Anrufen. 

Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

* Ein Azure-Konto.
* Die App zum Generieren von Anrufereignissen. Sie erhalten diese, indem Sie die Datei [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) aus dem Microsoft Download Center herunterladen. Entzippen Sie dieses Paket in einem Ordner auf Ihrem Computer. Wenn Sie den Quellcode prüfen und die App in einem Debugger ausführen möchten, können Sie den Quellcode der App über [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator) beziehen. 

    >[!NOTE]
    >Windows blockiert möglicherweise die heruntergeladene ZIP-Datei. Wenn Sie die Datei nicht entzippen können, klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie **Eigenschaften**. Wenn die Meldung „Die Datei stammt von einem anderen Computer. Der Zugriff wurde aus Sicherheitsgründen eventuell blockiert.“ angezeigt wird, wählen Sie die Option **Nicht mehr blockieren**, und klicken Sie dann auf **Übernehmen**.

Wenn Sie die Ergebnisse des Stream Analytics-Auftrags untersuchen möchten, benötigen Sie darüber hinaus ein Tool zum Anzeigen der Inhalte eines Azure Blob Storage-Containers. Wenn Sie Visual Studio einsetzen, können Sie [Azure-Tools für Visual Studio](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) oder [Visual Studio Cloud-Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-resources-managing-with-cloud-explorer) verwenden. Alternativ können Sie eigenständige Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) oder [Azure-Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) installieren. 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Erstellen von Azure Event Hubs zum Erfassen von Ereignissen

Um einen Datenstrom zu analysieren, *erfassen* Sie diesen in Azure. Eine typische Vorgehensweise zum Erfassen von Daten ist die Verwendung von [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), mit denen Sie Millionen von Ereignissen pro Sekunde verarbeiten und die Ereignisinformationen dann verarbeiten und speichern können. Für dieses Tutorial erstellen Sie einen Event Hub und senden über die App zum Generieren von Anrufereignissen Anrufdaten an den betreffenden Event Hub. Weitere Informationen zu Event Hubs finden Sie in der [Dokumentation zu Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus/).

>[!NOTE]
>Eine ausführlichere Version dieses Verfahrens finden Sie unter [Erstellen eines Event Hub-Namespace und eines Event Hubs mithilfe des Azure-Portals](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Erstellen eines Namespace und eines Event Hubs
In diesem Verfahren erstellen Sie zuerst einen Event Hub-Namespace und fügen diesem Namespace dann einen Event Hub hinzu. Event Hub-Namespaces werden verwendet, um verwandte Ereignisbusinstanzen logisch zu gruppieren. 

1. Melden Sie sich beim Azure-Portal an, und klicken Sie auf **Neu** > **Internet der Dinge** > **Event Hub**. 

2. Geben Sie auf dem Blatt **Namespace erstellen** einen Namen für den Namespace ein (z.B. `<yourname>-eh-ns-demo`). Sie können einen beliebigen Namen für den Namespace verwenden. Der Name muss lediglich für eine URL gültig und innerhalb von Azure eindeutig sein. 
    
3. Wählen Sie ein Abonnement aus, und erstellen oder wählen Sie eine Ressourcengruppe. Klicken Sie dann auf **Erstellen**. 

    ![Erstellen eines Event Hub-Namespace](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png)
 
4. Wenn die Bereitstellung des Namespace abgeschlossen ist, suchen Sie in Ihrer Liste der Azure-Ressourcen den Event Hub-Namespace. 

5. Klicken Sie auf den neuen Namespace, und wählen Sie auf dem Blatt „Namespace“ **+&nbsp;Event Hub** aus. 

    ![Schaltfläche „Event Hub hinzufügen“ zum Erstellen eines neuen Event Hubs ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Vergeben Sie einen Namen für den neuen Event Hub `sa-eh-frauddetection-demo`. Sie können auch einen anderen Namen verwenden. In diesem Fall sollten Sie sich den Namen notieren, da Sie ihn später noch benötigen. Vorerst müssen Sie keine weiteren Optionen für den Event Hub festlegen.

    ![Blatt zum Erstellen eines neuen Event Hubs](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png)
    
 
7. Klicken Sie auf **Erstellen**.
### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Gewähren des Zugriffs auf den Event Hub und Abrufen einer Verbindungszeichenfolge

Damit ein Prozess Daten an einen Event Hub senden kann, muss der Event Hub mit einer Richtlinie versehen sein, die einen entsprechenden Zugriff ermöglicht. Die Zugriffsrichtlinie erzeugt eine Verbindungszeichenfolge, die Autorisierungsinformationen enthält.

1.  Klicken Sie auf dem Blatt „Ereignis-Namespace“ auf **Event Hubs** und dann auf den Namen Ihres neuen Event Hubs.

2.  Klicken Sie auf dem Blatt „Event Hub“ auf **SAS-Richtlinien** und dann auf **+&nbsp;Hinzufügen**.

    >[!NOTE]
    >Stellen Sie sicher, dass Sie mit dem Event Hub arbeiten und nicht mit dem Event Hub-Namespace.

3.  Fügen Sie die Richtlinie namens `sa-policy-manage-demo` hinzu, und wählen Sie für **Anspruch** die Option **Verwalten**.

    ![Blatt zum Erstellen einer neuen Event Hub-Zugriffsrichtlinie](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png)
 
4.  Klicken Sie auf **Erstellen**.

5.  Nachdem die Richtlinie bereitgestellt wurde, klicken sie in der Liste der SAS-Richtlinien darauf.

6.  Navigieren Sie zum Feld **VERBINDUNGSZEICHENFOLGE – PRIMÄRSCHLÜSSEL**, und klicken Sie neben der Verbindungszeichenfolge auf die Schaltfläche „Kopieren“. 
    
    ![Kopieren des Primärschlüssels der Verbindungszeichenfolge aus der Zugriffsrichtlinie](./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png)
 
7.  Fügen Sie die Verbindungszeichenfolge in einen Texteditor ein. Sie benötigen diese Verbindungszeichenfolge für den nächsten Abschnitt, nachdem Sie einige kleine Änderungen vorgenommen haben.

    Die Verbindungszeichenfolge sieht folgendermaßen aus:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=sa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=sa-eh-frauddetection-demo

    Beachten Sie, dass die Verbindungszeichenfolge mehrere durch ein Semikolon getrennte Schlüssel/Wert-Paare enthält: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey` und `EntityPath`.  

## <a name="configure-and-start-the-event-generator-application"></a>Konfigurieren und starten der Ereignisgenerator-Anwendung

Bevor Sie die TelcoGenerator-App starten, konfigurieren Sie sie so, dass Anrufdatensätze an den soeben erstellten Event Hub gesendet werden.

### <a name="configure-the-telcogeneratorapp"></a>Konfigurieren der TelcoGenerator-App

1.  Notieren Sie im Editor, in den Sie die kopierte Verbindungszeichenfolge eingefügt haben, den Wert `EntityPath`, und entfernen Sie das `EntityPath`-Paar (danken Sie daran, das voranstehende Semikolon zu entfernen). 

2.  Öffnen Sie in einem Editor die Datei „telcodatagen.exe.config“ im Ordner, in dem sich die entzippte Datei „TelcoGenerator.zip“ befindet. (Es gibt mehrere Konfigurationsdateien. Stellen Sie daher sicher, dass Sie die richtige Datei öffnen.)

3.  Legen Sie im Element `<appSettings>` Folgendes fest:

    * Legen Sie den Wert des Schlüssels `EventHubName` auf den Event Hub-Namen fest (d.h. auf den Wert des Entitätspfads).
    * Legen Sie den Wert des Schlüssels `Microsoft.ServiceBus.ConnectionString` auf die Verbindungszeichenfolge fest. 

    Der Abschnitt `<appSettings>` sollte wie im folgenden Beispiel aussehen. (Der Übersichtlichkeit halber wurden die Zeilen umschlossen und einige Zeichen aus dem Autorisierungstoken entfernt.)

    ![Konfigurationsdatei der TelcoGenerator-App mit dem Event Hub-Namen und der Verbindungszeichenfolge](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Speichern Sie die Datei. 

### <a name="start-the-app"></a>Starten der App
1.  Öffnen Sie ein Befehlsfenster, und wechseln Sie zum Ordner, in dem sich die entzippte TelcoGenerator-App befindet.
2.  Geben Sie den folgenden Befehl ein:

        telcodatagen.exe 1000 .2 2

    Die Parameter lauten wie folgt: 

    * Anzahl der KDS pro Stunde 
    * Wahrscheinlichkeit eines SIM-Kartenbetrugs: Die Häufigkeit als Prozentsatz aller Anrufe, mit der die App einen betrügerischen Anruf simulieren soll. Der Wert „.2“ bedeutet, dass etwa 20 % der Anrufdatensätze betrügerische Anrufe simulieren.
    * Dauer in Stunden: Die Anzahl der Stunden, in denen die App ausgeführt werden soll. Sie können die App auch jederzeit beenden, indem Sie in der Befehlszeile Strg+C drücken.

    Nach wenigen Sekunden werden auf dem Bildschirm in der App die Telefonanrufdatensätze angezeigt, während diese an den Event Hub gesendet werden.

Zu einigen wichtigen Feldern dieser Anwendung zur Betrugserkennung in Echtzeit zählen Folgende:

|**Datensatz**|**Definition**|
|----------|--------------|
|`CallrecTime`|Der Zeitstempel für die Startzeit des Anrufs. |
|`SwitchNum`|Die für die Anrufverbindung verwendete Vermittlungsstelle. In diesem Beispiel werden die Vermittlungen durch Zeichenfolgen ausgedrückt, die das Ursprungsland (USA, China, Großbritannien, Deutschland oder Australien) darstellen. |
|`CallingNum`|Die Telefonnummer des Anrufers. |
|`CallingIMSI`|Die IMSI (International Mobile Subscriber Identity). Dies ist die eindeutige ID des Anrufers. |
|`CalledNum`|Die Telefonnummer des Angerufenen. |
|`CalledIMSI`|International Mobile Subscriber Identity (IMSI). Dies ist die eindeutige ID des Angerufenen. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Erstellen eines Stream Analytics-Auftrags zum Verwalten von Streamingdaten

Nachdem Sie einen Datenstrom von Anrufereignissen eingerichtet haben, können Sie nun einen Stream Analytics-Auftrag einrichten. Der Auftrag liest Daten aus dem von Ihnen eingerichteten Event Hub. 

### <a name="create-the-job"></a>Erstellen des Auftrags 

1. Klicken Sie im Azure-Portal auf **Neu** > **Internet der Dinge** > **Stream Analytics-Auftrag**.

2. Vergeben Sie einen Namen für den Auftrag `sa_frauddetection_job_demo`, und geben Sie ein Abonnement, eine Ressourcengruppe und einen Speicherort an.

    Es empfiehlt sich, den Auftrag und den Event Hub in derselben Region zu platzieren, damit Sie von einer optimalen Leistung profitieren und Ihnen keine Kosten für die Übertragung von Daten zwischen Regionen entstehen.

    ![Erstellen eines neuen Stream Analytics-Auftrags](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png)

3. Klicken Sie auf **Erstellen**.

    Der Auftrag wird erstellt, und die Auftragsdetails werden im Portal angezeigt. Die Einrichtung ist jedoch noch nicht abgeschlossen. Bevor Sie den Auftrag starten können, müssen Sie ihn konfigurieren.

### <a name="configure-job-input"></a>Konfigurieren einer Auftragseingabe

1. Suchen Sie im Dashboard oder auf dem Blatt **Alle Ressourcen** nach dem Stream Analytics-Auftrag `sa_frauddetection_job_demo`, und wählen Sie ihn aus. 
2. Klicken Sie auf dem Blatt „Stream Analytics-Auftrag“ im Abschnitt **Auftragstopologie** auf das Feld **Eingabe**.

    ![Feld „Eingabe“ unter „Topologie“ auf dem Blatt „Stream Analytics-Auftrag“](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Klicken Sie auf **+&nbsp;Hinzufügen**, und füllen Sie das Blatt dann mit den folgenden Werten aus:

    * **Eingabealias**: Verwenden Sie den Namen `CallStream`. Wenn Sie einen anderen Namen verwenden, notieren Sie sich diesen, da Sie ihn später benötigen.
    * **Quelltyp**: Wählen Sie **Datenstrom**. (**Verweisdaten** beziehen sich auf statische Nachschlagedaten, die Sie in diesem Tutorial nicht benötigen.)
    * **Quelle**: Wählen Sie **Event Hub**.
    * **Importoption**: Wählen Sie **Event Hub aus aktuellem Abonnement verwenden**. 
    * **Service Bus-Namespace**: Wählen Sie den Event Hub-Namespace, den Sie zuvor erstellt haben (`<yourname>-eh-ns-demo`).
    * **Event Hub**: Wählen Sie den Event Hub, den Sie zuvor erstellt haben (`sa-eh-frauddetection-demo`).
    * **Name der Event Hub-Richtlinie**: Wählen Sie die Zugriffsrichtlinie aus, die Sie zuvor erstellt haben (`sa-policy-manage-demo`).

    ![Erstellen einer neuen Eingabe für einen Stream Analytics-Auftrag](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png)

4. Klicken Sie auf **Erstellen**.

## <a name="create-queries-to-transform-real-time-data"></a>Erstellen von Abfragen zum Transformieren von Daten in Echtzeit

An diesem Punkt haben Sie einen Stream Analytics-Auftrag zum Lesen eines eingehenden Datenstroms eingerichtet. Der nächste Schritt besteht darin, eine Transformation zu erstellen, die die Daten in Echtzeit analysiert. Dazu müssen Sie eine Abfrage erstellen. Stream Analytics unterstützt ein einfaches, deklaratives Abfragemodell zum Beschreiben der Transformationen für Echtzeitverarbeitung. Die Abfragen verwenden eine SQL-ähnliche Sprache, die einige Erweiterungen speziell für Stream Analytics enthält. 

Eine sehr einfache Abfrage kann möglicherweise alle eingehenden Daten lesen. Allerdings erstellen Sie häufig Abfragen, die nach bestimmten Daten oder Beziehungen in den Daten suchen. In diesem Abschnitt des Tutorials erstellen und testen Sie verschiedene Abfragen, um einige Möglichkeiten kennenzulernen, mit denen Sie eine Datenstromeingabe zur Analyse transformieren können. 

Die Abfragen, die Sie hier erstellen, zeigen nur die transformierten Daten auf dem Bildschirm an. In einem Abschnitt weiter unten konfigurieren Sie eine Ausgabesenke und eine Abfrage, die die transformierten Daten in diese Senke schreibt.

Weitere Informationen zur Sprache finden Sie in der [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx)(in englischer Sprache).

### <a name="get-sample-data-for-testing-queries"></a>Abrufen von Beispieldaten zum Testen von Abfragen

Die TelcoGenerator-App sendet Anrufdatensätze an den Event Hub, und Ihr Stream Analytics-Auftrag wird zum Lesen von Daten aus dem Event Hub konfiguriert. Mithilfe einer Abfrage können Sie testen, ob die Daten des Auftrags korrekt gelesen werden. Zum Testen einer Abfrage in der Azure-Konsole benötigen Sie Beispieldaten. In dieser exemplarischen Vorgehensweise extrahieren Sie Beispieldaten aus dem Datenstrom, der vom Event Hub stammt.

1. Stellen Sie sicher, dass die TelcoGenerator-App ausgeführt wird und Anrufdatensätze erzeugt.
2. Kehren Sie im Portal zum Blatt „Stream Analytics-Auftrag“ zurück. (Wenn das Blatt geschlossen ist, suchen Sie auf dem Blatt **Alle Ressourcen** nach `sa_frauddetection_job_demo`.)
3. Klicken Sie in das Feld **Abfrage**. In Azure werden die Eingaben und Ausgaben aufgeführt, die für den Auftrag konfiguriert sind. Zudem können Sie eine Abfrage erstellen, mit der Sie die Datenstromeingabe beim Senden an die Ausgabe transformieren können.
4. Klicken Sie auf dem Blatt **Abfrage** auf die Punkte neben der `CallStream`-Eingabe, und wählen Sie anschließend **Beispieldaten aus Eingabe**.

    ![Menüoptionen zur Verwendung von Beispieldaten für den Stream Analytics-Auftragseintrag bei ausgewählter Option „Beispieldaten aus Eingabe“](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)

    Daraufhin wird ein Blatt geöffnet, auf dem Sie angeben können, wie viele Beispieldaten in einem bestimmten definierten Zeitraum, in dem die Datenstromeingabe gelesen wird, abgerufen werden sollen.

5. Legen Sie **Minuten** auf „3“ fest, und klicken Sie dann auf **OK**. 
    
    ![Optionen zur Entnahme von Stichproben aus der Datenstromeingabe mit ausgewählter Option „3 Minuten“](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure entnimmt für einen Zeitraum von 3 Minuten Stichprobendaten aus der Datenstromeingabe und benachrichtigt Sie, wenn die Beispieldaten bereit sind. (Dies dauert einen Augenblick.) 

Die Beispieldaten werden vorübergehend gespeichert und sind verfügbar, solange das Abfragefenster geöffnet ist. Wenn Sie das Abfragefenster schließen, werden die Beispieldaten verworfen, und Sie müssen einen neuen Satz von Beispieldaten erstellen. 

Alternativ können Sie [über GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) eine JSON-Datei mit Beispieldaten abrufen und diese JSON-Datei dann zur Verwendung als Beispieldaten für die Eingabe `CallStream` hochladen. 

### <a name="test-using-a-pass-through-query"></a>Testen durch eine Pass-Through-Abfrage

Wenn Sie alle Ereignisse archivieren möchten, können Sie eine Pass-Through-Abfrage verwenden, um alle Felder in der Nutzlast des Ereignisses zu lesen.

1. Geben Sie im Abfragefenster die folgende Abfrage ein:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >Wie bei SQL muss die Groß-/Kleinschreibung bei Schlüsselwörtern nicht beachtet werden, und auch Leerräume spielen keine Rolle.

    In dieser Abfrage ist `CallStream` der Alias, den Sie beim Erstellen der Eingabe angegeben haben. Wenn Sie einen anderen Alias verwendet haben, verwenden Sie stattdessen diesen Namen.

2. Klicken Sie auf **Test**.

    Der Stream Analytics-Auftrags führt die Abfrage für die Beispieldaten aus und zeigt die Ausgabe im unteren Bereich des Fensters an. Dies weist darauf hin, dass der Event Hub und der Stream Analytics-Auftrag korrekt konfiguriert sind. (Wie bereits erwähnt wurde, erstellen Sie später eine Ausgabesenke, in die die Abfrage Daten schreiben kann.)

    ![Stream Analytics-Auftragsausgabe mit 73 generierten Datensätzen](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Die genaue Anzahl der angezeigten Datensätze hängt davon ab, wie viele Datensätze in Ihrer 3-minütigen Stichprobe erfasst wurden.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Reduzieren der Anzahl von Feldern durch eine Spaltenprojektion

In vielen Fällen sind bei Ihrer Analyse nicht alle Spalten der Datenstromeingabe erforderlich. Mithilfe einer Abfrage können Sie kleinere Sätze von zurückgegebenen Feldern projizieren als in der Pass-Through-Abfrage.

1. Ändern Sie die Abfrage im Code-Editor wie folgt:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Klicken Sie erneut auf **Test**. 

    ![Stream Analytics-Auftragsausgabe mit 25 generierten Datensätzen zur Projektion](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Zählen der eingehenden Anrufe pro Region: Rollierendes Fenster mit Aggregation

Nehmen wir an, Sie möchten die Anzahl der eingehenden Anrufe pro Region zählen. Wenn Sie bei Streamingdaten Aggregatfunktionen wie eine Zählung durchführen möchten, müssen Sie den Datenstrom in temporale Einheiten segmentieren (da der Datenstrom selbst faktisch endlos ist). Hierzu müssen Sie eine Stream Analytics-[Fensterfunktion](stream-analytics-window-functions.md) verwenden. Sie können dann mühelos in diesem Fenster mit den Daten arbeiten.

Für diese Transformation sollte eine Sequenz von temporalen Fenstern erzeugt werden, die sich nicht überlappen – jedes Fenster weist einen separaten Satz von Daten auf, den Sie gruppieren und aggregieren können. Diese Art von Fenster wird als *rollierendes Fenster* bezeichnet. In einem rollierenden Fenster können Sie die Anzahl eingehender Anrufe gruppiert nach `SwitchNum` abrufen, die für das Land steht, aus dem der Anruf stammt. 

1. Ändern Sie die Abfrage im Code-Editor wie folgt:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Diese Abfrage verwendet das Schlüsselwort `Timestamp By` in der Klausel `FROM`, um anzugeben, welches Zeitstempelfeld in der Datenstromeingabe für die Definition des rollierenden Fensters verwendet werden soll. In diesem Fall werden die Daten im Fenster in Segmente unterteilt, und zwar anhand des Felds `CallRecTime` in jedem Datensatz. (Wird kein Feld angegeben, verwendet der Windowingvorgang die Zeit, zu der jedes Ereignis beim Event Hub eingeht.) Weitere Informationen finden Sie unter „Ankunftszeit vs. Anwendungszeit“ in der [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    Die Projektion enthält `System.Timestamp`, die einen Zeitstempel für das Ende jedes Fensters zurückgibt. 

    Um anzugeben, dass Sie ein rollierendes Fenster verwenden möchten, verwenden Sie die Funktion [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) in der Klausel `GROUP BY `. In der Funktion geben Sie eine Zeiteinheit (von einer Mikrosekunde bis zu einem Tag) und eine Fenstergröße (Anzahl der Einheiten) an. In diesem Beispiel besteht das rollierende Fenster aus 5-Sekunden-Intervallen, sodass Sie alle 5 Sekunden der Anrufe eine Zählung pro Land erhalten.

2. Klicken Sie erneut auf **Test**. Beachten Sie, dass die Zeitstempel in den Ergebnissen unter **WindowEnd** in 5-Sekunden-Schritten angegeben werden.

    ![Stream Analytics-Auftragsausgabe mit 13 generierten Datensätzen zur Aggregation](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Erkennen von SIM-Kartenbetrug durch Selbstverknüpfung

In diesem Beispiel kann eine betrügerische Verwendung als Anrufe definiert werden, die vom selben Benutzer stammen, jedoch innerhalb von 5 Sekunden von verschiedenen Standorten aus getätigt werden. Beispielsweise kann derselbe Benutzer nicht gleichzeitig einen legitimen Anruf aus den USA und aus Australien tätigen. 

In diesen Fällen können Sie den Datenstrom durch eine Selbstverknüpfung der Streamingdaten basierend auf dem Wert `CallRecTime` mit sich selbst verknüpfen. Sie können dann nach Anrufdatensätzen suchen, bei denen der Wert `CallingIMSI` (die ursprüngliche Anzahl) identisch ist, jedoch nicht der Wert `SwitchNum` (Ursprungsland).

Wenn Sie eine Verknüpfung mit Streamingdaten durchführen, müssen bei der Verknüpfung einige Beschränkungen dazu festgelegt werden, wie stark die übereinstimmenden Zeilen zeitlich getrennt werden können. (Wie bereits erwähnt wurde, sind Streamingdaten faktisch endlos.) Die Zeitgrenzen für die Beziehung werden in der Klausel `ON` der Verknüpfung mit der Funktion `DATEDIFF` angegeben. In diesem Fall basiert die Verknüpfung auf Anrufdaten mit einem Intervall von 5 Sekunden.

1. Ändern Sie die Abfrage im Code-Editor wie folgt: 

        SELECT  System.Timestamp as Time, 
            CS1.CallingIMSI, 
            CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, 
            CS1.SwitchNum as Switch1, 
            CS2.SwitchNum as Switch2 
        FROM CallStream CS1 TIMESTAMP BY CallRecTime 
            JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
            ON CS1.CallingIMSI = CS2.CallingIMSI 
            AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
        WHERE CS1.SwitchNum != CS2.SwitchNum

    Diese Abfrage ähnelt einer beliebigen SQL-Verknüpfung mit Ausnahme der Funktion `DATEDIFF` in der Verknüpfung. Diese ist eine Version von `DATEDIFF`, die speziell für Stream Analytics gilt, und muss in der Klausel `ON...BETWEEN` angezeigt werden. Die Parameter umfassen eine Zeiteinheit (in diesem Beispiel Sekunden) und die Aliase der beiden Quellen für die Verknüpfung. (Diese unterscheiden sich von der standardmäßigen SQL-Funktion `DATEDIFF`.) 

    Die Klausel `WHERE` enthält die Bedingung, die den betrügerischen Anruf kennzeichnet: die ursprünglichen Vermittlungen sind nicht identisch. 

2. Klicken Sie erneut auf **Test**. 

    ![Stream Analytics-Auftragsausgabe mit 6 generierten Datensätzen zur Selbstverknüpfung](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Klicken Sie auf **Speichern**. Hierdurch wird die Selbstverknüpfungsabfrage als Teil des Stream Analytics-Auftrags gespeichert. (Die Beispieldaten werden nicht gespeichert.)

    ![Speichern des Stream Analytics-Auftrags](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png)

## <a name="create-an-output-sink-to-store-transformed-data"></a>Erstellen einer Ausgabesenke zum Speichern von transformierten Daten

Sie haben einen Ereignisdatenstrom, eine Event Hub-Eingabe zum Erfassen von Ereignissen und eine Abfrage zur Durchführung einer Transformation über den Datenstrom definiert. Der letzte Schritt besteht darin, eine Ausgabesenke für den Auftrag zu definieren, d.h. einen Ort, in den der transformierte Datenstrom geschrieben wird. 

Sie können viele Ressourcen als Ausgabesenken verwenden, z.B. eine SQL Server-Datenbank, Table Storage, Data Lake Storage, Power BI oder sogar einen anderen Event Hub. Für dieses Tutorial schreiben Sie den Datenstrom in den Azure Blob Storage – eine gängige Option zum Sammeln von Ereignisinformationen zur späteren Analyse, da es unstrukturierte Daten aufnimmt.

Wenn ein Blob Storage-Konto vorhanden ist, können Sie dieses verwenden. In diesem Tutorial zeigen wir Ihnen, wie Sie ein neues Speicherkonto speziell für dieses Tutorial erstellen.

### <a name="create-an-azure-blob-storage-account"></a>Erstellen eines Azure Blob Storage-Kontos

1. Kehren Sie im Azure-Portal zum Blatt „Stream Analytics-Auftrag“ zurück. (Wenn das Blatt geschlossen ist, suchen Sie auf dem Blatt **Alle Ressourcen** nach `sa_frauddetection_job_demo`.)
2. Klicken Sie im Abschnitt **Auftragstopologie** in das Feld **Ausgabe**. 
3. Klicken Sie auf dem Blatt **Ausgaben** auf **+&nbsp;Hinzufügen**, und füllen Sie das Blatt dann mit den folgenden Werten aus:

    * **Ausgabealias**: Verwenden Sie den Namen `CallStream-FraudulentCalls`. 
    * **Senke**: Wählen Sie **Blobspeicher** aus.
    * **Importoptionen**: Wählen Sie **Blob Storage aus aktuellem Abonnement verwenden** aus.
    * **Speicherkonto**: Wählen Sie **Neues Speicherkonto erstellen** aus.
    * **Speicherkonto** (zweites Feld): Geben Sie `YOURNAMEsademo` ein, wobei `YOURNAME` für Ihren Namen oder eine andere eindeutige Zeichenfolge steht. Der Name darf nur Kleinbuchstaben und Zahlen enthalten und muss innerhalb von Azure eindeutig sein. 
    * **Container**: Geben Sie `sa-fraudulentcalls-demo` ein.
    Der Name des Speicherkontos und der Containername werden zusammen verwendet, um einen URI für Blob Storage nach folgendem Muster bereitzustellen: 

    `http://yournamesademo.blob.core.windows.net/sa-fraudulentcalls-demo/...`
    
    ![Blatt „Neue Ausgabe“ für den Stream Analytics-Auftrag](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png)
    
4. Klicken Sie auf **Erstellen**. 

    Azure erstellt das Speicherkonto und generiert automatisch einen Schlüssel. 

5. Schließen Sie das Blatt **Ausgaben**. 

## <a name="start-the-streaming-analytics-job"></a>Starten des Stream Analytics-Auftrags

Der Auftrag ist jetzt konfiguriert. Sie haben eine Eingabe (den Event Hub), eine Transformation (die Abfrage zum Suchen nach betrügerischen Anrufen) und eine Ausgabe (Blob Storage) festgelegt. Nun können Sie den Auftrag starten. 

1. Stellen Sie sicher, dass die App TelcoGenerator ausgeführt wird.

2. Klicken Sie auf dem Blatt „Auftrag“ auf **Starten**.

    ![Starten des Stream Analytics-Auftrags](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-output.png)

3. Wählen Sie auf dem Blatt **Auftrag starten** als Startzeit der Auftragsausgabe **Jetzt**. 

4. Klicken Sie auf **Start**. 

    ![Blatt „Auftrag starten“ für den Stream Analytics-Auftrag](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-job-blade.png)

    Azure benachrichtigt Sie, wenn der Auftrag gestartet wurde, und auf dem Blatt „Auftrag“ wird der Status **Wird ausgeführt** angezeigt.

    ![Stream Analytics-Auftrag mit dem Status „Wird ausgeführt“](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-running-status.png)
    

## <a name="examine-the-transformed-data"></a>Überprüfen der transformierten Daten

Sie haben nun einen Stream Analytics-Auftrag durchgeführt. Der Auftrag überprüft einen Datenstrom von Telefonanrufmetadaten. Dabei wird in Echtzeit nach betrügerischen Telefonanrufen gesucht und Informationen zu diesen betrügerischen Anrufen in den Speicher geschrieben. 

Zum Abschluss dieses Tutorials sollten Sie sich die Daten ansehen, die vom Stream Analytics-Auftrag erfasst werden. Die Daten werden in Blöcken (Dateien) in Azure Blob Storage geschrieben. Sie können ein beliebiges Tool verwenden, das Daten aus dem Azure Blob Storage liest. Wie im Abschnitt „Voraussetzungen“ erwähnt wurde, können Sie Azure-Erweiterungen in Visual Studio oder ein Tool wie [Azure-Speicher-Explorer](http://storageexplorer.com/) oder [Azure-Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) verwenden. 

Wenn Sie die Inhalte einer Datei in Blob Storage überprüfen, wird beispielsweise Folgendes angezeigt:

![Azure Blob Storage mit Stream Analytics-Ausgabe](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Es sind weitere Artikel vorhanden, die das Betrugserkennungsszenario behandeln und die die in diesem Tutorial erstellten Ressourcen verwenden. Wenn Sie fortfahren möchten, lesen Sie die Empfehlungen unter **Nächste Schritte** weiter unten.

Wenn Sie jedoch fertig sind und die erstellten Ressourcen nicht benötigen, können Sie sie löschen, damit keine unnötigen Gebühren seitens Azure anfallen. In diesem Fall wird empfohlen, folgendermaßen vorzugehen:

1. Halten Sie den Stream Analytics-Auftrag an. Klicken Sie auf dem Blatt **Aufträge** oben auf **Beenden**.
2. Beenden Sie die TelcoGenerator-App. Drücken Sie im Befehlsfenster, in dem Sie die App gestartet haben, die Tasten Strg+C.
3. Wenn Sie ein neues Blob Storage-Konto speziell für dieses Tutorial erstellt haben, löschen Sie es. 
4. Löschen Sie den Stream Analytics-Auftrag.
5. Löschen Sie den Event Hub.
6. Löschen Sie den Event Hub-Namespace.

## <a name="get-support"></a>Support

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

Sie können dieses Tutorial mit den folgenden Artikeln fortsetzen:

* [Stream Analytics und Power BI: Ein Dashboard zur Echtzeitanalyse von Streamingdaten](stream-analytics-power-bi-dashboard.md). In diesem Artikel wird gezeigt, wie die TelCo-Ausgabe des Stream Analytics-Auftrags zur Visualisierung und Analyse in Echtzeit an Power BI gesendet wird.
* [Vorgehensweise zum Speichern von Daten aus Azure Stream Analytics in einem Azure Redis Cache mithilfe von Azure Functions](stream-analytics-functions-redis.md). In diesem Artikel wird gezeigt, wie mithilfe von Azure Functions betrügerische Anrufe an einen Azure Redis Cache über eine Service Bus-Warteschlange geschrieben werden.

Weitere Informationen zu Stream Analytics allgemein finden Sie auch in diesen Artikeln:

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
