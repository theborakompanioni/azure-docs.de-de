
1. Klicken Sie auf dem Blatt **Hybridverbindungen** auf die Hybridverbindung, die Sie gerade erstellt haben, und klicken Sie dann auf **Listenereinrichtung**.
   
    ![Listener-Einrichtung](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. Der Fensterbereich **Hybrid connection properties** wird geöffnet. Wählen Sie unter **Manager für lokale Hybridverbindungen** die Option **Herunterladen und manuell konfigurieren** aus, speichern Sie das heruntergeladene Paket „HybridConnectionManager.msi“, und kopieren Sie die Gatewayverbindungszeichenfolge.
   
    ![Click here to install](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Geben Sie an der Administratoreingabeaufforderung den folgenden Befehl ein, um das Installationsprogramm zu starten:
   
        start HybridConnectionManager.msi
4. Klicken Sie nach Ausführung des Installationsprogramms auf **Nicht jetzt**, navigieren Sie zum Ordner „%ProgramFiles%\Microsoft\HybridConnectionManager“, führen Sie die Datei „HCMConfigWizard.exe“ aus, und klicken Sie im Dialogfeld **Benutzerkontensteuerung** auf **Ja**.
5. Fügen Sie die Hybridverbindungszeichenfolge ein, die Sie zuvor kopiert haben, und klicken Sie auf **OK**. 
   
    ![Installation](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Sobald die Installation abgeschlossen ist, klicken Sie auf **Close**.
   
    ![Klicken Sie auf "Schließen"](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Auf dem Blatt **Hybridverbindungen** wird in der Spalte **Status** jetzt **Verbunden** angezeigt. 
   
    ![Status Verbunden](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

