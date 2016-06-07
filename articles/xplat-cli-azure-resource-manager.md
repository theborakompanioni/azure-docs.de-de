
<properties
	pageTitle="Azure-Befehlszeilenschnittstelle mit dem Ressourcen-Manager | Microsoft Azure"
	description="Verwenden Sie die Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI), um mehrere Ressourcen als eine Ressourcengruppe bereitzustellen."
	editor=""
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services="azure-resource-manager"/>

<tags
	ms.service="azure-resource-manager"
	ms.workload="multiple"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/20/2016"
	ms.author="danlep"/>

# Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure-Ressourcen-Manager

> [AZURE.SELECTOR]
- [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)



Dieser Artikel enthält gängige Vorgehensweisen zum Erstellen und Verwalten von Azure-Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle (Azure Command-Line Interface, Azure CLI) im Azure Resource Manager-Modus.

>[AZURE.NOTE] Für die befehlszeilenbasierte Erstellung und Verwaltung von Azure-Ressourcen benötigen Sie ein Azure-Abonnement. (Ein kostenloses Azure-Konto erhalten Sie [hier](https://azure.microsoft.com/free/).) Außerdem müssen Sie die [Azure-Befehlszeilenschnittstelle installieren](xplat-cli-install.md) und sich [für die Verwendung der mit Ihrem Konto verknüpften Azure-Ressourcen anmelden](xplat-cli-connect.md). Damit sind dann alle nötigen Vorbereitungen abgeschlossen.

## Azure-Ressourcen

Mit dem Azure-Ressourcen-Manager können Sie eine Gruppe von _Ressourcen_ (vom Benutzer verwaltete Entitäten wie etwa virtuelle Computer, Datenbankserver, Datenbanken oder Websites) als einzelne logische Einheit (_Ressourcengruppe_) erstellen und verwalten.

Ein Vorteil des Azure Resource Managers besteht darin, dass Sie Ihre Azure-Ressourcen _deklarativ_ erstellen können, indem Sie die Struktur und die Beziehungen einer bereitstellbaren Gruppe von Ressourcen in *JSON-Vorlagen* beschreiben. Die Vorlage gibt Parameter an, die entweder beim Ausführen eines Befehls (Inline) ausgefüllt oder in einer separaten JSON-Parameterdatei gespeichert werden. Auf diese Weise können Sie mühelos neue Ressourcen mit derselben Vorlage erstellen, indem Sie unterschiedliche Parameter bereitstellen. So enthält beispielsweise eine Vorlage für die Websiteerstellung Parameter für den Websitenamen und die Region, in der sich die Website befinden soll, sowie andere allgemeine Einstellungen.

Wenn mit einer Vorlage eine Gruppe geändert oder erstellt wird, wird eine _Bereitstellung_ erstellt, die dann auf die Gruppe angewendet wird. Weitere Informationen zum Azure-Ressourcen-Manager finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](resource-group-overview.md).

Nach dem Erstellen einer Bereitstellung können Sie genau wie beim klassischen Bereitstellungsmodell die einzelnen Ressourcen imperativ über die Befehlszeile verwalten. Verwenden Sie beispielsweise CLI-Befehle im Resource Manager-Modus, um Ressourcen wie [virtuelle Azure Resource Manager-Computer](./virtual-machines/virtual-machines-linux-cli-deploy-templates.md) zu starten, zu beenden oder zu löschen.

## Authentifizierung

Wenn Sie den Azure Resource Manager über die Azure-Befehlszeilenschnittstelle nutzen möchten, müssen Sie sich aktuell bei Microsoft Azure authentifizieren. Verwenden Sie hierzu den Befehl `azure login`, und geben Sie ein von Azure Active Directory verwaltetes Konto an. Dabei kann es sich um ein Geschäfts- oder Schulkonto (Organisationskonto) oder um ein Microsoft-Konto handeln. Die Authentifizierung mit einem durch eine PUBLISHSETTINGS-Datei installierten Zertifikat ist in diesem Modus nicht möglich.

Weitere Informationen zur Authentifizierung bei Microsoft Azure finden Sie unter [Herstellen einer Verbindung mit einem Azure-Abonnement über die Azure-Befehlszeilenschnittstelle](xplat-cli-connect.md).

>[AZURE.NOTE] Bei Verwendung eines von Azure Active Directory verwalteten Kontos können Sie auch die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure nutzen, um den Zugriff auf und die Verwendung von Azure-Ressourcen zu verwalten. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](./active-directory/role-based-access-control-configure.md).

## Festlegen des Resource Manager-Modus

Da sich die Befehlszeilenschnittstelle standardmäßig nicht im Resource Manager-Modus befindet, müssen Sie die Resource Manager-Befehle der Azure-Befehlszeilenschnittstelle mithilfe des folgenden Befehls aktivieren.

	azure config mode arm

## Suchen der Standorte

Bei den meisten Azure-Ressourcen-Manager-Befehlen muss für die Ressourcenerstellung oder -suche ein gültiger Standort angegeben werden. Verwenden Sie zum Ermitteln aller verfügbaren Standorte für die verschiedenen Azure-Ressourcen den folgenden Befehl.

	azure location list

Damit werden die verfügbaren Azure-Regionen aufgelistet (etwa „USA, Westen“, „USA, Osten“ usw.). Führen Sie den Befehl `azure provider list` und anschließend den Befehl `azure provider show` aus, um ausführliche Informationen zu verfügbaren Ressourcenanbietern und den Orten zu erhalten, an denen sie verfügbar sind. Der folgende Befehl führt etwa die Speicherorte des Azure-Containerdiensts auf:

    azure provider show Microsoft.ContainerService 

## Erstellen einer Ressourcengruppe

Bei einer Ressourcengruppe handelt es sich um eine logische Gruppe von Ressourcen (beispielsweise Netzwerk-, Speicher- und Compute-Ressourcen). Für nahezu alle Befehle im Resource Manager-Modus ist eine Ressourcengruppe erforderlich. Der folgende Befehl erstellt beispielsweise eine Ressourcengruppe namens _testRG_ in der Region „USA, Westen“.

	azure group create -n "testRG" -l "West US"

Die Bereitstellung für die Ressourcengruppe *testRG* erfolgt später mithilfe einer Vorlage zum Starten eines virtuellen Ubuntu-Computers. Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie Ressourcen wie virtuelle Computer und Netzwerke oder Speicher hinzufügen.


## Verwenden von Ressourcengruppenvorlagen

### Suchen und Konfigurieren einer Ressourcengruppenvorlage

Sie können entweder [eine eigene Vorlage erstellen](resource-group-authoring-templates.md) oder eine der von der Community erstellten [Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) verwenden. Diese sind auch bei [GitHub](https://github.com/Azure/azure-quickstart-templates) verfügbar.

Das Erstellen einer neuen Vorlage würde jedoch den Rahmen dieses Artikels sprengen. Daher verwenden wir zum Einstieg die in den [Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux/) verfügbare Vorlage _101-simple-vm-from-image_. Standardmäßig wird dabei ein einzelner virtueller Ubuntu 14.04.2-LTS-Computer in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz erstellt. Sie müssen lediglich eine Ressourcengruppe und die folgenden Parameter angeben, um diese Vorlage zu verwenden:

* Administratorbenutzername für den virtuellen Computer: `adminUsername`
* Kennwort: `adminPassword`
* Domänenname für den virtuellen Computer: `dnsLabelPrefix`

>[AZURE.TIP] Diese Schritte veranschaulichen nur eine Möglichkeit der Verwendung einer VM-Vorlage mit der Azure-Befehlszeilenschnittstelle. Weitere Beispiele finden Sie unter [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](./virtual-machines/virtual-machines-linux-cli-deploy-templates.md).

1. Klicken Sie auf den Link „Mehr erfahren via GitHub“, und laden Sie die Dateien „azuredeploy.json“ und „azuredeploy.parameters.json“ von [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux) in einen Arbeitsordner auf Ihrem lokalen Computer herunter. (Stellen Sie sicher, dass Sie in GitHub für jede Datei das _Rohformat_ auswählen.)

2. Öffnen Sie die Datei „azuredeploy.parameters.json“ in einem Text-Editor, und geben Sie geeignete Parameterwerte für Ihre Umgebung ein (behalten Sie den Wert **ubuntuOSVersion** bei).

	```
			{
			  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
			  "contentVersion": "1.0.0.0",
			  "parameters": {
			    "adminUsername": {
			      "value": "azureUser"
			    },
			    "adminPassword": {
			      "value": "GEN-PASSWORD"
			    },
			    "dnsLabelPrefix": {
			      "value": "GEN-UNIQUE"
			    },
			    "ubuntuOSVersion": {
			      "value": "14.04.2-LTS"
			    }
			  }
			}

	```
3.  Nach dem Ändern der Bereitstellungsparameter stellen Sie nun den virtuellen Ubuntu-Computer in der zuvor erstellten Ressourcengruppe *testRG* bereit. Wählen Sie einen Namen für die Bereitstellung (in diesem Beispiel *testRGDeploy*), und verwenden Sie dann den folgenden Befehl, um sie starten.

	```
	azure group deployment create -f azuredeploy.json -e azuredeploy.parameters.json testRG testRGDeploy
	```

	Die Option `-e` gibt die Datei „azuredeploy.parameters.json“ an, die Sie im vorherigen Schritt angepasst haben. Die Option `-f` gibt die Vorlagendatei „azuredeploy.json“ an.

	Dieser Befehl gibt „OK“ zurück, nachdem die Bereitstellung hochgeladen wurde, jedoch bevor die Bereitstellung auf die Ressourcen in der Gruppe angewendet wurde.

4. Verwenden Sie den folgenden Befehl, um den Status der Bereitstellung zu prüfen.

	```
	azure group deployment show testRG testRGDeploy
	```

	Mit **ProvisioningState** wird der Status der Bereitstellung angezeigt.

	Ist die Bereitstellung erfolgreich, sieht die Ausgabe etwa wie folgt aus:

		azure-cli@0.8.0:/# azure group deployment show testRG testRGDeploy
		info:    Executing command group deployment show
		+ Getting deployments
		+ Getting deployments
		data:    DeploymentName     : testDeploy
		data:    ResourceGroupName  : testRG
		data:    ProvisioningState  : Succeeded
		data:    Timestamp          : 
		data:    Mode               : Incremental
		data:    Name                   Type          Value
		data:    ---------------------  ------------  ---------------------
		data:    adminUsername          String        MyUserName
		data:    adminPassword          SecureString  undefined
		data:    dnsLabelPrefix    String        MyDomainName
		data:    ubuntuOSVersion        String        14.04.2-LTS
		info:    group deployment show command OK

	>[AZURE.NOTE] Wenn Sie feststellen, dass Ihre Konfiguration nicht richtig ist und eine Bereitstellung mit langer Laufzeit angehalten werden muss, verwenden Sie den folgenden Befehl:
	>
	> `azure group deployment stop testRG testRGDeploy`
	>
	> Wenn Sie keinen Bereitstellungsnamen angeben, wird automatisch einer anhand des Namens der Vorlagendatei erstellt. Dieser Name wird als Teil der Ausgabe des Befehls `azure group create` zurückgegeben.

	Sie können jetzt unter Verwendung des angegebenen Domänennamens per SSH auf den virtuellen Computer zugreifen. Beim Herstellen einer Verbindung mit dem virtuellen Computer müssen Sie einen vollqualifizierten Domänennamen im Format `<domainName>.<region>.cloudapp.azure.com` verwenden, z. B. `MyDomainName.westus.cloudapp.azure.com`.

5. Verwenden Sie zum Anzeigen der Gruppe den folgenden Befehl.

		azure group show testRG

	Dieser Befehl gibt Informationen zu den Ressourcen in der Gruppe zurück. Sind mehrere Gruppen vorhanden, rufen Sie mit dem Befehl `azure group list` eine Liste mit Gruppennamen ab. Verwenden Sie dann `azure group show`, um die Details einer bestimmten Gruppe anzuzeigen.

Sie können auch eine Vorlage direkt auf [GitHub](https://github.com/Azure/azure-quickstart-templates) verwenden, ohne sie auf Ihren Computer herunterzuladen. Übergeben Sie dazu die URL zur Datei „azuredeploy.json“ für die Vorlage in Ihrem Befehl mithilfe der Option **--template-uri**. Öffnen Sie zum Abrufen der URL die Datei „azuredeploy.json“ auf GitHub im _Rohmodus_, und kopieren Sie die URL aus der Adressleiste des Browsers. Mit dieser URL können Sie direkt eine Bereitstellung erstellen, indem Sie ungefähr folgenden Befehl verwenden:

	azure group deployment create testRG testRGDeploy --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json
Sie werden aufgefordert, die erforderlichen Vorlagenparameter einzugeben.

> [AZURE.NOTE] Die JSON-Vorlage muss im _Rohmodus_ geöffnet werden. Im normalen Modus wird in der Adressleiste des Browsers eine andere URL angezeigt. Klicken Sie rechts oben auf die Schaltfläche **Raw**, während die Datei in GitHub angezeigt wird, um sie im _Rohmodus_ zu öffnen.

## Verwenden von Ressourcen

Auch wenn Vorlagen es Ihnen ermöglichen, gruppenweite Änderungen an der Konfiguration zu deklarieren, müssen Sie manchmal mit nur einer einzelnen Ressource arbeiten. Verwenden Sie dazu die Befehle vom Typ `azure resource`.

> [AZURE.NOTE] Mit Ausnahme des Befehls `list` müssen Sie allen anderen Befehlen vom Typ `azure resource` mithilfe des Parameters `-o` die API-Version der Ressource angeben, mit der Sie arbeiten. Wenn Sie die zu verwendende API-Version nicht kennen, öffnen Sie die Vorlagendatei und gehen Sie zum Feld **apiVersion** der Ressource.

1. Verwenden Sie den folgenden Befehl, um alle Ressourcen in einer Gruppe aufzulisten.

		azure resource list testRG

2. Verwenden Sie einen Befehl wie den folgenden, um einzelne Ressourcen in der Gruppe anzuzeigen (beispielsweise den virtuellen Computer *MyUbuntuVM*):

		azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

	Beachten Sie den Parameter **Microsoft.Compute/virtualMachines**. Mit diesem Parameter wird der Typ der Ressourcen angegeben, für den Sie Informationen anfordern. In der zuvor heruntergeladenen Vorlagendatei wird der gleiche Wert verwendet, um die Art der in der Vorlage beschriebenen VM-Ressource zu definieren.

	Dieser Befehl gibt Informationen zum virtuellen Computer zurück.

3. Wenn Sie Details für eine Ressource anzeigen, ist es oft hilfreich, den Parameter `--json` zu verwenden. Dadurch wird die Ausgabe besser lesbar, da einige Werte als geschachtelte Strukturen oder Sammlungen angegeben werden. Im folgenden Beispiel sehen Sie, wie die Ergebnisse des **show**-Befehls als JSON-Dokument zurückgegeben werden.

		azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

	>[AZURE.NOTE] Sie können die JSON-Daten in einer Datei speichern. Verwenden Sie dazu das Zeichen „ &gt;“, um die Ausgabe in eine Datei zu umzuleiten. Beispiel:
	>
	> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

4. Verwenden Sie etwa folgenden Befehl, um eine bestehende Ressource zu löschen:

		azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

## Anzeigen von Gruppenprotokollen

Verwenden Sie den Befehl `azure group log show`, um protokollierte Informationen zu Vorgängen für eine Gruppe anzuzeigen. Standardmäßig wird dadurch der letzte Vorgang aufgelistet, der für die Gruppe durchgeführt wurde. Mit dem optionalen Parameter `--all` können Sie alle Vorgänge anzeigen. Für die letzte Bereitstellung können Sie `--last-deployment` verwenden. Wenn Sie sich dagegen für eine bestimmte Bereitstellung interessieren, verwenden Sie `--deployment`, und geben Sie den Namen der Bereitstellung an. Im folgenden Beispiel wird ein Protokoll aller Vorgänge zurückgegeben, die für die Gruppe *testRG* durchgeführt wurden.

	azure group log show testRG --all
    
## Exportieren einer Ressourcengruppe als Vorlage

Sie können die Resource Manager-Vorlage für eine vorhandene Ressourcengruppe anzeigen. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können zukünftige Bereitstellungen der Lösung leicht automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.

2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich den JSON-Code für Ihre Lösung ansehen.

Mithilfe der Azure-Befehlszeilenschnittstelle können Sie entweder eine Vorlage exportieren, die dem aktuellen Zustand Ihrer Ressourcengruppe entspricht, oder die Vorlage herunterladen, die für eine bestimmte Bereitstellung verwendet wurde.

* **Exportieren der Vorlage für eine Ressourcengruppe**: Dies ist hilfreich, wenn Sie Änderungen an einer Ressourcengruppe vorgenommen haben und die JSON-Darstellung ihres aktuellen Zustands abrufen müssen. Die generierte Vorlage enthält aber nur eine minimale Anzahl von Parametern und keine Variablen. Die meisten Werte in der Vorlage sind hartcodiert. Vor dem Bereitstellen der generierten Vorlage kann es ratsam sein, weitere Werte in Parameter zu konvertieren, damit Sie die Bereitstellung für unterschiedliche Umgebungen anpassen können.

    Führen Sie den Befehl `azure group export` wie im folgenden Beispiel gezeigt aus, um die Vorlage für eine Ressourcengruppe in ein lokales Verzeichnis zu exportieren. (Geben Sie ein für Ihre Betriebssystemumgebung geeignetes lokales Verzeichnis an.)

        azure group export testRG ~/azure/templates/

* **Herunterladen der Vorlage für eine bestimmte Bereitstellung**: Dies ist hilfreich, wenn Sie die Vorlage anzeigen möchten, die tatsächlich zum Bereitstellen von Ressourcen verwendet wurde. Die Vorlage enthält alle Parameter und Variablen, die für die ursprüngliche Bereitstellung definiert wurden. Wenn eine Person in Ihrer Organisation aber Änderungen an der Ressourcengruppe vorgenommen hat, die über die Definition in der Vorlage hinausgehen, stellt diese Vorlage nicht den aktuellen Zustand der Ressourcengruppe dar.

    Führen Sie den Befehl `azure group deployment template download` aus, um die für eine bestimmte Bereitstellung verwendete Vorlage in ein lokales Verzeichnis herunterzuladen.

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Der Vorlagenexport befindet sich noch in der Vorschauphase, und das Exportieren einer Vorlage wird derzeit nicht für alle Ressourcentypen unterstützt. Wenn Sie versuchen, eine Vorlage zu exportieren, wird ggf. ein Fehler mit dem Hinweis angezeigt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf können Sie diese Ressourcen nach dem Herunterladen manuell in der Vorlage definieren.

## Nächste Schritte

* Weitere Informationen zur Verwendung des Azure-Ressourcen-Managers mit Azure PowerShell finden Sie unter [Verwenden von Windows PowerShell mit dem Azure-Ressourcen-Manager](powershell-azure-resource-manager.md).
* Weitere Informationen zum Verwenden von Azure Resource Manager im Azure-Portal finden Sie unter [Verwenden des Azure-Portals zum Bereitstellen und Verwalten Ihrer Azure-Ressourcen](./azure-portal/resource-group-portal.md).

<!---HONumber=AcomDC_0525_2016-->