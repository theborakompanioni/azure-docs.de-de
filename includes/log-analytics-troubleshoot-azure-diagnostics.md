### <a name="troubleshoot-azure-diagnostics"></a>Behandeln von Problemen mit Azure Diagnostics

Wenn Sie die folgende Fehlermeldung erhalten, ist der Ressourcenanbieter „Microsoft.insights“ nicht registriert:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Führen Sie im Azure-Portal die folgenden Schritte aus, um den Ressourcenanbieter zu registrieren:

1.  Klicken Sie im linken Navigationsbereich auf *Abonnements*.
2.  Wählen Sie das in der Fehlermeldung angegebene Abonnement aus.
3.  Klicken Sie auf *Ressourcenanbieter*.
4.  Suchen Sie nach dem Anbieter *Microsoft.insights*.
5.  Klicken Sie auf den Link *Registrieren*.

![Registrierung des Ressourcenanbieters „Microsoft.insights“](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Wiederholen Sie nach der Registrierung des Ressourcenanbieters *Microsoft.insights* die Diagnosekonfiguration.


<!--HONumber=Feb17_HO2-->


