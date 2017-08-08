### <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein [Service Bus](https://azure.microsoft.com/services/service-bus/)-Konto.  

Bevor Sie Ihr Azure Service Bus-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für Verbindungen mit Ihrem Service Bus-Konto autorisieren. Diesen Schritt können Sie ganz einfach innerhalb Ihrer Logik-App im Azure-Portal durchführen.  

Hier erfahren Sie, wie Sie Ihre Logik-App für Verbindungen mit Ihrem Service Bus-Konto autorisieren:  

1. Wählen Sie zum Erstellen einer Service Bus-Verbindung im Logik-App-Designer in der Dropdownliste die Option **Von Microsoft verwaltete APIs anzeigen** aus. Geben Sie dann **Service Bus** in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus:  
    ![Service Bus-Verbindung – Abbildung 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Wenn Sie noch keine Verbindungen mit Service Bus hergestellt haben, werden Sie aufgefordert, Ihre Service Bus-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen wird Ihre Logik-App autorisiert, eine Verbindung mit dem Service Bus-Konto herzustellen und auf dessen Daten zuzugreifen. Der Service Bus-Connector benötigt die Verbindungszeichenfolge für den Service Bus-Namespace. Außerdem werden **Verwaltungsberechtigungen** benötigt. Um zu ermitteln, ob die Verbindungszeichenfolge für den Namespace oder eine bestimmte Entität gilt, prüfen Sie, ob sie den Parameter `EntityPath` enthält. Falls ja, handelt es sich nicht um die richtige Verbindungszeichenfolge für eine Logik-App.  
    ![Service Bus-Verbindungszeichenfolge](./media/connectors-create-api-servicebus/connectionstring.png)
3. Nachdem Sie die Verbindungszeichenfolge für den Namespace erhalten haben, können Sie sie für die API-Verbindung in Logik-Apps verwenden:  
    ![Service Bus-Verbindung – Abbildung 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit anderen Schritten in Ihrer Logik-App fortfahren können:  
    ![Service Bus-Verbindung – Abbildung 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

