<properties
   pageTitle="Visual Studio-Projekte für Azure-Ressourcengruppen | Microsoft Azure"
   description="Verwenden Sie Visual Studio, um ein Azure-Ressourcengruppenprojekt zu erstellen und die Ressourcen in Azure bereitzustellen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="tomfitz" />

# Erstellen und Bereitstellen von Azure-Ressourcengruppen mit Visual Studio

Mit Visual Studio und dem [Azure SDK](https://azure.microsoft.com/downloads/) können Sie ein Projekt erstellt, das Ihre Infrastruktur und Ihren Code auf Azure bereitstellt. Sie können z. B. den Webhost, die Website und die Datenbank für Ihre App definieren und diese Infrastruktur zusammen mit dem Code bereitstellen. Oder Sie können einen virtuellen Computer, ein virtuelles Netzwerk und ein Speicherkonto definieren und diese Infrastruktur zusammen mit einem Skript, das auf dem virtuellen Computer ausgeführt wird, bereitstellen. Das **Azure-Ressourcengruppe**-Bereitstellungsprojekt ermöglicht die Bereitstellung aller erforderlichen Ressourcen in einem einzigen, wiederholbaren Vorgang. Weitere Informationen zum Bereitstellen und Verwalten von Ressourcengruppen finden Sie in der [Übersicht über den Azure-Ressourcen-Manager](resource-group-overview.md).

Azure-Ressourcengruppenprojekte enthalten Azure Resource Manager-JSON-Vorlagen, die die für Azure bereitgestellten Ressourcen definieren. Weitere Informationen über die Elemente von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md). Visual Studio ermöglicht das Bearbeiten dieser Vorlagen und bietet Tools, die das Arbeiten mit Vorlagen vereinfachen.

In diesem Thema stellen Sie eine Web-App und SQL-Datenbank bereit. Die Schritte sind jedoch für jeden Ressourcentyp nahezu identisch. Genauso einfach können Sie einen virtuellen Computer und die dazugehörigen Ressourcen bereitstellen. Visual Studio bietet viele verschiedene Starter-Vorlagen für die Bereitstellung gängiger Szenarien.

Im diesem Artikel werden Visual Studio 2015 Update 2 und Microsoft Azure SDK für .NET 2.9 verwendet. Bei Verwendung von Visual Studio 2013 mit Azure SDK 2.9 ist der Ablauf größtenteils identisch. Sie können ein Azure SDK ab Version 2.6 verwenden. Die Benutzeroberfläche kann sich dann aber von der Benutzeroberfläche in diesem Artikel unterscheiden. Es wird dringend empfohlen, die neueste Version des [Azure SDK](https://azure.microsoft.com/downloads/) zu installieren, bevor Sie mit der Ausführung der Schritte beginnen.

## Erstellen eines Azure-Ressourcengruppenprojekts

In diesem Verfahren erstellen Sie ein Azure-Ressourcengruppenprojekt mit einer Vorlage vom Typ **Web-App und SQL**.

1. Wählen Sie in Visual Studio **Datei**, **Neues Projekt** und anschließend **C#** oder **Visual Basic** aus. Wählen Sie dann **Cloud** und anschließend das **Azure-Ressourcengruppe**-Projekt aus.

    ![Cloudbereitstellungsprojekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Wählen Sie die Vorlage aus, die Sie im Azure-Ressourcen-Manager bereitstellen möchten. Je nach Art des bereitzustellenden Projekts stehen Ihnen viele verschiedene Optionen zur Verfügung. In diesem Thema wählen wir die Vorlage **Web-App und SQL** aus.

    ![Vorlage auswählen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Die Vorlage, die Sie auswählen, ist lediglich der Ausgangspunkt. Sie können Ressourcen hinzufügen und entfernen, um Ihr Szenario auszuführen.

    >[AZURE.NOTE] Visual Studio ruft online eine Liste mit verfügbaren Vorlagen ab. Die Liste kann sich ändern.

    Visual Studio erstellt ein Ressourcengruppen-Bereitstellungsprojekt für eine Web-App und SQL-Datenbank.

1. Erweitern Sie die Knoten im Bereitstellungsprojekt, um zu ermitteln, was erstellt wurde.

    ![Knoten anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Da wir für dieses Beispiel die Vorlage „Web-App + SQL“ gewählt haben, werden die folgenden Dateien angezeigt.

    |Dateiname|Beschreibung|
    |---|---|
    |Deploy-AzureResourceGroup.ps1|Ein PowerShell-Skript, das PowerShell-Befehle zum Bereitstellen im Azure Resource Manager aufruft.<br />**Hinweis:** Dieses PowerShell-Skript wird von Visual Studio zum Bereitstellen Ihrer Vorlage verwendet. Alle Änderungen, die Sie an diesem Skript vornehmen, haben Auswirkungen auf die Bereitstellung in Visual Studio. Es ist also Vorsicht geboten.|
    |WebSiteSQLDatabase.json|Die Resource Manager-Vorlage, die sowohl die für Azure bereitzustellende Infrastruktur als auch die Parameter definiert, die Sie bei der Bereitstellung angeben können. Sie definiert auch die Abhängigkeiten zwischen Ressourcen, damit sie von Resource Manager in der richtigen Reihenfolge bereitgestellt werden.|
    |WebSiteSQLDatabase.parameters.json|Eine Parameterdatei, die Werte enthält, die für die Vorlage erforderlich sind. Parameterwerte werden zum Anpassen der einzelnen Bereitstellungen übergeben.|

    Alle Ressourcengruppen-Bereitstellungsprojekte enthalten diese grundlegenden Dateien. Andere Projekte enthalten möglicherweise zusätzliche Dateien zur Unterstützung weiterer Funktionen.

## Anpassen der Ressourcen-Manager-Vorlage

Sie können ein Bereitstellungsprojekt anpassen, indem Sie die JSON-Vorlagen bearbeiten, in denen bereitzustellenden Ressourcen beschrieben werden. JSON ist die Kurzform von "JavaScript Object Notation". Es handelt sich um ein serialisiertes Datenformat, das einfach zu verwenden ist. Die JSON-Dateien verwenden ein Schema, auf das Sie am Anfang jeder Datei verweisen. Sie können das Schema herunterladen und analysieren, um es besser zu verstehen. Das Schema definiert die zulässigen Elemente und legt die Arten und Formate von Feldern, die möglichen Werte aufgezählter Werte und Ähnliches fest. Weitere Informationen zu den Elementen von Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).

Öffnen Sie **WebSiteSQLDatabase.json**, um die Vorlage zu bearbeiten.

Der Visual Studio-Editor bietet Tools, die das Bearbeiten der Resource Manager-Vorlage erleichtern. Die in der Vorlage definierten Elemente werden gut sichtbar im Fenster **JSON-Gliederung** angezeigt.

![JSON-Gliederung anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Wenn Sie eines der Elemente in der Gliederung auswählen, gelangen Sie zu diesem Teil der Vorlage und die entsprechende JSON wird hervorgehoben.

![In JSON navigieren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Sie können eine Ressource hinzufügen, indem Sie entweder die Schaltfläche **Ressource hinzufügen** am oberen Rand der JSON-Gliederung auswählen oder mit der rechten Maustaste auf **Ressourcen** und anschließend auf **Neue Ressource hinzufügen** klicken.

![Ressource hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Wählen Sie für dieses Tutorial die Option **Speicherkonto** aus, und vergeben Sie einen Namen. Der Name eines Speicherkontos darf nur aus Zahlen und Kleinbuchstaben bestehen maximal 23 Zeichen lang sein. Da das Projekt dem Namen noch eine eindeutige 13-stellige Zeichenfolge hinzufügt, darf Ihr Name maximal 11 Zeichen umfassen.

![Speicher hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Beachten Sie, dass nicht nur die Ressource hinzugefügt wurde, sondern auch ein Parameter für das Typenspeicherkonto sowie eine Variable für den Namen des Speicherkontos.

![Gliederung anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Der Parameter **storageType** ist mit zulässigen Typen und einem Standardtyp vordefiniert. Diese Werte können Sie so beibehalten oder für Ihr Szenario bearbeiten. Wenn Sie verhindern möchten, dass über diese Vorlage ein **Premium\_LRS**-Speicherkonto bereitgestellt wird, entfernen Sie es einfach aus den zulässigen Typen.

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio bietet auch Intellisense, damit Sie besser sehen, welche Eigenschaften beim Bearbeiten der Vorlage verfügbar sind. Wenn Sie beispielsweise die Eigenschaften für Ihren App Service-Plan bearbeiten möchten, navigieren Sie zur Ressource **HostingPlan**, und fügen Sie einen Wert für die Eigenschaften (**properties**) hinzu. Intellisense zeigt daraufhin die verfügbaren Werte sowie eine Beschreibung des Werts an.

![IntelliSense anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Sie können **numberOfWorkers** auf „1“ festlegen.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## Bereitstellen des Ressourcengruppen-Projekts in Azure

Sie können das Projekt jetzt bereitstellen. Wenn Sie ein Azure-Ressourcengruppenprojekt bereitstellen, stellen Sie es für eine Azure-Ressourcengruppe bereit. Die Ressourcengruppe ist eine logische Gruppierung von Ressourcen mit einem gemeinsamen Lebenszyklus.

1. Wählen Sie im Kontextmenü des Bereitstellungsprojektknotens **Bereitstellen** > **Neue Bereitstellung** aus.

    ![Bereitstellen, Menüelement "Neue Bereitstellung"](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Das Dialogfeld **Für Ressourcengruppe bereitstellen** wird angezeigt.

    ![Dialogfeld "In Ressourcengruppe bereitstellen"](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Wählen Sie im Dropdownfeld **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue. Öffnen Sie zum Erstellen einer Ressourcengruppe das Dropdownfeld **Ressourcengruppe**, und wählen Sie die Option **Neu erstellen** aus.

    ![Dialogfeld "In Ressourcengruppe bereitstellen"](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Das Dialogfeld **Ressourcengruppe erstellen** wird angezeigt. Vergeben Sie einen Namen und Speicherort für die Gruppe, und betätigen Sie die Schaltfläche **Erstellen** .

    ![Dialogfeld "Ressourcengruppe erstellen"](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Sie können die Parameter für die Bereitstellung bearbeiten, wenn Sie die Schaltfläche **Parameter bearbeiten** betätigen. Geben Sie Werte für die Parameter ein, und betätigen Sie die Schaltfläche **Speichern**.

    ![Dialogfeld "Parameter bearbeiten"](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
    Die Option **Kennwörter als Nur-Text in der Parameterdatei speichern** ist nicht sicher.

1. Wählen Sie die Schaltfläche **Bereitstellen** aus, um das Projekt in Azure bereitzustellen. Sie können den Verlauf der Bereitstellung im Fenster **Ausgabe** sehen. Abhängig von Ihrer Konfiguration kann die Bereitstellung mehrere Minuten in Anspruch nehmen. Geben Sie das Datenbankadministratorkennwort in der PowerShell-Konsole ein, wenn Sie dazu aufgefordert werden. Wenn der Bereitstellungsvorgang nicht mehr weiter ausgeführt wird, kann dies daran liegen, dass der Prozess auf Ihre Eingabe des Kennworts in der PowerShell-Konsole wartet.

    >[AZURE.NOTE] Visual Studio fordert Sie unter Umständen zur Installation der die Azure PowerShell-Cmdlets auf. Die Azure PowerShell-Cmdlets werden zur erfolgreichen Bereitstellung von Ressourcengruppen benötigt. Führen Sie die Installation durch, wenn Sie dazu aufgefordert werden.
    
1. Wenn die Bereitstellung abgeschlossen ist, erscheint im Fenster **Ausgabe** eine Meldung, die etwa so aussieht:

        ...
        15:19:19 - DeploymentName     : websitesqldatabase-0212-2318
        15:19:19 - CorrelationId      : 6cb43be5-86b4-478f-9e2c-7e7ce86b26a2
        15:19:19 - ResourceGroupName  : DemoSiteGroup
        15:19:19 - ProvisioningState  : Succeeded
        ...

1. Öffnen Sie in einem Browser das [Azure-Portal](https://portal.azure.com/), und melden Sie sich bei Ihrem Konto an. Wählen Sie zum Anzeigen der Ressourcengruppe die Option **Ressourcengruppen** sowie die bereitgestellte Ressourcengruppe.

    ![Gruppe auswählen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Alle bereitgestellten Ressourcen werden angezeigt.

    ![Ressourcen anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Wenn Sie Änderungen vorgenommen haben und das Projekt erneut bereitstellen möchten, wählen Sie über das Kontextmenü des Azure-Ressourcengruppenprojekts die vorhandene Ressourcengruppe aus. Wählen Sie im Kontextmenü die Option **Bereitstellen** und dann die bereitgestellte Ressourcengruppe aus.

    ![Bereitgestellte Azure-Ressourcengruppe](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## Bereitstellen von Code mit Ihrer Infrastruktur

Jetzt haben Sie die Infrastruktur für Ihre App bereitgestellt, im Projekt ist jedoch kein Code bereitgestellt. In diesem Thema sehen Sie, wie Sie während der Bereitstellung eine Web-App sowie SQL-Datenbanktabellen bereitstellen können. Wenn Sie anstelle einer Web-App eine virtuelle Maschine bereitstellen, müssen Sie bei der Bereitstellung Code auf der Maschine ausführen. Der Prozess für die Bereitstellung von Code für eine Web-App oder für das Einrichten einer virtuellen Maschine ist fast identisch.

1. Fügen Sie in Ihrer Visual Studio-Lösung eine **ASP.NET-Webanwendung** hinzu.

    ![Web-App hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Wählen Sie **MVC** aus, und deaktivieren Sie das Kontrollkästchen **In der Cloud hosten**, da diese Aufgabe vom Ressourcengruppenprojekt ausgeführt wird.

    ![MVC auswählen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Fügen Sie im Ressourcengruppenprojekt einen Verweis zum Web-App-Projekt hinzu, nachdem Visual Studio Ihre Web-App erstellt hat.

    ![Referenz hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Durch Hinzufügen eines Verweises verknüpfen Sie das Web-App-Projekt mit dem Ressourcengruppenprojekt und legen automatisch drei Schlüsseleigenschaften fest.
    
    - **Zusätzliche Eigenschaften** enthält den Stagingspeicherort für das Webbereitstellungspaket, der mittels Push an Azure Storage übertragen wird.
    - **Dateipfad einbeziehen** enthält den Pfad, in dem das Paket erstellt wird. **Ziele einbeziehen** enthält den Befehl, der von der Bereitstellung ausgeführt wird.
    - Der Standardwert **Erstellen;Paket** ermöglicht der Bereitstellung das Erstellen eines Webbereitstellungspakets (package.zip).
    
    Ein Veröffentlichungsprofil ist nicht erforderlich, da die Bereitstellung die erforderlichen Informationen aus den Eigenschaften zum Erstellen des Pakets bezieht.
    
      ![Referenz anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
      
1. Fügen Sie der Vorlage eine Ressource hinzu, und wählen Sie dieses Mal **Web Deploy für Web Apps** aus.

    ![Web Deploy hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Stellen Sie das Ressourcengruppenprojekt für die Ressourcengruppe erneut bereit. Diesmal sind einige neue Parameter enthalten. Die Werte für **\_artifactsLocation** oder **\_artifactsLocationSasToken** werden von Visual Studio automatisch generiert und müssen daher nicht angegeben werden. Legen Sie den Ordner und den Dateinamen auf den Pfad fest, der das Bereitstellungspaket enthält.

    ![Web Deploy hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Wählen Sie für **Artefaktspeicherkonto** die mit dieser Ressourcengruppe bereitgestellte Option aus.
    
Navigieren Sie nach Abschluss der Bereitstellung zu der Website. Dort sehen Sie, dass die Standard-ASP.NET-App erfolgreich bereitgestellt wurde.

![Bereitgestellte App anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## Nächste Schritte

- Informationen zur Ressourcenverwaltung über das Portal finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](./azure-portal/resource-group-portal.md).
- Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).

<!---HONumber=AcomDC_0810_2016-->