Sie können jetzt den Daten-Explorer verwenden, um einen Graph-Container zu erstellen und Daten zu Ihrer Datenbank hinzuzufügen. 

1. Klicken Sie im Azure-Portal im Navigationsmenü auf **Daten-Explorer**. 
2. Klicken Sie auf dem Blatt „Daten-Explorer“ auf **Neue Tabelle**, und füllen Sie dann die Seite mit den folgenden Informationen aus.

    ![Daten-Explorer im Azure-Portal](./media/cosmosdb-create-table/azure-cosmosdb-data-explorer.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Datenbank-ID|sample-database|Die ID Ihrer neuen Datenbank. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/ \ # ?` noch nachgestellte Leerzeichen enthalten.
    Tabellen-ID|sample-table|Die ID Ihrer neuen Tabelle. Für Tabellennamen gelten dieselben Zeichenanforderungen wie für Datenbank-IDs.
    Speicherkapazität| 10 GB|Behalten Sie den Standardwert bei. Dies ist die Speicherkapazität der Datenbank.
    Durchsatz|400 RUs|Behalten Sie den Standardwert bei. Sie können den Durchsatz später zentral hochskalieren, wenn Sie Latenzen reduzieren möchten.

3. Wenn das Formular ausgefüllt ist, klicken Sie auf **OK**.