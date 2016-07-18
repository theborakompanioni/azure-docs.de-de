
In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie eine VM zwischen Abonnements verschieben. Dies kann nützlich sein, wenn Sie eine VM ursprünglich in einem persönlichen Abonnement erstellt haben und sie nun in das Abonnement Ihres Unternehmens verschieben möchten, damit Sie weiterarbeiten können.

> [AZURE.NOTE] Im Rahmen der Verschiebung werden neue Ressourcen-IDs erstellt. Nachdem die VM verschoben wurde, müssen Sie Ihre Tools und Skripts aktualisieren, damit die neuen Ressourcen-IDs verwendet werden.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Durchsuchen** > **Virtuelle Computer**, und wählen Sie in der Liste die zu verschiebende VM aus.
	
	![Screenshot des Abschnitts „Essentials“, in dem Sie auf das Stiftsymbol klicken, um das Blatt „Ressourcen verschieben“ zu öffnen.](./media/virtual-machines-common-move-vm/move-button.png)
	
3. Klicken Sie im Abschnitt **Essentials** auf das Stiftsymbol für **Abonnement ändern** neben dem Abonnementnamen. Das Blatt **Ressourcen verschieben** wird geöffnet.
	
	![Screenshot des Blatts „Ressourcen verschieben“](./media/virtual-machines-common-move-vm/move.png)
	
4. Wählen Sie alle Ressourcen aus, die verschoben werden sollen. In den meisten Fällen sollten Sie alle aufgeführten optionalen Ressourcen verschieben.
5. Wählen Sie das **Abonnement** aus, in das Sie die VM verschieben möchten.
6. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen.
7. Bestätigen Sie anschließend den Hinweis, dass neue Ressourcen-IDs erstellt werden und nach dem Verschieben für die VM verwendet werden müssen, und klicken Sie auf **OK**.



## Nächste Schritte

Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../articles/resource-group-move-resources.md).

<!---HONumber=AcomDC_0706_2016-->