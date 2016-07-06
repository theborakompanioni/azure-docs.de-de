<properties
	pageTitle="Übersicht über Azure Batch-Features für Entwickler | Microsoft Azure"
	description="Lernen Sie die Features des Batch-Diensts und seiner APIs aus der Sicht eines Entwicklers kennen."
	services="batch"
	documentationCenter=".net"
	authors="mmacy"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.devlang="multiple"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="big-compute"
	ms.date="06/29/2016"
	ms.author="marsma"/>

# Übersicht über Batch-Features für Entwickler

In dieser Übersicht über die Hauptkomponenten des Azure Batch-Diensts werden die wichtigsten Dienstfunktionen beschrieben, die Batch-Entwickler zum Erstellen paralleler Computelösungen in größerem Umfang verwenden können.

Sie werden viele der hier beschriebenen Ressourcen und Features nutzen. Dies gilt unabhängig davon, ob Sie eine verteilte Computeanwendung oder einen Computedienst mit direkter Ausgabe von [Batch-REST][batch_rest_api]-API-Aufrufen entwickeln oder eines der [Batch SDKs](batch-technical-overview.md#batch-development-apis) verwenden.

> [AZURE.TIP] Eine allgemeiner gehaltene Einführung in den Batch-Dienst finden Sie unter [Grundlagen von Azure Batch](batch-technical-overview.md).

## Workflow des Batch-Diensts

Der folgende allgemeine Workflow ist typisch für fast alle Anwendungen und Dienste, die den Batch-Dienst für die Verarbeitung von parallelen Workloads verwenden:

1. Laden Sie die **Datendateien**, die verarbeitet werden sollen, in ein [Azure Storage][azure_storage]-Konto hoch. Batch verfügt über integrierte Unterstützung für den Zugriff auf Azure-Blobspeicher, und Ihre Tasks können diese Dateien auf [Computeknoten](#compute-node) herunterladen, wenn die Tasks ausgeführt werden.

2. Laden Sie die **Anwendungsdateien** hoch, die von Ihren Tasks ausgeführt werden sollen. Bei diesen Dateien kann es sich um Binärdateien oder Skripts und ihre Abhängigkeiten handeln, und sie werden von den Tasks in Ihren Aufträgen ausgeführt. Diese Dateien können von den Tasks aus Ihrem Speicherkonto abgerufen und auf die Computeknoten heruntergeladen werden, oder Sie können das Feature [Anwendungspakete](#application-packages) von Batch für die Anwendungsverwaltung und -bereitstellung verwenden.

3. Erstellen Sie einen [Pool](#pool) aus Computeknoten. Beim Erstellen eines Pools geben Sie die Anzahl von Computeknoten für den Pool, deren Größe und das Betriebssystem an. Wenn die einzelnen Tasks des Auftrags ausgeführt werden, erfolgt die Zuweisung zur Ausführung auf einem der Knoten im Pool.

4. Erstellen Sie einen [Auftrag](#job). Ein Auftrag verwaltet eine Sammlung von Tasks, und Sie ordnen jeden Auftrag einem bestimmten Pool zu, in dem die Tasks des Auftrags ausgeführt werden.

5. Fügen Sie dem Auftrag [Tasks](#task) hinzu. Jeder Task führt die hochgeladene Anwendung bzw. das Skript aus, um die Datendateien zu verarbeiten, die aus Ihrem Speicherkonto heruntergeladen werden. Nachdem ein Task abgeschlossen ist, kann die Ausgabe jeweils in Azure Storage hochgeladen werden.

6. Überwachen Sie den Auftragsstatus, und rufen Sie die Taskausgabe aus Azure Storage ab.

In den folgenden Abschnitten erfahren Sie mehr zu den einzelnen Ressourcen, die im obigen Workflow genannt werden, sowie zu vielen weiteren Features von Batch, die Ihr verteiltes Rechenszenario unterstützen.

> [AZURE.NOTE] Sie benötigen ein [Batch-Konto](batch-account-create-portal.md) zum Verwenden des Batch-Diensts, und fast alle Lösungen verwenden ein [Azure Storage][azure_storage]-Konto zum Speichern und Abrufen von Dateien. Von Batch wird derzeit ausschließlich der Speicherkontotyp **Allgemein** unterstützt, wie in [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md#create-a-storage-account) unter Schritt 5 ([Erstellen Sie ein Speicherkonto.](../storage/storage-create-storage-account.md)) beschrieben.

## Komponenten des Batch-Diensts

Einige der folgenden Ressourcen – Konto, Computeknoten, Pool, Auftrag, Task – sind für alle Lösungen erforderlich, die den Batch-Dienst nutzen. Andere, wie etwa Auftragszeitpläne und Anwendungspakete, sind hilfreiche, aber optionale Funktionen.

- [Konto](#account)
- [Computeknoten](#compute-node)
- [Pool](#pool)
- [Job](#job)

  - [Auftragszeitpläne](#scheduled-jobs)

- [Aufgabe](#task)

  - [Startaufgabe](#start-task)
  - [Auftrags-Manager-Aufgabe](#job-manager-task)
  - [Tasks zur Auftragsvorbereitung und -freigabe](#job-preparation-and-release-tasks)
  - [Tasks mit mehreren Instanzen](#multi-instance-tasks)
  - [Abhängigkeiten von Aufgaben](#task-dependencies)

- [Anwendungspakete](#application-packages)

## Konto

Ein Batch-Konto ist eine eindeutig identifizierte Entität innerhalb des Batch-Diensts. Die gesamte Verarbeitung ist einem Batch-Konto zugeordnet. Beim Ausführen von Vorgängen mit dem Batch-Dienst benötigen Sie sowohl den Kontonamen als auch einen der dazugehörigen Kontoschlüssel. Sie können ein [Azure Batch-Konto im Azure-Portal erstellen und verwalten](batch-account-create-portal.md).

## Computeknoten

Ein Computeknoten ist ein virtueller Azure-Computer, der für die Verarbeitung eines Teils der Anwendungsworkload fest zugeordnet ist. Die Größe eines Knotens bestimmt die Anzahl von CPU-Kernen, die Speicherkapazität und die lokale Dateisystemgröße, die dem Knoten zugeordnet werden. Sie können Pools mit Windows- oder Linux-Knoten erstellen, indem Sie entweder Cloud Services oder VM-Marketplace-Images verwenden. Weitere Informationen zu diesen Optionen finden Sie unter [Pool](#pool).

Knoten können beliebige ausführbare Dateien oder Skripts ausführen, die von der Betriebssystemumgebung des Knotens unterstützt werden. Die sind beispielsweise EXE-, CMD-, BAT- und PowerShell-Skripts für Windows und Binärdateien sowie Shell- und Python-Skripts für Linux.

Alle Computeknoten in Batch enthalten außerdem Folgendes:

- Eine Standard[ordnerstruktur](#files-and-directories) und die dazugehörigen [Umgebungsvariablen](#environment-settings-for-tasks), die für Tasks verfügbar sind.
- Für die Zugriffssteuerung konfigurierte **Firewalleinstellungen**.
- [Remotezugriff](#connecting-to-compute-nodes) auf Windows-Knoten (RDP) und Linux-Knoten (SSH)

> [AZURE.NOTE] Die Unterstützung von Linux in Batch befindet sich derzeit in der Vorschauphase. Ausführlichere Informationen finden Sie unter [Bereitstellen von Linux-Computeknoten in Azure Batch-Pools](batch-linux-nodes.md).

## Pool

Ein Pool besteht aus einer Sammlung von Knoten, auf denen Ihre Anwendung ausgeführt wird. Der Pool kann manuell von Ihnen oder automatisch vom Batch-Dienst erstellt werden, wenn Sie die zu erbringende Arbeitsleistung angeben. Sie können einen Pool erstellen und verwalten, der den Ressourcenanforderungen Ihrer Anwendung entspricht. Pools können nur von dem Batch-Konto verwendet werden, in dem sie erstellt wurden. Ein Batch-Konto kann über mehrere Pools verfügen.

Azure Batch-Pools basieren auf der grundlegenden Azure-Computeplattform und bieten Funktionen für umfangreiche Zuordnungen, für die Anwendungsinstallation, die Datenverteilung und die Systemüberwachung sowie für die flexible Anpassung der Computeknotenanzahl innerhalb eines Pools ([Skalierung](#scaling-compute-resources)).

Jedem Knoten, der einem Pool hinzugefügt wird, werden ein eindeutiger Name und eine IP-Adresse zugewiesen. Beim Entfernen eines Knotens aus einem Pool gehen alle Änderungen am Betriebssystem oder an den Dateien verloren, und sein Name und die IP-Adresse werden zur zukünftigen Verwendung freigegeben. Wenn ein Knoten einen Pool verlässt, endet seine Lebensdauer.

Wenn Sie einen Pool erstellen, können Sie die folgenden Attribute angeben:

- **Betriebssystem** und **Version** von Computeknoten

	Bei der Auswahl eines Betriebssystems für die Knoten im Pool haben Sie zwei Optionen: **Konfiguration des virtuellen Computers** und **Clouddienstkonfiguration**.

	Bei **Konfiguration des virtuellen Computers** werden sowohl Linux- als auch Windows-Images für Computeknoten über den [Marketplace für virtuelle Computer][vm_marketplace] bereitgestellt. Beim Erstellen eines Pools mit Knoten mit der Konfiguration des virtuellen Computers müssen Sie nicht nur die Knotengröße angeben, sondern auch die **VM-Imagereferenz** und die **Knoten-Agent-SKU** von Batch, die auf den Knoten installiert werden soll. Weitere Informationen zum Angeben dieser Pooleigenschaften finden Sie unter [Bereitstellen von Linux-Computeknoten in Azure Batch-Pools](batch-linux-nodes.md).

	Die **Clouddienstkonfiguration** stellt *nur* Windows-Computeknoten bereit. Verfügbare Betriebssysteme für Pools vom Typ „Clouddienstkonfiguration“ sind unter [Azure-Gastbetriebssystemversionen und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md) aufgeführt. Beim Erstellen eines Pools, der Cloud Services-Knoten enthält, müssen Sie nur die Knotengröße und dessen *Betriebssystemfamilie* angeben. Beim Erstellen von Pools von Windows-Computeknoten wird Cloud Services am häufigsten verwendet.

    - Die *Betriebssystemfamilie* bestimmt auch, welche Versionen von .NET mit dem Betriebssystem installiert werden.
	- Wie bei Workerrollen innerhalb von Clouddiensten kann eine *Betriebssystemversion* angegeben werden. (Weitere Informationen zu Workerrollen finden Sie im Abschnitt [Informationen zu Cloud Services](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) unter [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md).)
    - Genau wie bei Workerrollen empfiehlt sich auch bei der *Betriebssystemversion* die Verwendung von `*`, damit die Knoten automatisch per Upgrade aktualisiert werden und für neue Versionen kein Zusatzaufwand entsteht. Mit der Wahl einer bestimmten Betriebssystemversion wird in erster Linie die Anwendungskompatibilität sichergestellt. Hierzu wird die Überprüfung der Abwärtskompatibilität vor der Versionsaktualisierung ermöglicht. Nach der Überprüfung kann die *Betriebssystemversion* für den Pool aktualisiert und das neue Betriebssystemimage installiert werden. Dabei werden alle ausgeführten Tasks unterbrochen und wieder der Warteschlange hinzugefügt.

- **Größe der Knoten**

	Die Größen der Computeknoten vom Typ **Clouddienstkonfiguration** sind unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md) aufgeführt. Batch unterstützt alle Clouddienstgrößen mit Ausnahme von `ExtraSmall`.

	Die Größen der Computeknoten vom Typ **Konfiguration des virtuellen Computers** sind unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) bzw. [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) aufgelistet. Batch unterstützt alle Größen von Azure-VMs mit Ausnahme von `STANDARD_A0` und Größen mit Storage Premium (Serien `STANDARD_GS`, `STANDARD_DS` und `STANDARD_DSV2`).

	Sie sollten die Merkmale und Anforderungen der Anwendungen berücksichtigen, die auf den Computeknoten ausgeführt werden, wenn Sie eine Knotengröße auswählen. Die Knotengröße wird normalerweise unter der Annahme ausgewählt, dass jederzeit immer nur ein Task auf dem Knoten ausgeführt wird. Die Beantwortung der Fragen, ob es sich beispielsweise um eine Multithreadanwendung handelt und wie viel Arbeitsspeicher sie beansprucht, hilft Ihnen, die am besten geeignete und kostengünstigste Knotengröße zu bestimmen. Es ist möglich, mehrere Tasks zu verwenden und so mehrere Anwendungsinstanzen [parallel auszuführen](batch-parallel-node-tasks.md). In diesem Fall wählen Sie normalerweise einen größeren Knoten. Weitere Informationen finden Sie unten im Abschnitt „Richtlinie zur Taskplanung“.

	Alle Knoten in einem Pool haben dieselbe Größe. Falls Sie Anwendungen mit unterschiedlichen Systemanforderungen und/oder Auslastungsgraden ausführen, sollten Sie separate Pools verwenden.

- **Vorgegebene Anzahl von Knoten**

	Dies ist die Anzahl von Computeknoten, die Sie im Pool bereitstellen möchten. Dies wird als *Ziel* bezeichnet, da Ihr Pool – in einigen Fällen – unter Umständen nicht die gewünschte Anzahl von Knoten erreicht. Beispiele für die Ursachen dafür, dass ein Pool nicht die gewünschte Anzahl von Knoten aufweist, sind das Erreichen des [Kernkontingents](batch-quota-limit.md#batch-account-quotas) für Ihr Batch-Konto oder eine Formel für die automatische Skalierung, die Sie auf den Pool angewendet haben und mit der die maximale Anzahl von Knoten beschränkt wird (siehe *Skalierungsrichtlinie* unten).

- **Skalierungsrichtlinie**

	Zusätzlich zum Angeben einer statischen Anzahl von Knoten können Sie auch eine [Formel für die automatische Skalierung](#scaling-compute-resources) schreiben und auf einen Pool anwenden. Der Batch-Dienst wertet die Formel regelmäßig aus und passt die Anzahl von Knoten innerhalb des Pools basierend auf verschiedenen Parametern an, die Sie für Pools, Aufträge und Tasks angeben können.

- **Richtlinie zur Taskplanung**

	Die Konfiguration der [maximalen Tasks pro Knoten](batch-parallel-node-tasks.md) bestimmt die maximale Anzahl von Tasks, die auf den einzelnen Computeknoten im Pool parallel ausgeführt werden können. In der Standardkonfiguration wird auf einem Knoten immer nur ein einziger Task ausgeführt. Es sind aber auch Szenarien denkbar, in denen die gleichzeitige Ausführung mehrerer Tasks auf einem Knoten von Vorteil ist. Das [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) im Artikel [Gleichzeitige Knotenaufgaben](batch-parallel-node-tasks.md) veranschaulicht, wie Sie von mehreren Tasks pro Knoten profitieren können.

	Sie können auch einen *Fülltyp* angeben. Dieser legt fest, ob Batch die Tasks gleichmäßig über alle Knoten eines Pools verteilt oder jeden Knoten mit der maximalen Anzahl von Tasks auffüllt, bevor Tasks einem weiteren Knoten zugewiesen werden.

- **Kommunikationsstatus** von Computeknoten

	In den meisten Szenarien werden Tasks unabhängig voneinander ausgeführt und müssen nicht miteinander kommunizieren. Es sind jedoch auch einige Anwendungen denkbar, in denen Tasks kommunizieren müssen (z.B. in [MPI-Szenarien](batch-mpi.md)).

	Sie können einen Pool so konfigurieren, dass die Kommunikation zwischen den darin enthaltenen Knoten zugelassen wird. Dies ist die so genannte **Kommunikation zwischen Knoten** (Inter-Node Communication). Wenn die Kommunikation zwischen Knoten aktiviert ist, können Knoten in Pools vom Typ „Clouddienstkonfiguration“ miteinander über Ports kommunizieren, die über 1100 liegen, und in Pools vom Typ „Konfiguration des virtuellen Computers“ bestehen keine Portbeschränkungen für Datenverkehr.

	Beachten Sie, dass sich die Aktivierung der Kommunikation zwischen Knoten auch auf die Anordnung der Knoten in Clustern auswirkt. Aufgrund von Bereitstellungsbeschränkungen wird unter Umständen die maximale Anzahl von Knoten in einem Pool begrenzt. Wenn für Ihre Anwendung keine Kommunikation zwischen Knoten erforderlich ist, kann der Batch-Dienst potenziell eine große Anzahl von Knoten in vielen verschiedenen Clustern oder Datencentern für den Pool reservieren, um die Möglichkeit zur parallelen Verarbeitung zu erhöhen.

- **Starttask** für Computeknoten

	Der optionale *Starttask* wird auf jedem Knoten ausgeführt, wenn dieser dem Pool hinzugefügt wird, sowie bei jedem Neustart oder Reimaging eines Knotens. Der Starttask eignet sich besonders gut zum Vorbereiten von Computeknoten für die Ausführung von Tasks, z.B. das Installieren der Anwendungen, die von den Tasks ausgeführt werden.

> [AZURE.IMPORTANT] Alle Batch-Konten verfügen über ein Standard**kontingent**, das die Anzahl von **Kernen** (und somit auch von Computeknoten) in einem Batch-Konto begrenzt. Die Standardkontingente und eine Anleitung zum [Erhöhen des Kontingents](batch-quota-limit.md#increase-a-quota) (wie etwa der Anzahl von Kernen in Ihrem Batch-Konto) finden Sie unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md). Wenn Sie sich z.B. die Frage stellen, weshalb Ihr Pool nur eine bestimmte Anzahl von Knoten erreicht, liegt dies möglicherweise am Kernkontingent.

## Auftrag

Ein Auftrag ist eine Sammlung von Tasks. Darüber wird verwaltet, wie die Berechnung der Tasks auf Computeknoten in einem Pool durchgeführt wird.

- Der Auftrag gibt den **Pool** an, in dem der Task ausgeführt wird. Sie können einen neuen Pool für jeden Auftrag erstellen oder einen Pool für viele Aufträge verwenden. Sie können einen Pool für jeden Auftrag erstellen, der einem Auftragszeitplan zugeordnet ist, oder für alle dem Auftragszeitplan zugeordneten Aufträge gemeinsam.
- Sie können eine optionale **Auftragspriorität** angeben. Wenn ein Auftrag übermittelt wird, der eine höhere Priorität als bereits in Bearbeitung befindliche Aufträge besitzt, werden die Tasks des Auftrags mit der höheren Priorität vor den Tasks der Aufträge mit niedrigerer Priorität in die Warteschlange eingefügt. Bereits ausgeführte Tasks mit niedrigerer Priorität werden nicht nach hinten verschoben.
- Mit **Einschränkungen** für Aufträge können bestimmte Grenzwerte für Ihre Aufträge angegeben werden:

	Sie können eine **Gesamtbetrachtungszeit** festlegen. Wenn ein Auftrag dann länger als für die angegebene Gesamtbetrachtungszeit ausgeführt wird, werden der Auftrag und alle seine Tasks beendet.

	Batch kann fehlgeschlagene Tasks erkennen und dann erneut versuchen, sie auszuführen. Die **maximale Anzahl von Taskwiederholungen** kann als Einschränkung angegeben werden. Hierzu gehört auch, ob ein Task *immer* oder *niemals* wiederholt werden soll. Bei der Wiederholung eines Tasks wird dieser nochmals der Warteschlange hinzugefügt und erneut ausgeführt.

- Tasks können einem Auftrag von Ihrer Clientanwendung hinzugefügt werden, oder es kann ein [Auftrags-Manager-Task](#job-manager-task) angegeben werden. Ein Auftrags-Manager-Task enthält die Informationen, die zum Erstellen der erforderlichen Tasks für einen Auftrag benötigt werden, wobei der Auftrags-Manager-Task auf einem der Computeknoten innerhalb des Pools ausgeführt wird. Der Auftrags-Manager-Task wird von Batch speziell behandelt: Er wird sofort nach der Auftragserstellung der Warteschlange hinzugefügt und erneut gestartet, falls er nicht erfolgreich ausgeführt werden konnte. Ein Auftrags-Manager-Task ist für Aufträge *obligatorisch*, die von einem [Auftragszeitplan](#scheduled-jobs) erstellt werden, da sich Tasks nur so vor der Auftragsinstanziierung definieren lassen.

### Auftragspriorität

Aufträgen, die Sie in Batch erstellen, können Sie eine Priorität zuweisen. Der Batch-Dienst verwendet den Prioritätswert des Auftrags, um die Reihenfolge der Auftragszeitplanung in einem Konto zu bestimmen (dies darf nicht mit einem [geplanten Auftrag](#scheduled-jobs) verwechselt werden). Die Prioritätswerte reichen von -1000 bis 1000, wobei -1000 die niedrigste und 1000 die höchste Priorität darstellt. Mit dem Vorgang [Eigenschaften eines Auftrags aktualisieren][rest_update_job] \(Batch REST) oder durch Ändern der Eigenschaft [CloudJob.Priority][net_cloudjob_priority] \(Batch .NET) können Sie die Priorität eines Auftrags aktualisieren.

Innerhalb desselben Kontos haben Aufträge mit höherer Priorität bei der Planung Vorrang vor Aufträgen mit niedrigerer Priorität. Ein Auftrag mit einem höheren Prioritätswert in einem Konto hat bei der Planung keinen Vorrang vor einem anderen Auftrag mit einem niedrigeren Prioritätswert in einem anderen Konto.

Die Auftragsplanung erfolgt über Pools hinweg unabhängig. Zwischen verschiedenen Pools wird nicht sichergestellt, dass ein Auftrag mit einer höheren Priorität zuerst geplant wird, wenn der ihm zugeordnete Pool nicht über ausreichend Knoten verfügt, die sich im Leerlauf befinden. Innerhalb eines Pools verfügen Aufträge mit gleicher Prioritätsstufe über eine identische Planungswahrscheinlichkeit.

### Geplante Aufträge

Mit [Auftragszeitplänen][rest_job_schedules] können Sie wiederkehrende Aufträge im Batch-Dienst erstellen. Ein Auftragszeitplan gibt an, wann Aufträge ausgeführt werden, und enthält die Spezifikationen für die auszuführenden Aufträge. Ein Auftragszeitplan ermöglicht die Angabe der Dauer des Zeitplans – für wie lange und wann der Zeitplan in Kraft tritt – und wie oft während dieses Zeitraums Aufträge erstellt werden sollen.

## Aufgabe

Ein Task ist eine Berechnungseinheit, die einem Auftrag zugeordnet ist und auf einem Knoten ausgeführt wird. Tasks werden einem Knoten zur Ausführung zugewiesen oder der Warteschlange hinzugefügt, bis ein Knoten verfügbar wird. Einfach ausgedrückt: Ein Task führt mindestens ein Programm oder Skript auf einem Computeknoten aus, um die erforderlichen Arbeitsschritte zu erledigen.

Beim Erstellen eines Tasks können Sie Folgendes angeben:

- Die **Befehlszeile** des Tasks. Dies ist die Befehlszeile, mit der die Anwendung oder das Skript auf dem Computeknoten ausgeführt wird.

	Es ist wichtig zu beachten, dass diese Befehlszeile eigentlich nicht unter einer Shell ausgeführt wird und dass die Shellfunktionen daher nicht auf native Weise genutzt werden können, z.B. die Erweiterung der [Umgebungsvariablen](#environment-settings-for-tasks) (umfasst auch den `PATH`). Um diese Funktionen nutzen zu können, müssen Sie **die Shell in der Befehlszeile aufrufen**. Hierzu können Sie beispielsweise `cmd.exe` auf Windows-Knoten oder `/bin/sh` unter Linux starten:

	`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

	`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

	Wenn Ihre Tasks eine Anwendung oder ein Skript ausführen müssen, die bzw. das sich nicht unter dem `PATH` oder den Referenzumgebungsvariablen des Knotens befindet, können Sie die Shell explizit in der Taskbefehlszeile aufrufen.

- Die **Ressourcendateien**, die die zu verarbeitenden Daten enthalten. Diese Dateien werden automatisch aus dem Blobspeicher in einem Azure Storage-Konto vom Typ **Allgemein** auf den Knoten kopiert, bevor die Befehlszeile des Tasks ausgeführt wird. Weitere Informationen finden Sie weiter unten unter [Starttask](#start-task) und [Dateien und Verzeichnisse](#files-and-directories).

- Die von Ihrer Anwendung benötigten **Umgebungsvariablen**. Weitere Informationen finden Sie unten im Abschnitt [Umgebungseinstellungen für Tasks](#environment-settings-for-tasks).

- Die **Einschränkungen**, mit denen der Task ausgeführt werden soll. Beispielsweise die maximale Zeit, in der der Task ausgeführt werden darf, die maximale Anzahl der Wiederholungen für einen nicht erfolgreichen Task und die maximal zulässige Aufbewahrungsdauer für Dateien im dazugehörigen Arbeitsverzeichnis.

Zusätzlich zu Tasks, die Sie zur Berechnung auf einem Knoten definieren können, werden vom Batch-Dienst die folgenden speziellen Tasks bereitgestellt:

- [Startaufgabe](#start-task)
- [Auftrags-Manager-Aufgabe](#job-manager-task)
- [Tasks zur Auftragsvorbereitung und -freigabe](#job-preparation-and-release-tasks)
- [Tasks mit mehreren Instanzen](#multi-instance-tasks)
- [Abhängigkeiten von Aufgaben](#task-dependencies)

#### Starttask

Durch das Zuordnen eines **Starttasks** zu einem Pool können Sie die Betriebssystemumgebung der zugehörigen Knoten vorbereiten und Aktionen wie das Installieren der Anwendungen, die von Ihren Tasks ausgeführt werden, und das Starten von Hintergrundprozessen durchführen. Der Starttask wird jedes Mal ausgeführt, wenn ein Knoten gestartet wird. Dies gilt, solange sich der Knoten im Pool befindet, und zwar auch beim ersten Hinzufügen des Knotens zum Pool und bei einem Neustart oder Reimaging.

Ein wesentlicher Vorteil von Starttasks besteht darin, dass sie alle Informationen enthalten können, die zum Konfigurieren eines Computeknotens und zum Installieren der für die Taskausführung benötigten Anwendungen erforderlich sind. Daher kann zum Erhöhen der Anzahl von Knoten in einem Pool einfach die neue Zielknotenanzahl angegeben werden. Batch enthält bereits die Informationen, die zum Konfigurieren der neuen Knoten und zum Vorbereiten der Knoten für die Annahme von Tasks erforderlich sind.

Wie bei jedem anderen Azure Batch-Task kann zusätzlich zu einer auszuführenden **Befehlszeile** eine Liste mit **Ressourcendateien** in [Azure Storage][azure_storage] angegeben werden. Batch kopiert zunächst die Ressourcendateien aus Azure Storage auf den Knoten und führt dann die Befehlszeile aus. Bei einem poolbezogenen Starttask enthält die Dateiliste üblicherweise die Taskanwendung und die damit verbundenen Abhängigkeiten. Sie kann aber auch Referenzdaten für alle Tasks enthalten, die auf dem Computeknoten ausgeführt werden. Die Befehlszeile eines Starttasks kann beispielsweise einen `robocopy`-Vorgang zum Kopieren von Anwendungsdateien durchführen, die als Ressourcendateien angegeben und auf den Knoten heruntergeladen wurden. Der Kopiervorgang wird aus dem [Arbeitsverzeichnis](#files-and-directories) des Starttasks in den [freigegebenen Ordner](#files-and-directories) durchgeführt, und anschließend wird ein MSI oder `setup.exe` ausgeführt.

> [AZURE.IMPORTANT] Von Batch wird derzeit *ausschließlich* der Speicherkontotyp **Allgemein** unterstützt, wie in [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md) unter Schritt 5 ([Erstellen Sie ein Speicherkonto.](../storage/storage-create-storage-account.md#create-a-storage-account)) beschrieben. In Ihren Batch-Tasks (einschließlich Standardtasks, Starttasks und Tasks zur Auftragsvorbereitung und -freigabe) müssen Ressourcendateien angegeben werden, die sich *ausschließlich* in Speicherkonten vom Typ **Allgemein** befinden.

In der Regel ist es wünschenswert, dass der Batch-Dienst auf den Abschluss des Starttasks wartet und erst dann davon ausgeht, dass der Knoten nun für die Taskzuweisung bereit ist. Dieses Verhalten ist jedoch konfigurierbar.

Kann ein Starttask für einen Computeknoten nicht erfolgreich ausgeführt werden, wird der Status des Knotens entsprechend aktualisiert, und der Knoten steht für die Taskzuweisung nicht zur Verfügung. Ein Starttask wird nicht erfolgreich durchgeführt, wenn ein Problem beim Kopieren der Ressourcendateien aus dem Speicher auftritt oder wenn der Prozess, der von der Befehlszeile ausgeführt wird, einen Exitcode ungleich NULL zurückgibt.

#### Auftrags-Manager-Task

Ein **Auftrags-Manager-Task** wird in der Regel zum Steuern und/oder Überwachen der Auftragsausführung verwendet. Dazu gehören beispielsweise das Erstellen und Übermitteln der Tasks für einen Auftrag, das Bestimmen zusätzlich auszuführender Tasks sowie das Festlegen, wann die Arbeit abgeschlossen ist. Ein Auftrags-Manager-Task ist jedoch nicht auf diese Aktivitäten beschränkt. Vielmehr handelt es sich um einen Task mit vollem Funktionsumfang, der alle für den Auftrag erforderlichen Aktionen durchführen kann. Z. B. kann ein Auftrags-Manager-Task eine Datei herunterladen, die als Parameter angegeben ist, den Inhalt der Datei analysieren und basierend auf diesem Inhalt zusätzliche Tasks übermitteln.

Ein Auftrags-Manager-Task wird vor allen anderen Tasks gestartet und bietet die folgenden Features:

- Er wird automatisch vom Batch-Dienst als Task übermittelt, wenn der Auftrag erstellt wird.

- Seine Ausführung wird so geplant, dass er vor den anderen Tasks in einem Auftrag ausgeführt wird.

- Der zugeordnete Knoten wird als letzter aus einem Pool entfernt, wenn der Pool verkleinert wird.

- Die Beendigung kann an die Beendigung aller Aufgaben im Auftrag gebunden sein.

- Dem Auftrags-Manager-Task wird die höchste Priorität zugewiesen, wenn er neu gestartet werden muss. Wenn ein Knoten im Leerlauf nicht verfügbar ist, kann der Batch-Dienst einen der anderen ausgeführten Tasks im Pool beenden, um Platz für die Ausführung des Auftrags-Manager-Tasks zu schaffen.

- Ein Auftrags-Manager-Task in einem Auftrag besitzt keine höhere Priorität als die Tasks in anderen Aufträgen. Auftragsübergreifend werden nur Prioritäten auf Auftragsebene beachtet.

#### Tasks zur Auftragsvorbereitung und -freigabe

Batch stellt den Auftragsvorbereitungstask für das Setup vor der Auftragsausführung und den Auftragsfreigabetask für die Wartung oder Bereinigung nach dem Auftrag bereit.

- **Auftragsvorbereitungstask**: Der Auftragsvorbereitungstask wird vor der Ausführung der anderen Tasks eines Auftrags auf allen zum Ausführen von Tasks geplanten Computeknoten ausgeführt. Verwenden Sie Jobvorbereitungstasks beispielsweise zum Kopieren von Daten, die von allen Tasks gemeinsam genutzt werden, aber für den Auftrag einzigartig sind.
- **Jobfreigabetask**: Nach Abschluss des Auftrags wird der Auftragsfreigabetask auf jedem Knoten im Pool ausgeführt, der mindestens einen Task ausgeführt hat. Verwenden Sie den Auftragsfreigabetask z. B. zum Löschen von Daten, die vom Auftragsvorbereitungstask kopiert wurden, oder komprimieren Sie Diagnoseprotokolldaten, und laden Sie sie hoch.

Sowohl Auftragsvorbereitungs- als auch Auftragsfreigabeaufgaben bieten die Möglichkeit zur Angabe einer Befehlszeile, die beim Aufrufen der Aufgabe ausgeführt wird, sowie Funktionen wie Dateidownload, Ausführung mit erhöhten Rechten, benutzerdefinierte Umgebungsvariablen, maximale Ausführungsdauer, Wiederholungsanzahl und Dateiaufbewahrungsdauer.

Weitere Informationen zu Auftragsvorbereitungs- und -freigabetasks finden Sie unter [Ausführen von Auftragsvorbereitungs- und Auftragsabschlusstasks auf Azure Batch-Computeknoten](batch-job-prep-release.md).

#### Tasks mit mehreren Instanzen

Ein [Task mit mehreren Instanzen](batch-mpi.md) ist ein Task, der für die gleichzeitige Ausführung auf mehreren Computeknoten konfiguriert ist. Tasks mit mehreren Instanzen ermöglichen Szenarien, die eine hohe Leistung benötigen, wie z. B. das Message Passing Interface (MPI), und erfordern, dass Computeknoten zu einer Gruppe gebündelt werden, um einen einzelnen Workload zu bearbeiteten.

Ausführliche Informationen zum Ausführen von MPI-Aufträgen in Batch mithilfe der Batch .NET-Bibliothek finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](batch-mpi.md).

#### Abhängigkeiten von Aufgaben

Mithilfe von [Abhängigkeiten von Aufgaben](batch-task-dependencies.md) können Sie – wie der Name schon vermuten lässt – angeben, dass die Ausführung einer Aufgabe vom Abschluss einer anderen Aufgaben abhängig ist. Dieses Feature bietet Unterstützung in Situationen, in denen eine Downstreamaufgabe die Ausgabe einer Upstreamaufgabe nutzt oder eine Upstreamaufgabe eine Initialisierung ausführt, die für einer Downstreamaufgabe erforderlich ist. Um dieses Feature verwenden zu können, müssen Sie zuerst Aufgabenabhängigkeiten für den Batchauftrag aktivieren. Geben Sie dann für jede Aufgabe, die von einer (oder mehreren) anderen abhängt, die übergeordneten Aufgaben an.

Mit Abhängigkeiten von Aufgaben können Sie Szenarien wie etwa die folgenden konfigurieren:

* *TaskB* hängt von *TaskA* ab (*TaskB* kann erst ausgeführt werden, wenn *TaskA* abgeschlossen ist).
* *TaskC* hängt sowohl von *TaskA* als auch von *TaskB* ab.
* *TaskD* hängt vor der Ausführung von einem Taskbereich ab, z.B. den Tasks *1* bis *10*.

Weitere ausführliche Informationen zu diesem Feature finden Sie unter [Abhängigkeiten von Aufgaben in Azure Batch](batch-task-dependencies.md) und im Codebeispiel [TaskDependencies][github_sample_taskdeps] im GitHub-Repository [azure-batch-samples][github_samples].

## Umgebungseinstellungen für Tasks

Jeder Task, der in einem Batch-Auftrag ausgeführt wird, hat Zugriff auf Umgebungsvariablen, die sowohl durch den Batch-Dienst (dienstdefiniert, siehe Tabelle unten) als auch durch benutzerdefinierte Umgebungsvariablen festgelegt werden, die Sie für Ihre Tasks angeben können. Die Anwendungen und Skripts, die von Ihren Tasks auf den Knoten ausgeführt werden, haben während der Ausführung Zugriff auf diese Umgebungsvariablen.

Sie können benutzerdefinierte Umgebungsvariablen auf Task- oder Auftragsebene festlegen, indem Sie die Eigenschaft *Umgebungseinstellungen* für diese Entitäten auffüllen. Informieren Sie sich beispielsweise über den Vorgang zum [Hinzufügen einer Aufgabe zu einem Auftrag][rest_add_task] \(Batch REST-API) oder über die Eigenschaften [CloudTask.EnvironmentSettings][net_cloudtask_env] und [CloudJob.CommonEnvironmentSettings][net_job_env] in Batch .NET.

Die Clientanwendung bzw. der Dienst kann die Umgebungsvariablen eines Tasks abrufen – sowohl dienstdefiniert als auch benutzerdefiniert –, indem der Vorgang [Abrufen von Informationen zu einer Aufgabe][rest_get_task_info] \(Batch REST) verwendet oder auf die Eigenschaft [CloudTask.EnvironmentSettings][net_cloudtask_env] \(Batch .NET) zugegriffen wird. Prozesse, die auf einem Computeknoten ausgeführt werden, können auf diese und andere Umgebungsvariablen auf dem Knoten zugreifen, z.B. mit der vertrauten `%VARIABLE_NAME%`-Syntax (Windows) oder `$VARIABLE_NAME`-Syntax (Linux).

Die folgenden Umgebungsvariablen werden vom Batch-Dienst festgelegt und sind für den Zugriff durch Ihre Tasks verfügbar:

| Name der Umgebungsvariablen | Beschreibung |
|---------------------------------|--------------------------------------------------------------------------|
| `AZ_BATCH_ACCOUNT_NAME` | Der Name des Kontos, zu dem die Aufgabe gehört. |
| `AZ_BATCH_JOB_ID` | Die ID des Auftrags, zu dem der Task gehört |
| `AZ_BATCH_JOB_PREP_DIR` | Der vollständige Pfad des Aufgabenverzeichnisses für die Auftragsvorbereitung auf dem Knoten. |
| `AZ_BATCH_JOB_PREP_WORKING_DIR` | Der vollständige Pfad des Aufgabenarbeitsverzeichnisses für die Auftragsvorbereitung auf dem Knoten. |
| `AZ_BATCH_NODE_ID` | Die ID des Knotens, auf dem der Task ausgeführt wird |
| `AZ_BATCH_NODE_ROOT_DIR` | Der vollständige Pfad des Stammverzeichnisses auf dem Knoten. |
| `AZ_BATCH_NODE_SHARED_DIR` | Der vollständige Pfad des freigegebenen Verzeichnisses auf dem Knoten. |
| `AZ_BATCH_NODE_STARTUP_DIR` | Der vollständige Pfad des Starttaskverzeichnisses des Computeknotens auf dem Knoten |
| `AZ_BATCH_POOL_ID` | Die ID des Pools, in dem der Task ausgeführt wird |
| `AZ_BATCH_TASK_DIR` | Der vollständige Pfad des Aufgabenverzeichnisses auf dem Knoten. |
| `AZ_BATCH_TASK_ID` | Die ID des aktuellen Tasks |
| `AZ_BATCH_TASK_WORKING_DIR` | Der vollständige Pfad des Aufgabenarbeitsverzeichnisses auf dem Knoten. |

>[AZURE.IMPORTANT] Diese Umgebungsvariablen sind nur im Kontext des **Taskbenutzers** verfügbar. Dies ist das Benutzerkonto auf dem Knoten, unter dem ein Task ausgeführt wird. Sie werden **nicht** angezeigt, wenn Sie eine [Remoteverbindung](#connecting-to-compute-nodes) mit einem Computeknoten per RDP oder SSH herstellen und die Umgebungsvariablen auflisten. Dies liegt daran, dass das Benutzerkonto für die Remoteverbindung nicht dem vom Task verwendeten Konto entspricht.

## Dateien und Verzeichnisse

Jeder Task verfügt über ein *Arbeitsverzeichnis*, unter dem null oder mehr Dateien und Verzeichnisse erstellt werden. Dieses Arbeitsverzeichnis kann für das Speichern des von einem Task ausgeführten Programms, der dabei verarbeiteten Daten und der Ausgabe der Verarbeitung verwendet werden. Alle Dateien und Verzeichnisse eines Tasks befinden sich im Besitz des Taskbenutzers.

Der Batch-Dienst stellt einen Teil des Dateisystems auf einem Knoten als Stammverzeichnis bereit. Das Stammverzeichnis ist für einen Task durch den Zugriff auf die `AZ_BATCH_NODE_ROOT_DIR`-Umgebungsvariable verfügbar. Weitere Informationen zur Verwendung von Umgebungsvariablen finden Sie unter [Umgebungseinstellungen für Tasks](#environment-settings-for-tasks).

Das Stammverzeichnis enthält die folgenden Verzeichnisstruktur:

![Computeknoten-Verzeichnisstruktur][1]

- **shared**: Dieses Verzeichnis bietet Lese-/Schreibzugriff auf *alle* Tasks, die auf einem Knoten ausgeführt werden. Mit jedem Task, der auf dem Knoten ausgeführt wird, können Dateien in diesem Verzeichnis erstellt, gelesen, aktualisiert und gelöscht werden. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_SHARED_DIR` verweisen.

- **startup**: Dieses Verzeichnis wird von einem Starttask als Arbeitsverzeichnis verwendet. Alle Dateien, die vom Starttask auf den Knoten heruntergeladen werden, werden hier gespeichert. Der Starttask kann Dateien unter diesem Verzeichnis erstellen, lesen, aktualisieren und löschen. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_STARTUP_DIR` verweisen.

- **Tasks**: Für jeden Task, der auf dem Knoten ausgeführt wird, wird ein Verzeichnis erstellt. Sie können darauf zugreifen, indem Sie auf die Umgebungsvariable `AZ_BATCH_TASK_DIR` verweisen.

	Innerhalb jedes Taskverzeichnisses erstellt der Batch-Dienst ein Arbeitsverzeichnis (`wd`), dessen eindeutiger Pfad durch die `AZ_BATCH_TASK_WORKING_DIR`-Umgebungsvariable angegeben wird. Dieses Verzeichnis bietet Lese-/Schreibzugriff auf den Task. Der Task kann Dateien in diesem Verzeichnis erstellen, lesen, aktualisieren und löschen, und dieses Verzeichnis bleibt auf Basis der für diesen Task angegebenen *RetentionTime*-Einschränkung erhalten.

	`stdout.txt` und `stderr.txt`: Diese Dateien werden während der Ausführung des Tasks in den Taskordner geschrieben.

>[AZURE.IMPORTANT] Wenn ein Knoten aus einem Pool entfernt wird, werden *alle* auf dem Knoten gespeicherten Dateien entfernt.

## Anwendungspakete

Das Feature [Anwendungspakete](batch-application-packages.md) ermöglicht eine einfache Verwaltung und Bereitstellung von Anwendungen für die in Ihren Pools benötigten Computeknoten. Mit Anwendungspaketen können Sie mehrere Versionen der von Ihren Aufgaben ausgeführten Anwendungen, einschließlich der Binärdaten und Unterstützungsdateien, einfach hochladen und verwalten und daraufhin eine oder mehrere dieser Anwendungen für die Computeknoten in Ihrem Pool bereitstellen.

Batch kümmert sich im Hintergrund um die Details für die Arbeit mit Azure Storage, um Ihre Anwendungspakete sicher zu speichern und für Computeknoten bereitzustellen, damit sowohl Ihr Code als auch der Verwaltungsaufwand vereinfacht wird.

Weitere Informationen zum Anwendungspaketfeature finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

## Gültigkeitsdauer für Pools und Computeknoten

Beim Entwerfen Ihrer Azure Batch-Lösung müssen Sie entscheiden, wie und wann Pools erstellt werden und wie lange Computeknoten innerhalb dieser Pools verfügbar bleiben.

Auf der einen Seite könnte beispielsweise für jeden Auftrag ein Pool erstellt werden, wenn der Auftrag übermittelt wird, und die zugehörigen Knoten könnten gleich nach Abschluss der Tasks wieder entfernt werden. Dadurch maximiert sich die Auslastung: Die Knoten werden nur zugewiesen, wenn dies unbedingt notwendig ist, und sofort heruntergefahren, wenn sie sich im Leerlauf befinden. Das bedeutet, dass der Auftrag bis zur Zuteilung der Knoten warten muss. Dabei ist aber wichtig zu erwähnen, dass Tasks jeweils für Knoten geplant werden, sobald diese einzeln verfügbar sind, zugeteilt wurden und der Starttask abgeschlossen ist. Batch wartet mit dem Zuweisen von Tasks *nicht*, bis alle Knoten in einem Pool verfügbar sind, um eine maximale Auslastung aller verfügbaren Knoten zu gewährleisten.

Auf der anderen Seite, wenn der sofortige Start von Aufträgen die höchste Priorität besitzt, wird ein Pool möglicherweise zu früh erstellt, und die zugehörigen Knoten werden zur Verfügung gestellt, bevor Aufträge übermittelt werden. In diesem Szenario können Auftragstasks sofort starten, aber Knoten befinden sich möglicherweise noch im Leerlauf und warten darauf, dass Tasks zugewiesen werden.

Ein kombinierter Ansatz, der in der Regel zum Behandeln einer variablen, aber kontinuierlichen Auslastung eingesetzt wird, lautet wie folgt: Es wird ein Pool verwendet, an den mehrere Aufträge übermittelt werden, aber die Anzahl der Knoten wird je nach Auftragslast nach oben oder nach unten skaliert. (Informationen hierzu finden Sie unten im Abschnitt [Skalieren von Computeressourcen](#scaling-compute-resources).) Dies kann reaktiv, basierend auf der aktuellen Auslastung, oder proaktiv erfolgen, wenn die Auslastung vorausgesagt werden kann.

## Skalieren von Computeressourcen

Mit der [automatischen Skalierung](batch-automatic-scaling.md) kann der Batch-Dienst die Anzahl von Computeknoten in einem Pool dynamisch an die aktuelle Workload und die Ressourcennutzung Ihres Computeszenarios anpassen. So können Sie die Gesamtkosten für die Ausführung Ihrer Anwendung senken, indem Sie nur die erforderlichen Ressourcen verwenden und nicht benötigte Ressourcen freigeben.

Sie aktivieren die automatische Skalierung, indem Sie eine [Formel für die automatische Skalierung](batch-automatic-scaling.md#automatic-scaling-formulas) schreiben und diese Formel einem Pool zuordnen. Der Batch-Dienst verwendet die Formel, um die vorgegebene Anzahl von Knoten im Pool für das nächste Skalierungsintervall (welches Sie konfigurieren können) zu bestimmen. Sie können für einen Pool die Einstellungen für die automatische Skalierung angeben, wenn Sie diesen erstellen, oder Sie können die Skalierung für einen Pool später aktivieren. Außerdem können Sie die Skalierungseinstellungen für einen Pool aktualisieren, für den die Skalierung aktiviert ist.

Beispielsweise kann es für einen Auftrag erforderlich sein, eine sehr große Anzahl von Tasks zu übermitteln, die ausgeführt werden sollen. Sie können dem Pool eine Skalierungsformel zuweisen, die die Anzahl der Knoten im Pool auf Grundlage der aktuellen Anzahl von Tasks in der Warteschlange sowie der Abschlussrate der Tasks im Auftrag anpasst. Der Batch-Dienst wertet die Formel in regelmäßigen Abständen aus und ändert dabei die Größe des Pools je nach Workload (viele Tasks in der Warteschlange = Knoten hinzufügen, keine Tasks in der Warteschlange oder keine ausgeführten Tasks = Knoten entfernen) und Ihren anderen Formeleinstellungen.

Eine Skalierungsformel kann auf den folgenden Metriken basieren:

- **Zeitmetriken**: Basieren auf Statistiken, die alle fünf Minuten für die angegebene Anzahl von Stunden erfasst werden.

- **Ressourcenmetriken**: Basieren auf CPU-Auslastung, Bandbreitenauslastung, Speicherauslastung und der Anzahl von Knoten.

- **Taskmetriken**: Basieren auf dem Taskstatus, z.B. *Aktiv* (in der Warteschlange), *Wird ausgeführt* oder *Abgeschlossen*.

Wenn die Anzahl von Computeknoten in einem Pool durch die automatische Skalierung verringert wird, müssen Sie sich überlegen, wie Sie Tasks behandeln, die zum Zeitpunkt des Verringerungsvorgangs ausgeführt werden. Hierfür verfügt Batch über eine *Option zum Aufheben der Knotenzuordnung*, die Sie in Ihre Formeln einfügen können. Sie können beispielsweise angeben, dass ausgeführte Tasks sofort beendet und dann für die Ausführung auf einem anderen Knoten erneut in die Warteschlange eingereiht werden oder dass die Fertigstellung abgewartet werden soll, bevor der Knoten aus dem Pool entfernt wird.

Weitere Informationen zur automatischen Skalierung einer Anwendung finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](batch-automatic-scaling.md).

> [AZURE.TIP] Legen Sie die vorgegebene Anzahl von Knoten am Ende eines Auftrags auf Null fest, und lassen Sie das Abschließen ausgeführter Tasks zu, um die Auslastung von Computeressourcen zu maximieren.

## Sicherheit mit Zertifikaten

In der Regel müssen Sie Zertifikate beim Ver- und Entschlüsseln vertraulicher Informationen für Tasks verwenden, z. B. Schlüssel für ein [Azure Storage-Konto][azure_storage]. Zu diesem Zweck können Zertifikate auf Knoten installiert werden. Verschlüsselte geheime Schlüssel werden über Befehlszeilenparameter an Tasks übergeben oder in einer der Taskressourcen eingebettet. Zum Entschlüsseln können dann installierte Zertifikate verwendet werden.

Sie verwenden den Vorgang [Zertifikat hinzufügen][rest_add_cert] \(Batch REST) bzw. die Methode [CertificateOperations.CreateCertificate][net_create_cert] \(Batch .NET) zum Hinzufügen eines Zertifikats zu einem Batch-Konto. Sie können das Zertifikat dann einem neuen oder vorhandenen Pool zuordnen. Wenn ein Zertifikat einem Pool zugeordnet ist, installiert der Batch-Dienst das Zertifikat auf jedem Knoten im Pool. Der Batch-Dienst installiert die entsprechenden Zertifikate beim Start des Knotens, bevor Tasks gestartet werden (einschließlich Starttask und Auftrags-Manager-Task).

## Fehlerbehandlung

Möglicherweise ist es für Sie notwendig, sowohl Task- als auch Anwendungsfehler innerhalb Ihrer Batch-Lösung zu behandeln.

### Behandeln von Taskfehlern
Bei Taskfehlern wird zwischen folgenden Kategorien unterschieden:

- **Planungsfehler**

	Wenn die Übertragung von Dateien, die für einen Task angegeben sind, aus irgendeinem Grund nicht erfolgreich durchgeführt werden kann, wird für den Task ein „Planungsfehler“ festgelegt.

	Ursachen für Planungsfehler liegen eventuell darin, dass die Ressourcendateien des Tasks verschoben wurden, das Storage-Konto nicht mehr zur Verfügung steht oder ein anderes Problem aufgetreten ist, das ein erfolgreiches Kopieren von Dateien auf den Knoten verhindert hat.

- **Anwendungsfehler**

	Ein Fehler kann auch beim in der Befehlszeile des Tasks angegebenen Prozess auftreten. Der Prozess gilt als nicht erfolgreich, wenn der Exitcode, der vom durch den Task ausgeführten Prozess zurückgegeben wird, ungleich NULL ist (siehe *Exitcodes für Tasks* unten).

	Batch kann so konfiguriert werden, dass der Task im Falle eines Anwendungsfehlers automatisch mit einer bestimmten Häufigkeit wiederholt wird.

- **Einschränkungsfehler**

	Eine Einschränkung kann festgelegt werden, in der die maximale Ausführungsdauer für einen Auftrag oder Task angegeben wird, die *maxWallClockTime*. Dies kann nützlich für das Beenden stillstehender Tasks sein.

	Wenn die maximal zulässige Zeitspanne überschritten wurde, wird der Task als *abgeschlossen* gekennzeichnet. Als Exitcode wird jedoch `0xC000013A` zurückgegeben, und das Feld *schedulingError* wird als `{ category:"ServerError", code="TaskEnded"}` markiert.

### Debuggen von Anwendungsfehlern

- `stderr` und `stdout`

	Während der Ausführung generiert eine Anwendung unter Umständen eine Diagnoseausgabe für die Problembehandlung. Wie oben unter [Dateien und Verzeichnisse](#files-and-directories) bereits erwähnt, schreibt der Batch-Dienst „standard out“- und „standard error“-Ausgabe in `stdout.txt`- und `stderr.txt`-Dateien im Taskverzeichnis auf dem Computeknoten. Sie können das Azure-Portal zum Herunterladen dieser Dateien verwenden oder hierfür ein Batch SDK nutzen. Mithilfe von [ComputeNode.GetNodeFile][net_getfile_node] und [CloudTask.GetNodeFile][net_getfile_task] in der .NET-Bibliothek von Batch können Sie diese und andere Dateien zu Problembehandlungszwecken abrufen.

- **Exitcodes für Tasks**

	Wie oben bereits erwähnt, wird eine Task vom Batch-Dienst als fehlgeschlagen gekennzeichnet, wenn der vom Task ausgeführte Prozess einen Exitcode ungleich NULL zurückgibt. Wenn ein Task einen Prozess ausführt, füllt Batch die Exitcode-Eigenschaft des Tasks mit dem *Rückgabecode des Prozesses*. Es ist wichtig zu beachten, dass der Exitcode eines Tasks **nicht** vom Batch-Dienst bestimmt wird, sondern vom Prozess selbst oder von dem Betriebssystem, unter dem der Prozess ausgeführt wird.

### Erläuterung zu Taskfehlern oder Unterbrechungen

Tasks werden gelegentlich nicht erfolgreich durchgeführt oder unterbrochen. Es tritt ein Fehler in der Taskanwendung selbst auf, oder der Knoten, auf dem der Task ausgeführt wird, wird neu gestartet. Außerdem wird der Knoten während einer Änderung der Poolgröße ggf. entfernt, wenn die Richtlinie für die Aufhebung der Zuordnung des Knotens so konfiguriert ist, dass der Knoten sofort entfernt wird, ohne auf den Abschluss des Tasks zu warten. In diesen Fällen kann der Task automatisch per Batch wieder der Warteschlange hinzugefügt und auf einem anderen Knoten ausgeführt werden.

Außerdem können zeitweilig Probleme auftreten, die dazu führen, dass eine Aufgabe nicht mehr reagiert oder zu lange dauert. Hierzu kann für den Task eine maximal zulässige Ausführungszeit festgelegt werden. Wird dieser Wert überschritten, unterbricht Batch die Taskanwendung.

### Herstellen einer Verbindung mit Computeknoten

Sie können weitere Debug- und Problembehandlungsmaßnahmen durchführen, indem Sie sich per Remotezugriff an einem Computeknoten anmelden. Sie können das Azure-Portal verwenden, um eine RDP-Datei (Remotedesktop) für Windows-Knoten herunterzuladen und SSH-Verbindungsinformationen für Linux-Knoten abzurufen. Hierfür können Sie auch die Batch-APIs verwenden, z.B. mit [Batch .NET][net_rdpfile] oder [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Um über RDP oder SSH eine Verbindung mit einem Knoten herzustellen, müssen Sie zuerst einen Benutzer auf dem Knoten erstellen. Zu diesem Zweck können Sie das Azure-Portal verwenden, über die Batch REST-API [einem Knoten ein Benutzerkonto hinzufügen][rest_create_user], die [ComputeNode.CreateComputeNodeUser][net_create_user]-Methode in Batch .NET aufrufen oder die [add\_user][py_add_user]-Methode im Batch Python-Modul aufrufen.

### Problembehandlung bei fehlerhaften Computeknoten

Wenn bei einigen Ihrer Tasks Fehler auftreten, kann Ihre Batch-Clientanwendung oder der Dienst die Metadaten der fehlgeschlagenen Tasks prüfen, um einen fehlerhaften Knoten zu finden. Jeder Knoten in einem Pool erhält eine eindeutige ID, und der Knoten, auf dem ein Task ausgeführt wird, ist in den Metadaten des Tasks angegeben. Nachdem Sie einen „Problemknoten“ identifiziert haben, können Sie dafür verschiedene Aktionen durchführen:

- **Neustarten des Knotens** ([REST][rest_reboot] | [.NET][net_reboot]).

	Durch einen Neustart des Knotens können latente Probleme wie etwa hängende oder abgestürzte Prozesse behoben werden. Wenn Ihr Pool ein Starttask nutzt, oder wenn der Auftrag ein Auftragsvorbereitungstask nutzt, werden diese Tasks beim Neustart des Knotens ausgeführt.

- **Reimaging des Knotens** ([REST][rest_reimage] | [.NET][net_reimage]).

	Dadurch wird das Betriebssystem auf dem Knoten neu installiert. Wie beim Neustart eines Knotens werden Start- und Auftragsvorbereitungstasks nach dem Reimaging des Knotens erneut ausgeführt.

- **Entfernen des Knotens aus dem Pool** ([REST][rest_remove] | [.NET][net_remove]).

	Manchmal ist es erforderlich, den Knoten aus dem Pool vollständig zu entfernen.

- **Deaktivieren der Taskplanung auf dem Knoten** ([REST][rest_offline] | [.NET][net_offline]).

	Dadurch geht der Knoten „offline“, damit ihm keine weiteren Tasks mehr zugewiesen werden. Er wird jedoch weiterhin ausgeführt und verbleibt im Pool. So können Sie die Fehlerursache weiter untersuchen, ohne dass die Daten der fehlgeschlagenen Tasks verloren gehen und durch den Knoten weitere Fehler auftreten. Aktivieren Sie z.B. die Taskplanung auf dem Knoten, [melden Sie sich remote an](#connecting-to-compute-nodes), um die Ereignisprotokolle des Knotens zu prüfen, oder führen Sie andere Schritte zur Problembehandlung aus. Schalten Sie den Knoten nach Abschluss der Prüfung wieder online, indem Sie die Taskplanung ([REST][rest_online] | [.NET][net_online]) aktivieren oder eine andere der o.g. Aktionen durchführen.

> [AZURE.IMPORTANT] Mit jeder dieser Aktionen (Neustart, Reimaging, Deaktivieren der Taskplanung) können Sie festlegen, wie mit auf dem Knoten ausgeführten Tasks verfahren wird, wenn Sie die Aktion durchführen. Wenn Sie z.B. auf einem Knoten mit der Batch .NET-Clientbibliothek die Taskplanung deaktivieren, können Sie durch das Festlegen des Enumerationswerts [DisableComputeNodeSchedulingOption][net_offline_option] angeben, ob ausgeführte Tasks **beendet**, für die Planung auf anderen Knoten **wieder in eine Warteschlange eingereiht** oder vor dem Ausführen der Aktion abgeschlossen werden (**TaskCompletion**).

## Nächste Schritte

- Lesen Sie sich [Erste Schritte mit der Azure-Batch-Bibliothek für .NET](batch-dotnet-get-started.md) mit einer Schritt-für-Schritt-Anleitung für eine Batch-Beispielanwendung durch. Es gibt auch eine [Python-Version](batch-python-tutorial.md) des Tutorials, in der eine Workload auf Linux-Computeknoten ausgeführt wird.

- Informieren Sie sich über das [Erstellen von Pools mit Linux-Computeknoten](batch-linux-nodes.md).

- Besuchen Sie das [Azure Batch-Forum][batch_forum] auf der MSDN-Website. Das Forum ist ein guter Ort zum Stellen von Fragen. Dabei spielt es keine Rolle, ob Sie Anfänger oder Experte sind.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/de-DE/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies

[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

<!---HONumber=AcomDC_0629_2016-->