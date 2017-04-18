
1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Klicken Sie auf **Compute** > **Funktionen-App**, wählen Sie Ihr **Abonnement** aus, geben Sie einen eindeutigen **App-Namen** zur Identifizierung Ihrer Funktionen-App ein, und geben Sie anschließend folgende Einstellungen an:
   
   * **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die neue Ressourcengruppe ein. 
   * **[Hostingplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**. Mögliche Pläne: 
     * **Verbrauchsplan**: Der Standardplantyp für Azure Functions. Beim Auswählen eines Verbrauchsplans müssen Sie auch den **Standort** wählen.  
     * **App Service-Plan**: Bei Verwendung eines App Service-Plans müssen Sie einen **App Service-Plan/Standort** erstellen oder einen bereits vorhandenen App Service-Plan/Standort auswählen. Diese Einstellungen bestimmen den [Standort, die Funktionen, die Kosten und die Computeressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer App.  
   * **Speicherkonto**: Für jede Funktionen-App ist ein Speicherkonto erforderlich. Sie können ein vorhandenes Speicherkonto auswählen oder ein [Speicherkonto erstellen](../articles/storage/storage-create-storage-account.md#create-a-storage-account). 
     
    ![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

3. Klicken Sie auf **Erstellen** , um die neue Funktionen-App bereitzustellen.  
