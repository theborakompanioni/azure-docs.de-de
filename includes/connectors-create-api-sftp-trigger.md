Fügen wir jetzt einen Trigger hinzu.

1. Geben Sie im Suchfeld des Designers für Logik-Apps die Zeichenfolge *SFTP* ein, und wählen Sie anschließend den Trigger **SFTP – Wenn eine Datei hinzugefügt oder geändert wird** aus.   
   ![SFTP-Trigger – Abbildung 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. Das Steuerelement **Wenn eine Datei hinzugefügt oder geändert wird** wird geöffnet.  
   ![SFTP-Trigger – Abbildung 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Wählen Sie rechts im Steuerelement die drei Punkte (**...**) aus. Die Ordnerauswahl wird geöffnet.  
   ![SFTP-Trigger – Abbildung 3](./media/connectors-create-api-sftp/action-1.png)  
4. Wählen Sie **SFTP** aus, um den Stammordner als den Ordner auszuwählen, der auf neue oder geänderte Dateien überwacht werden soll. Beachten Sie, dass der Stammordner nun im Steuerelement **Ordner** angezeigt wird.  
   ![SFTP-Trigger – Abbildung 4](./media/connectors-create-api-sftp/action-2.png)   

Sie haben jetzt Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet, wenn im angegebenen SFTP-Ordner eine Datei geändert oder erstellt wird. 

> [!NOTE]
> Damit eine Logik-App funktionsfähig ist, muss sie mindestens einen Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt aus, um eine Aktion hinzuzufügen.  
> 
> 

