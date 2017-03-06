---
title: "Übersicht über Azure Batch für Entwickler | Microsoft-Dokumentation"
description: Lernen Sie die Features des Batch-Diensts und seiner APIs aus der Sicht eines Entwicklers kennen.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 416b95f8-2d7b-4111-8012-679b0f60d204
ms.service: batch
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6b6c548ca1001587e2b40bbe9ee2fcb298f40d72
ms.openlocfilehash: 9a17b749a50c0f75890fa71b4ae00391aa63876a
ms.lasthandoff: 02/28/2017


---
# <a name="develop-large-scale-parallel-compute-solutions-with-batch"></a>Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch

In dieser Übersicht über die Hauptkomponenten des Azure Batch-Diensts werden die wichtigsten Dienstfunktionen und Ressourcen beschrieben, die Batch-Entwickler zum Erstellen paralleler Computelösungen in größerem Umfang verwenden können.

Sie werden viele der in diesem Artikel beschriebenen Ressourcen und Features nutzen. Dies gilt unabhängig davon, ob Sie eine verteilte Computeanwendung oder einen Computedienst mit direkter Ausgabe von [REST-API][batch_rest_api]-Aufrufen entwickeln oder eines der [Batch SDKs](batch-technical-overview.md#batch-development-apis) verwenden.

> [!TIP]
> Eine allgemeiner gehaltene Einführung in den Batch-Dienst finden Sie unter [Grundlagen von Azure Batch](batch-technical-overview.md).
>
>

## <a name="batch-service-workflow"></a>Workflow des Batch-Diensts
Der folgende allgemeine Workflow ist typisch für fast alle Anwendungen und Dienste, die den Batch-Dienst für die Verarbeitung von parallelen Workloads verwenden:

1. Laden Sie die **Datendateien**, die verarbeitet werden sollen, in ein [Azure-Speicherkonto][azure_storage] hoch. Batch verfügt über integrierte Unterstützung für den Zugriff auf Azure-Blobspeicher, und Ihre Tasks können diese Dateien auf [Computeknoten](#compute-node) herunterladen, wenn die Tasks ausgeführt werden.
2. Laden Sie die **Anwendungsdateien** hoch, die von Ihren Tasks ausgeführt werden sollen. Bei diesen Dateien kann es sich um Binärdateien oder Skripts und ihre Abhängigkeiten handeln, und sie werden von den Tasks in Ihren Aufträgen ausgeführt. Diese Dateien können von Ihren Tasks aus Ihrem Storage-Konto heruntergeladen werden. Alternativ können Sie das Batch-Feature [Anwendungspakete](#application-packages) für die Anwendungsverwaltung und -bereitstellung verwenden.
3. Erstellen Sie einen [Pool](#pool) mit Computeknoten. Beim Erstellen eines Pools geben Sie die Anzahl von Computeknoten für den Pool, deren Größe und das Betriebssystem an. Wenn die einzelnen Tasks des Auftrags ausgeführt werden, erfolgt die Zuweisung zur Ausführung auf einem der Knoten im Pool.
4. Erstellen Sie einen [Auftrag](#job). Ein Auftrag verwaltet eine Sammlung von Tasks. Jeder Auftrag wird einem bestimmten Pool zugeordnet, in dem die Tasks des Auftrags ausgeführt werden.
5. Fügen Sie dem Auftrag [Aufgaben](#task) hinzu. Jeder Task führt die hochgeladene Anwendung bzw. das Skript aus, um die Datendateien zu verarbeiten, die aus Ihrem Speicherkonto heruntergeladen werden. Nachdem ein Task abgeschlossen ist, kann die Ausgabe jeweils in Azure Storage hochgeladen werden.
6. Überwachen Sie den Auftragsstatus, und rufen Sie die Taskausgabe aus Azure Storage ab.

In den folgenden Abschnitten werden diese und andere Ressourcen von Batch besprochen, die Ihr verteiltes Computingszenario ermöglichen.

> [!NOTE]
> Zur Verwendung des Batch-Diensts wird ein [Batch-Konto](batch-account-create-portal.md) benötigt. Darüber hinaus wird bei fast allen Lösungen ein [Azure-Speicherkonto][azure_storage] zum Speichern und Abrufen von Dateien verwendet. Von Batch wird derzeit ausschließlich der Speicherkontotyp **Allgemein** unterstützt, wie in [Informationen zu Azure Storage-Konten](../storage/storage-create-storage-account.md) unter Schritt 5 von [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) beschrieben.
>
>

## <a name="batch-service-resources"></a>Ressourcen für den Batch-Dienst
Einige der folgenden Ressourcen – Konten, Computeknoten, Pools, Aufträge, Tasks – sind für alle Lösungen erforderlich, die den Batch-Dienst nutzen. Andere Features (wie etwa Auftragszeitpläne und Anwendungspakete) sind zwar hilfreich, aber optional.

* [Konto](#account)
* [Computeknoten](#compute-node)
* [Pool](#pool)
* [Job](#job)

  * [Auftragszeitpläne](#scheduled-jobs)
* [Aufgabe](#task)

  * [Startaufgabe](#start-task)
  * [Auftrags-Manager-Aufgabe](#job-manager-task)
  * [Tasks zur Auftragsvorbereitung und -freigabe](#job-preparation-and-release-tasks)
  * [Task mit mehreren Instanzen](#multi-instance-tasks)
  * [Abhängigkeiten von Aufgaben](#task-dependencies)
* [Anwendungspakete](#application-packages)

## <a name="account"></a>Konto
Ein Batch-Konto ist eine eindeutig identifizierte Entität innerhalb des Batch-Diensts. Die gesamte Verarbeitung ist einem Batch-Konto zugeordnet. Beim Ausführen von Vorgängen mit dem Batch-Dienst benötigen Sie sowohl den Kontonamen als auch einen der dazugehörigen Kontoschlüssel. Sie können [über das Azure-Portal ein Azure Batch-Konto erstellen](batch-account-create-portal.md).

## <a name="compute-node"></a>Computeknoten
Ein Computeknoten ist ein virtueller Azure-Computer (Virtual Machine, VM), der für die Verarbeitung eines Teils der Anwendungsworkload fest zugeordnet ist. Die Größe eines Knotens bestimmt die Anzahl von CPU-Kernen, die Speicherkapazität und die lokale Dateisystemgröße, die dem Knoten zugeordnet werden. Sie können Pools mit Windows- oder Linux-Knoten erstellen, indem Sie entweder Azure Cloud Services oder VM-Marketplace-Images verwenden. Weitere Informationen zu diesen Optionen finden Sie weiter unten im Abschnitt [Pool](#pool) .

Knoten können beliebige ausführbare Dateien oder Skripts ausführen, die von der Betriebssystemumgebung des Knotens unterstützt werden. Hierzu zählen etwa \*EXE-, \*CMD-, \*BAT- und PowerShell-Skripts für Windows und Binärdateien sowie Shell- und Python-Skripts für Linux.

Alle Computeknoten in Batch enthalten außerdem Folgendes:

* Eine standardmäßige [Ordnerstruktur](#files-and-directories) und die dazugehörigen [Umgebungsvariablen](#environment-settings-for-tasks), die zur Referenzierung durch Tasks verfügbar sind.
* **Firewalleinstellungeneinstellungen** .
* [Remotezugriff](#connecting-to-compute-nodes) auf Windows-Knoten (Remotedesktopprotokoll, RDP) und Linux-Knoten (Secure Shell, SSH).

## <a name="pool"></a>Pool
Ein Pool besteht aus einer Sammlung von Knoten, auf denen Ihre Anwendung ausgeführt wird. Der Pool kann manuell von Ihnen oder automatisch vom Batch-Dienst erstellt werden, wenn Sie die zu erledigende Arbeit angeben. Sie können einen Pool erstellen und verwalten, der die Ressourcenanforderungen Ihrer Anwendung erfüllt. Ein Pool kann nur von dem Batch-Konto verwendet werden, unter dem er erstellt wurde. Ein Batch-Konto kann über mehrere Pools verfügen.

Azure Batch-Pools basieren auf der Azure-Computeplattform. Sie bieten Funktionen für umfangreiche Zuordnungen, für die Anwendungsinstallation, die Datenverteilung und die Systemüberwachung sowie für die flexible Anpassung der Computeknotenanzahl innerhalb eines Pools ([Skalierung](#scaling-compute-resources)).

Jedem Knoten, der einem Pool hinzugefügt wird, werden ein eindeutiger Name und eine IP-Adresse zugewiesen. Beim Entfernen eines Knotens aus einem Pool gehen alle Änderungen am Betriebssystem oder an den Dateien verloren, und sein Name und die IP-Adresse werden zur späteren Verwendung freigegeben. Wenn ein Knoten einen Pool verlässt, endet seine Lebensdauer.

Wenn Sie einen Pool erstellen, können Sie die folgenden Attribute angeben:

* **Betriebssystem** und **Version** von Computeknoten

    Bei der Wahl eines Betriebssystems für die Knoten im Pool haben Sie zwei Optionen: **Konfiguration des virtuellen Computers** und **Clouddienstkonfiguration**.

    Bei **Konfiguration des virtuellen Computers** werden sowohl Linux- als auch Windows-Images für Computeknoten über den [Azure Virtual Machines Marketplace][vm_marketplace] bereitgestellt.
    Beim Erstellen eines Pools, der Knoten mit der Konfiguration des virtuellen Computers enthält, müssen Sie nicht nur die Knotengröße angeben, sondern auch die **VM-Imagereferenz** und die **Knoten-Agent-SKU** von Batch, die auf den Knoten installiert werden soll. Weitere Informationen zum Angeben dieser Pooleigenschaften finden Sie unter [Bereitstellen von Linux-Computeknoten in Azure Batch-Pools](batch-linux-nodes.md).

    **Clouddienstkonfiguration** stellt *nur*. Verfügbare Betriebssysteme für Pools vom Typ „Clouddienstkonfiguration“ sind unter [Azure-Gastbetriebssystemversionen und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md)aufgeführt. Beim Erstellen eines Pools, der Cloud Services-Knoten enthält, müssen Sie nur die Knotengröße und die *Betriebssystemfamilie*angeben. Beim Erstellen von Pools mit Windows-Computeknoten wird üblicherweise Cloud Services verwendet.

  * Die *Betriebssystemfamilie* bestimmt auch, welche Versionen von .NET mit dem Betriebssystem installiert werden.
  * Genau wie bei Workerrollen innerhalb von Cloud Services können Sie eine *Betriebssystemversion* angeben. (Weitere Informationen zu Workerrollen finden Sie im Abschnitt [Informationen zu Cloud Services](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) in der [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md).)
  * Und genau wie bei Workerrollen empfiehlt sich auch bei der *Betriebssystemversion* die Angabe von `*`, damit die Knoten automatisch per Upgrade aktualisiert werden und für neue Versionen kein Zusatzaufwand entsteht. Mit der Wahl einer bestimmten Betriebssystemversion wird in erster Linie die Anwendungskompatibilität sichergestellt. Hierzu wird die Überprüfung der Abwärtskompatibilität vor der Versionsaktualisierung ermöglicht. Nach der Überprüfung kann die *Betriebssystemversion* für den Pool aktualisiert und das neue Betriebssystemimage installiert werden. Dabei werden alle ausgeführten Tasks unterbrochen und wieder der Warteschlange hinzugefügt.
* **Größe der Knoten**

    **Clouddienstkonfiguration** sind unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md). Batch unterstützt alle Clouddienstgrößen mit Ausnahme von `ExtraSmall`.

    Die Größen der Computeknoten vom Typ **Konfiguration des virtuellen Computers** sind unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) bzw. [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) aufgelistet. Batch unterstützt alle Größen von Azure-VMs mit Ausnahme von `STANDARD_A0` und Größen mit Storage Premium (Serien `STANDARD_GS`, `STANDARD_DS` und `STANDARD_DSV2`).

    Berücksichtigen Sie beim Auswählen einer Computeknotengröße die Merkmale und Anforderungen der Anwendungen, die auf den Knoten ausgeführt werden sollen. Die Beantwortung der Fragen, ob es sich beispielsweise um eine Multithreadanwendung handelt und wie viel Arbeitsspeicher sie beansprucht, kann Ihnen dabei behilflich sein, die am besten geeignete und kostengünstigste Knotengröße zu bestimmen. Die Knotengröße wird normalerweise unter der Annahme ausgewählt, dass jederzeit immer nur ein Task auf einem Knoten ausgeführt wird. Es ist aber auch möglich, mehrere Tasks (und somit mehrere Anwendungsinstanzen) während der Auftragsausführung auf Computeknoten [parallel zu nutzen](batch-parallel-node-tasks.md). In diesem Fall wird häufig eine höhere Knotengröße gewählt, um den höheren Bedarf an parallelen Taskausführungen decken zu können. Weitere Informationen finden Sie unter [Richtlinie zur Taskplanung](#task-scheduling-policy).

    Alle Knoten in einem Pool haben dieselbe Größe. Wenn Sie Anwendungen mit unterschiedlichen Systemanforderungen bzw. Auslastungsgraden ausführen möchten, empfehlen wir Ihnen die Verwendung von separaten Pools.
* **Vorgegebene Anzahl von Knoten**

    Dies ist die Anzahl von Computeknoten, die Sie im Pool bereitstellen möchten. Dies wird als *Ziel* bezeichnet, da Ihr Pool – in einigen Fällen – unter Umständen nicht die gewünschte Anzahl von Knoten erreicht. Dieser Fall kann eintreten, wenn der Pool das [Kernkontingent](batch-quota-limit.md) für Ihr Batch-Konto erreicht oder Sie eine Formel für die automatische Skalierung auf den Pool angewendet haben, die die maximale Knotenanzahl beschränkt. (Weitere Informationen finden Sie im Abschnitt „Skalierungsrichtlinie“.)
* **Skalierungsrichtlinie**

    Für dynamische Workloads können Sie eine [Formel für automatische Skalierung](#scaling-compute-resources) schreiben und auf einen Pool anwenden. Der Batch-Dienst wertet die Formel regelmäßig aus und passt die Anzahl von Knoten innerhalb des Pools auf der Grundlage verschiedener Parameter an, die Sie für Pools, Aufträge und Tasks angeben können.
* **Richtlinie zur Taskplanung**

    Die Konfiguration der [maximalen Tasks pro Knoten](batch-parallel-node-tasks.md) bestimmt die maximale Anzahl von Tasks, die parallel auf den einzelnen Computeknoten im Pool ausgeführt werden können.

    In der Standardkonfiguration wird auf einem Knoten immer nur ein einziger Task ausgeführt. Es sind aber auch Szenarien denkbar, in denen die gleichzeitige Ausführung von zwei oder mehr Tasks auf einem Knoten von Vorteil ist. Das [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) im Artikel zu [parallelen Knotenaufgaben](batch-parallel-node-tasks.md) veranschaulicht, wie Sie von mehreren Tasks pro Knoten profitieren können.

    Sie können auch einen *Fülltyp* angeben. Dieser legt fest, ob Batch die Tasks gleichmäßig auf alle Knoten eines Pools verteilt oder jeden Knoten mit der maximalen Anzahl von Tasks auffüllt, bevor Tasks einem weiteren Knoten zugewiesen werden.
* **Kommunikationsstatus** von Computeknoten

    In den meisten Szenarien werden Tasks unabhängig voneinander ausgeführt und müssen nicht miteinander kommunizieren. Es gibt aber einige Anwendungen, bei denen Tasks kommunizieren müssen, z.B. [MPI-Szenarien](batch-mpi.md).

    Sie können einen Pool zum Zulassen der **Kommunikation zwischen Knoten** konfigurieren, sodass Knoten in einem Pool zur Laufzeit miteinander kommunizieren können. Wenn die Kommunikation zwischen Knoten aktiviert ist, können Knoten in Pools vom Typ „Clouddienstkonfiguration“ miteinander über Ports kommunizieren, die über 1100 liegen. In Pools vom Typ „Konfiguration des virtuellen Computers“ bestehen keine Portbeschränkungen für Datenverkehr.

    Beachten Sie, dass sich die Aktivierung der Kommunikation zwischen Knoten auch auf die Anordnung der Knoten in Clustern auswirkt. Aufgrund von Bereitstellungsbeschränkungen wird unter Umständen die maximale Anzahl von Knoten in einem Pool begrenzt. Wenn für Ihre Anwendung keine Kommunikation zwischen Knoten erforderlich ist, kann der Batch-Dienst potenziell eine große Anzahl von Knoten in vielen verschiedenen Clustern und Datencentern für den Pool reservieren, um mehr Leistung für die parallele Verarbeitung bereitzustellen.
* **Starttask** für Computeknoten

    Der optionale *Starttask* wird auf jedem Knoten ausgeführt, wenn dieser dem Pool hinzugefügt wird, sowie bei jedem Neustart oder Reimaging eines Knotens. Der Starttask eignet sich besonders gut zum Vorbereiten von Computeknoten für die Ausführung von Tasks, z.B. das Installieren der Anwendungen, die von den Tasks auf den Computeknoten ausgeführt werden.
* **Anwendungspakete**

    Sie können [Anwendungspakete](#application-packages) für die Bereitstellung auf den Computeknoten im Pool festlegen. Anwendungspakete bieten eine vereinfachte Bereitstellung und Versionsverwaltung der Anwendungen, die von Ihren Tasks ausgeführt werden. Die Anwendungspakete, die Sie für einen Pool angeben, werden auf jedem Knoten installiert, der dem Pool hinzugefügt wird, sowie wenn der Knoten neu gestartet oder ein Reimaging durchgeführt wird. Anwendungspakete werden auf Linux-Computeknoten derzeit nicht unterstützt.
* **Network Configuration**

    Sie können die ID eines [virtuellen Azure-Netzwerks (VNet)](../virtual-network/virtual-networks-overview.md) , in dem die Computeknoten des Pools erstellt werden sollen, festlegen. Weitere Informationen finden Sie im Abschnitt [Konfiguration von Poolnetzwerken](#pool-network-configuration).

> [!IMPORTANT]
> Alle Batch-Konten verfügen über ein **Standardkontingent**, das die Anzahl von **Kernen** (und somit auch von Computeknoten) in einem Batch-Konto begrenzt. Die Standardkontingente und eine Anleitung zum [Erhöhen des Kontingents](batch-quota-limit.md#increase-a-quota) (wie etwa der Anzahl von Kernen in Ihrem Batch-Konto) finden Sie unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md). Wenn Sie sich z.B. die Frage stellen, weshalb Ihr Pool nur eine bestimmte Anzahl von Knoten erreicht, liegt dies möglicherweise am Kernkontingent.
>
>

## <a name="job"></a>Auftrag
Ein Auftrag ist eine Sammlung von Aufgaben. Er verwaltet, wie die Berechnung der Tasks auf Computeknoten in einem Pool durchgeführt wird.

* Der Auftrag gibt den **Pool** an, in dem der Task ausgeführt werden soll. Sie können einen neuen Pool für jeden Auftrag erstellen oder einen Pool für viele Aufträge verwenden. Sie können einen Pool für jeden Auftrag erstellen, der einem Auftragszeitplan zugeordnet ist, oder für alle dem Auftragszeitplan zugeordneten Aufträge gemeinsam.
* Sie können eine optionale **Auftragspriorität**angeben. Wenn ein Auftrag übermittelt wird, der eine höhere Priorität besitzt als bereits in Bearbeitung befindliche Aufträge, werden die Tasks des Auftrags mit der höheren Priorität vor den Tasks der Aufträge mit niedrigerer Priorität in die Warteschlange eingefügt. Tasks in Aufträgen mit niedrigerer Priorität, die bereits ausgeführt werden, werden nicht zeitlich nach hinten verschoben.
* Mithilfe von **Einschränkungen** für Aufträge können Sie bestimmte Grenzwerte für Ihre Aufträge angeben:

    Sie können eine **Gesamtbetrachtungszeit** festlegen. Wenn ein Auftrag länger ausgeführt wird, werden der Auftrag und alle seine Tasks beendet.

    Batch kann fehlgeschlagene Tasks erkennen und dann erneut versuchen, sie auszuführen. Sie können die **maximale Anzahl von Taskwiederholungen** als Einschränkung angeben. Hierzu gehört auch, ob ein Task *immer* oder *niemals* wiederholt werden soll. Bei der Wiederholung eines Tasks wird dieser nochmals der Warteschlange hinzugefügt und erneut ausgeführt.
* Ihre Clientanwendung kann einem Auftrag Tasks hinzufügen. Alternativ können Sie einen [Auftrags-Manager-Task](#job-manager-task) angeben. Ein Auftrags-Manager-Task enthält die Informationen, die zum Erstellen der erforderlichen Tasks für einen Auftrag benötigt werden, wobei der Auftrags-Manager-Task auf einem der Computeknoten innerhalb des Pools ausgeführt wird. Der Auftrags-Manager-Task wird von Batch speziell behandelt: Er wird sofort nach der Auftragserstellung der Warteschlange hinzugefügt und erneut gestartet, falls er nicht erfolgreich ausgeführt werden konnte. Ein Auftrags-Manager-Task ist für durch einen [Auftragszeitplan](#scheduled-jobs) erstellte Aufträge *obligatorisch*, da sich Tasks nur so vor der Auftragsinstanziierung definieren lassen.
* Standardmäßig bleiben Aufträge im aktiven Zustand, wenn alle Tasks innerhalb des Auftrags abgeschlossen sind. Sie können dieses Verhalten ändern, sodass der Auftrag automatisch beendet wird, wenn alle Aufgaben im Auftrag abgeschlossen sind. Legen Sie die Eigenschaft **onAllTasksComplete** des Auftrags ([OnAllTasksComplete][net_onalltaskscomplete] in Batch.NET) auf *terminatejob* fest, damit der Auftrag automatisch beendet wird, wenn alle Tasks abgeschlossen sind.

    Beachten Sie, dass ein Auftrag *ohne* Tasks vom Batch-Dienst als Auftrag angesehen wird, bei dem alle Tasks abgeschlossen sind. Diese Option wird daher mit einer [Auftrags-Manager-Aufgabe](#job-manager-task)am häufigsten verwendet. Wenn Sie die automatische Autragsbeendigung ohne einen Auftrags-Manager verwenden möchten, müssen Sie zunächst die Eigenschaft **onAllTasksComplete** des Auftrags auf *noaction* festlegen und erst nachdem Sie Tasks zum Auftrag hinzugefügt haben, auf *terminatejob*.

### <a name="job-priority"></a>Auftragspriorität
Aufträgen, die Sie in Batch erstellen, können Sie eine Priorität zuweisen. Der Batch-Dienst verwendet den Prioritätswert des Auftrags, um die Reihenfolge der Auftragszeitplanung in einem Konto zu bestimmen (dies darf nicht mit einem [geplanten Auftrag](#scheduled-jobs)verwechselt werden). Die Prioritätswerte reichen von -1000 bis 1000, wobei -1000 die niedrigste und 1000 die höchste Priorität darstellt. Rufen Sie zum Aktualisieren der Priorität eines Auftrags den Vorgang [Eigenschaften eines Auftrags aktualisieren][rest_update_job] (Batch REST) auf, oder ändern Sie die Eigenschaft [CloudJob.Priority][net_cloudjob_priority] (Batch .NET).

Innerhalb eines Kontos haben Aufträge mit höherer Priorität bei der Planung Vorrang vor Aufträgen mit niedrigerer Priorität. Ein Auftrag mit einem höheren Prioritätswert hat bei der Planung keinen Vorrang vor einem anderen Auftrag mit einem niedrigeren Prioritätswert, wenn sich dieser in einem anderen Konto befindet.

Die Auftragsplanung erfolgt über Pools hinweg unabhängig. Poolübergreifend wird nicht sichergestellt, dass ein Auftrag mit einer höheren Priorität zuerst geplant wird, wenn der ihm zugeordnete Pool nicht über ausreichend Knoten verfügt, die sich im Leerlauf befinden. Innerhalb eines Pools verfügen Aufträge mit gleicher Prioritätsstufe über eine identische Planungswahrscheinlichkeit.

### <a name="scheduled-jobs"></a>Geplante Aufträge
Mit [Auftragszeitplänen][rest_job_schedules] können Sie wiederkehrende Aufträge im Batch-Dienst erstellen. Ein Auftragszeitplan gibt an, wann Aufträge ausgeführt werden, und enthält die Spezifikationen für die auszuführenden Aufträge. Sie können die Dauer des Zeitplans festlegen (also wann und für wie lange der Zeitplan in Kraft sein soll) und angeben, wie oft während des geplanten Zeitraums Aufträge erstellt werden sollen.

## <a name="task"></a>Task
Ein Task ist eine Berechnungseinheit, die einem Auftrag zugeordnet ist. Er wird auf einem Knoten ausgeführt. Tasks werden einem Knoten zur Ausführung zugewiesen oder der Warteschlange hinzugefügt, bis ein Knoten verfügbar wird. Einfach ausgedrückt: Ein Task führt mindestens ein Programm oder Skript auf einem Computeknoten aus, um die erforderlichen Arbeitsschritte zu erledigen.

Beim Erstellen eines Tasks können Sie Folgendes angeben:

* Die **Befehlszeile** für den Task. Dies ist die Befehlszeile, mit der die Anwendung oder das Skript auf dem Computeknoten ausgeführt wird.

    Beachten Sie, dass die Befehlszeile nicht wirklich unter einer Shell ausgeführt wird. Aus diesem Grund kann sie nicht einfach Shell-Features wie die Erweiterung von[ Umgebungsvariablen](#environment-settings-for-tasks) nutzen. (Dies gilt auch für `PATH`.) Um solche Features nutzen zu können, müssen Sie die Shell über die Befehlszeile aufrufen – beispielsweise durch Starten von `cmd.exe` (Windows-Knoten) oder von `/bin/sh` (Linux):

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Wenn Ihre Tasks eine Anwendung oder ein Skript ausführen müssen, die bzw. das sich nicht unter dem `PATH` oder in den Referenzumgebungsvariablen des Knotens befindet, können Sie die Shell explizit in der Taskbefehlszeile aufrufen.
* **Ressourcendateien** , die die zu verarbeitenden Daten enthalten. Diese Dateien werden automatisch aus Blob Storage in einem Azure-Speicherkonto vom Typ „Allgemein“ auf den Knoten kopiert, bevor die Befehlszeile des Tasks ausgeführt wird. Weitere Informationen finden Sie in den Abschnitten [Starttask](#start-task) und [Dateien und Verzeichnisse](#files-and-directories).
* Die von Ihrer Anwendung benötigten **Umgebungsvariablen** . Weitere Informationen finden Sie im Abschnitt [Umgebungseinstellungen für Tasks](#environment-settings-for-tasks) .
* Die **Einschränkungen** , mit denen der Task ausgeführt werden soll. Beispielsweise enthalten Einschränkungen die maximale Ausführungsdauer des Tasks, die maximale Anzahl von Wiederholungen für einen nicht erfolgreichen Task und die maximal zulässige Beibehaltungsdauer für Dateien im Arbeitsverzeichnis des Tasks.
* **Anwendungspakete** für die Bereitstellung auf dem Computeknoten, auf dem der Task ausgeführt werden soll. [Anwendungspakete](#application-packages) bieten eine vereinfachte Bereitstellung und Versionsverwaltung der Anwendungen, die von Ihren Tasks ausgeführt werden. Anwendungspakete auf Task-Ebene sind insbesondere in Umgebungen mit gemeinsam genutzten Pools praktisch, bei denen verschiedene Aufträge in einem Pool ausgeführt werden und der Pool nach Abschluss des Auftrags nicht gelöscht wird. Wenn Ihr Auftrag über weniger Tasks als Knoten im Pool verfügt, können Task-Anwendungspakete die Datenübertragung minimieren, da Ihre Anwendung nur auf den Knoten bereitgestellt wird, die Tasks ausführen.

Zusätzlich zu Tasks, die Sie zur Berechnung auf einem Knoten definieren, werden vom Batch-Dienst die folgenden speziellen Tasks bereitgestellt:

* [Startaufgabe](#start-task)
* [Auftrags-Manager-Aufgabe](#job-manager-task)
* [Tasks zur Auftragsvorbereitung und -freigabe](#job-preparation-and-release-tasks)
* [Tasks mit mehreren Instanzen](#multi-instance-tasks)
* [Abhängigkeiten von Aufgaben](#task-dependencies)

### <a name="start-task"></a>Startaufgabe
Durch Verknüpfen eines **Starttasks** mit einem Pool können Sie die Betriebssystemumgebung der dazugehörigen Knoten vorbereiten. So können Sie beispielsweise Aktionen wie das Installieren der Anwendungen, die von Ihren Tasks ausgeführt werden, oder das Starten von Hintergrundprozessen durchführen. Der Starttask wird jedes Mal ausgeführt, wenn ein Knoten gestartet wird. Dies gilt, solange sich der Knoten im Pool befindet, und zwar auch beim ersten Hinzufügen des Knotens zum Pool und bei einem Neustart oder Reimaging.

Ein wesentlicher Vorteil von Starttasks besteht darin, dass sie alle Informationen enthalten können, die zum Konfigurieren eines Computeknotens und zum Installieren der für die Taskausführung benötigten Anwendungen erforderlich sind. Das Erhöhen der Anzahl von Knoten in einem Pool ist daher so einfach wie das Angeben der neuen Zielknotenanzahl. Der Starttask stellt dem Batch-Dienst die Informationen zur Verfügung, die erforderlich sind, um die neuen Knoten zu konfigurieren und zur Annahme von Tasks vorzubereiten.

Wie bei jedem anderen Azure Batch-Task kann zusätzlich zu einer auszuführenden **Befehlszeile** eine Liste mit **Ressourcendateien** in [Azure Storage][azure_storage] angegeben werden. Der Batch-Dienst kopiert zunächst die Ressourcendateien aus Azure Storage auf den Knoten und führt dann die Befehlszeile aus. Bei einem poolbezogenen Starttask enthält die Dateiliste üblicherweise die Taskanwendung und die damit verbundenen Abhängigkeiten.

Der Starttask kann aber auch Referenzdaten für alle Tasks enthalten, die auf dem Computeknoten ausgeführt werden. Die Befehlszeile eines Starttasks kann etwa einen `robocopy`-Vorgang ausführen, um Anwendungsdateien (die als Ressourcendateien angegeben und auf den Knoten heruntergeladen wurden) aus dem [Arbeitsverzeichnis](#files-and-directories) des Starttasks in den [freigegebenen Ordner](#files-and-directories) zu kopieren, und anschließend eine MSI-Datei oder `setup.exe` ausführen.

> [!IMPORTANT]
> Von Batch wird derzeit *ausschließlich* der Speicherkontotyp **Allgemein** unterstützt, wie in [Informationen zu Azure Storage-Konten](../storage/storage-create-storage-account.md) unter Schritt 5 von [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) beschrieben. In Ihren Batch-Tasks (einschließlich Standardtasks, Starttasks und Tasks zur Auftragsvorbereitung und -freigabe) müssen Ressourcendateien angegeben werden, die sich *ausschließlich* in Speicherkonten vom Typ **Allgemein** befinden.
>
>

In der Regel empfiehlt es sich, dass der Batch-Dienst auf den Abschluss des Starttasks wartet und erst dann davon ausgeht, dass der Knoten nun für die Taskzuweisung bereit ist. Dieses Verhalten ist jedoch konfigurierbar.

Kann ein Starttask für einen Computeknoten nicht erfolgreich ausgeführt werden, wird der Status des Knotens entsprechend aktualisiert, und dem Knoten werden keine Tasks zugewiesen. Ein Starttask wird nicht erfolgreich durchgeführt, wenn ein Problem beim Kopieren der Ressourcendateien aus dem Speicher auftritt oder wenn der Prozess, der von der Befehlszeile ausgeführt wird, einen Exitcode ungleich NULL zurückgibt.

Wenn Sie den Starttask einem *vorhandenen* Pool hinzufügen oder ihn für diesen aktualisieren, müssen Sie dessen Computeknoten für den Starttask, der auf die Knoten angewendet wird, neu starten.

### <a name="job-manager-task"></a>Auftrags-Manager-Aufgabe
Ein **Auftrags-Manager-Task** dient in der Regel zum Steuern und Überwachen der Auftragsausführung – also etwa zum Erstellen und Übermitteln der Tasks für einen Auftrag, zum Bestimmen zusätzlich auszuführender Tasks oder zum Bestimmen, wann die Arbeit abgeschlossen ist. Ein Auftrags-Manager-Task ist jedoch nicht auf diese Aktivitäten beschränkt. Vielmehr handelt es sich um einen vollwertigen Task, der jegliche für den Auftrag erforderliche Aktionen ausführen kann. So kann ein Auftrags-Manager-Task beispielsweise eine als Parameter angegebene Datei herunterladen, den Inhalt der Datei analysieren und basierend auf diesem Inhalt zusätzliche Tasks übermitteln.

Eine Auftrags-Manager-Aufgabe wird vor allen anderen Aufgaben gestartet. Er zeichnet sich durch Folgendes aus:

* Er wird automatisch vom Batch-Dienst als Task übermittelt, wenn der Auftrag erstellt wird.
* Seine Ausführung wird so geplant, dass er vor den anderen Tasks in einem Auftrag ausgeführt wird.
* Der zugeordnete Knoten wird als letzter aus einem Pool entfernt, wenn der Pool verkleinert wird.
* Die Beendigung kann an die Beendigung aller Aufgaben im Auftrag gebunden sein.
* Ein neu zu startender Auftrags-Manager-Task hat höchste Priorität. Wenn kein Knoten im Leerlauf verfügbar ist, kann der Batch-Dienst einen der anderen ausgeführten Tasks im Pool beenden, um Platz für die Ausführung des Auftrags-Manager-Tasks zu schaffen.
* Ein Auftrags-Manager-Task in einem Auftrag besitzt keine höhere Priorität als die Tasks in anderen Aufträgen. Auftragsübergreifend werden nur Prioritäten auf Auftragsebene beachtet.

### <a name="job-preparation-and-release-tasks"></a>Tasks zur Auftragsvorbereitung und -freigabe
Batch stellt Tasks zur Auftragsvorbereitung für die Einrichtung vor der Auftragsausführung bereit. Tasks zur Auftragsfreigabe dienen zur Wartung oder Bereinigung nach der Auftragsausführung.

* **Task zur Auftragsvorbereitung**: Wird vor der Ausführung der anderen Tasks eines Auftrags auf allen Computeknoten ausgeführt, für die eine Taskausführung geplant ist. Mithilfe eines Tasks zur Auftragsvorbereitung können Sie beispielsweise auftragsspezifische Daten kopieren, die von allen Tasks gemeinsam genutzt werden.
* **Task zur Auftragsfreigabe**: Wird nach Abschluss eines Auftrags auf jedem Knoten im Pool ausgeführt, auf dem mindestens ein Task ausgeführt wurde. Mit einem Task zur Auftragsfreigabe können Sie Daten löschen, die vom Task zur Auftragsvorbereitung kopiert wurden, oder Diagnoseprotokolldaten komprimieren und hochladen.

Sowohl für Tasks zur Auftragsvorbereitung als auch für Tasks zur Auftragsfreigabe können Sie eine Befehlszeile angeben, die ausgeführt werden soll, wenn der Task aufgerufen wird. Sie ermöglichen beispielsweise das Herunterladen von Dateien, das Ausführen mit erhöhten Rechten, die Verwendung benutzerdefinierter Umgebungsvariablen sowie das Angeben der maximalen Ausführungsdauer, der Anzahl von Wiederholungsversuchen und der Dateiaufbewahrungsdauer.

Weitere Informationen zu Auftragsvorbereitungs- und -freigabetasks finden Sie unter [Ausführen von Auftragsvorbereitungs- und Auftragsabschlusstasks auf Azure Batch-Computeknoten](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Task mit mehreren Instanzen
Ein [Task mit mehreren Instanzen](batch-mpi.md) ist ein Task, der für die gleichzeitige Ausführung auf mehreren Computeknoten konfiguriert ist. Tasks mit mehreren Instanzen ermöglichen Computeszenarien mit hohem Leistungsbedarf, in denen eine einzelne Workload von einer gemeinsam zugeordneten Gruppe von Computeknoten verarbeitet werden muss – etwa bei MPI (Message Passing Interface).

Ausführliche Informationen zum Ausführen von MPI-Aufträgen in Batch mithilfe der Batch .NET-Bibliothek finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](batch-mpi.md).

### <a name="task-dependencies"></a>Abhängigkeiten von Aufgaben
Mithilfe von [Abhängigkeiten von Aufgaben](batch-task-dependencies.md) können Sie – wie der Name schon vermuten lässt – angeben, dass die Ausführung einer Aufgabe vom Abschluss einer anderen Aufgaben abhängig ist. Dieses Feature ist hilfreich, wenn ein Downstreamtask die Ausgabe eines Upstreamtasks nutzt oder ein Upstreamtask eine erforderliche Initialisierung für einen Downstreamtask vornimmt. Um dieses Feature verwenden zu können, müssen Sie zuerst Aufgabenabhängigkeiten für den Batchauftrag aktivieren. Geben Sie dann für jede Aufgabe, die von einer (oder mehreren) anderen abhängt, die übergeordneten Aufgaben an.

Mit Abhängigkeiten von Aufgaben können Sie beispielsweise folgende Szenarien konfigurieren:

* *TaskB* hängt von *TaskA* ab. (*TaskB* kann erst ausgeführt werden, wenn *TaskA* abgeschlossen ist.)
* *TaskC* hängt sowohl von *TaskA* als auch von *TaskB* ab.
* Die Ausführung von *TaskD* hängt von mehreren Tasks (beispielsweise von den Tasks *1* bis *10*) ab.

Weitere ausführliche Informationen zu diesem Feature finden Sie unter [Taskabhängigkeiten in Azure Batch](batch-task-dependencies.md) und im Codebeispiel [TaskDependencies][github_sample_taskdeps] im GitHub-Repository [azure-batch-samples][github_samples].

## <a name="environment-settings-for-tasks"></a>Umgebungseinstellungen für Tasks
Jeder Task, der vom Batch-Dienst ausgeführt wird, hat Zugriff auf die Umgebungsvariablen, die für Computeknoten festgelegt werden. Dies gilt auch für Umgebungsvariablen, die vom Batch-Dienst definiert werden ([dienstdefiniert][msdn_env_vars]), und für benutzerdefinierte Umgebungsvariablen, die Sie für Ihre Tasks definieren können. Die Anwendungen und Skripts, die von Ihren Tasks ausgeführt werden, haben während der Ausführung Zugriff auf diese Umgebungsvariablen.

Sie können benutzerdefinierte Umgebungsvariablen auf Task- oder Auftragsebene festlegen, indem Sie die Eigenschaft *Umgebungseinstellungen* für diese Entitäten auffüllen. Informieren Sie sich beispielsweise über den Vorgang [Hinzufügen einer Aufgabe zu einem Auftrag][rest_add_task] (Batch REST-API) oder über die Eigenschaften [CloudTask.EnvironmentSettings][net_cloudtask_env] und [CloudJob.CommonEnvironmentSettings][net_job_env] in Batch .NET.

Die Clientanwendung bzw. der Dienst kann die Umgebungsvariablen eines Tasks abrufen – sowohl dienstdefiniert als auch benutzerdefiniert –, indem der Vorgang [Abrufen von Informationen zu einer Aufgabe][rest_get_task_info] (Batch REST) verwendet oder auf die Eigenschaft [CloudTask.EnvironmentSettings][net_cloudtask_env] (Batch .NET) zugegriffen wird. Prozesse, die auf einem Computeknoten ausgeführt werden, können auf diese und andere Umgebungsvariablen auf dem Knoten zugreifen – beispielsweise mit der vertrauten `%VARIABLE_NAME%`-Syntax (Windows) oder mit der `$VARIABLE_NAME`-Syntax (Linux).

Eine vollständige Liste mit allen vom Dienst definierten Umgebungsvariablen finden Sie unter [Compute node environment variables][msdn_env_vars] (Computeknoten-Umgebungsvariablen).

## <a name="files-and-directories"></a>Dateien und Verzeichnisse
Jeder Task verfügt über ein *Arbeitsverzeichnis* , unter dem null oder mehr Dateien und Verzeichnisse erstellt werden. Dieses Arbeitsverzeichnis kann für das Speichern des von einem Task ausgeführten Programms, der dabei verarbeiteten Daten und der Ausgabe der Verarbeitung verwendet werden. Alle Dateien und Verzeichnisse eines Tasks befinden sich im Besitz des Taskbenutzers.

Der Batch-Dienst stellt einen Teil des Dateisystems auf einem Knoten als *Stammverzeichnis*bereit. Tasks können auf das Stammverzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_ROOT_DIR` verweisen. Weitere Informationen zur Verwendung von Umgebungsvariablen finden Sie unter [Umgebungseinstellungen für Tasks](#environment-settings-for-tasks).

Das Stammverzeichnis enthält die folgenden Verzeichnisstruktur:

![Computeknoten-Verzeichnisstruktur][1]

* **shared**: Dieses Verzeichnis bietet Lese-/Schreibzugriff auf *alle* Tasks, die auf einem Knoten ausgeführt werden. Mit jedem Task, der auf dem Knoten ausgeführt wird, können Dateien in diesem Verzeichnis erstellt, gelesen, aktualisiert und gelöscht werden. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_SHARED_DIR` verweisen.
* **startup**: Dieses Verzeichnis wird von einem Starttask als Arbeitsverzeichnis verwendet. Alle Dateien, die vom Starttask auf den Knoten heruntergeladen werden, werden hier gespeichert. Der Starttask kann Dateien unter diesem Verzeichnis erstellen, lesen, aktualisieren und löschen. Tasks können auf dieses Verzeichnis zugreifen, indem sie auf die Umgebungsvariable `AZ_BATCH_NODE_STARTUP_DIR` verweisen.
* **Tasks**: Für jeden Task, der auf dem Knoten ausgeführt wird, wird ein Verzeichnis erstellt. Der Zugriff erfolgt durch einen Verweis auf die Umgebungsvariable `AZ_BATCH_TASK_DIR` .

    Innerhalb jedes Taskverzeichnisses erstellt der Batch-Dienst ein Arbeitsverzeichnis (`wd`), dessen eindeutiger Pfad durch die `AZ_BATCH_TASK_WORKING_DIR`-Umgebungsvariable angegeben wird. Dieses Verzeichnis bietet Lese-/Schreibzugriff auf den Task. Die Aufgabe kann Dateien unter diesem Verzeichnis erstellen, lesen, aktualisieren und löschen. Dieses Verzeichnis wird auf der Grundlage der für den Vorgang angegebenen Einschränkung *RetentionTime* beibehalten.

    `stdout.txt` und `stderr.txt`: Diese Dateien werden während der Ausführung des Tasks in den Taskordner geschrieben.

> [!IMPORTANT]
> Wenn ein Knoten aus einem Pool entfernt wird, werden *alle* auf dem Knoten gespeicherten Dateien entfernt.
>
>

## <a name="application-packages"></a>Anwendungspakete
Das Feature [Anwendungspakete](batch-application-packages.md) ermöglicht eine einfache Verwaltung und Bereitstellung von Anwendungen für die in Ihren Pools benötigten Computeknoten. Sie können mehrere Versionen der von Ihren Tasks ausgeführten Anwendungen hochladen und verwalten (einschließlich Binärdaten und Unterstützungsdateien). Anschließend können Sie dann eine oder mehrere dieser Anwendungen für die Computeknoten in Ihrem Pool bereitstellen.

Sie können die Anwendungspakete auf Pool- und Task-Ebene angeben. Wenn Sie Pool-Anwendungspakete angeben, wird die Anwendung für jeden Knoten im Pool bereitgestellt. Wenn Sie Task-Anwendungspakete angeben, wird die Anwendung nur für Knoten bereitgestellt, die mindestens einen Task des Auftrags ausführen, bevor die Befehlszeilen des Tasks ausgeführt werden.

Batch kümmert sich um die Details für die Arbeit mit Azure Storage, um Anwendungspakete zu speichern und für Serverknoten bereitzustellen, damit sowohl Ihr Code als auch der Verwaltungsaufwand vereinfacht wird.

Weitere Informationen zum Anwendungspaketfeature finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

> [!NOTE]
> Wenn Sie die Pool-Anwendungspakete zu einem *vorhandenen* Pool hinzufügen, müssen sie dessen Computeknoten für die Anwendungspakete, die auf den Knoten bereitgestellt werden, neu starten.
>
>

## <a name="pool-and-compute-node-lifetime"></a>Gültigkeitsdauer für Pools und Computeknoten
Beim Entwerfen Ihrer Azure Batch-Lösung müssen Sie entscheiden, wie und wann Pools erstellt werden und wie lange Computeknoten innerhalb dieser Pools verfügbar bleiben sollen.

An einem Ende des Spektrums können Sie einen Pool für jeden Auftrag erstellen, den Sie senden, und den Pool dann löschen, sobald die Ausführung seiner Tasks abgeschlossen ist. Auf diese Weise wird die Effektivität der Nutzung erhöht, da die Knoten nur bei Bedarf zugeordnet und sofort heruntergefahren werden, wenn sie sich im Leerlauf befinden. Dies bedeutet, dass der Auftrag bis zur Zuteilung der Knoten warten muss. Dabei ist aber wichtig zu erwähnen, dass für die Tasks die Ausführung geplant wird, sobald die Knoten einzeln verfügbar sind, zugeteilt wurden und der Starttask abgeschlossen ist. Batch wartet mit dem Zuweisen von Tasks zu den Knoten *nicht*, bis alle Knoten in einem Pool verfügbar sind. Dadurch ist eine maximale Auslastung aller verfügbaren Knoten gewährleistet.

Falls dagegen der sofortige Start von Aufträgen höchste Priorität hat, können Sie bereits vorab einen Pool erstellen und die zugehörigen Knoten vor der Auftragsübermittlung verfügbar machen. Dieses Szenario ermöglicht zwar den sofortigen Start von Tasks, führt aber unter Umständen auch dazu, dass sich Knoten im Leerlauf befinden, während sie auf die Zuteilung warten.

Zur Bewältigung einer variablen, kontinuierlichen Auslastung wird in der Regel ein kombinierter Ansatz verwendet. Sie können über einen Pool verfügen, an den mehrere Aufträge übermittelt werden, und die Anzahl von Knoten mittels Skalierung flexibel an die jeweilige Auslastung anpassen. (Weitere Informationen finden Sie im nächsten Abschnitt unter [Skalieren von Computeressourcen](#scaling-compute-resources).) Dies kann reaktiv auf der Grundlage der aktuellen Auslastung oder proaktiv erfolgen, sofern die Auslastung vorausgesagt werden kann.

## <a name="pool-network-configuration"></a>Konfiguration von Poolnetzwerken

Wenn Sie einen Pool mit Computeknoten in Azure Batch erstellen, können Sie die ID eines [virtuellen Azure-Netzwerks (VNet)](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/) festlegen, in dem die Computeknoten des Pools erstellt werden sollen, festlegen.

* Nur Pools der **Konfiguration von Clouddiensten** können einem VNet zugewiesen werden.

* Das VNet muss folgende Eigenschaften aufweisen:

   * Es muss sich in derselben Azure-**Region** wie das Azure Batch-Konto befinden.
   * Es muss sich im selben **Abonnement** wie das Azure Batch-Konto befinden.
   * Es muss ein **klassisches** VNet sein. VNets, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden, werden nicht unterstützt.

* Das VNet sollte über genügend freie **IP-Adressen** verfügen, um die `targetDedicated`-Eigenschaft des Pools zu ermöglichen. Wenn das Subnetz nicht über genügend freie IP-Adressen verfügt, belegt der Batch-Dienst teilweise die Computeknoten im Pool und gibt einen Anpassungsfehler zurück.
* Das Dienstprinzipal *MicrosoftAzureBatch* muss über die rollenbasierte Zugriffssteuerungsrolle [Mitwirkender von klassischen virtuellen Computern](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor) für den angegebenen VNet verfügen. Führen Sie im Azure-Portal die folgenden Schritte aus:

  * Wählen Sie **VNet** aus, klicken Sie dann auf **Zugriffssteuerung (IAM)** > **Rollen** > **Mitwirkender von klassischen virtuellen Computern** > **Hinzufügen**
  * Geben Sie „MicrosoftAzureBatch“ in das **Suchfeld** ein.
  * Aktivieren Sie das Kontrollkästchen **MicrosoftAzureBatch**.
  * Klicken Sie auf die Schaltfläche **Auswählen**.

* Wenn die Kommunikation mit den Computeknoten durch **Netzwerksicherheitsgruppen (NSG)** verweigert wird, die dem VNet zugeordnet sind, dann legt der Batch-Dienst den Status der Computeknoten auf **nicht verwendbar** fest. Das Subnetz muss eine Kommunikation vom Azure Batch-Dienst aus zulassen, um Tasks auf den Computeknoten planen zu können.

## <a name="scaling-compute-resources"></a>Skalieren von Computeressourcen
Mit der [automatischen Skalierung](batch-automatic-scaling.md)kann der Batch-Dienst die Anzahl von Computeknoten in einem Pool dynamisch an die aktuelle Workload und die Ressourcenverwendung Ihres Computeszenarios anpassen. So können Sie die Gesamtkosten für die Ausführung Ihrer Anwendung senken, indem Sie nur die erforderlichen Ressourcen verwenden und nicht benötigte Ressourcen freigeben.

Sie aktivieren die automatische Skalierung, indem Sie eine [Formel für die automatische Skalierung](batch-automatic-scaling.md#automatic-scaling-formulas) erstellen und diese Formel einem Pool zuordnen. Der Batch-Dienst verwendet die Formel, um die vorgegebene Anzahl von Knoten im Pool für das nächste Skalierungsintervall (welches Sie konfigurieren können) zu bestimmen. Sie können für einen Pool die Einstellungen für die automatische Skalierung angeben, wenn Sie diesen erstellen, oder Sie können die Skalierung für einen Pool später aktivieren. Außerdem können Sie die Skalierungseinstellungen für einen Pool aktualisieren, für den die Skalierung aktiviert ist.

Beispielsweise kann es für einen Auftrag erforderlich sein, eine sehr große Anzahl von Tasks zu übermitteln, die ausgeführt werden sollen. Sie können dem Pool eine Skalierungsformel zuweisen, die die Anzahl von Knoten im Pool auf der Grundlage der aktuellen Anzahl von Tasks in der Warteschlange sowie der Abschlussrate der Tasks im Auftrag anpasst. Der Batch-Dienst wertet die Formel in regelmäßigen Abständen aus und ändert die Größe des Pools auf Grundlage von Workload und anderen Formeleinstellungen. Der Dienst fügt bei Bedarf Knoten hinzu, wenn eine große Anzahl von Tasks in der Warteschlange vorhanden ist und entfernt Knoten, wenn keine Tasks in der Warteschlange sind oder ausgeführt werden. 

Eine Skalierungsformel kann auf den folgenden Metriken basieren:

* **Zeitmetriken** : Basieren auf Statistiken, die alle fünf Minuten für die angegebene Anzahl von Stunden erfasst werden.
* **Ressourcenmetriken** : Basieren auf CPU-Auslastung, Bandbreitenauslastung, Speicherauslastung und Knotenanzahl.
* **Taskmetriken**: Basieren auf dem Taskstatus – beispielsweise *Aktiv* (in der Warteschlange), *Wird ausgeführt* oder *Abgeschlossen*.

Wenn die Anzahl von Computeknoten in einem Pool durch die automatische Skalierung verringert wird, müssen Sie sich überlegen, wie Sie Tasks behandeln, die zum Zeitpunkt des Verringerungsvorgangs ausgeführt werden. Hierfür verfügt Batch über eine *Option zum Aufheben der Knotenzuordnung* , die Sie in Ihre Formeln einfügen können. Sie können beispielsweise angeben, dass ausgeführte Tasks sofort beendet und dann für die Ausführung auf einem anderen Knoten erneut in die Warteschlange eingereiht werden oder dass die Fertigstellung abgewartet werden soll, bevor der Knoten aus dem Pool entfernt wird.

Weitere Informationen zur automatischen Skalierung einer Anwendung finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](batch-automatic-scaling.md).

> [!TIP]
> Legen Sie die vorgegebene Anzahl von Knoten am Ende eines Auftrags auf Null fest, und lassen Sie das Abschließen ausgeführter Tasks zu, um die Auslastung von Computeressourcen zu maximieren.
>
>

## <a name="security-with-certificates"></a>Sicherheit mit Zertifikaten
Zertifikate müssen in der Regel beim Ver- und Entschlüsseln vertraulicher Informationen für Tasks verwendet werden. Ein Beispiel wäre etwa der Schlüssel für ein [Azure-Speicherkonto][azure_storage]. Hierzu können Sie Zertifikate auf Knoten installieren. Verschlüsselte geheime Schlüssel werden über Befehlszeilenparameter an Tasks übergeben oder in einer der Taskressourcen eingebettet. Zum Entschlüsseln können dann installierte Zertifikate verwendet werden.

Zum Hinzufügen eines Zertifikats zu einem Batch-Konto können Sie den Vorgang [Hinzufügen eines Zertifikats zu einem Konto][rest_add_cert] (Batch REST) oder die Methode [CertificateOperations.CreateCertificate][net_create_cert] (Batch .NET) verwenden. Sie können das Zertifikat dann einem neuen oder vorhandenen Pool zuordnen. Wenn ein Zertifikat einem Pool zugeordnet ist, installiert der Batch-Dienst das Zertifikat auf jedem Knoten im Pool. Der Batch-Dienst installiert die entsprechenden Zertifikate beim Start des Knotens, bevor Tasks gestartet werden (einschließlich Starttask und Auftrags-Manager-Task).

Wenn Sie Zertifikate zu einem *vorhandenen* Pool hinzufügen, müssen sie dessen Computeknoten für die Zertifikate, die auf die Knoten angewandt werden, neu starten.

## <a name="error-handling"></a>Fehlerbehandlung
Es kann vorkommen, dass Sie in Ihrer Batch-Lösung sowohl Task- als auch Anwendungsfehler behandeln müssen.

### <a name="task-failure-handling"></a>Behandeln von Taskfehlern
Bei Taskfehlern wird zwischen folgenden Kategorien unterschieden:

* **Planungsfehler**

    Wenn die Übertragung von Dateien, die für einen Task angegeben sind, aus irgendeinem Grund nicht erfolgreich durchgeführt werden kann, wird für den Task ein *Planungsfehler* festgelegt.

    Planungsfehler können auftreten, wenn die Ressourcendateien des Tasks verschoben wurden, das Speicherkonto nicht mehr zur Verfügung steht oder ein anderes Problem aufgetreten ist, das ein erfolgreiches Kopieren von Dateien auf den Knoten verhindert hat.
* **Anwendungsfehler**

    Ein Fehler kann auch beim in der Befehlszeile des Tasks angegebenen Prozess auftreten. Der Prozess gilt als nicht erfolgreich, wenn der Exitcode, der vom durch den Task ausgeführten Prozess zurückgegeben wird, ungleich Null ist (siehe *Exitcodes für Tasks* im nächsten Abschnitt).

    Batch kann so konfiguriert werden, dass der Task im Falle eines Anwendungsfehlers automatisch mit einer bestimmten Häufigkeit wiederholt wird.
* **Einschränkungsfehler**

    Mit der Einschränkung *maxWallClockTime*können Sie die maximale Ausführungsdauer für einen Auftrag oder Task angeben. Dies kann nützlich sein für das Beenden von Tasks, bei denen kein Fortschritt stattfindet.

    Wenn die maximal zulässige Zeitspanne überschritten wurde, wird der Task als *abgeschlossen* gekennzeichnet. Als Exitcode wird jedoch `0xC000013A` zurückgegeben, und das Feld *schedulingError* wird als `{ category:"ServerError", code="TaskEnded"}` markiert.

### <a name="debugging-application-failures"></a>Debuggen von Anwendungsfehlern
* `stderr` und `stdout`

    Während der Ausführung generiert eine Anwendung unter Umständen eine Diagnoseausgabe für die Problembehandlung. Wie bereits unter [Dateien und Verzeichnisse](#files-and-directories) erwähnt, schreibt der Batch-Dienst eine Standardausgabe und eine Standardfehlerausgabe in die Dateien `stdout.txt` und `stderr.txt` im Taskverzeichnis auf dem Computeknoten. Diese Dateien können Sie über das Azure-Portal oder über ein Batch SDK herunterladen. So können Sie diese und andere Dateien beispielsweise mithilfe von [ComputeNode.GetNodeFile][net_getfile_node] und [CloudTask.GetNodeFile][net_getfile_task] in der .NET-Bibliothek von Batch zu Problembehandlungszwecken abrufen.
* **Exitcodes für Tasks**

    Wie bereits erwähnt, wird ein Task vom Batch-Dienst als nicht erfolgreich gekennzeichnet, wenn der vom Task ausgeführte Prozess einen Exitcode ungleich Null zurückgibt. Wenn ein Task einen Prozess ausführt, füllt Batch die Exitcode-Eigenschaft des Tasks mit dem *Rückgabecode des Prozesses*auf. Wichtig ist hierbei, dass der Exitcode eines Tasks **nicht** vom Batch-Dienst bestimmt wird. Der Exitcode eines Tasks wird vom Prozess selbst bestimmt oder von dem Betriebssystem, unter dem der Prozess ausgeführt wird.

### <a name="accounting-for-task-failures-or-interruptions"></a>Erläuterung zu Taskfehlern oder Unterbrechungen
Tasks werden gelegentlich nicht erfolgreich ausgeführt oder unterbrochen. Unter Umständen tritt in der Taskanwendung selbst ein Fehler auf, oder der Knoten, auf dem der Task ausgeführt wird, wird neu gestartet. Außerdem kann der Knoten während einer Änderung der Poolgröße ggf. entfernt werden, wenn die Richtlinie für die Aufhebung der Zuordnung des Knotens so konfiguriert ist, dass der Knoten sofort entfernt wird, ohne auf den Abschluss des Tasks zu warten. In diesen Fällen kann der Task von Batch automatisch wieder der Warteschlange hinzugefügt und auf einem anderen Knoten ausgeführt werden.

Außerdem können zeitweilig Probleme auftreten, die dazu führen, dass eine Aufgabe nicht mehr reagiert oder zu lange dauert. Sie können das maximale Ausführungsintervall für einen Task festlegen. Wenn das maximale Ausführungsintervall überschritten wird, unterbricht der Batch-Dienst die Taskanwendung.

### <a name="connecting-to-compute-nodes"></a>Herstellen einer Verbindung mit Computeknoten
Sie können weitere Debug- und Problembehandlungsmaßnahmen durchführen, indem Sie sich per Remotezugriff an einem Computeknoten anmelden. Sie können das Azure-Portal verwenden, um eine RDP-Datei (Remotedesktopprotokoll) für Windows-Knoten herunterzuladen und Secure Shell (SSH)-Verbindungsinformationen für Linux-Knoten abzurufen. Hierfür können Sie auch die Batch-APIs verwenden – beispielsweise mit [Batch .NET][net_rdpfile] oder [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes).

> [!IMPORTANT]
> Um über RDP oder SSH eine Verbindung mit einem Knoten herzustellen, müssen Sie zuerst einen Benutzer auf dem Knoten erstellen. Zu diesem Zweck können Sie das Azure-Portal verwenden, über die Batch REST-API [einem Knoten ein Benutzerkonto hinzufügen][rest_create_user], die Methode [ComputeNode.CreateComputeNodeUser][net_create_user] in Batch .NET aufrufen oder die Methode [add_user][py_add_user] im Batch Python-Modul aufrufen.
>
>

### <a name="troubleshooting-problematic-compute-nodes"></a>Problembehandlung bei problematischen Computeknoten
Wenn bei einigen Ihrer Tasks Fehler auftreten, kann Ihre Batch-Clientanwendung oder der Dienst die Metadaten der fehlgeschlagenen Tasks prüfen, um einen fehlerhaften Knoten zu finden. Jeder Knoten in einem Pool erhält eine eindeutige ID, und der Knoten, auf dem ein Task ausgeführt wird, ist in den Metadaten des Tasks angegeben. Nachdem Sie einen „Problemknoten“ identifiziert haben, können Sie dafür verschiedene Aktionen durchführen:

* **Neustarten des Knotens** ([REST][rest_reboot] | [.NET][net_reboot])

    Durch einen Neustart des Knotens können latente Probleme wie etwa hängende oder abgestürzte Prozesse behoben werden. Wenn Ihr Pool ein Starttask oder der Auftrag ein Auftragsvorbereitungstask nutzt, werden diese Tasks beim Neustart des Knotens ausgeführt.
* **Reimaging des Knotens** ([REST][rest_reimage] | [.NET][net_reimage])

    Dadurch wird das Betriebssystem auf dem Knoten neu installiert. Wie beim Neustart eines Knotens werden Start- und Auftragsvorbereitungstasks nach dem Reimaging des Knotens erneut ausgeführt.
* **Entfernen des Knotens aus dem Pool** ([REST][rest_remove] | [.NET][net_remove])

    Manchmal ist es erforderlich, den Knoten aus dem Pool vollständig zu entfernen.
* **Deaktivieren der Taskplanung auf dem Knoten** ([REST][rest_offline] | [.NET][net_offline])

    Dadurch wird der Knoten offline geschaltet, sodass ihm keine weiteren Tasks mehr zugewiesen werden. Er wird aber weiterhin ausgeführt und verbleibt im Pool. So können Sie die Fehlerursache weiter untersuchen, ohne dass die Daten der fehlgeschlagenen Tasks verloren gehen und durch den Knoten weitere Fehler auftreten. So können Sie beispielsweise die Taskplanung auf dem Knoten deaktivieren und sich [per Remotezugriff anmelden](#connecting-to-compute-nodes), um die Ereignisprotokolle des Knotens zu prüfen oder andere Schritte zur Problembehandlung auszuführen. Nach Abschluss der Prüfung können Sie den Knoten wieder online schalten, indem Sie die Taskplanung ([REST][rest_online] | [.NET][net_online]) aktivieren oder eine andere der oben genannten Aktionen durchführen.

> [!IMPORTANT]
> Bei jeder der in diesem Abschnitt beschriebenen Aktionen (Neustart, Reimaging und Deaktivieren der Taskplanung) können Sie festlegen, wie mit auf dem Knoten ausgeführten Tasks verfahren werden soll, wenn Sie die Aktion durchführen. Wenn Sie also etwa auf einem Knoten mit der Batch .NET-Clientbibliothek die Taskplanung deaktivieren, können Sie durch das Festlegen des Enumerationswerts [DisableComputeNodeSchedulingOption][net_offline_option] angeben, ob ausgeführte Tasks **beendet**, für die Planung auf anderen Knoten **wieder in eine Warteschlange eingereiht** oder vor dem Ausführen der Aktion abgeschlossen werden sollen (**TaskCompletion**).
>
>

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie sich [Erste Schritte mit der Azure-Batch-Bibliothek für .NET](batch-dotnet-get-started.md)mit einer Schritt-für-Schritt-Anleitung für eine Batch-Beispielanwendung durch. Es gibt auch eine [Python-Version](batch-python-tutorial.md) des Tutorials, in der eine Workload auf Linux-Computeknoten ausgeführt wird.
* Laden Sie das [Batch-Explorer][github_batchexplorer]-Beispielprojekt herunter, und erstellen Sie es, um es während der Entwicklung Ihrer Batch-Lösungen zu verwenden. Mit dem Batch-Explorer können Sie die folgenden Aktionen und viele mehr ausführen:

  * Überwachen und Bearbeiten von Pools, Aufträgen und Tasks in Ihrem Batch-Konto
  * Herunterladen von `stdout.txt`, `stderr.txt` und anderen Dateien von Knoten
  * Erstellen von Benutzern auf Knoten und Herunterladen von RDP-Dateien für die Remoteanmeldung
* Informieren Sie sich über das [Erstellen von Pools mit Linux-Computeknoten](batch-linux-nodes.md).
* Besuchen Sie das [Azure Batch-Forum][batch_forum] auf der MSDN-Website. Das Forum ist ein guter Ort für Fragen – ganz gleich, ob Sie Anfänger oder Experte sind.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
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
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

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

