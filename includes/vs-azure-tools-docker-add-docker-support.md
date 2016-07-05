1. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen > Docker-Unterstützung** aus.

    ![Kontextmenü für Docker-Unterstützung hinzufügen](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Durch das Hinzufügen von Docker-Unterstützung zu einem ASP.NET 5-Webprojekt werden dem Projekt mehrere Docker-verwandte Dateien hinzugefügt, darunter Docker-Compose-Dateien, Windows PowerShell-Bereitstellungsskripts und Docker-Eigenschaftsdateien.

    ![Zum Projekt hinzugefügte Docker-Dateien](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE] Bei Verwendung der [Betaversion von Docker für Windows](https://beta.docker.com) öffnen Sie „Properties\\Docker.props“. Entfernen Sie den Standardwert, und starten Sie Visual Studio neu, um die Einstellung zu übernehmen.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=="></DockerMachineName>
> ```

<!---HONumber=AcomDC_0622_2016-->