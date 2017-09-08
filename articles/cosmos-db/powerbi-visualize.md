---
title: "Power BI-Tutorial für den Azure Cosmos DB-Connector | Microsoft-Dokumentation"
description: "Verwenden Sie dieses Power BI-Tutorial, um JSON zu importieren, aufschlussreiche Berichte zu erstellen und Daten mithilfe des Azure Cosmos DB- und Power BI-Connectors zu visualisieren."
keywords: "Power BI-Tutorial, Daten visualisieren, Power BI-Connector"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: cd1b7f70-ef99-40b7-ab1c-f5f3e97641f7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 07e5e15f4f4c4281a93c8c3267c0225b1d79af45
ms.openlocfilehash: 03127c9d35b8dd0fe54310c84ff89ea087f175b7
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-the-power-bi-connector"></a>Power BI-Tutorial für Azure Cosmos DB: Visualisieren von Daten mithilfe des Power BI-Connectors
[PowerBI.com](https://powerbi.microsoft.com/) ist ein Onlinedienst zum Erstellen und Freigeben von Dashboards und Berichten mit Daten, die für Sie und Ihr Unternehmen wichtig sind.  Power BI Desktop ist ein dediziertes Berichterstellungstool, mit dem Sie Daten aus verschiedenen Datenquellen abrufen, die Daten zusammenführen und transformieren, leistungsstarke Berichte und Visualisierungen erstellen und Berichte in Power BI veröffentlichen können.  Mit der neuesten Version von Power BI Desktop können Sie jetzt über den Cosmos DB-Connector für Power BI eine Verbindung mit Ihrem Cosmos DB-Konto herstellen.   

In diesem Power BI-Tutorial gehen wir die Schritte zum Herstellen einer Verbindung mit einem Cosmos DB-Konto in Power BI Desktop durch, navigieren zu einer Sammlung, aus der die Daten mithilfe des Navigators extrahiert werden sollen, transformieren JSON-Daten mit dem Abfrage-Editor von Power BI Desktop in ein Tabellenformat und erstellen und veröffentlichen einen Bericht in PowerBI.com.

Nach Abschluss dieses Power BI-Tutorials können Sie die folgenden Fragen beantworten:  

* Wie kann ich mit Power BI Desktop Berichte mit Daten aus Cosmos DB erstellen?
* Wie kann ich in Power BI Desktop eine Verbindung mit einem Cosmos DB-Konto herstellen?
* Wie kann ich Daten aus einer Sammlung in Power BI Desktop abrufen?
* Wie kann ich verschachtelte JSON-Daten in Power BI Desktop transformieren?
* Wie kann ich meine Berichte in PowerBI.com veröffentlichen und freigeben?

> [!NOTE]
> Für die Extraktion und Transformation von Daten stellt der Power BI-Connector für Azure Cosmos DB eine Verbindung mit Power BI Desktop her. In Power BI Desktop erstellte Berichte können dann auf „PowerBI.com“ veröffentlicht werden. Direkte Extraktion und Transformation von Azure Cosmos DB-Daten kann nicht auf „PowerBI.com“ ausgeführt werden. 

> [!NOTE]
> Um Power BI mithilfe der MongoDB-API mit Azure Cosmos DB zu verbinden, müssen Sie den [Simba MongoDB-ODBC-Treiber](http://www.simba.com/drivers/mongodb-odbc-jdbc/) verwenden.

## <a name="prerequisites"></a>Voraussetzungen
Vergewissern Sie sich vor dem Ausführen der Anweisungen dieses Power BI-Tutorials, dass Sie über Zugriff auf folgende Ressourcen verfügen:

* [Die neueste Version von Power BI Desktop](https://powerbi.microsoft.com/desktop).
* Zugriff auf unser Demokonto oder Daten in Ihrem Cosmos DB-Konto.
  * Das Demokonto ist mit den Daten zu Vulkanen gefüllt, die in diesem Tutorial enthalten sind. Für dieses Demokonto, das nur zur Demonstration dient, gelten keine SLAs.  Wir behalten uns das Recht vor, Änderungen an diesem Demokonto vorzunehmen, dazu gehören u.a. das Kündigen des Kontos, das Ändern des Schlüssels, das Einschränken des Zugriffs, das Ändern und Löschen der Daten sowie weitere Änderungen, jederzeit und ohne Vorankündigung oder Grund.
    * URL: https://analytics.documents.azure.com
    * Schlüssel mit Leseberechtigung: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==
  * Sie können aber auch ein eigenes Konto erstellen. Weitere Informationen finden Sie unter [Erstellen eines Azure Cosmos DB-Datenbankkontos über das Azure-Portal](https://azure.microsoft.com/documentation/articles/create-account/). Um anschließend Beispieldaten zu Vulkanen abzurufen, die den in diesem Tutorial verwendeten ähneln (aber keine GeoJSON-Blöcke enthalten), besuchen Sie die [NOAA-Website](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5), und importieren Sie die Daten dann mithilfe des [Azure Cosmos DB-Datenmigrationstools](import-data.md).

Zum Freigeben von Berichten in PowerBI.com müssen Sie über ein Konto in PowerBI.com verfügen.  Weitere Informationen zur kostenlosen Power BI-Version und zu Power BI Pro erhalten Sie unter [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Erste Schritte
In diesem Tutorial wird angenommen, dass Sie als Geologe Vulkane auf der ganzen Welt studieren.  Die Daten zu Vulkanen werden in einem Cosmos DB-Konto gespeichert, und die JSON-Dokumente sehen wie das folgende Beispieldokument aus.

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Sie möchten die Daten zu Vulkanen aus dem Cosmos DB-Konto abrufen und in einem interaktiven Power BI-Bericht wie dem folgenden Bericht visualisieren.

![Nach Abschluss dieses Power BI-Tutorials mit dem Power BI-Connector können Sie Daten mit dem Power BI Desktop-Bericht zu Vulkanen visualisieren.](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Sind Sie bereit, es zu versuchen? Lassen Sie uns anfangen.

1. Führen Sie Power BI Desktop auf Ihrer Arbeitsstation aus.
2. Nach dem Start von Power BI Desktop wird eine *Willkommensseite* angezeigt.
   
    ![Power BI Desktop – Willkommensseite – Power BI-Connector](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Direkt über die *Willkommensseite* können Sie **Daten abrufen**, **aktuelle Quellen** anzeigen oder **andere Berichte öffnen**.  Klicken Sie oben rechts auf das X, um das Dialogfeld zu schließen. Die Ansicht **Bericht** von Power BI Desktop wird angezeigt.
   
    ![Power BI Desktop-Berichtsansicht – Power BI-Connector](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Wählen Sie das Menüband **Start** aus, und klicken Sie dann auf **Daten abrufen**.  Das Fenster **Daten abrufen** wird angezeigt.
5. Klicken Sie auf **Azure**, wählen Sie **Microsoft Azure DocumentDB (Beta)** aus, und klicken Sie dann auf **Verbinden**. 

    ![Power BI Desktop-Datenabruf – Power BI-Connector](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. Klicken Sie auf der Seite **Connector (Vorschau)** auf **Fortfahren**. Das Fenster **Verbindung mit Microsoft Azure DocumentDB** wird angezeigt.
7. Geben Sie die Endpunkt-URL des Cosmos DB-Kontos an, von dem Sie die Daten abrufen möchten, wie unten dargestellt, und klicken Sie dann auf **OK**. Um Ihr eigenes Konto zu verwenden, können Sie die URL aus dem Feld „URI“ auf dem Blatt **[Schlüssel](manage-account.md#keys)** des Azure-Portals abrufen. Geben Sie zum Verwenden des Demokontos `https://analytics.documents.azure.com` als URL ein. 
   
    Lassen Sie den Datenbanknamen, Sammlungsnamen und die SQL-Anweisung leer, da diese Felder optional sind.  Stattdessen verwenden wir den Navigator zum Auswählen der Datenbank und der Sammlung, um zu bestimmen, woher die Daten stammen.
   
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Fenster für Desktopverbindung](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Wenn Sie zum ersten Mal eine Verbindung mit diesem Endpunkt herstellen, werden Sie aufgefordert, den Kontoschlüssel anzugeben. Für Ihr eigenes Konto können Sie den Schlüssel aus dem Feld **Primärschlüssel** auf dem Blatt **[Schreibgeschützte Schlüssel](manage-account.md#keys)** des Azure-Portals abrufen. Für das Demokonto lautet der Schlüssel `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Geben Sie den richtigen Schlüssel ein, und klicken Sie dann auf **Verbinden**.
   
    Es wird empfohlen, den Schlüssel mit Leseberechtigung beim Erstellen von Berichten zu verwenden.  Dadurch wird verhindert, dass der Hauptschlüssel unnötig potenziellen Sicherheitsrisiken ausgesetzt wird. Der Schlüssel mit Leseberechtigung ist auf dem Blatt [Schlüssel](manage-account.md#keys) des Azure-Portals verfügbar. Alternativ können Sie die oben angegebenen Informationen für das Demokonto verwenden.
   
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Kontoschlüssel](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > Wenn Sie eine Fehlermeldung erhalten, die besagt, dass die angegebene Datenbank nicht gefunden wurde, finden Sie weitere Informationen in der Problemumgehung zu diesem [Power BI-Problem](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. Wenn das Konto erfolgreich verbunden wurde, wird der **Navigator** angezeigt.  Der **Navigator** zeigt eine Liste der Datenbanken für das Konto an.
10. Klicken Sie auf die Datenbank, aus der die Daten für den Bericht stammen sollen, und erweitern Sie sie. Wenn Sie das Demokonto nutzen, wählen Sie **volcanodb** aus.   
11. Wählen Sie jetzt eine Sammlung aus, aus der Sie die Daten abrufen möchten. Wenn Sie die Demokonto verwenden, wählen Sie **volcano1**aus.
    
    Im Vorschaubereich wird eine Liste der **Datensatz** -Elemente angezeigt.  Ein Dokument wird mit dem Typ **Datensatz** in Power BI dargestellt. Auch ein geschachtelter JSON-Block innerhalb eines Dokuments ist ein **Datensatz**.
    
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Navigatorfenster](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Klicken Sie auf **Bearbeiten**, um den Abfrage-Editor in einem neuen Fenster zu starten und die Daten zu transformieren.

## <a name="flattening-and-transforming-json-documents"></a>Vereinfachen und Transformieren von JSON-Dokumenten
1. Wechseln Sie zum Abfrage-Editor-Fenster von Power BI, in dem im mittleren Bereich die Spalte **Dokument** angezeigt wird.
   ![Power BI Desktop – Abfrage-Editor](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Klicken Sie auf das Erweiterungssteuerelement rechts in der Spaltenüberschrift **Dokument**.  Das Kontextmenü mit einer Liste von Feldern wird angezeigt.  Wählen Sie die Felder aus, die Sie für Ihren Bericht benötigen, z.B. „Volcano Name“, „Country“, „Region“, „Location“, „Elevation“, „Type“, „Status“ und „Last Known Eruption“. Klicken Sie dann auf **OK**.
   
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Erweitern von Dokumenten](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. Im mittleren Bereich wird eine Vorschau des Ergebnisses mit den ausgewählten Feldern angezeigt.
   
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Einfügen von Ergebnissen](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. In diesem Beispiel ist die Eigenschaft „Location“ ein GeoJSON-Block in einem Dokument.  Wie Sie sehen können, wird „Location“ mit dem Typ **Datensatz** in Power BI Desktop dargestellt.  
5. Klicken Sie rechts neben der Spaltenüberschrift „Location“ auf das Erweiterungssteuerelement.  Das Kontextmenü mit Typ- und Koordinatenfeldern wird angezeigt.  Wir wählen das Koordinatenfeld aus und klicken auf **OK**.
   
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Standortdatensatz](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. Im mittleren Bereich wird nun eine Spalte „coordinates“ mit dem Typ **Liste** angezeigt.  Wie am Anfang des Tutorials dargestellt, weisen die GeoJSON-Daten in diesem Tutorial den Typ „Punkt“ auf. Die Werte für den Breiten- und Längengrad wurden im Koordinatenarray aufgezeichnet.
   
    Das Element „coordinates[0]“ stellt den Längengrad und „coordinates[1]“ den Breitengrad dar.
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Koordinatenliste](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Um das Koordinatenarray zu vereinfachen, erstellen wir eine **benutzerdefinierte Spalte** mit dem Namen „LatLong“.  Wählen Sie das Menüband **Spalte hinzufügen** aus, und klicken Sie auf **Benutzerdefinierte Spalte hinzufügen**.  Das Fenster **Benutzerdefinierte Spalte hinzufügen** wird angezeigt.
8. Geben Sie einen Namen für die neue Spalte an, z. B. „LatLong“.
9. Geben Sie dann die benutzerdefinierte Formel für die neue Spalte ein.  In unserem Beispiel verketten wir die Werte für Breiten- und Längengrad, getrennt durch ein Komma, wie unten dargestellt, mit der folgenden Formel: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Klicken Sie auf **OK**.
   
    Weitere Informationen zu DAX (Data Analysis Expressions), einschließlich DAX-Funktionen, finden Sie unter [DAX-Grundlagen in Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Hinzufügen einer benutzerdefinierten Spalte](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)
10. Jetzt wird im mittleren Bereich die neue LatLong-Spalte mit den Werten für Breiten- und Längengrade, die durch ein Komma getrennt sind, angezeigt.
    
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Benutzerdefinierte „LatLong“-Spalte](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Wenn Sie in der neuen Spalte einen Fehler erhalten, stellen Sie sicher, dass die unter „Abfrageeinstellungen“ angewendeten Schritte der folgenden Abbildung entsprechen:
    
    ![Die angewendeten Schritte sollten lauten: Quelle, Navigation, Dokument erweitert, Dokumentspeicherort erweitert, Benutzerdefiniertes Element hinzugefügt](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Wenn die Schritte unterschiedlich sind, löschen Sie die zusätzlichen Schritte, und versuchen Sie erneut, die benutzerdefinierte Spalte hinzuzufügen. 
11. Jetzt ist das Vereinfachen der Daten in ein Tabellenformat abgeschlossen.  Sie können sämtliche im Abfrage-Editor verfügbare Funktionen zum Formen und Transformieren von Daten in Cosmos DB nutzen.  Wenn Sie das Beispiel verwenden, ändern Sie den Datentyp für „Elevation“ in **Ganze Zahl**, indem Sie den **Datentyp** im Menüband **Start** ändern.
    
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Ändern des Spaltentyps](./media/powerbi-visualize/power_bi_connector_pbichangetype.png)
12. Klicken Sie auf **Schließen und übernehmen** , um das Datenmodell zu speichern.
    
    ![Power BI-Tutorial für den Power BI-Connector für Azure Cosmos DB – Schließen und Übernehmen](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Erstellen der Berichte
In der Berichtsansicht von Power BI Desktop können Sie Berichte erstellen, um Daten zu visualisieren.  Sie können Berichte erstellen, indem Sie Felder in den Zeichenbereich **Bericht** ziehen.

![Power BI Desktop-Berichtsansicht – Power BI-Connector](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

In der Berichtsansicht wird Folgendes angezeigt:

1. Der Bereich **Felder** , in dem eine Liste von Datenmodellen mit Feldern, die Sie für Ihre Berichte verwenden können, angezeigt wird.
2. Der Bereich **Visualisierungen** . Ein Bericht kann eine einzelne oder mehrere Visualisierungen enthalten.  Wählen Sie die zu Ihren Bedürfnissen passenden Visualisierungstypen im Bereich **Visualisierungen** aus.
3. Der Zeichenbereich **Bericht** , in dem Sie die visuelle Struktur für den Bericht erstellen.
4. Die Seite **Bericht** . Sie können mehrere Berichtsseiten in Power BI Desktop hinzufügen.

Das folgende Beispiel zeigt die grundlegenden Schritte zum Erstellen eines einfachen interaktiven Berichts mit einer Kartenansicht.

1. In unserem Beispiel erstellen wir eine Kartenansicht, die den Standort der einzelnen Vulkane anzeigt.  Klicken Sie im Bereich **Visualisierungen** auf den Visualisierungstyp für Karten, wie im Screenshot oben dargestellt.  Der Visualisierungstyp für Karten wird im Zeichenbereich **Bericht** angezeigt.  Zudem wird im Bereich **Visualisierung** ein Satz von Eigenschaften angezeigt, die zum Visualisierungstyp für Karten gehören.
2. Ziehen Sie jetzt das Feld „LatLong“ aus dem Bereich **Felder** auf die Eigenschaft **Standort** im Bereich **Visualisierungen**.
3. Ziehen Sie dann das Feld „Volcano Name“ auf die Eigenschaft **Legende** .  
4. Ziehen Sie anschließend das Feld „Elevation“ auf die Eigenschaft **Größe** .  
5. Jetzt sollte eine Karte mit einer Reihe von Blasen angezeigt werden. Die Blasen markieren den Standort der einzelnen Vulkane, und die Größe der Blasen korreliert mit der Höhe (Elevation) der Vulkane.
6. Sie haben jetzt einen einfachen Bericht erstellt.  Sie können den Bericht weiter anpassen, indem Sie weitere Visualisierungen hinzufügen.  In unserem Fall haben wir einen Datenschnitt für den Vulkantyp hinzugefügt, um den Bericht interaktiv zu gestalten.  
   
    ![Screenshot des Power BI Desktop-Abschlussberichts nach Abschluss des Power BI-Tutorials für Azure Cosmos DB](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

## <a name="publish-and-share-your-report"></a>Veröffentlichen und Freigeben des Berichts
Zum Freigeben des Berichts müssen Sie über ein Konto in PowerBI.com verfügen.

1. Klicken Sie in Power BI Desktop auf das Menüband **Start** .
2. Klicken Sie auf **Veröffentlichen**.  Sie werden aufgefordert, den Benutzernamen und das Kennwort für Ihr PowerBI.com-Konto einzugeben.
3. Sobald die Anmeldeinformationen authentifiziert wurden, wird der Bericht im ausgewählten Ziel veröffentlicht.
4. Klicken Sie auf **'PowerBITutorial.pbix' in Power BI öffnen** , um Ihren Bericht auf PowerBI.com anzuzeigen und freizugeben.
   
    ![Erfolgreiches Veröffentlichen in Power BI! Öffnen des Tutorials in Power BI](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Erstellen eines Dashboards auf PowerBI.com
Nun, da Sie einen Bericht haben, können sie ihn auf PowerBi.com freigeben.

Beim Veröffentlichen Ihres Berichts aus Power BI Desktop auf „PowerBi.com“ werden in Ihrem PowerBi.com-Mandanten ein **Bericht** und ein **Dataset** generiert. Nachdem Sie beispielsweise einen Bericht namens **PowerBITutorial** auf „PowerBI.com“ veröffentlicht haben, wird „PowerBITutorial“ sowohl im Abschnitt **Berichte** als auch im Abschnitt **Datasets** auf „PowerBI.com“ angezeigt.

   ![Screenshot des neuen Berichts und Datasets auf PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

Zum Erstellen eines Dashboards, das freigegeben werden kann, klicken Sie in Ihrem PowerBi.com-Bericht auf die Schaltfläche **Live-Seite anheften** .

   ![Screenshot des neuen Berichts und Datasets auf PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Führen Sie dann die Anweisungen unter [Anheften einer Kachel aus einem Bericht](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) aus, um ein neues Dashboard zu erstellen. 

Sie können auch Ad-hoc-Änderungen am Bericht durchführen, ehe Sie ein Dashboard erstellen. Es wird allerdings empfohlen, dass Sie Power BI Desktop verwenden, um die Änderungen vorzunehmen und den Bericht erneut auf PowerBi.com zu veröffentlichen.

## <a name="refresh-data-in-powerbicom"></a>Aktualisieren von Daten auf PowerBI.com
Es gibt zwei Möglichkeiten zum Aktualisieren von Daten: ad hoc und geplant.

Für eine Ad-hoc-Aktualisierung klicken Sie auf die Auslassungszeichen neben dem **Dataset**, z.B. „PowerBITutorial“. Daraufhin sollte eine Liste mit Aktionen einschließlich **Jetzt aktualisieren**angezeigt werden. Klicken Sie zum Aktualisieren der Daten auf **Jetzt aktualisieren** .

![Screenshot von „Jetzt aktualisieren“ auf PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

Für eine geplante Aktualisierung führen Sie die folgenden Schritte aus.

1. Klicken Sie in der Aktionsliste auf **Aktualisierung planen** . 

    ![Screenshot von „Aktualisierung planen“ auf „PowerBI.com“](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. Erweitern Sie auf der Seite **Einstellungen** den Eintrag **Datenquellen-Anmeldeinformationen**. 
3. Klicken Sie auf **Anmeldeinformationen bearbeiten**. 
   
    Das Popupfenster „Konfigurieren“ wird angezeigt. 
4. Geben Sie den Schlüssel zum Herstellen einer Verbindung mit dem Cosmos DB-Konto für dieses Dataset ein, und klicken Sie dann **Anmelden**. 
5. Erweitern Sie **Aktualisierung planen** , und richten Sie den gewünschten Zeitplan zur Aktualisierung des Datasets ein. 
6. Klicken Sie auf **Übernehmen**. Damit ist das Einrichten der geplanten Aktualisierung abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Power BI finden Sie unter [Erste Schritte mit Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* Weitere Informationen zu Cosmos DB finden Sie auf der [Startseite der Azure Cosmos DB-Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/).


