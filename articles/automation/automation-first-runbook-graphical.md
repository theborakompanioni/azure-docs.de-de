<properties
    pageTitle="Mein erstes grafisches Runbook in Azure Automation | Microsoft Azure"
    description="Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen grafischen Runbooks vertraut machen können."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
	keywords="Runbooks, Runbookvorlage, Runbookautomatisierung, Azure-Runbook"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/06/2016"
    ms.author="magoedte;bwren"/>

# Mein erstes grafisches Runbook

> [AZURE.SELECTOR] - [Graphical](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell Workflow](automation-first-runbook-textual.md)

Dieses Tutorial führt Sie durch die Erstellung eines [grafischen Runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation. Wir beginnen mit einem einfachen Runbook, das wir testen und veröffentlichen. Dabei erläutern wir, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend ändern wir das Runbook, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Danach fügen wir Runbookparameter und bedingte Verknüpfungen hinzu, um die Stabilität des Runbooks zu erhöhen.

## Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

-	Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie [Ihre MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich <a href="/pricing/free-account/" target="_blank">[für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/).
-	Das [ausführende Azure-Konto](automation-sec-configure-azure-runas-account.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
-	Einen virtuellen Azure-Computer. Da dieser Computer gestartet und beendet wird, darf er sich nicht in der Produktionsumgebung befinden.


## Schritt 1: Erstellen eines neuen Runbooks

Wir erstellen zunächst ein einfaches Runbook, das den Text *Hello World* ausgibt.

1.	Öffnen Sie im Azure-Portal Ihr Automation-Konto. Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten handelt es sich um die Module, die automatisch in einem neuen Automation-Konto enthalten sind. Darunter müsste sich auch das in den [Voraussetzungen](#prerequisites) erwähnte Anmeldeinformationsobjekt befinden.
2.	Klicken Sie auf die Kachel **Runbooks**, um die Liste mit den Runbooks zu öffnen.<br> ![Steuerelement für Runbooks](media/automation-first-runbook-graphical/runbooks-control.png)
3.	Erstellen Sie ein neues Runbook, indem Sie auf die Schaltfläche **Runbook hinzufügen** und anschließend auf **Neues Runbook erstellen** klicken.
4.	Nennen Sie das Runbook *MyFirstRunbook-Graphical*.
5.	Da wir in diesem Fall ein [grafisches Runbook](automation-graphical-authoring-intro.md) erstellen, legen Sie den Runbooktyp auf **Grafisch** fest.<br> ![Neues Runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6.	Klicken Sie auf **Erstellen**, um das Runbook zu erstellen und den grafischen Editor zu öffnen.

## Schritt 2: Hinzufügen von Aktivitäten zum Runbook

Mithilfe des Bibliotheksteuerelements auf der linken Seite des Editors können Sie Aktivitäten auswählen und Ihrem Runbook hinzufügen. Wir fügen ein **Write-Output**-Cmdlet hinzu, um Text aus dem Runbook auszugeben.

1.	Klicken Sie im Bibliotheksteuerelement auf das Textfeld für die Suche, und geben Sie **Write-Output** ein. Die Suchergebnisse werden darunter angezeigt. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2.	Führen Sie einen Bildlauf zum Ende der Liste durch. Sie können entweder mit der rechten Maustaste auf **Write-Output** klicken und **Zum Zeichenbereich hinzufügen** auswählen oder auf die Ellipse neben dem Cmdlet klicken und **Zum Zeichenbereich hinzufügen** auswählen.
3.	Klicken Sie im Zeichenbereich auf die Aktivität **Write-Output**. Daraufhin öffnet sich das Blatt für die Konfigurationssteuerung, auf dem Sie die Aktivität konfigurieren können.
4.	Die Bezeichnung ist standardmäßig auf den Namen des Cmdlets festgelegt, kann aber in einen aussagekräftigeren Wert geändert werden. Legen Sie ihn auf *Write Hello World to output* fest.
5.	Klicken Sie auf **Parameter**, um Parameterwerte für das Cmdlet anzugeben. Einige Cmdlets verfügen über mehrere Parametersätze, und Sie müssen auswählen, welchen Sie verwenden möchten. **Write-Output** besitzt in diesem Fall lediglich einen einzelnen Parametersatz, sodass Sie keinen auswählen müssen. <br> ![Write-Output-Eigenschaften](media/automation-first-runbook-graphical/write-output-properties-b.png)
6.	Wählen Sie den **InputObject**-Parameter aus. Dies ist der Parameter, in dem wir den Text für den Ausgabestream angeben.
7.	Wählen Sie im Dropdownfeld **Datenquelle** die Option **PowerShell-Ausdruck** aus. Das Dropdownfeld **Datenquelle** enthält verschiedene Quellen, die Sie zum Auffüllen eines Parameterwerts verwenden. Sie können Ausgaben von solchen Quellen (etwa eine andere Aktivität, ein Automation-Objekt oder ein PowerShell-Ausdruck) verwenden. In diesem Fall soll lediglich der Text *Hello World* ausgeben werden. Hierzu können wir einen PowerShell-Ausdruck verwenden und eine Zeichenfolge angeben.
8.	Geben Sie im Feld **Ausdruck** die Zeichenfolge *"Hello World"* ein, und klicken Sie dann zweimal auf **OK**, um zum Zeichenbereich zurückzukehren.<br> ![PowerShell-Ausdruck](media/automation-first-runbook-graphical/expression-hello-world.png)
9.	Klicken Sie auf **Speichern**, um das Runbook zu speichern.<br> ![Runbook speichern](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## Schritt 3: Testen des Runbooks

Bevor wir das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, möchten wir uns vergewissern, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die **Entwurfsversion** des Runbooks aus und sehen sich interaktiv die Ausgabe an.

1.	Klicken Sie auf **Testbereich**, um das Blatt „Test“ zu öffnen.<br> ![Testbereich](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2.	Klicken Sie auf **Starten**, um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
3.	Ein [Runbookauftrag](automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt. Der Auftrag besitzt zunächst den Status *In Warteschlange*, um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet...* und anschließend zu *Wird ausgeführt...*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.
4.	Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In unserem Fall: *Hello World*.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5.	Schließen Sie das Testblatt, um zum Zeichenbereich zurückzukehren.

## Schritt 4: Veröffentlichen und Starten des Runbooks

Das soeben erstellte Runbook befindet sich immer noch im Entwurfsmodus. Wir müssen das Runbook veröffentlichen, um es in der Produktionsumgebung ausführen zu können. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In unserem Fall ist noch keine veröffentlichte Version vorhanden, da wir das Runbook gerade erst erstellt haben.

1.	Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.<br> ![Veröffentlichen](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2.	Wenn Sie jetzt einen Bildlauf nach links durchführen, um das Runbook auf dem Blatt **Runbooks** anzuzeigen, besitzt das Runbook den Erstellungsstatus **Veröffentlicht**.
3.	Führen Sie wieder einen Bildlauf nach rechts durch, um das Blatt für **MyFirstRunbook** anzuzeigen. Mit den Optionen am oberen Rand können wir das Runbook starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um den Start über einen HTTP-Aufruf zu ermöglichen.
4.	Wir möchten das Runbook einfach nur starten. Klicken Sie daher auf **Starten** und anschließend auf **Ja**.<br> ![Runbook starten](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5.	Ein Auftragsblatt für den soeben erstellten Runbookauftrag erscheint. Dieses Blatt kann zwar geschlossen werden, in diesem Fall lassen wir es jedoch geöffnet, um den Status des Auftrags verfolgen zu können.
6.	Der Auftragsstatus wird unter **Auftragszusammenfassung** angezeigt und entspricht den Statusoptionen, die wir bereits beim Testen des Runbooks gesehen haben.<br> ![Auftragszusammenfassung](media/automation-first-runbook-graphical/runbook-job-summary.png)
7.	Wenn der Runbookstatus *Abgeschlossen* lautet, klicken Sie auf **Ausgabe**. Das Blatt **Ausgabe** wird geöffnet, und der Text *Hello World* wird in dem Bereich angezeigt.<br> ![Auftragszusammenfassung](media/automation-first-runbook-graphical/runbook-job-output.png)
8.	Schließen Sie das Ausgabeblatt.
9.	Klicken Sie auf **Alle Protokolle**, um das Blatt „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabestream sollte nur *Hello World* angezeigt werden. Hier können aber auch andere Datenströme für einen Runbookauftrag (wie etwa "Ausführlich" und "Fehler") angezeigt werden, sofern das Runbook in diese schreibt.<br> ![Auftragszusammenfassung](media/automation-first-runbook-graphical/runbook-job-AllLogs.png)
10.	Schließen Sie die Blätter „Alle Protokolle“ und „Auftrag“, um zum Blatt „MyFirstRunbook“ zurückzukehren.
11.	Klicken Sie auf **Aufträge**, um das Blatt „Aufträge“ für dieses Runbook zu öffnen. Dadurch werden alle von diesem Runbook erstellten Aufträge aufgeführt. Hier wird nur ein einzelner Auftrag aufgeführt, da wir den Auftrag bislang erst einmal ausgeführt haben.<br> ![Aufträge](media/automation-first-runbook-graphical/runbook-control-jobs.png)
12.	Wenn Sie auf diesen Auftrag klicken, wird wieder der Auftragsbereich geöffnet, den wir uns beim Starten des Runbooks angesehen haben. So können Sie bereits ausgeführte Aufträge öffnen und Details zu jedem Auftrag anzeigen, der für ein bestimmtes Runbook erstellt wurde.

## Schritt 5: Erstellen variabler Ressourcen

Wir haben unser Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Wir möchten damit Azure-Ressourcen verwalten. Vor dem Konfigurieren des Runbooks für die Authentifizierung erstellen wir eine Variable zum Speichern der Abonnement-ID, auf die wir dann in Schritt 6 nach dem Einrichten der Authentifizierungsaktivität verweisen können. Das Einfügen eines Verweises in den Abonnementkontext ermöglicht Ihnen die einfache Nutzung mehrerer Abonnements. Kopieren Sie Ihre Abonnement-ID aus der Option „Abonnements“ (im Navigationsbereich), bevor Sie fortfahren.

1. Klicken Sie auf dem Blatt „Automation-Konten“ auf die Kachel **Ressourcen**, um das Blatt **Ressourcen** zu öffnen.
2. Klicken Sie auf dem Blatt „Ressourcen“ auf die Kachel **Variablen**.
3. Klicken Sie auf dem Blatt „Variablen“ auf **Variable hinzufügen**.<br>![Automation-Variable](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. Geben Sie auf dem Blatt „Neue Variable“ im Feld **Name** die Zeichenfolge **AzureSubscriptionId** und im Feld **Wert** Ihre Abonnement-ID ein. Behalten Sie unter **Typ** die Option *Zeichenfolge* sowie den Standardwert für **Verschlüsselung** bei.
5. Klicken Sie auf **Erstellen**, um die Variable zu erstellen.


## Schritt 6: Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen

Nachdem wir nun über eine Variable für unsere Abonnement-ID verfügen, können wir unser Runbook für die Authentifizierung mit den Anmeldeinformationen für das ausführende Konto konfigurieren, die unter [Voraussetzungen](#prerequisites) angegeben sind. Hierzu fügen wir dem Zeichenbereich die **Ressource** für die Verbindung mit dem ausführenden Azure-Konto und das **Add-AzureRMAccount**-Cmdlet hinzu.

1.	Klicken Sie auf dem Blatt „MyFirstRunbook“ auf **Bearbeiten**, um den grafischen Editor zu öffnen.<br> ![Runbook bearbeiten](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2.	**Write Hello World to output** wird nicht mehr benötigt. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Löschen**.
3.	Erweitern Sie im Bibliotheksteuerelement die Option **Verbindungen**, und fügen Sie dem Zeichenbereich **AzureRunAsConnection** hinzu, indem Sie **Zum Zeichenbereich hinzufügen** auswählen.
4.	Wählen Sie im Zeichenbereich **AzureRunAsConnection** aus, und geben Sie im Konfigurationssteuerungsbereich im Textfeld **Bezeichnung** den Text **Get Run As Connection** ein. Dies ist die Verbindung.
5.	Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Add-AzureRmAccount** ein.
6.	Fügen Sie **Add-AzureRmAccount** dem Zeichenbereich hinzu.<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7.	Zeigen Sie auf **Get Run As Connection**, bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil zu **Add-AzureRmAccount**. Bei dem soeben erstellten Pfeil handelt es sich um eine *Verknüpfung*. Das Runbook wird mit **Get Run As Connection** gestartet und führt dann **Add-AzureRmAccount** aus.<br> ![Verknüpfung zwischen Aktivitäten erstellen](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8.	Wählen Sie im Zeichenbereich **Add-AzureRmAccount** aus, und geben Sie im Konfigurationssteuerungsbereich im Textfeld **Bezeichnung** den Text **Login to Azure** ein.
9.	Klicken Sie auf **Parameter**, um das Konfigurationsblatt für Aktivitätsparameter anzuzeigen.
10.  Da **Add-AzureRmAccount** über mehrere Parametersätze verfügt, müssen wir einen davon auswählen, um Parameterwerte angeben zu können. Klicken Sie auf **Parametersatz**, und wählen Sie dann den Parametersatz **ServicePrincipalCertificate** aus.
11.  Wenn Sie den Parametersatz auswählen, werden die Parameter auf dem Konfigurationsblatt für die Aktivitätsparameter angezeigt. Klicken Sie auf **APPLICATIONID**.<br> ![Azure RM-Kontoparameter hinzufügen](media/automation-first-runbook-graphical/add-azurermaccount-parameterset.png)
12.  Wählen Sie im Blatt „Parameterwert“ für die **Datenquelle** die Option **Aktivitätsausgabe** und dann in der Liste **Get Run As Connection** aus. Geben Sie im Textfeld **Feldpfad** den Text **ApplicationId** ein, und klicken Sie auf **OK**. Wir geben den Namen der Eigenschaft für den Feldpfad an, da die Aktivität ein Objekt mit mehreren Eigenschaften ausgibt.
13.  Klicken Sie auf **CERTIFICATETHUMBPRINT**, und wählen Sie im Blatt „Parameterwert“ für die **Datenquelle** die Option **Aktivitätsausgabe**. Wählen Sie in der Liste die Option **Get Run As Connection** aus, und geben Sie im Textfeld **Feldpfad** den Text **CertificateThumbprint** ein. Klicken Sie anschließend auf **OK**.
14.  Klicken Sie auf **SERVICEPRINCIPAL**, und wählen Sie im Blatt „Parameterwert“ für die **Datenquelle** die Option **ConstantValue** aus. Klicken Sie auf die Option **True** und dann auf **OK**.
15.  Klicken Sie auf **TENANTID**, und wählen Sie im Blatt „Parameterwert“ für die **Datenquelle** die **Aktivitätsausgabe**. Wählen Sie in der Liste die Option **Get Run As Connection** aus, und geben Sie im Textfeld **Feldpfad** den Text **TenantId** ein. Klicken Sie anschließend zweimal auf **OK**.
16.  Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Set-AzureRmContext** ein.
17.	 Fügen Sie **Set-AzureRmContext** dem Zeichenbereich hinzu.
18.	 Wählen Sie im Zeichenbereich die Option **Set-AzureRmContext** aus, und geben Sie im Konfigurationssteuerungsbereich im Feld **Bezeichnung** die Zeichenfolge **Specify Subscription Id** ein.
19.	 Klicken Sie auf **Parameter**, um das Konfigurationsblatt für Aktivitätsparameter anzuzeigen.
20. Da **Set-AzureRmContext** über mehrere Parametersätze verfügt, müssen wir einen davon auswählen, um Parameterwerte angeben zu können. Klicken Sie auf **Parametersatz**, und wählen Sie anschließend den Parametersatz **SubscriptionId** aus.
21.	 Wenn Sie den Parametersatz auswählen, werden die Parameter auf dem Konfigurationsblatt für die Aktivitätsparameter angezeigt. Klicken Sie auf **SubscriptionID**.
22.	 Wählen Sie im Blatt „Parameterwert“ für die **Datenquelle** die Option **Variablenasset** und in der Liste die Option **AzureSubscriptionId** aus. Klicken Sie anschließend zweimal auf **OK**.
23.  Zeigen Sie auf **Login to Azure**, bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil auf **Specify Subscription Id**.


Ihr Runbook sollte nun wie folgt aussehen: <br>![Konfiguration der Runbook-Authentifizierung](media/automation-first-runbook-graphical/runbook-auth-config.png)

## Schritt 7: Hinzufügen einer Aktivität zum Starten eines virtuellen Computers

In diesem Schritt fügen wir eine Aktivität vom Typ **Start-AzureRmVM** hinzu, um einen virtuellen Computer zu starten. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen. Wir werden den Namen vorerst innerhalb des Cmdlets hartcodieren.

1. Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Start-AzureRm** ein.
2. Fügen Sie **Start-AzureRmVM** dem Zeichenbereich hinzu, und ziehen Sie das Element anschließend unter **Specify Subscription Id**.
3. Zeigen Sie auf **Specify Subscription Id**, bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil zu **Start-AzureRmVM**.
4.	Wählen Sie **Start-AzureRmVM** aus. Klicken Sie auf **Parameter** und anschließend auf **Parametersatz**, um die Sätze für **Start-AzureRmVM** anzuzeigen. Wählen Sie den Parametersatz **ResourceGroupNameParameterSetName** aus. Beachten Sie, dass neben **ResourceGroupName** und **Name** jeweils ein Ausrufezeichen angezeigt wird. Das bedeutet, dass es sich dabei um erforderliche Parameter handelt. Beachten Sie außerdem, dass für beide ein Zeichenfolgenwert erwartet wird.
5.	Wählen Sie **Name**. Wählen Sie unter **Datenquelle** die Option **PowerShell-Ausdruck** aus, und geben Sie den Namen des virtuellen Computers (in doppelten Anführungszeichen) ein, der mit diesem Runbook gestartet werden soll. Klicken Sie auf **OK**.<br>![Start-AzureRmVM – Wert für den Namensparameter](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6.	Wählen Sie **ResourceGroupName** aus. Wählen Sie unter **Datenquelle** die Option **PowerShell-Ausdruck** aus, und geben Sie den Namen der Ressourcengruppe in doppelten Anführungszeichen ein. Klicken Sie auf **OK**.<br> ![Start-AzureRmVM-Parameter](media/automation-first-runbook-graphical/startazurermvm-params.png)
8.	Klicken Sie auf den Testbereich, um das Runbook zu testen.
9.	Klicken Sie auf **Starten**, um den Test zu starten. Vergewissern Sie sich nach Abschluss des Tests, dass der virtuelle Computer gestartet wurde.

Ihr Runbook sollte nun wie folgt aussehen: <br>![Konfiguration der Runbook-Authentifizierung](media/automation-first-runbook-graphical/runbook-startvm.png)

## Schritt 8: Hinzufügen zusätzlicher Eingabeparameter für das Runbook

Unser Runbook startet zwar nun den virtuellen Computer in der Ressourcengruppe, die wir im Cmdlet **Start-AzureRmVM** angegeben haben. Es wäre aber praktischer, wenn wir beim Start des Runbooks beides angeben könnten. Zu diesem Zweck fügen wir dem Runbook nun Eingabeparameter hinzu.

1. Öffnen Sie den grafischen Editor, indem Sie im Bereich **MyFirstRunbook** auf **Bearbeiten** klicken.
2. Klicken Sie auf **Eingabe und Ausgabe** und anschließend auf **Eingabe hinzufügen**, um den Eingabeparameterbereich für das Runbook zu öffnen.<br> ![Runbookeingabe und -ausgabe](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. Geben Sie als **Name** die Zeichenfolge *VMName* an. Behalten Sie für **Typ** die Einstellung *string* bei, aber ändern Sie **Obligatorisch** in *Ja*. Klicken Sie auf **OK**.
4. Erstellen Sie einen zweiten obligatorischen Eingabeparameter namens *ResourceGroupName*, und klicken Sie anschließend auf **OK**, um den Bereich **Eingabe und Ausgabe** zu schließen.<br> ![Runbook-Eingabeparameter](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. Wählen Sie die Aktivität **Start-AzureRmVM** aus, und klicken Sie anschließend auf **Parameter**.
6. Ändern Sie die Datenquelle für **Name** in **Runbookeingabe**, und wählen Sie anschließend **VMName** aus.<br>
7. Ändern Sie die Datenquelle für **ResourceGroupName** in **Runbookeingabe**, und wählen Sie anschließend **ResourceGroupName** aus.<br> ![Start-AzureVM – Parameter](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Beachten Sie, dass Sie nun Werte für die beiden Eingabevariablen angeben können, die im Test verwendet werden.
9. Schließen Sie den Testbereich.
10.	Klicken Sie auf **Veröffentlichen**, um die neue Version des Runbooks zu veröffentlichen.
11.	Beenden Sie den virtuellen Computer, den Sie im vorherigen Schritt gestartet haben.
12.	Klicken Sie auf **Starten**, um das Runbook zu starten. Geben Sie **VMName** und **ResourceGroupName** für den virtuellen Computer ein, der gestartet wird.<br> ![Starten des Runbooks](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13.	Vergewissern Sie sich nach Abschluss des Runbooks, dass der virtuelle Computer gestartet wurde.

## Schritt 9: Erstellen einer bedingten Verknüpfung

In diesem Schritt passen wir das Runbook so an, dass der virtuelle Computer nur gestartet wird, wenn er noch nicht gestartet wurde. Zu diesem Zweck fügen wir dem Runbook ein Cmdlet vom Typ **Get-AzureRmVM** hinzu, das den Instanzebenenstatus des virtuellen Computers abruft. Anschließend fügen wir ein PowerShell-Workflow-Codemodul namens **Get Status** mit einem PowerShell-Codeausschnitt hinzu, mit dem ermittelt wird, ob der virtuelle Computer ausgeführt wird oder beendet ist. Eine bedingte Verknüpfung des Moduls **Get Status** führt **Start-AzureRmVM** nur dann aus, wenn der aktuelle Ausführungszustand angibt, dass der virtuelle Computer beendet ist. Schließlich geben wir mithilfe des PowerShell-Cmdlets „Write-Output“ noch eine Nachricht aus, die darüber informiert, ob der virtuelle Computer erfolgreich gestartet wurde.

1. Öffnen Sie **MyFirstRunbook** im grafischen Editor.
2. Entfernen Sie die Verknüpfung zwischen **Specify Subscription Id** und **Start-AzureRmVM**, indem Sie auf die Verknüpfung klicken und die ENTF-TASTE drücken.
3. Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Get-AzureRm** ein.
4. Fügen Sie dem Zeichenbereich **Get-AzureRmVM** hinzu.
5. Wählen Sie **Get-AzureRmVM** und anschließend **Parametersatz** aus, um die Sätze für **Get-AzureRmVM** anzuzeigen. Wählen Sie den Parametersatz **GetVirtualMachineInResourceGroupNameParamSet** aus. Beachten Sie, dass neben **ResourceGroupName** und **Name** jeweils ein Ausrufezeichen angezeigt wird. Das bedeutet, dass es sich dabei um erforderliche Parameter handelt. Beachten Sie außerdem, dass für beide ein Zeichenfolgenwert erwartet wird.
6. Wählen Sie unter **Datenquelle** für **Name** die Option **Runbookeingabe** und anschließend **VMName** aus. Klicken Sie auf **OK**.
7. Wählen Sie unter **Datenquelle** für **ResourceGroupName** die Option **Runbookeingabe** und anschließend **ResourceGroupName** aus. Klicken Sie auf **OK**.
8. Wählen Sie unter **Datenquelle** für **Status** die Option **Konstanter Wert** aus, und klicken Sie anschließend auf **True**. Klicken Sie auf **OK**.
9. Erstellen Sie eine Verknüpfung zwischen **Specify Subscription Id** und **Get-AzureRmVM**.
10. Erweitern Sie im Bibliotheksteuerelement den Bereich **Runbooksteuerung**, und fügen Sie dem Zeichenbereich **Code** hinzu.
11. Erstellen Sie eine Verknüpfung zwischen **Get-AzureRmVM** und **Code**.
12. Klicken Sie auf **Code**, und ändern Sie im Konfigurationsbereich die Bezeichnung in **Get Status**.
13. Wählen Sie den Parameter **Code** aus, um das Blatt **Code-Editor** anzuzeigen.
14. Fügen Sie im Code-Editor den folgenden Codeausschnitt ein:

     ```
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText 
     $Statuses = ConvertFrom-Json $StatusesJson 
     $StatusOut ="" 
     foreach ($Status in $Statuses){ 
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"} 
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"} 
     } 
     $StatusOut 
     ```

15. Erstellen Sie eine Verknüpfung zwischen **Get Status** und **Start-AzureRmVM**.<br> ![Runbook mit Codemodul](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)
16. Wählen Sie die Verknüpfung aus, und legen Sie **Bedingung anwenden** im Konfigurationsbereich auf **Ja** fest. Beachten Sie, dass die Verknüpfung nun als gestrichelte Linie dargestellt wird. Das bedeutet, dass die Aktivität nur ausgeführt wird, wenn die Bedingung erfüllt ist.
17. Geben Sie für den Bedingungsausdruck Folgendes ein: *$ActivityOutput['Get Status'] -eq "Stopped"*. Nun wird **Start-AzureRmVM** nur noch ausgeführt, wenn der virtuelle Computer beendet ist.
18.	Erweitern Sie im Bibliotheksteuerelement den Knoten **Cmdlets** und anschließend **Microsoft.PowerShell.Utility**.
19.	Fügen Sie dem Zeichenbereich zweimal **Write-Output** hinzu.<br> ![Runbook mit Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. Klicken Sie im ersten Steuerelement vom Typ **Write-Output** auf **Parameter**, und legen Sie die Bezeichnung auf *Notify VM Started* fest.
21. Legen Sie die Datenquelle für **InputObject** auf **PowerShell-Ausdruck** fest, und geben Sie den Ausdruck *"$VMName successfully started."* ein.
22. Klicken Sie im zweiten Steuerelement vom Typ **Write-Output** auf **Parameter**, und legen Sie die Bezeichnung auf *Notify VM Start Failed* fest.
23. Legen Sie die Datenquelle für **InputObject** auf **PowerShell-Ausdruck** fest, und geben Sie den Ausdruck *"$VMName could not start."* ein.
24. Verknüpfen Sie **Start AzureRmVM** mit **Notify VM Started** und **Notify VM Start Failed**.
25. Wählen Sie die Verknüpfung mit **Notify VM Started** aus, und legen Sie **Bedingung anwenden** auf **True** fest.
26. Geben Sie als Bedingungsausdruck Folgendes ein: *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -eq $true*. Dieses Write-Output-Steuerelement wird nur ausgeführt, wenn der virtuelle Computer erfolgreich gestartet wurde.
27. Wählen Sie die Verknüpfung mit **Notify VM Start Failed** aus, und legen Sie **Bedingung anwenden** auf **True** fest.
28. Geben Sie als Bedingungsausdruck Folgendes ein: *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -ne $true*. Dieses Write-Output-Steuerelement wird nur ausgeführt, wenn der virtuelle Computer nicht erfolgreich gestartet wurde.
29.	Speichern Sie das Runbook, und öffnen Sie den Testbereich.
30.	Wenn Sie nun das Runbook starten, während der virtuellen Computer beendet ist, wird der virtuelle Computer gestartet.

## Nächste Schritte

-	Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
-	Erste Schritte mit PowerShell-Runbooks werden in [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
-	Informationen über die ersten Schritte mit PowerShell-Workflow-Runbooks finden Sie unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md).

<!---HONumber=AcomDC_0928_2016-->