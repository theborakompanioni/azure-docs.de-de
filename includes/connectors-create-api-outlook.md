## <a name="connect-to-outlookcom"></a>Verbinden mit Outlook.com
### <a name="prerequisites"></a>Voraussetzungen
* Ein Outlook.com-Konto

Bevor Sie Ihr Outlook.com-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für Verbindungen mit Ihrem Outlook.com-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen. 

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem Outlook.com-Konto:

1. Alle Logik-Apps müssen durch einen Trigger gestartet werden. Deshalb öffnet der Designer nach dem Erstellen Ihrer Logik-App eine Liste mit Triggern, die Sie zum Starten Ihrer Logik-App verwenden können:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Geben Sie „Outlook“ in das Suchfeld ein. Beachten Sie, dass die Liste gefiltert wird, um alle Trigger mit „Outlook“ im Namen anzuzeigen: ![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Wählen Sie **Office 365 Outlook - On new email** aus.   
   Wenn Sie noch keine Verbindungen mit Outlook hergestellt haben, werden Sie aufgefordert, Ihre Outlook.com-Anmeldeinformationen anzugeben. Mithilfe dieser Anmeldeinformationen wird Ihre Logik-App autorisiert, die eine Verbindung herstellen und auf Ihr Outlook.com-Konto zugreifen soll: ![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Geben Sie Ihre Anmeldeinformationen für Outlook an, und melden Sie sich an: ![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   Das ist alles. Sie haben damit eine Verbindung mit Outlook erstellt. Diese Verbindung steht auch für die Verwendung in anderen Logik-Apps zur Verfügung, die Sie erstellen.

