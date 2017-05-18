Sie können jetzt den Daten-Explorer verwenden, um eine Sammlung zu erstellen und Daten zu Ihrer Datenbank hinzuzufügen. 

1. Klicken Sie im Azure-Portal im Navigationsmenü auf **Daten-Explorer**. 
2. Klicken Sie auf dem Blatt „Daten-Explorer“ auf **Neue Sammlung**, und füllen Sie dann die Seite mithilfe der folgenden Informationen aus.

    ![Daten-Explorer im Azure-Portal](./media/cosmosdb-create-collection/azure-cosmosdb-data-explorer.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Datenbank-ID|Items|Die ID Ihrer neuen Datenbank. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/ \ # ?` noch nachgestellte Leerzeichen enthalten.
    Sammlungs-ID|ToDoList|Die ID Ihrer neuen Sammlung. Für Sammlungsnamen gelten dieselben Zeichenanforderungen wie bei Datenbank-IDs.
    Speicherkapazität| Fixed (10 GB)|Behalten Sie den Standardwert bei. Dies ist die Speicherkapazität der Datenbank.
    Durchsatz|400 RUs|Behalten Sie den Standardwert bei. Sie können den Durchsatz später zentral hochskalieren, wenn Sie Latenzen reduzieren möchten.
    Partitionsschlüssel|/userid|Ein Partitionsschlüssel, der Daten gleichmäßig auf alle Partitionen verteilt. Die Auswahl des richtigen Partitionsschlüssels ist wichtig für die Erstellung einer leistungsfähigen Sammlung. Weitere Informationen finden Sie unter [Entwerfen für Partitionierung](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    



3. Wenn das Formular ausgefüllt ist, klicken Sie auf **OK**.