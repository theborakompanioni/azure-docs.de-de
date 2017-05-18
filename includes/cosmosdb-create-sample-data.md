Sie können nun mithilfe des Daten-Explorers Daten zu einer neuen Sammlung hinzufügen.

1. Im Daten-Explorer wird die neue Datenbank im Bereich „Sammlungen“ angezeigt. Erweitern Sie die **Items**-Datenbank, die **ToDoList**-Sammlung, klicken Sie auf **Dokumente**, und klicken Sie anschließend auf **Neue Dokumente**. 

   ![Neue Dokumente im Daten-Explorer im Azure-Portal erstellen](./media/cosmosdb-create-sample-data/azure-cosmosdb-data-explorer-emulator-new-document.png)
  
2. Fügen Sie jetzt einige neue Dokumente zu der Sammlung mit der folgenden Struktur hinzu, in der Sie eindeutige Werte für die ID jedes Dokuments einfügen und die anderen Eigenschaften nach Bedarf ändern. Ihre neuen Dokumente können jede gewünschte Struktur aufweisen, da Azure Cosmos DB kein Schema für Ihre Daten vorgibt.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries."
     }
     ```

     Sie können jetzt zum Abrufen Ihrer Daten Abfragen im Daten-Explorer verwenden. Standardmäßig verwendet der Daten-Explorer „SELECT * FROM c“ zum Abrufen aller Dokumente in der Sammlung, aber Sie können das zu „SELECT * FROM c ORDER BY c.name ASC“ ändern, um alle Dokumente in alphabetischer aufsteigender Reihenfolge der Eigenschaft „Name“ zurückzugeben. 
 
     Der Daten-Explorer kann auch zum Erstellen von gespeicherten Prozeduren, UDFs und Triggern verwendet werden, um serverseitige Geschäftslogik auszuführen. Der Daten-Explorer stellt den gesamten integrierten programmgesteuerten Datenzugriff in den APIs zur Verfügung, bietet jedoch einfachen Zugriff auf Ihre Daten im Azure-Portal.