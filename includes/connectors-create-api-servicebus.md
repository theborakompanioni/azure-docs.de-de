### Voraussetzungen

- Ein [ServiceBus](https://azure.microsoft.com/services/service-bus/)-Konto  


Bevor Sie Ihr ServiceBus-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für die Verbindung mit Ihrem ServiceBus-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem ServiceBus-Konto:
1. Um eine Verbindung mit ServiceBus herzustellen, wählen Sie im Logik-App-Designer in der Dropdownliste **Verwaltete Microsoft-APIs anzeigen** aus, und geben Sie dann *ServiceBus* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus: ![](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Wenn Sie noch keine Verbindungen mit ServiceBus hergestellt haben, werden Sie aufgefordert, Ihre ServiceBus-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen kann Ihre Logik-App eine Verbindung mit dem ServiceBus-Konto herstellen und auf dessen Daten zugreifen: ![](./media/connectors-create-api-servicebus/servicebus-2.png)  
3. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit anderen Schritten in Ihrer Logik-App fortfahren können: ![](./media/connectors-create-api-servicebus/servicebus-3.png)   

<!---HONumber=AcomDC_0525_2016-->