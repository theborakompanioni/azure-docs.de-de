Sie können nun mithilfe des Daten-Explorer-Tools im Azure-Portal eine Datenbank und eine Sammlung erstellen. 

1. Klicken Sie im Azure-Portal im linken Navigationsmenü auf **Daten-Explorer (Vorschau)**. 

2. Klicken Sie auf dem Blatt **Daten-Explorer (Vorschau)** auf **Neue Sammlung**, und geben Sie dann die folgenden Informationen an:

    ![Blatt „Daten-Explorer“ im Azure-Portal](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Datenbank-ID|Aufgaben|Der Name Ihrer neuen Datenbank. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder /, \\, #, ? noch nachgestellte Leerzeichen enthalten.
    Sammlungs-ID|Items|Der Name Ihrer neuen Sammlung. Für Sammlungsnamen gelten dieselben Zeichenanforderungen wie bei Datenbank-IDs.
    Speicherkapazität| Fixed (10 GB)|Verwenden Sie den Standardwert. Dieser Wert gibt die Speicherkapazität der Datenbank an.
    Durchsatz|400 RU|Verwenden Sie den Standardwert. Sie können den Durchsatz später zentral hochskalieren, wenn Sie Wartezeiten reduzieren möchten.
    RU/Minute|Aus|Behalten Sie den Standardwert bei. Wenn Sie später Workloads mit Lastspitzen verarbeiten müssen, können Sie dann das Feature [RU/Minute](../articles/cosmos-db/request-units-per-minute.md) aktivieren.
    Partitionsschlüssel|/category|Ein Partitionsschlüssel, der Daten gleichmäßig auf alle Partitionen verteilt. Die Auswahl des richtigen Partitionsschlüssels ist wichtig für die Erstellung einer leistungsfähigen Sammlung. Weitere Informationen finden Sie unter [Entwerfen für Partitionierung](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    
3. Klicken Sie nach dem Ausfüllen des Formulars auf **OK**.

Im Daten-Explorer werden die neue Datenbank und die neue Sammlung angezeigt. 