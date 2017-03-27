
1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

2. Klicken Sie auf **+Neu** > **Compute** > **Funktionen-App**, wählen Sie Ihr **Abonnement** aus, geben Sie einen eindeutigen **App-Namen** zur Identifizierung Ihrer Funktionen-App ein, und geben Sie anschließend folgende Einstellungen an:
   
   * **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die neue Ressourcengruppe ein. 
   * **[Hostingplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**. Mögliche Pläne: 
     * **Verbrauchsplan**: Der Standardplantyp für Azure Functions. Wenn Sie einen Verbrauchsplan auswählen, müssen Sie auch den **Standort** und die **Speicherbelegung** (in MB) festlegen. Informationen zu den Auswirkungen der Speicherbelegung auf die Kosten finden Sie unter [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/). 
     * **App Service-Plan**: Bei Verwendung eines App Service-Plans müssen Sie einen **App Service-Plan/Standort** erstellen oder einen bereits vorhandenen App Service-Plan/Standort auswählen. Diese Einstellungen bestimmen den [Standort, die Funktionen, die Kosten und die Computeressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer App.  
   * **Speicherkonto**: Für jede Funktionen-App ist ein Speicherkonto erforderlich. Sie können ein vorhandenes Speicherkonto auswählen oder ein [Speicherkonto erstellen](../articles/storage/storage-create-storage-account.md#create-a-storage-account). 
     
    ![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

3. Klicken Sie auf **Erstellen** , um die neue Funktionen-App bereitzustellen.  