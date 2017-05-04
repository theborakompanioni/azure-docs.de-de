### <a name="prerequisites"></a>Voraussetzungen
* Ein [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)-Konto  

Bevor Sie Ihr SMTP-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für die Verbindung mit Ihrem SMTP-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.  

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem SMTP-Konto:  

1. Zum Herstellen einer Verbindung mit SMTP wählen Sie im Designer für Logik-Apps in der Dropdownliste **Verwaltete Microsoft-APIs anzeigen** aus, und geben Sie dann *SMTP* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus:   
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Wenn Sie noch keine Verbindungen mit SMTP hergestellt haben, werden Sie aufgefordert, Ihre SMTP-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen wird Ihre Logik-App autorisiert, eine Verbindung mit dem SMTP-Konto herzustellen und auf dessen Daten zuzugreifen:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit den anderen Schritten in Ihrer Logik-App fortfahren können:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  

