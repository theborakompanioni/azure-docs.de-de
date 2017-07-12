Sie können jetzt den Daten-Explorer verwenden, um eine Tabelle zu erstellen und Daten zu Ihrer Datenbank hinzuzufügen. 

1. Klicken Sie im Azure-Portal im Navigationsmenü auf **Daten-Explorer (Vorschau)**. 
2. Klicken Sie auf dem Blatt „Daten-Explorer“ auf **Neue Tabelle**, und füllen Sie dann die Seite mit den folgenden Informationen aus.

    ![Daten-Explorer im Azure-Portal](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Tabellen-ID|sample-table|Die ID Ihrer neuen Tabelle. Für Tabellennamen gelten dieselben Zeichenanforderungen wie für Datenbank-IDs. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/ \ # ?` noch nachgestellte Leerzeichen enthalten.
    Speicherkapazität| 10 GB|Behalten Sie den Standardwert bei. Dies ist die Speicherkapazität der Datenbank.
    Durchsatz|400 RUs|Behalten Sie den Standardwert bei. Sie können den [Durchsatz](../articles/cosmos-db/request-units.md) später zentral hochskalieren, wenn Sie Latenzen reduzieren möchten.
    RU/Minute|Aus|Verwenden Sie den Standardwert. Sie können das Feature [RU/Minute](../articles/cosmos-db/request-units-per-minute.md) später aktivieren, wenn Sie Workloads mit Lastspitzen verarbeiten müssen.

3. Wenn das Formular ausgefüllt ist, klicken Sie auf **OK**.