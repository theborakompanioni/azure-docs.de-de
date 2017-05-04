### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein [Azure Blob Storage-Konto](../articles/storage/storage-create-storage-account.md) einschließlich Name und Zugriffsschlüssel des Speicherkontos. Diese Informationen werden in den Eigenschaften des Speicherkontos im Azure-Portal aufgeführt. Erfahren Sie mehr über [Azure Storage](../articles/storage/storage-introduction.md).

Bevor Sie Ihr Azure Blob Storage-Konto in einer Logik-App verwenden, stellen Sie eine Verbindung mit Ihrem Azure Blob Storage-Konto her. Sie können dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.  

Stellen Sie die Verbindung mit Ihrem Azure Blob Storage-Konto mit den folgenden Schritten her:  

1. Erstellen Sie eine Logik-App. Fügen Sie im Designer für Logik-Apps einen Trigger hinzu, und fügen Sie dann eine Aktion hinzu. Wählen Sie in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann in das Suchfeld „Blob“ ein. Wählen Sie eine der Aktionen aus:  
   
    ![Schritt zur Erstellung der Azure Blob Storage-Verbindung](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Wenn Sie zuvor noch keine Verbindungen mit dem Azure-Speicher hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben:   
   
    ![Schritt zur Erstellung der Azure Blob Storage-Verbindung](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Geben Sie die Speicherkontodetails ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Kontoname für Azure-Speicher* |Geben Sie den Namen des Speicherkontos ein. Der Name des Speicherkontos wird im Azure-Portal in den Speichereigenschaften angezeigt. |
   | Zugriffsschlüssel für das Azure-Speicherkonto* |Geben Sie den Schlüssel des Speicherkontos ein. Die Zugriffsschlüssel werden im Azure-Portal in den Speichereigenschaften angezeigt. |
   
    Mit diesen Anmeldeinformationen kann Ihre Logik-App eine Verbindung herstellen und auf Ihre Daten zugreifen. 
4. Klicken Sie auf **Erstellen**.
5. Beachten Sie, dass die Verbindung erstellt wurde. Fahren Sie nun mit den weiteren Schritten in Ihrer Logik-App fort: 
   
    ![Schritt zur Erstellung der Azure Blob Storage-Verbindung](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

