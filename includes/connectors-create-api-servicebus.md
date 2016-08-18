### Voraussetzungen

- Ein [ServiceBus](https://azure.microsoft.com/services/service-bus/)-Konto


Bevor Sie Ihr Azure Service Bus-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für Verbindungen mit Ihrem Service Bus-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach aus Ihrer Logik-App heraus im Azure-Portal durchführen.

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem Service Bus-Konto:

1. Um eine Verbindung mit Service Bus herzustellen, wählen Sie im Designer für Logik-Apps in der Dropdownliste **Verwaltete Microsoft-APIs anzeigen** aus, und geben Sie dann *Service Bus* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus:  
    ![Service Bus-Verbindung – Abbildung 1](./media/connectors-create-api-servicebus/servicebus-1.png)
    
2. Wenn Sie noch keine Verbindungen mit Service Bus hergestellt haben, werden Sie aufgefordert, Ihre Service Bus-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen wird Ihre Logik-App autorisiert, eine Verbindung mit dem Service Bus-Konto herzustellen und auf dessen Daten zuzugreifen. Der Service Bus-Connector benötigt die Verbindungszeichenfolge für den Service Bus-Namespace. Darüber hinaus sind Berechtigungen zum **Verwalten** erforderlich. Um zu ermitteln, ob die Verbindungszeichenfolge für den Namespace oder eine bestimmte Entität gilt, prüfen Sie, ob sie den Parameter `EntityPath` enthält. Dies ist nicht die richtige Verbindungszeichenfolge für eine Logik-App.  
    ![Service Bus-Verbindungszeichenfolge](./media/connectors-create-api-servicebus/connectionstring.png)

1. Nachdem Sie die Verbindungszeichenfolge für den Namespace erhalten haben, können Sie sie für die API-Verbindung in Logik-Apps verwenden.  
    ![Service Bus-Verbindung – Abbildung 2](./media/connectors-create-api-servicebus/servicebus-2.png)

3. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit anderen Schritten in Ihrer Logik-App fortfahren können:  
    ![Service Bus-Verbindung – Abbildung 3](./media/connectors-create-api-servicebus/servicebus-3.png)

<!---HONumber=AcomDC_0727_2016-->
