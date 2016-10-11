<properties
   pageTitle="Erste Schritte mit der Azure Batch-Befehlszeilenschnittstelle | Microsoft Azure"
   description="Hier erhalten Sie eine kurze Einführung in die Batch-Befehle der Azure-Befehlszeilenschnittstelle zum Verwalten von Ressourcen des Azure Batch-Diensts."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# Erste Schritte mit der Azure Batch-Befehlszeilenschnittstelle

Die plattformübergreifende Azure-Befehlszeilenschnittstelle (Azure Command-Line Interface, Azure CLI) ermöglicht die Verwaltung Ihrer Batch-Konten und -Ressourcen wie Pools, Aufträge und Aufgaben in Linux-, Mac- und Windows-Befehlsshells. Mit der Azure-Befehlszeilenschnittstelle können Sie viele der Aufgaben durchführen und skripten, die Sie mit den Batch-APIs, dem Azure-Portal und den Batch PowerShell-Cmdlets durchführen.

Diesem Artikel liegt die Version 0.10.5 der Azure-Befehlszeilenschnittstelle zugrunde.

## Voraussetzungen

* [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)

* [Herstellen einer Verbindung zwischen der Azure-Befehlszeilenschnittstelle und Ihrem Azure-Abonnement](../xplat-cli-connect.md)

* Wechseln Sie in den **Ressource Manager-Modus**: `azure config mode arm`

>[AZURE.TIP] Es empfiehlt sich, die Installation der Azure-Befehlszeilenschnittstelle regelmäßig zu aktualisieren, um von Dienstupdates und Verbesserungen zu profitieren.

## Hilfe zu Befehlen

Sie können Hilfetext für jeden Befehl der Azure-Befehlszeilenschnittstelle anzeigen, indem Sie `-h` als einzige Option an den Befehl anhängen. Beispiel:

* Geben Sie Folgendes ein, um Hilfe zum Befehl `azure` zu erhalten: `azure -h`
* Verwenden Sie Folgendes, um eine Liste mit allen Batch-Befehlen der Befehlszeilenschnittstelle zu erhalten: `azure batch -h`
* Geben Sie Folgendes ein, um ein Batch-Konto zu erstellen: `azure batch account create -h`

Verwenden Sie im Zweifelsfall die Befehlszeilenoption `-h`, um Hilfe zu einem Befehl der Azure-Befehlszeilenschnittstelle zu erhalten.

## Erstellen eines Batch-Kontos

Syntax:

    azure batch account create [options] <name>

Beispiel:

	azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Erstellt ein neues Batch-Konto mit den angegebenen Parametern. Sie müssen mindestens einen Speicherort, eine Ressourcengruppe und einen Kontonamen angeben. Falls Sie noch nicht über eine Ressourcengruppe verfügen, führen Sie `azure group create` aus, um eine Ressourcengruppe zu erstellen, und geben Sie für die Option `--location` eine der Azure-Regionen (beispielsweise „USA, Westen“) an. Beispiel:

	azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Der Name des Batch-Kontos muss innerhalb der Azure-Region, in der das Konto erstellt wird, eindeutig sein. Er darf nur Kleinbuchstaben und Ziffern enthalten und muss 3 bis 24 Zeichen lang sein. Sonderzeichen wie `-` oder `_` sind in Batch-Kontonamen nicht zulässig.

### Verknüpftes Speicherkonto (autostorage)

Bei der Erstellung können Sie optional ein Speicherkonto vom Typ **Allgemein** mit Ihrem Batch-Konto verknüpfen. Das Feature [Anwendungspakete](batch-application-packages.md) von Batch verwendet ebenso wie die .NET-Bibliothek [Batch-Dateikonventionen ](batch-task-output.md) Blobspeicher in einem verknüpften allgemeinen Speicherkonto. Diese optionalen Features unterstützen Sie beim Bereitstellen der von Ihren Batch-Aufgaben ausgeführten Anwendungen und Beibehalten der von ihnen erzeugten Daten.

Wenn Sie bei der Erstellung ein vorhandenes Azure Storage-Konto mit einem neuen Batch-Konto verknüpfen möchten, geben Sie die Option `--autostorage-account-id` an. Für diese Option muss die vollqualifizierte Ressourcen-ID des Speicherkontos angegeben werden.

Zeigen Sie zunächst die Details Ihres Speicherkontos an:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Verwenden Sie dann den **Url**-Wert für die Option `--autostorage-account-id`. Der Url-Wert beginnt mit „/Subscriptions/“ und enthält Ihre Abonnement-ID und den Ressourcenpfad zum Speicherkonto:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## Löschen eines Batch-Kontos

Syntax:

    azure batch account delete [options] <name>

Beispiel:

	azure batch account delete --resource-group "resgroup001" "batchaccount001"

Löscht das angegebene Batch-Konto. Bestätigen Sie ggf., dass Sie das Konto entfernen möchten. (Das Entfernen eines Kontos kann etwas dauern.)

## Verwalten von Kontozugriffsschlüsseln

Zum [Erstellen und Ändern von Ressourcen](#create-and-modify-batch-resources) in Ihrem Batch-Konto benötigen Sie einen Zugriffsschlüssel.

### Auflisten von Zugriffsschlüsseln

Syntax:

	azure batch account keys list [options] <name>

Beispiel:

	azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Listet die Kontoschlüssel für das angegebene Batch-Konto auf.

### Generieren eines neuen Zugriffsschlüssels

Syntax:

    azure batch account keys renew [options] --<primary|secondary> <name>

Beispiel:

	azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Generiert den angegebenen Kontoschlüssel für das angegebene Batch-Konto neu.

## Erstellen und Ändern von Batch-Ressourcen

Mithilfe der Azure-Befehlszeilenschnittstelle können Sie Batch-Ressourcen wie Pools, Computeknoten, Aufträge und Aufgaben erstellen, lesen, aktualisieren und löschen (Create, Read, Update, Delete, CRUD). Bei diesen CRUD-Vorgängen müssen Sie Ihren Batch-Kontonamen, Zugriffsschlüssel und Endpunkt angeben. Diese Werte können Sie mithilfe der Optionen `-a`, `-k` und `-u` angeben. Alternativ können Sie auch [Umgebungsvariablen](#credential-environment-variables) festlegen, die dann automatisch von der Befehlszeilenschnittstelle verwendet werden (sofern aufgefüllt).

### Umgebungsvariablen für Anmeldeinformationen

Sie können die Umgebungsvariablen `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY` und `AZURE_BATCH_ENDPOINT` festlegen, anstatt in der Befehlszeile für jeden Befehl die Optionen `-a`, `-k` und `-u` anzugeben. Die Batch-Befehlszeilenschnittstelle verwendet dann diese Variablen (sofern festgelegt), sodass die Optionen `-a`, `-k` und `-u` nicht mehr angegeben werden müssen. In weiteren Verlauf des Artikels wird davon ausgegangen, dass Sie diese Umgebungsvariablen verwenden.

>[AZURE.TIP] Listen Sie mit `azure batch account keys list` Ihre Schlüssel auf, und zeigen Sie mit `azure batch account show` den Endpunkt des Kontos an.

### JSON-Dateien

Beim Erstellen von Batch-Ressourcen wie Pools und Aufträgen können Sie eine JSON-Datei mit der Konfiguration der neuen Ressource angeben, anstatt die entsprechenden Parameter als Befehlszeilenoptionen zu übergeben. Beispiel:

`azure batch pool create my_batch_pool.json`

Viele Ressourcenerstellungsvorgänge können zwar allein mithilfe von Befehlszeilenoptionen ausgeführt werden, manche Features erfordern jedoch die Verwendung einer JSON-Datei mit den Ressourcendetails. Eine JSON-Datei ist beispielsweise erforderlich, wenn Sie Ressourcendateien für eine Startaufgabe angeben möchten.

Den JSON-Code für die Ressourcenerstellung finden Sie auf MSDN in der [Batch REST-API-Referenz][rest_api]. Jedes Thema vom Typ „Hinzufügen von *Ressourcentyp* “ enthält JSON-Code zum Erstellen der entsprechenden Ressource. Diesen Code können Sie als Vorlage für Ihre JSON-Dateien verwenden. Den JSON-Code für die Poolerstellung finden Sie etwa unter [Add a pool to an account][rest_add_pool] \(Hinzufügen eines Pools zu einem Konto).

>[AZURE.NOTE] Wenn Sie bei der Ressourcenerstellung eine JSON-Datei angeben, werden alle anderen Parameter, die Sie an der Befehlszeile für diese Ressource angeben, ignoriert.

## Erstellen eines Pools

Syntax:

    azure batch pool create [options] [json-file]

Konfiguration des virtuellen Computers (Beispiel):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Clouddienstkonfiguration (Beispiel):

	azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Erstellt im Batch-Dienst einen Pool mit Computeknoten.

Wie in der [Übersicht über Batch-Features](batch-api-basics.md#pool) erwähnt, stehen Ihnen bei der Wahl eines Betriebssystems für die Knoten im Pool zwei Optionen zur Verfügung: **Konfiguration des virtuellen Computers** und **Clouddienstkonfiguration**. Verwenden Sie die Optionen vom Typ `--image-*`, um VM-Konfigurationspools zu erstellen, und `--os-family`, um Clouddienstkonfigurations-Pools zu erstellen. Die Optionen `--os-family` und `--image-*` können nicht gemeinsam angegeben werden.

Sie können poolspezifische [Anwendungspakete](batch-application-packages.md) und die Befehlszeile für eine [Startaufgabe](batch-api-basics.md#start-task) angeben. Zum Angeben von Ressourcendateien für die Startaufgabe müssen Sie hingegen eine [JSON-Datei](#json-files) verwenden.

Befehl zum Löschen eines Pools:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Suchen Sie in der [Liste mit den VM-Images](batch-linux-nodes.md#list-of-virtual-machine-images) nach geeigneten Werten für die Optionen vom Typ `--image-*`.

## Erstellen eines Auftrags

Syntax:

    azure batch job create [options] [json-file]

Beispiel:

    azure batch job create --id "job001" --pool-id "pool001"

Fügt dem Batch-Konto einen Auftrag hinzu und gibt den Pool an, für den die entsprechenden Aufgaben ausgeführt werden sollen.

Befehl zum Löschen eines Auftrags:

    azure batch job delete [job-id]

## Auflisten von Pools, Aufträgen, Aufgaben und anderen Ressourcen

Jeder Batch-Ressourcentyp unterstützt einen Befehl vom Typ `list`, um Ihr Batch-Konto abzufragen und Ressourcen des entsprechenden Typs aufzulisten. So können Sie beispielsweise die Pools in Ihrem Konto und die Aufgaben in einem Auftrag auflisten:

    azure batch pool list
    azure batch task list --job-id "job001"

### Effizientes Auflisten von Ressourcen

Zur Beschleunigung von Abfragen können Sie bei Vorgängen vom Typ `list` die Klauseloptionen **select** (Auswählen), **filter** (Filtern) und **expand** (Erweitern) angeben. Mithilfe dieser Optionen können Sie die vom Batch-Dienst zurückgegebene Datenmenge beschränken. Da sämtliche Filtervorgänge auf dem Server stattfinden, werden nur die für Sie interessanten Daten übermittelt. Verwenden Sie diese Klauseln, um bei Auflistungsvorgängen Bandbreite (und Zeit) zu sparen.

Der folgende Befehl gibt beispielsweise nur Pools zurück, deren ID mit „renderTask“ beginnt:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Die Batch-Befehlszeilenschnittstelle unterstützt alle drei vom Batch-Dienst unterstützten Klauseln:

* `--select-clause [select-clause]`: Gibt für jede Entität eine Teilmenge der Eigenschaften zurück.
* `--filter-clause [filter-clause]`: Gibt nur Entitäten zurück, die dem angegebenen OData-Ausdruck entsprechen.
* `--expand-clause [expand-clause]`: Ruft die Entitätsinformationen in einem einzelnen zugrunde liegenden REST-Aufruf ab. Die expand-Klausel unterstützt derzeit nur die `stats`-Eigenschaft.

Ausführliche Informationen zu den drei Klauseln und zu deren Verwendung bei Listenabfragen finden Sie unter [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md).

## Anwendungspaketverwaltung

Anwendungspakete ermöglichen eine vereinfachte Anwendungsbereitstellung auf den Computeknoten in Ihren Pools. Mit der Azure-Befehlszeilenschnittstelle können Sie Anwendungspakete hochladen, Paketversionen verwalten und Pakete löschen.

Führen Sie zum Erstellen einer neuen Anwendung und zum Hinzufügen einer Paketversion die folgenden Schritte aus:

Anwendung **erstellen**:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

Anwendungspaket **hinzufügen**:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

Paket **aktivieren**:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

**Standardversion** für die Anwendung festlegen:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### Bereitstellen eines Anwendungspakets

Beim Erstellen eines neuen Pools können Sie Anwendungspakete angeben, die Sie bereitstellen möchten. Wenn Sie ein Paket bei der Poolerstellung angeben, wird es auf den einzelnen Knoten bereitgestellt, sobald diese dem Pool hinzugefügt werden. Pakete werden auch bereitgestellt, wenn ein Knoten neu gestartet oder ein Reimaging für den Knoten durchgeführt wird.

Geben Sie bei der Poolerstellung die `--app-package-ref`-Option an, um ein Anwendungspaket auf den Knoten des Pools bereitzustellen, wenn diese dem Pool hinzugefügt werden. Die `--app-package-ref`-Option akzeptiert eine durch Semikolons getrennte Liste mit Anwendung-IDs, die auf den Computeknoten bereitgestellt werden sollen.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Wenn Sie einen Pool mithilfe von Befehlszeilenoptionen erstellen, können Sie derzeit nicht angeben, *welche* Anwendungspaketversion auf den Computeknoten bereitgestellt werden soll (beispielsweise „1.10-beta3“). Daher müssen Sie mit `azure batch application set [options] --default-version <version-id>` vor der Poolerstellung (siehe vorheriger Abschnitt) zunächst eine Standardversion für die Anwendung angeben. Wenn Sie bei der Poolerstellung anstelle von Befehlszeilenoptionen eine [JSON-Datei](#json-files) verwenden, können Sie hingegen eine Paketversion für den Pool angeben.

Weitere Informationen zu Anwendungspaketen finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

>[AZURE.IMPORTANT] Damit Sie Anwendungspakete verwenden können, müssen Sie zunächst ein [Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen](#linked-storage-account-autostorage).

### Aktualisieren der Anwendungspakete eines Pools

Verwenden Sie zum Aktualisieren der zugewiesenen Anwendungen eines vorhandenen Pools den `azure batch pool set`-Befehl mit der `--app-package-ref`-Option:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Um das neue Anwendungspaket für Computeknoten bereitzustellen, die sich bereits in einem vorhandenen Pool befinden, müssen Sie diese Knoten neu starten oder ein Reimaging für diese Knoten durchführen:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Eine Liste mit den Knoten in einem Pool sowie deren IDs können Sie mit `azure batch node list` abrufen.

Beachten Sie, dass die Anwendung vor der Bereitstellung bereits mit einer Standardversion konfiguriert worden sein muss (`azure batch application set [options] --default-version <version-id>`).

## Tipps zur Problembehandlung

Dieser Abschnitt enthält Ressourcen zur Behandlung von Problemen mit der Azure-Befehlszeilenschnittstelle. Er löst nicht unbedingt alle auftretenden Probleme, ermöglicht aber unter Umständen eine Eingrenzung der Ursache und unterstützt Sie bei der Suche nach Hilferessourcen.

* Verwenden Sie `-h`, um **Hilfetext** für Befehle der Befehlszeilenschnittstelle anzuzeigen.

* Verwenden Sie `-v` und `-vv`, um eine **ausführliche** Befehlsausgabe zu erhalten. Mit `-vv` wird die Ausgabe noch ausführlicher und enthält die tatsächlichen REST-Anforderungen und -Antworten. Mit diesen praktischen Switches können Sie die vollständige Fehlerausgabe anzeigen.

* Mit der Option `--json` können Sie die **Befehlsausgabe als JSON** anzeigen. `azure batch pool show "pool001" --json` zeigt beispielsweise die Eigenschaften von „pool001“ im JSON-Format an. Diese Ausgabe können Sie dann kopieren und ändern, um sie in einem Element vom Typ `--json-file` zu verwenden. (Weitere Informationen finden Sie weiter oben in diesem Artikel unter [JSON-Dateien](#json-files).)

* Das [Batch-Forum auf MSDN][batch_forum] ist eine großartige Hilferessource und wird aufmerksam von den Mitgliedern des Batch-Teams verfolgt. Stellen Sie Ihre Fragen dort, wenn bei Ihnen Probleme auftreten oder Sie Hilfe bei einem bestimmten Vorgang benötigen.

* Von der Azure-Befehlszeilenschnittstelle werden derzeit nicht alle Batch-Ressourcenvorgänge unterstützt. So kann momentan beispielsweise nur die Paket-ID eines Anwendungspakets für einen Pool angegeben werden, aber keine *Version*. In solchen Fällen müssen Sie anstelle von Befehlszeilenoptionen für Ihren Befehl ggf. ein Element vom Typ `--json-file` verwenden. Verwenden Sie immer die neueste Version der Befehlszeilenschnittstelle, um zukünftige Erweiterungen nutzen zu können.

## Nächste Schritte

*  Unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md) erfahren Sie, wie Sie dieses Feature zum Verwalten und Bereitstellen der Anwendungen verwenden, die Sie auf Batch-Computeknoten ausführen.

* Unter [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md) finden Sie weitere Informationen dazu, wie Sie die Anzahl von Elementen sowie die Art der Informationen verringern, die bei Abfragen an Batch zurückgegeben werden.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/de-DE/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx

<!---HONumber=AcomDC_1005_2016-->