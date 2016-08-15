<properties
	pageTitle="Einfache Anwendungsinstallation und -verwaltung in Azure Batch | Microsoft Azure"
	description="Verwenden Sie das Feature „Anwendungspakete“ von Azure Batch zur einfachen Verwaltung mehrerer Anwendungen und Versionen für die Installation auf Batch-Serverknoten."
	services="batch"
	documentationCenter=".net"
	authors="mmacy"
	manager="timlt"
	editor="" />

<tags
	ms.service="batch"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows"
	ms.workload="big-compute"
	ms.date="06/30/2016"
	ms.author="marsma" />

# Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen

Das Feature „Anwendungspakete“ von Azure Batch ermöglicht eine einfache Verwaltung und Bereitstellung von Anwendungen für die in Ihrem Pool benötigten Serverknoten. Mit Anwendungspaketen können Sie mehrere Versionen der Anwendungen hochladen und verwalten (einschließlich den von Ihren Aufgaben ausgeführten Binärdaten und den dazugehörigen Unterstützungsdateien). Die Anwendungen können Sie dann für die Computeknoten in Ihrem Pool bereitstellen.

In diesem Artikel erfahren Sie, wie Sie Anwendungspakete über das Azure-Portal hochladen und verwalten. Außerdem erfahren Sie, wie Sie sie unter Verwendung der [Batch .NET][api_net]-Bibliothek auf den Computeknotens eines Pools installieren.

> [AZURE.NOTE] Das hier beschriebene Feature „Anwendungspakete“ ersetzt das Feature „Batch-Apps“, das in früheren Versionen des Diensts verfügbar war.

## Anforderungen für Anwendungspakete

Das in diesem Artikel beschriebene Feature „Anwendungspakete“ ist *nur* mit Batch-Pools kompatibel, die nach dem 10. März 2016 erstellt wurden. Anwendungspakete werden nicht für Serverknoten bereitgestellt, die sich in Pools befinden, die vor diesem Datum erstellt wurden.

Dieses Feature wurde in [Batch-REST-API][api_rest]-Version 2015-12-01.2.2 eingeführt. Die entsprechende [Batch .NET][api_net]-Bibliotheksversion ist 3.1.0. Sie sollten bei der Arbeit mit Batch immer die neueste Version der API verwenden.

> [AZURE.IMPORTANT] Anwendungspakete werden derzeit nur von *CloudServiceConfiguration*-Pools unterstützt. In Pools, die unter Verwendung von VirtualMachineConfiguration-Images erstellt wurden, können keine Anwendungspakete verwendet werden. Im Abschnitt [Konfiguration des virtuellen Computers](batch-linux-nodes.md#virtual-machine-configuration) des Artikels [Bereitstellen von Linux-Computeknoten in Azure Batch-Pools](batch-linux-nodes.md) erhalten Sie weitere Informationen zu den beiden verschiedenen Konfigurationen.

## Informationen zu Anwendungen und Anwendungspaketen

In Azure Batch ist eine *Anwendung* ein Satz von Binärdateien mit Versionsangabe, die automatisch auf die Computeknoten in Ihrem Pool heruntergeladen werden können. Ein *Anwendungspaket* ist ein *bestimmter Satz* dieser Binärdateien und stellt eine bestimmte *Version* der Anwendung dar.

![Übersichtsdiagramm zu Anwendungen und Anwendungspaketen][1]

### Anwendungen

Eine Anwendung in Batch enthält mindestens ein Anwendungspaket und gibt Konfigurationsoptionen für die Anwendung an. So gibt eine Anwendung etwa die Standardversion des Anwendungspakets an, die auf Computeknoten installiert werden soll, und sie gibt an, ob die dazugehörigen Pakete aktualisiert oder gelöscht werden können.

### Anwendungspakete

Ein Anwendungspaket ist eine ZIP-Datei mit den Binärdateien der Anwendung und den Unterstützungsdateien, die für die Ausführung Ihrer Aufgaben erforderlich sind. Jedes Anwendungspaket stellt eine bestimmte Version der Anwendung dar. Wenn Sie im Batch-Dienst einen Pool erstellen, können Sie Anwendungen und (optional) eine Version angeben. Die Anwendungspakete werden automatisch heruntergeladen und auf den einzelnen Knoten extrahiert, wenn diese in den Pool aufgenommen werden.

> [AZURE.IMPORTANT] Es gibt Einschränkungen für die Anzahl der Anwendungen und Anwendungspakete in einem Batch-Konto sowie bezüglich der maximalen Anwendungspaketgröße. Ausführlichere Informationen zu diesen Einschränkungen finden Sie unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md).

### Vorteile von Anwendungspaketen

Anwendungspakete können sowohl den Code in Ihrer Batch-Lösung vereinfachen als auch den Verwaltungsaufwand für die Anwendungen verringern, die Ihre Aufgaben ausführen.

Mit Anwendungspaketen muss die Startaufgabe Ihres Pools keine lange Liste einzelner, auf den Knoten zu installierender Ressourcendateien angeben. Sie müssen nicht manuell mehrere Versionen dieser Dateien in Azure Storage oder auf Ihren Knoten verwalten. Sie müssen sich ebenso keine Gedanken über das Generieren von [SAS-URLs](../storage/storage-dotnet-shared-access-signature-part-1.md) für den Zugriff auf die Dateien in Ihrem Azure Storage-Konto machen.

Batch arbeitet im Hintergrund mit Azure Storage zusammen, um Anwendungspakete zu speichern und für Computeknoten bereitzustellen und dadurch sowohl Ihren Code zu vereinfachen als auch den Verwaltungsaufwand zu verringern.

## Hochladen und Verwalten von Anwendungen

Im Azure-Portal können Sie Anwendungspakete hinzufügen, aktualisieren und löschen. Sie können für jede Anwendung eine Standardversion konfigurieren.

In den nächsten Abschnitten erfahren Sie, wie Sie ein Storage-Konto mit Ihrem Batch-Konto verknüpfen und anschließend die im Azure-Portal verfügbaren Paketverwaltungsfunktionen überprüfen. Danach erfahren Sie dann, wie Sie diese Pakete mit der [Batch .NET][api_net]-Bibliothek für Computeknoten bereitstellen.

### Verknüpfen eines Storage-Kontos

Vor der Verwendung von Anwendungspaketen müssen Sie zunächst ein Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen. Falls Sie noch kein Storage-Konto für Ihr Batch-Konto konfiguriert haben, zeigt das Azure-Portal eine Warnung an, wenn Sie auf dem Blatt **Batch-Konto** zum ersten Mal auf die Kachel **Anwendungen** klicken.

> [AZURE.IMPORTANT] Von Batch wird derzeit *ausschließlich* der Speicherkontotyp **Allgemein** unterstützt, wie in [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md) unter Schritt 5 ([Erstellen Sie ein Speicherkonto.](../storage/storage-create-storage-account.md#create-a-storage-account)) beschrieben. Verwenden Sie beim Verknüpfen eines Azure Storage-Kontos mit Ihrem Batch-Konto *ausschließlich* ein Speicherkonto vom Typ **Allgemein**.

![Warnung im Azure-Portal, dass kein Storage-Konto konfiguriert ist][9]

Der Batch-Dienst verwendet das zugeordnete Storage-Konto für Speicherung und Abruf von Anwendungspaketen. Nachdem Sie die beiden Konten verknüpft haben, kann Batch die im verknüpften Storage-Konto gespeicherten Pakete automatisch für Ihre Computeknoten bereitstellen. Klicken Sie auf dem Blatt **Warnung** auf **Speicherkontoeinstellungen** und anschließend auf dem Blatt **Speicherkonto** auf **Speicherkonto**, um ein Speicherkonto mit Ihrem Batch-Konto zu verknüpfen.

![Auswählen des Storage-Kontoblatts im Azure-Portal][10]

Es empfiehlt sich ein Speicherkonto *speziell* für die Verwendung mit Ihrem Batch-Konto zu erstellen und hier auszuwählen. Ausführliche Informationen zum Erstellen eines Speicherkontos finden Sie in den [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md) unter „Erstellen Sie ein Speicherkonto.“. Wenn Sie ein Speicherkonto erstellt haben, können Sie es anschließend mithilfe des Blatts **Speicherkonto** mit Ihrem Batch-Konto verknüpfen.

> [AZURE.WARNING] Da Batch Ihre Anwendungspakete mit Azure Storage speichert, werden Ihnen für die Blockblobdaten die [üblichen Gebühren][storage_pricing] in Rechnung gestellt. Beachten Sie unbedingt Größe und Anzahl der Anwendungspakete, und entfernen Sie in regelmäßigen Abständen veraltete Pakete, um Kosten zu minimieren.

### Anzeigen aktueller Anwendungen

Klicken Sie auf dem Blatt **Batch-Konto** auf die Kachel **Anwendungen**, um die Anwendungen in Ihrem Batch-Konto anzuzeigen.

![Kachel „Anwendungen“][2]

Daraufhin wird das Blatt **Anwendungen** geöffnet:

![Auflisten von Anwendungen][3]

Das Blatt **Anwendungen** zeigt neben der ID der einzelnen Anwendungen in Ihrem Konto auch die folgenden Eigenschaften an:

* **Pakete**: Die Anzahl von Versionen, die dieser Anwendung zugeordnet sind.
* **Standardversion**: Die Version, die installiert wird, wenn Sie beim Festlegen der Anwendung für einen Pool keine Version angeben. Diese Einstellung ist optional.
* **Updates zulassen**: Gibt an, ob Pakete aktualisiert, gelöscht und hinzugefügt werden dürfen. Ist diese Option auf **Nein** festgelegt, sind Paketaktualisierungen und -löschungen für die Anwendung deaktiviert. In diesem Fall können nur neue Anwendungspaketversionen hinzugefügt werden. Die Option ist standardmäßig auf **Ja** festgelegt.

### Anzeigen von Anwendungsdetails

Klicken Sie auf dem Blatt **Anwendungen** auf eine Anwendung, um das Blatt mit den Anwendungsdetails zu öffnen.

![Anwendungsdetails][4]

Auf dem Blatt mit den Details für die Anwendung können Sie die folgenden Einstellungen für Ihre Anwendung konfigurieren.

* **Updates zulassen**: Geben Sie an, ob die Anwendungspakete aktualisiert oder gelöscht werden können. Weitere Informationen finden Sie weiter unten in diesem Artikel unter „Aktualisieren oder Löschen eines Anwendungspakets“.
* **Standardversion**: Geben Sie ein Standardanwendungspaket für die Bereitstellung auf Computeknoten an.
* **Anzeigename**: Geben Sie einen benutzerfreundlichen Namen an, den Ihre Batch-Lösung zur Anzeige von anwendungsbezogenen Informationen verwenden kann (beispielsweise auf der Benutzeroberfläche eines Diensts, den Sie für Ihre Kunden über Batch bereitstellen).

### Hinzufügen einer neuen Anwendung

Fügen Sie zum Erstellen einer neuen Anwendung ein Anwendungspaket mit einer neuen, eindeutigen ID hinzu. Das erste Anwendungspaket, das Sie mit der neuen Anwendungs-ID hinzufügen, wird auch zur Erstellung der neuen Anwendung verwendet.

Klicken Sie auf dem Blatt **Anwendungen** auf **Hinzufügen**, um das Blatt **Neue Anwendung** zu öffnen.

![Blatt „Neue Anwendung“ im Azure-Portal][5]

Das Blatt **Neue Anwendung** beinhaltet die folgenden Felder zur Angabe der Einstellungen Ihrer neuen Anwendung und des neuen Anwendungspakets:

**Anwendungs-ID**

Dieses Feld gibt die ID Ihrer neuen Anwendung an und unterliegt den standardmäßigen Gültigkeitsprüfungsregeln für Azure Batch-IDs:

* Sie kann eine beliebige Kombination von alphanumerischen Zeichen einschließlich Bindestrichen und Unterstrichen enthalten.
* Sie darf nicht mehr als 64 Zeichen enthalten.
* Sie muss innerhalb des Batch-Kontos eindeutig sein.
* Die Groß-/Kleinschreibung wird beibehalten und nicht unterschieden.

**Version**

Gibt die Version des Anwendungspakets an, das Sie hochladen. Für die Versionszeichenfolge gelten die folgenden Gültigkeitsprüfungsregeln:

* Sie kann eine beliebige Kombination von alphanumerischen Zeichen einschließlich Bindestrichen, Unterstrichen und Punkten enthalten.
* Sie darf nicht mehr als 64 Zeichen enthalten.
* Sie muss innerhalb der Anwendung eindeutig sein.
* Groß-/Kleinschreibung wird beibehalten und nicht unterschieden.

**Anwendungspaket**

Dieses Feld gibt die ZIP-Datei an, die die Binärdateien der Anwendung und jegliche unterstützenden Dateien enthält, die für die Ausführung der Anwendung erforderlich sind. Klicken Sie auf das Feld **Datei auswählen** oder auf das Symbol zum Durchsuchen, um zum gewünschten Speicherort zu navigieren und eine ZIP-Datei mit den Anwendungsdateien auszuwählen.

Klicken Sie nach dem Auswählen einer Datei auf **OK**, um mit dem Hochladen an Azure Storage zu beginnen. Wenn das Hochladen abgeschlossen ist, werden Sie benachrichtigt, und das Blatt wird geschlossen. Dieser Vorgang kann je nach Größe der hochzuladenden Datei und der Geschwindigkeit Ihrer Netzwerkverbindung einige Zeit dauern.

> [AZURE.WARNING] Schließen Sie das Blatt **Neue Anwendung** erst nach Abschluss des Uploadvorgangs. Andernfalls wir das Hochladen abgebrochen.

### Hinzufügen eines neuen Anwendungspakets

Wenn Sie für eine vorhandene Anwendung eine neue Anwendungspaketversion hinzufügen möchten, wählen Sie auf dem Blatt **Anwendungen** eine Anwendung aus, und klicken Sie auf **Pakete** > **Hinzufügen**, um das Blatt **Paket hinzufügen** zu öffnen.

![Hinzufügen des Anwendungspaketblatts im Azure-Portal][8]

Wie Sie sehen, entsprechen die Felder den Feldern des Blatts **Neue Anwendung** (mit Ausnahme des deaktivierten Felds **Anwendungs-ID**). Geben Sie wie bei der neuen Anwendung die **Version** für das neue Paket ein, navigieren Sie zu Ihrer ZIP-Datei mit dem **Anwendungspaket**, und klicken Sie anschließend zum Hochladen des Pakets auf **OK**.

### Aktualisieren oder Löschen eines Anwendungspakets

Öffnen Sie zum Aktualisieren oder Löschen eines vorhandenen Anwendungspakets das Blatt mit den Details für die Anwendung, klicken Sie auf **Pakete**, um das Blatt **Pakete** zu öffnen, klicken Sie auf die **Auslassungspunkte** in der Zeile des Anwendungspakets, das Sie ändern möchten, und wählen Sie die gewünschte Aktion aus.

![Aktualisieren oder Löschen des Pakets im Azure-Portal][7]

**Aktualisieren**

Durch Klicken auf **Aktualisieren** wird das Blatt *Paket aktualisieren* angezeigt. Dieses Blatt ist vergleichbar mit dem Blatt *Neues Anwendungspaket*, hier ist jedoch nur das Paketauswahlfeld aktiviert, sodass Sie eine neue ZIP-Datei zum Hochladen angeben können.

![Aktualisieren des Paketblatts im Azure-Portal][11]

**Löschen**

Wenn Sie auf **Löschen** klicken, werden Sie aufgefordert, die Löschung der Paketversion zu bestätigen, und Batch löscht das Paket aus Azure Storage. Wenn Sie die Standardversion einer Anwendung löschen, wird die Einstellung **Standardversion** für die Anwendung entfernt.

![Löschen der Anwendung][12]

## Installieren von Anwendungen auf Serverknoten

Nachdem wir nun das Hochladen und Verwalten von Anwendungspaketen über das Azure-Portal behandelt haben, können wir uns der eigentlichen Bereitstellung für Computeknoten und der Ausführung mit Batch-Aufgaben widmen.

Geben Sie mindestens einen Anwendungspaket*verweis* für den Pool an, um ein Anwendungspaket auf den Computeknoten in einem Pool zu installieren. Fügen Sie in Batch .NET einem neu erstellten oder bereits vorhandenen Pool mindestens einen Verweis vom Typ [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] hinzu.

Die [ApplicationPackageReference][net_pkgref]-Klasse gibt eine Anwendungs-ID und Anwendungsversion für die Installation auf den Computeknoten eines Pools an.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

Die Anwendungspakete, die Sie für einen Pool angeben, werden auf jedem Computeknoten installiert, wenn der jeweilige Knoten dem Pool hinzugefügt und der Knoten neu gestartet oder ein Reimaging durchgeführt wird. Falls die Bereitstellung eines Anwendungspakets aus einem beliebigem Grund nicht erfolgreich ist, markiert der Batch-Dienst den Knoten als [unbrauchbar][net_nodestate], sodass keine Aufgaben zur Ausführung auf diesem Knoten geplant werden. In diesem Fall empfiehlt es sich, einen **Neustart** des Knotens durchzuführen, um die Initiierung der Paketbereitstellung zu wiederholen. Durch den Neustart des Knotens wird auch die Aufgabenplanung wieder aktiviert.

## Ausführen der installierten Anwendungen

Wenn die einzelnen Computeknoten einem Pool hinzugefügt oder neu gestartet werden bzw. wenn ein Reimaging für sie durchgeführt wird, werden die angegebenen Pakete heruntergeladen und in einem benannten Verzeichnis innerhalb von `AZ_BATCH_ROOT_DIR` auf dem Knoten extrahiert. Batch erstellt auch eine Umgebungsvariable für Ihre Aufgabenbefehlszeilen, die Sie beim Aufrufen der Binärdateien der Anwendung verwenden können. Für diese Variable gilt das folgende Benennungsschema:

`AZ_BATCH_APP_PACKAGE_appid#version`

Wenn Sie also beispielsweise angeben, dass Version 2.7 der Anwendung *Blender* installiert werden soll, können Ihre Aufgaben über die folgende Umgebungsvariable in ihren Befehlszeilen auf die Binärdateien verweisen:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Wenn Ihre Anwendung eine Standardversion angibt, können Sie ohne Versionszeichenfolgen-Suffix auf die Umgebungsvariable zugreifen. Wenn Sie also im Azure-Portal beispielsweise die Standardversion 2.7 für die Anwendung *Blender* angegeben hatten, können Ihre Aufgaben auf die folgende Umgebungsvariable verweisen:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Der folgende Codeausschnitt zeigt, wie eine Aufgabe konfiguriert werden kann, wenn eine Standardversion für die Anwendung *Blender* angegeben wurde.

```csharp
string taskId = "blendertask01";
string commandLine = @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -my -command -args";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Im Artikel [Übersicht über Azure Batch-Features](batch-api-basics.md) finden Sie unter „Umgebungseinstellungen für Aufgaben“ weitere Informationen zu Umgebungseinstellungen für Computeknoten.

## Aktualisieren der Anwendungspakete eines Pools

Wenn ein vorhandener Pool bereits mit einem Anwendungspaket konfiguriert wurde, können Sie ein neues Paket für den Pool angeben. Wenn Sie einen neuen Paketverweis für einen Pool angeben, gilt Folgendes:

* Alle neuen Knoten, die dem Pool hinzugefügt werden, sowie jegliche bereits vorhandene Knoten, die neu gestartet werden bzw. für die ein Reimaging durchgeführt wird, installieren das neu angegebene Paket.
* Serverknoten, die sich bereits im Pool befinden, wenn Sie die Paketverweise aktualisieren, werden nicht automatisch im neuen Anwendungspaket installiert. Für diese Computeknoten muss ein Neustart oder ein Reimaging durchgeführt werden, um das neue Paket zu erhalten.
* Wenn ein neues Paket bereitgestellt wird, spiegeln die neu erstellten Umgebungsvariablen die neuen Anwendungspaketverweise wider.

In diesem Beispiel hat der vorhandene Pool die Version 2.7 der Anwendung *Blender* als eine seiner [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] konfiguriert. Wenn Sie die Knoten des Pools mit der Version 2.76b aktualisieren möchten, geben Sie eine neue [ApplicationPackageReference][net_pkgref] mit der neuen Version an, und committen Sie die Änderung.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nach dem Konfigurieren der neuen Version wird auf jedem *neuen* Knoten, der dem Pool beitritt, die Version 2.76b bereitgestellt. Um 2.76b auf den Knoten zu installieren, die sich *bereits im Pool befinden*, muss für die Knoten ein Neustart oder Reimaging durchgeführt werden. Beachten Sie, dass neu gestartete Knoten die Dateien aus vorherigen Paketbereitstellungen beibehalten.

## Auflisten der Anwendungen in einem Batch-Konto

Sie können die Anwendungen und ihre Pakete mit der Methode [ApplicationOperations][net_appops].[ListApplicationSummaries][net_appops_listappsummaries] in einem Batch-Konto auflisten.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## Zusammenfassung

Mit Anwendungspaketen können Sie Ihre Kunden dabei unterstützen, die passende Anwendungen für ihre Aufträge auszuwählen und genau anzugeben, welche Version beim Verarbeiten von Aufträgen mit Ihrem Batch-fähigen Dienst verwendet werden soll. Sie könnten Ihren Kunden auch die Möglichkeit bieten, ihre eigenen Anwendungen in Ihren Dienst hochzuladen und dort zu verfolgen.

## Nächste Schritte

* Die [Batch-REST-API][api_rest] unterstützt auch die Arbeit mit Anwendungspaketen. In [Hinzufügen eines Pools mit einem Konto][rest_add_pool] finden Sie unter dem Element [applicationPackageReferences][rest_add_pool_with_packages] beispielsweise Informationen zum Angeben von zu installierenden Paketen mithilfe der REST-API. Ausführliche Informationen zum Abrufen von Anwendungsinformationen mit der Batch-REST-API finden Sie unter [Client Applications][rest_applications] \(Clientanwendungen).

* Informieren Sie sich über das programmgesteuerte [Verwalten von Azure Batch-Konten und -Kontingenten mit Batch Management .NET](batch-management-dotnet.md). Die [Batch Management .NET][api_net_mgmt]-Bibliothek kann Features zum Erstellen und Löschen von Konten für Ihre Batch-Anwendung oder Ihren Dienst aktivieren.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Übersichtsdiagramm für Anwendungspakete"
[2]: ./media/batch-application-packages/app_pkg_02.png "Kachel „Anwendungen“ im Azure-Portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Blatt „Anwendungen“ im Azure-Portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Blatt „Anwendungsdetails“ im Azure-Portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Blatt „Neue Anwendung“ im Azure-Portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualisieren oder Löschen der Paketedropdownliste im Azure-Portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Blatt „Neues Anwendungspaket“ im Azure-Portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Warnung, dass kein verknüpftes Storage-Konto vorhanden ist"
[10]: ./media/batch-application-packages/app_pkg_10.png "Auswählen des Storage-Kontoblatts im Azure-Portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Aktualisieren des Paketblatts im Azure-Portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Dialogfeld zum Bestätigen der Paketlöschung im Azure-Portal"

<!---HONumber=AcomDC_0803_2016-->