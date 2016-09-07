Nachdem die Datensätze für Ihren Domänennamen weitergegeben wurden, müssen Sie sie Ihrer Web-App zuweisen. Verwenden Sie die folgenden Schritte, um die Domänennamen mit dem Webbrowser zu aktivieren.

> [AZURE.NOTE] Es kann einige Zeit dauern, bis die TXT-Einträge, die in den vorherigen Schritten erstellt wurden, über das DNS-System weitergegeben werden. Sie können den Domänennamen Ihrer Web-App erst hinzufügen, nachdem der TXT-Eintrag weitergegeben wurde. Wenn Sie einen A-Eintrag verwenden, können Sie den Domänennamen des A-Eintrags erst dann der Web-App hinzufügen, wenn der im vorherigen Schritt erstellte TXT-Eintrag weitergegeben wurde.
>
> Mithilfe eines Diensts wie <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der TXT-Eintrag verfügbar ist.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in Ihrem Browser.

2. Klicken Sie auf der Registerkarte **Web-Apps** auf den Namen der Web-App, und wählen Sie **Benutzerdefinierte Domänen** aus.

	![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Klicken Sie auf dem Blatt **Benutzerdefinierte Domänen** auf **Hostnamen hinzufügen**.
	
4. Verwenden Sie die Textfelder **Hostname**, um die Domänennamen einzugeben, die dieser Web-App zugeordnet werden sollen.

	![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klicken Sie auf **Überprüfen**.

7.  Nach dem Klicken auf **Überprüfen** startet Azure den Workflow zur Domänenüberprüfung. Dieser Workflow überprüft den Domänenbesitz sowie die Verfügbarkeit des Hostnamens und meldet die erfolgreiche Durchführung oder erstellt einen detaillierten Fehlerbericht mit Empfehlungen, wie der Fehler behoben werden kann.

Jetzt sollten Sie den benutzerdefinierten Domänennamen in Ihren Browser eingeben können und und auf diese Weise erfolgreich zu Ihrer Web-App gelangen.

<!---HONumber=AcomDC_0824_2016-->