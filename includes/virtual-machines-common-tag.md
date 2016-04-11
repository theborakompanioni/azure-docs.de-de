


## Markieren eines virtuellen Computers mittels Vorlagen

Zunächst sehen wir uns das Verwenden von Tags mithilfe von Vorlagen an. [Diese Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-tags-vm) platziert Tags auf den folgenden Ressourcen: Compute (virtueller Computer), Speicher (Storage-Konto) und Netzwerk (öffentliche IP-Adresse, Virtual Network und Netzwerkschnittstelle). (Diese Beispielvorlage gilt für eine Windows-VM, kann aber für eine Linux-VM angepasst werden.)

Klicken Sie unter dem [Vorlagenlink](https://github.com/Azure/azure-quickstart-templates/tree/master/101-tags-vm) auf die Schaltfläche **Bereitstellen in Azure**. Dadurch werden Sie zum [Azure-Portal](https://portal.azure.com/) weitergeleitet, in dem Sie diese Vorlage bereitstellen können.

![Einfache Bereitstellung mit Tags](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Diese Vorlage enthält die folgenden Tags: *Abteilung*, *Anwendung* und *Erstellt von*. Sie können diese Tags direkt in der Vorlage hinzufügen oder bearbeiten, wenn Sie andere Tagnamen wünschen.

![Azure-Tags in einer Vorlage](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Wie Sie sehen können, werden die Tags als Schlüssel-Wert-Paare definiert und durch einen Doppelpunkt (:) getrennt. Die Tags müssen im folgenden Format definiert werden:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Speichern Sie Vorlagendatei nach dem Bearbeiten mit den Tags Ihrer Wahl.

Anschließend können Sie im Abschnitt **Parameter bearbeiten** die Werte für die Tags ausfüllen.

![Bearbeiten von Tags im Azure-Portal](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klicken Sie auf **Erstellen**, um diese Vorlage mit Ihren Tagwerten bereitzustellen.


## Tags über das Portal erstellen

Nachdem Sie Ihre Ressourcen mit Tags erstellt haben, können Sie sie im Portal anzeigen, hinzufügen und löschen.

Wählen Sie das Tag-Symbol, um die Tags anzuzeigen:

![Symbol für Tags im Azure-Portal](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Fügen Sie ein neues Tag über das Portal durch Definieren Ihres eigenen Schlüssels-Wert-Paares hinzu, und speichern Sie es.

![Hinzufügen eines neuen Tags im Azure-Portal](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Ihr neues Tag sollte jetzt in der Liste der Tags für die Ressource angezeigt werden.

![Neues gespeichertes Tag im Azure-Portal](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)


## Anzeigen der Tags in den Nutzungsdetails

Tags, die vom Azure-Ressourcen-Manager in Compute-, Netzwerk- und Storage-Ressourcen platziert werden, finden Sie in Ihren Nutzungsdetails im [Abrechnungsportal](https://account.windowsazure.com/) wieder.

Klicken Sie auf **Nutzungsdetails herunterladen**, um die Nutzungsdetails in Ihrem Abonnement anzuzeigen.

![Nutzungsdetails im Azure-Portal](./media/virtual-machines-common-tag/azure-portal-tags-usage-details.png)

Wählen Sie die Abrechnungsanweisung und die **Version 2** der Nutzungsdetails aus:

![Version 2 der Vorschau der Nutzungsdetails im Azure-Portal](./media/virtual-machines-common-tag/azure-portal-version2-usage-details.png)

Den Verwendungsdetails können Sie alle Tags in der Spalte **Tags** entnehmen:

![Spalte für Tags im Azure-Portal](./media/virtual-machines-common-tag/azure-portal-tags-column.png)

Durch Analysieren dieser Tags zusammen mit der Nutzung erhalten Organisationen neue Einblicke in die Nutzung Ihrer Daten.

<!---HONumber=AcomDC_0330_2016-->