### <a name="prerequisites"></a>Voraussetzungen
* Ein [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)-Konto  

Bevor Sie Ihr SFTP-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für Verbindungen mit Ihrem SFTP-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach aus Ihrer Logik-App heraus im Azure-Portal durchführen.  

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem SFTP-Konto:  

1. Um eine Verbindung mit SFTP herzustellen, wählen Sie im Logik-App-Designer in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann *SFTP* in das Suchfeld ein. Wählen Sie den Trigger **SFTP - When a file is added or modified** (SFTP - Wenn eine Datei hinzugefügt oder geändert wird) aus:  
   ![SFTP-Onlineverbindung – Abbildung 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Wenn Sie noch keine Verbindungen mit SFTP hergestellt haben, werden Sie aufgefordert, Ihre SFTP-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen wird Ihre Logik-App autorisiert, eine Verbindung mit dem SFTP-Konto herzustellen und auf dessen Daten zuzugreifen:  
   ![SFTP-Onlineverbindung – Abbildung 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit den anderen Schritten in Ihrer Logik-App fortfahren können:   
   ![SFTP-Onlineverbindung – Abbildung 3](./media/connectors-create-api-sftp/sftp-3.png) 

