<properties
   pageTitle="Verwenden von VS Code mit Resource Manager-Vorlagen | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Visual Studio Code einrichten, um Azure Resource Manager-Vorlagen zu erstellen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# Verwenden von Azure Resource Manager-Vorlagen in Visual Studio Code

Bei Azure Resource Manager-Vorlagen handelt es sich um JSON-Dateien, die eine Ressource und die dazugehörigen Abhängigkeiten beschreiben. Da diese Dateien groß und kompliziert aufgebaut sein können, ist eine Unterstützung durch Tools unerlässlich. Visual Studio Code ist ein neuer, einfacher Open-Source-Code-Editor für alle Plattformen. Er unterstützt die Erstellung und Bearbeitung von Resource Manager-Vorlagen mit einer [neuen Erweiterung](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). VS Code kann überall ausgeführt werden und benötigt keinen Internetzugriff, es sei denn, Sie möchten Ihre Resource Manager-Vorlagen auch bereitstellen.

Wenn Sie nicht bereits über VS Code verfügen, können Sie die Anwendung unter [https://code.visualstudio.com/](https://code.visualstudio.com/) installieren.

## Installieren der Resource Manager-Erweiterung

Sie müssen eine Erweiterung installieren, um die JSON-Vorlagen in VS Code verwenden zu können. Mit den folgenden Schritten wird die Sprachunterstützung für Resource Manager-JSON-Vorlagen heruntergeladen und installiert:

1. Starten Sie VS Code.
2. Führen Sie das schnelle Öffnen (Quick Open) mit STRG+P aus.
3. Führen Sie den folgenden Befehl aus:

        ext install azurerm-vscode-tools

4. Starten Sie VS Code neu, wenn Sie dazu aufgefordert werden, um die Erweiterung zu aktivieren.

 Fertig!

## Einrichten von Resource Manager-Codeausschnitten

Mit den obigen Schritten wurde die Toolunterstützung installiert, aber jetzt müssen wir VS Code für die Verwendung von JSON-Vorlagencodeausschnitten konfigurieren.

1. Kopieren Sie den Inhalt der Datei aus dem Repository [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) in die Zwischenablage.
2. Starten Sie VS Code.
3. In VS Code können Sie die Datei mit den JSON-Codeausschnitten öffnen, indem Sie entweder zu **Datei** > **Einstellungen** > **User Snippets** (Benutzercodeausschnitte) > **JSON** navigieren oder **F1** wählen und **Einstellungen** eingeben, bis Sie **Preferences: Snippets** (Einstellungen: Codeausschnitte) auswählen können.

    ![Einstellungen: Ausschnitte](./media/resource-manager-vs-code/preferences-snippets.png)

    Wählen Sie in den Optionen **JSON** aus.

    ![JSON wählen](./media/resource-manager-vs-code/select-json.png)

4. Fügen Sie den Inhalt der Datei aus Schritt 1 in die Datei mit den Benutzercodeausschnitten vor der abschließenden geschweiften Klammer („}“) ein.
5. Stellen Sie sicher, dass der JSON-Code korrekt ist und keine Wellenlinien aufweist.
6. Speichern und schließen Sie die Datei mit den Codeausschnitten.

Dies sind alle Voraussetzungen, die für die Verwendung von Resource Manager-Codeausschnitten erforderlich sind. Als Nächstes probieren wir aus, was wir eingerichtet haben.

## Verwenden von Vorlagen in VS Code

Die einfachste Möglichkeit zur Verwendung einer Vorlage besteht darin, entweder eine der Schnellstartvorlagen von [GitHub](https://github.com/Azure/azure-quickstart-templates) oder eine eigene Vorlage zu nutzen. Sie können für Ihre Ressourcengruppen über das Portal ohne viel Aufwand [eine Vorlage exportieren](resource-manager-export-template.md).

1. Wenn Sie eine Vorlage aus einer Ressourcengruppe exportiert haben, können Sie die extrahierten Dateien in VS Code öffnen.

    ![Dateien anzeigen](./media/resource-manager-vs-code/show-files.png)

2. Öffnen Sie die Datei „template.json“, damit Sie sie bearbeiten und einige zusätzliche Ressourcen hinzufügen können. Drücken Sie nach der Zeile **"resources": [** die EINGABETASTE, um eine neue Zeile zu beginnen. Wenn Sie **arm** eingeben, wird eine Liste mit Optionen angezeigt. Diese Optionen sind die Vorlagencodeausschnitte, die Sie installiert haben. Diese sollte wie folgt aussehen:

    ![Codeausschnitte anzeigen](./media/resource-manager-vs-code/type-snippets.png)

3. Wählen Sie den gewünschten Codeausschnitt. Für diesen Artikel wähle ich **arm-ip**, um eine neue öffentliche IP-Adresse zu erstellen. Setzen Sie nach der schließenden Klammer („}“) der neu erstellten Ressource ein Komma, um sicherzustellen, dass die Vorlagensyntax gültig ist.

     ![Komma hinzufügen](./media/resource-manager-vs-code/add-comma.png)

4. In VS Code ist IntelliSense integriert. Beim Bearbeiten der Vorlagen schlägt VS Code verfügbare Werte vor. Um der Vorlage beispielsweise einen Variablenabschnitt hinzuzufügen, fügen Sie **""** (zwei doppelte Anführungszeichen) ein und drücken zwischen den Anführungszeichen **STRG+LEERTASTE**. Es werden Optionen angezeigt, z.B. **variables**.

    ![Variablen hinzufügen](./media/resource-manager-vs-code/add-variables.png)

5. Außerdem kann IntelliSense verfügbare Werte oder Funktionen vorschlagen. Um eine Eigenschaft auf einen Parameterwert festzulegen, erstellen Sie einen Ausdruck mit **""** und drücken **STRG+LEERTASTE**. Sie können mit der Eingabe des Namens einer Funktion beginnen. Wählen Sie die **TAB-TASTE**, wenn Sie die gewünschte Funktion gefunden haben.

    ![Parameter hinzufügen](./media/resource-manager-vs-code/select-parameters.png)

6. Drücken Sie erneut **STRG+LEERTASTE** innerhalb der Funktion, um in der Vorlage eine Liste mit den verfügbaren Parametern anzuzeigen.

    ![Parameter hinzufügen](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Falls in der Vorlage Probleme mit der Schemaüberprüfung bestehen, werden im Editor die vertrauten Wellenlinien angezeigt. Sie können die Liste mit den Fehlern und Warnungen anzeigen, indem Sie **STRG+UMSCHALT+M** eingeben oder die Glyphen in der Statusleiste unten links auswählen.

    ![errors](./media/resource-manager-vs-code/errors.png)

    Die Überprüfung der Vorlage hilft Ihnen beim Erkennen von Syntaxproblemen. Unter Umständen werden aber auch Fehler angezeigt, die Sie ignorieren können. In einigen Fällen vergleicht der Editor die Vorlage mit einem Schema, das nicht aktuell ist, und meldet daher auch dann einen Fehler, wenn dies nicht berechtigt ist. Angenommen, eine Funktion wurde dem Resource Manager vor Kurzem hinzugefügt, aber das Schema wurde nicht aktualisiert. Der Editor meldet trotz der Tatsache, dass die Funktion während der Bereitstellung richtig funktioniert, einen Fehler.

    ![Fehlermeldung](./media/resource-manager-vs-code/unrecognized-function.png)

## Bereitstellen von neuen Ressourcen

Wenn die Vorlage fertig ist, können Sie die neuen Ressourcen wie unten beschrieben bereitstellen:

### Windows

1. Öffnen Sie eine PowerShell-Eingabeaufforderung.
2. Geben Sie zum Anmelden Folgendes ein:

        Login-AzureRmAccount 

3. Wenn Sie über mehrere Abonnements verfügen, erhalten Sie wie folgt eine Liste mit den Abonnements:

        Get-AzureRmSubscription

    Wählen Sie anschließend das zu verwendende Abonnement aus.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Aktualisieren Sie die Parameter in der Datei „parameters.json“.
5. Führen Sie die Datei „Deploy.ps1“ aus, um die Vorlage unter Azure bereitzustellen.

### OSX/Linux

1. Öffnen Sie ein Terminalfenster.
2. Geben Sie zum Anmelden Folgendes ein:

        azure login 

3. Wenn Sie über mehrere Abonnements verfügen, wählen Sie das richtige Abonnement wie folgt aus:

        azure account set <subscriptionNameOrId> 

4. Aktualisieren Sie die Parameter in der Datei „parameters.json“.
5. Führen Sie Folgendes aus, um die Vorlage bereitzustellen:

        azure group deployment create -f <PathToTemplate> 

## Nächste Schritte

- Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
- Informationen zu den Vorlagenfunktionen finden Sie unter [Vorlagenfunktionen im Azure-Ressourcen-Manager](resource-group-template-functions.md).
- Weitere Beispiele zur Verwendung von Visual Studio Code finden Sie unter [Build cloud apps with Visual Studio Code](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) (Erstellen von Cloud-Apps mit Visual Studio Code) in der [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect-[Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstarts aus der HealthClinic.biz-Demo finden Sie unter [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Schnellstarts zu Azure-Entwicklungstools).

<!---HONumber=AcomDC_0928_2016-->