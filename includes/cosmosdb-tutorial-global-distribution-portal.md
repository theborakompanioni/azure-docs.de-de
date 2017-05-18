
Informationen zur globalen Verteilung von Azure Cosmos DB erhalten Sie in diesem Azure Friday-Video mit Scott Hanselman und Karthik Raman, dem Principal Engineering Manager.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Cosmos DB finden Sie unter [Distribute data globally with Cosmos DB (Globale Verteilung von Daten mit Cosmos DB)](../articles/documentdb/documentdb-distribute-data-globally.md).

## <a id="addregion"></a>Hinzufügen von globalen Datenbankregionen mit dem Azure-Portal
Azure Cosmos DB ist standardmäßig in allen [Azure-Regionen][azureregions] weltweit verfügbar. Nachdem Sie die Standardkonsistenzebene für Ihr Datenbankkonto ausgewählt haben, können Sie dem Konto eine oder mehrere Regionen zuordnen (je nachdem, welche Konsistenzebene Sie ausgewählt haben und welche Anforderungen an eine globale Verteilung bestehen).

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der linken Leiste auf **Azure Cosmos DB**.
2. Wählen Sie auf dem Blatt **Azure Cosmos DB** das zu ändernde Datenbankkonto aus.
3. Klicken Sie auf dem Blatt „Konto“ im Menü auf **Daten global replizieren**.
4. Wählen Sie auf dem Blatt **Daten global replizieren** die Regionen aus, die Sie hinzufügen oder entfernen möchten, indem Sie auf die Regionen in der Karte klicken, und klicken Sie anschließend auf **Speichern**. Für das Hinzufügen von Regionen entstehen Kosten. Weitere Informationen hierzu finden Sie auf der Seite mit [Preisinformationen](https://azure.microsoft.com/pricing/details/documentdb/) sowie im Artikel [Globale Verteilung von Daten mit DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md).
   
    ![Hinzufügen oder Entfernen von Regionen per Klick auf die Regionen auf der Karte][1]
    
Nachdem Sie eine zweite Region hinzugefügt haben, wird auf dem Blatt **Daten lokal replizieren** im Portal die Option **Manuelles Failover** aktiviert. Sie können diese Option verwenden, um den Failovervorgang, zu testen oder die primäre Schreibregion zu ändern. Nachdem Sie eine dritte Region hinzugefügt haben, wird auf dem gleichen Blatt die Option **Failoverprioritäten** aktiviert, sodass Sie die Failoverreihenfolge für Lesevorgänge ändern können.  

### <a name="selecting-global-database-regions"></a>Auswählen von globalen Datenbankregionen
Es gibt zwei gängige Szenarios zum Konfigurieren von mindestens zwei oder mehr Regionen:

1. Übermitteln von niedriger Latenz beim Zugriff auf Daten für Endbenutzer, unabhängig davon, wo sie sich befinden
2. Hinzufügen von regionaler Resilienz für Geschäftskontinuität und Notfallwiederherstellung (BCDR)

Für die Übermittlung von niedriger Latenz für Endbenutzer wird empfohlen, die Anwendung bereitzustellen und Azure Cosmos-DB in den Regionen hinzuzufügen, in denen sich der Benutzer der Anwendung befindet.

Für BCDR empfiehlt es sich, die Regionen basierend auf den Regionspaaren hinzuzufügen, die im Artikel [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare][bcdr] beschrieben werden.

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmosdb-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmosdb-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmosdb-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/documentdb/
