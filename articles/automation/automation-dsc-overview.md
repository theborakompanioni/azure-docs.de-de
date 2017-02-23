---
title: "Azure Automation DSC – Übersicht | Microsoft Docs"
description: "Eine Übersicht über Azure Automation Desired State Configuration (DSC), die verwendeten Begriffe und bekannte Fehler."
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "PowerShell DSC, Desired State Configuration, Konfiguration des gewünschten Zustands, PowerShell DSC Azure"
ms.assetid: fd40cb68-c1a6-48c3-bba2-710b607d1555
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 02/02/2017
ms.author: magoedte;eslesar
translationtype: Human Translation
ms.sourcegitcommit: f0d3a5c1929bb6dafef735ae3f4291e329861cc9
ms.openlocfilehash: b750878703baf143a2e8247bdd01f358e462e598

---
# <a name="azure-automation-dsc-overview"></a>Azure Automation DSC – Übersicht

## <a name="what-is-azure-automation-dsc"></a>Was ist Azure Automation DSC?

Das Bereitstellen und Beibehalten des gewünschten Zustands von Servern und Anwendungsressourcen kann mühsam und fehleranfällig sein. Mit Azure Automation Desired State Configuration (DSC) können Sie den gewünschten Zustand aller Ihrer IT-Ressourcen nach Maß in der Cloud einheitlich bereitstellen, zuverlässig überwachen und automatisch aktualisieren. Automation DSC basiert auf PowerShell DSC und kann die Computerkonfiguration auf physischen Computern und virtuellen VMs unter Windows und Linux in der Cloud und lokal mit einem bestimmten Zustand abstimmen. Sie können eine kontinuierliche Bereitstellung von IT-Diensten mit einheitlicher Steuerung ermöglichen und schnelle Änderungen in Ihrer gesamten heterogenen Hybrid-IT-Umgebung problemlos verwalten.

Azure Automation DSC setzt auf den mit PowerShell DSC eingeführten Grundlagen auf, um eine einfachere Konfigurationsverwaltung zu ermöglichen. Azure Automation DSC führt dieselbe Verwaltungsschicht in [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) ein, wie Azure Automation sie bereits für die PowerShell-Skripterstellung bietet.

Mithilfe von Azure Automation DSC können Sie [PowerShell DSC-Konfigurationen erstellen und verwalten](https://technet.microsoft.com/library/dn249918.aspx), [DSC-Ressourcen importieren](https://technet.microsoft.com/library/dn282125.aspx) und DSC-Knotenkonfigurationen (MOF-Dokumente) generieren – und das alles in der Cloud. Diese DSC-Elemente werden auf dem [DSC-Pullserver](https://technet.microsoft.com/library/dn249913.aspx) von Azure Automation abgelegt, sodass Zielknoten Konfigurationen automatisch empfangen, dem gewünschten Zustand entsprechen und ihre Konformität melden. Azure Automation eignet sich für virtuelle oder physische Computer in Cloud- oder lokalen Umgebungen. 

Möchten Sie sich lieber ein Video ansehen? Betrachten Sie das folgende Video vom Mai 2015, als Azure Automation DSC erstmals angekündigt wurde. **Hinweis:** Die Konzepte und der Lebenszyklus, die in diesem Video erläutert werden, sind noch zutreffend, jedoch wurde Azure Automation DSC seit der Aufzeichnung des Videos beträchtlich weiterentwickelt. Es ist jetzt allgemeine verfügbar, besitzt eine umfangreichere Benutzeroberfläche im Azure-Portal und unterstützt viele zusätzliche Funktionen.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="azure-automation-dsc-terms"></a>Azure Automation DSC – Begriffe

### <a name="configuration"></a>Konfiguration

PowerShell DSC führt ein neues Konzept der sogenannten Konfigurationen ein. Mithilfe von Konfigurationen können Sie über die PowerShell-Syntax den gewünschten Zustand Ihrer Umgebung definieren. Um DSC zum Konfigurieren Ihrer Umgebung zu verwenden, definieren Sie zunächst einen Windows PowerShell-Skriptblock mit dem Schlüsselwort „Configuration“. Auf den Block muss ein Bezeichner folgen. Anschließend wird der Block mit Klammern ({}) abgetrennt.

![alt text](./media/automation-dsc-overview/AADSC_1.png)

Innerhalb des Konfigurationsblocks können Sie Knotenkonfigurationen definieren, mit denen die gewünschte Konfiguration für eine Gruppe von Knoten (Computern) in Ihrer Umgebung festgelegt wird, die gleich konfiguriert werden sollen. Auf diese Weise stellt eine Knotenkonfiguration eine „Rolle“ für einen oder mehrere Knoten dar. Ein Knotenkonfigurationsblock beginnt mit dem Schlüsselwort "Node". Nach diesem Schlüsselwort folgt der Name der Rolle, hierbei kann es sich um eine Variable oder einen Ausdruck handeln. Verwenden Sie nach dem Rollennamen Klammern {}, um den Knotenkonfigurationsblock abzutrennen.

![alt text](./media/automation-dsc-overview/AADSC_2.png)

Im Knotenkonfigurationsblock können Sie Ressourcenblöcke definieren, um spezifische DSC-Ressourcen zu konfigurieren. Ein Ressourcenblock beginnt mit dem Namen der Ressource, gefolgt vom gewünschten Bezeichner für diesen Block und Klammern {} zum Abtrennen des Blocks.

![alt text](./media/automation-dsc-overview/AADSC_3.png)

Ausführlichere Informationen zum Schlüsselwort „Configuration“ finden Sie unter [DSC-Konfigurationen](https://msdn.microsoft.com/en-us/powershell/dsc/configurations "DSC-Konfigurationen").

Das Ausführen (Kompilieren) einer DSC-Konfiguration führt zur Erstellung von einer oder mehreren DSC-Knotenkonfigurationen (MOF-Dokumenten), die die Einstellungen vorgeben, die von den DSC-Knoten zum Erreichen des gewünschten Zustands angewendet werden müssen.

Azure Automation DSC ermöglicht Ihnen das Importieren, Erstellen und Kompilieren von DSC-Konfigurationen in Azure Automation – ähnlich wie Runbooks in Azure Automation importiert, erstellt und gestartet werden können.

> [!IMPORTANT]
> Eine Konfiguration sollte nur einen Konfigurationsblock mit demselben Namen wie die Konfiguration in Azure Automation DSC enthalten.

### <a name="node-configuration"></a>Knotenkonfiguration

Wenn eine DSC-Konfiguration kompiliert wird, werden in Abhängigkeit von den Node-Blöcken in der Konfiguration eine oder mehrere Knotenkonfigurationen erstellt. Eine Knotenkonfiguration entspricht einem „MOF“- oder „Konfigurationsdokument“. Knotenkonfigurationen stellen eine „Rolle“ dar, z.B. Webserver oder Worker, deren gewünschten Zustand ein oder mehrere Knoten aufweisen sollen oder deren Konformität geprüft werden soll. Namen von Knotenkonfigurationen in Azure Automation DSC weisen das Format „Konfigurationsname.NodeConfigurationBlockName“ auf.

PS DSC-Knoten erkennen Knotenkonfigurationen, die entweder per DSC-Push oder mithilfe von Pullmethoden angewendet werden sollen. Azure Automation DSC basiert auf der DSC-Pullmethode, bei der Knoten Knotenkonfigurationen anfordern, die sie von Azure Automation DSC-Pullservern anwenden sollen. Da die Knoten die Anforderung an Azure Automation DSC senden, können sich die Knoten hinter Firewalls befinden, keine eingehenden Ports geöffnet haben usw. Sie benötigen nur ausgehenden Zugriff auf das Internet (entweder direkt oder über einen Proxy).

### <a name="node"></a>Knoten

Jeder Computer, dessen Konfiguration von DSC verwaltet wird, ist ein DSC-Knoten. Bei diesem Computer kann es sich um einen virtuellen Azure-Computer unter Windows oder Linux, eine lokale VM, einen physischen Host oder eine VM in einer anderen öffentlichen Cloud handeln. Knoten stellen Knotenkonfigurationen dar, die übernommen werden sollen, und bewahren die Konformität mit dem gewünschten Zustand, den sie definieren. Knoten melden ferner einem Berichtsserver ihre Konfiguration und ihren Konformitätszustand.

Mithilfe von Azure Automation DSC wird die Einbindung von Knoten einfach und flexibel. Azure Automation DSC erlaubt das Ändern der Knotenkonfiguration, die jedem Knoten zugewiesen ist. Wenn eine Knotenkonfigurationsänderung erfolgt, übernimmt der Knoten automatisch eine andere Rolle, indem er seine Konfiguration ändert und beginnt, Informationen zu seiner neuen Rolle zu melden. 

### <a name="resource"></a>Ressource

DSC-Ressourcen sind Bausteine, die Sie zum Definieren einer Windows PowerShell DSC-Konfiguration (Desired State Configuration) verwenden können. DSC verfügt über einen Satz integrierter Ressourcen, beispielsweise für Dateien und Ordner, Serverfunktionen und -rollen, Registrierungseinstellungen, Umgebungsvariablen sowie Dienste und Prozesse. Eine vollständige Liste der integrierten DSC-Ressourcen und Informationen zu deren Verwendung finden Sie unter [Integrierte Windows PowerShell DSC-Ressourcen](https://msdn.microsoft.com/powershell/dsc/builtinresource).

DSC-Ressourcen können auch als Bestandteil der PowerShell-Module importiert werden, um den Satz der integrierten DSC-Ressourcen zu erweitern. Wenn eine Knotenkonfiguration des Knotens Verweise auf nicht standardmäßige Ressourcen aufweist, werden diese Ressourcen über den DSC-Pullserver von DSC-Knoten abgerufen. Informationen zum Erstellen von benutzerdefinierten Ressourcen finden Sie unter [Erstellen von benutzerdefinierten Windows PowerShell DSC-Ressourcen](https://msdn.microsoft.com/powershell/dsc/authoringresource).

Zum Lieferumfang von Azure Automation DSC gehören dieselben integrierten DSC-Ressourcen wie bei PS DSC. Sie können Azure Automation DSC zusätzliche Ressourcen hinzufügen, indem Sie PowerShell-Module mit den gewünschten Ressourcen in Azure Automation importieren.

### <a name="compilation-job"></a>Kompilierungsauftrag

Ein Kompilierungsauftrag in Azure Automation DSC ist eine Instanz einer Konfigurationskompilierung, um eine oder mehrere Knotenkonfigurationen zu erstellen. Sie ähneln Runbookaufträgen in Azure Automation, führen jedoch abgesehen von der Erstellung von Knotenkonfigurationen keine wirklichen Aufgaben aus. Alle durch einen Kompilierungsauftrag erstellten Knotenkonfigurationen werden automatisch auf dem Azure Automation DSC-Pullserver platziert und überschreiben vorherige Versionen der Knotenkonfigurationen, sofern vorhanden. Der Name einer über einen Kompilierungsauftrag erstellten Knotenkonfiguration weist das Format „Konfigurationsname.NodeConfigurationBlockName“ auf. Beispielsweise würde durch das Kompilieren der folgenden Konfiguration eine einzelne Knotenkonfiguration mit dem Namen „MyConfiguration.webserver“ erstellt werden.

![alt text](./media/automation-dsc-overview/AADSC_5.png)

> [!NOTE]
> Ebenso wie Runbooks können Konfigurationen veröffentlicht werden. Dies hat nichts mit dem Platzieren von DSC-Elementen auf dem Azure Automation DSC-Pullserver zu tun. Kompilierungsaufträge sorgen dafür, dass DSC-Elemente auf dem Azure Automation DSC-Pullserver platziert werden. Weitere Informationen zum „Veröffentlichen“ in Azure Automation finden Sie unter [Veröffentlichen eines Runbooks](https://msdn.microsoft.com/library/dn903765.aspx).


## <a name="azure-automation-dsc-lifecycle"></a>Azure Automation DSC – Lebenszyklus

Der Weg von einem leeren Automation-Konto zu einer verwalteten und ordnungsgemäß konfigurierten Knotengruppe umfasst eine Reihe von Verfahren zum Definieren von Konfigurationen, zum Umwandeln dieser Konfigurationen in Knotenkonfigurationen und zum Integrieren von Knoten und Knotenkonfigurationen in Azure Automation DSC. Das folgende Diagramm veranschaulicht den Azure Automation DSC-Lebenszyklus:

![alt text](./media/automation-dsc-overview/DSCLifecycle.png)

Die folgende Abbildung veranschaulicht detailliert die Prozessschritte im DSC-Lebenszyklus. Dies umfasst verschiedene Arten, auf die eine Konfiguration importiert und auf Knoten in Azure Automation angewendet werden kann, sowie Komponenten, die erforderlich sind, damit ein lokaler Computer DSC und Interaktionen zwischen verschiedenen Komponenten unterstützen kann.

![DSC-Architektur](./media/automation-dsc-overview/dsc-architecture.png)

## <a name="gotchas--known-issues"></a>Häufige / bekannte Probleme:

* Für den PowerShell DSC-Agent für Windows muss die neueste Version von WMF 5 installiert werden, um eine Kommunikation mit Azure Automation zu ermöglichen. Die neueste Version des PowerShell DSC-Agents für Linux muss installiert sein, um unter Linux eine Kommunikation mit Azure Automation zu ermöglichen.
* Wenn vor dem Upgrade auf WMF 5 RTM ein Computer bereits als Knoten in Azure Automation DSC registriert ist, heben Sie seine Registrierung in Azure Automation DSC auf, und registrieren Sie ihn nach dem Upgrade auf WMF 5 RTM erneut. Löschen Sie vor der Neuregistrierung die Datei „$env:windir\system32\configuration\DSCEngineCache.mof“.
* Wenn WMF 5 RTM zusätzlich zu WMF 5 Produktion Preview installiert ist, funktioniert DSC möglicherweise nicht ordnungsgemäß. Um dieses Problem zu beheben, führen Sie den folgenden Befehl in einer PowerShell-Sitzung mit erhöhten Rechten (als Administrator) aus: `mofcomp $env:windir\system32\wbem\DscCoreConfProv.mof`
* Azure Automation DSC bietet derzeit keine Unterstützung für teilweise oder zusammengesetzte DSC-Konfigurationen. Zusammengesetzte DSC-Ressourcen können jedoch importiert und in Azure Automation DSC-Konfigurationen wie in der lokalen PowerShell verwendet werden, um die Wiederverwendung von Konfigurationen zu ermöglichen.
* Der traditionelle PowerShell DSC-Pullserver erwartet, dass Modul-ZIP-Dateien im Format **Modulname_Version.zip** auf dem Pullserver platziert werden. Azure Automation erwartet, dass PowerShell-Module mit Namen im Format **Modulname.zip**importiert werden. In [diesem Blogbeitrag](https://azure.microsoft.com/blog/2014/12/15/authoring-integration-modules-for-azure-automation/) finden Sie weitere Informationen zum erforderlichen Format von Integrationsmodulen, um diese Module in Azure Automation zu importieren.
* In Azure Automation importierte PowerShell-Module dürfen keine DOC- oder DOCX-Dateien enthalten. Einige PowerShell-Module mit DSC-Ressourcen enthalten diese Dateien zur Bereitstellung von Hilfeinformationen. Diese Dateien müssen vor dem Import in Azure Automation aus den Modulen entfernt werden.
* Wenn ein Knoten zuerst bei einem Azure Automation-Konto registriert wurde oder der Knoten so geändert wurde, dass er einer anderen Knotenkonfiguration auf Serverseite zugeordnet ist, lautet sein Zustand „Konform“. Dies ist auch der Fall, wenn der Zustand des Knotens nicht konform mit der Knotenkonfiguration ist, der er nun zugeordnet ist. Sobald der Knoten nach der Registrierung oder einer Änderungen der Knotenkonfigurationszuweisung seinen ersten Bericht sendet, kann der Knotenzustand als vertrauenswürdig eingestuft werden.
* Beim Integrieren eines virtuellen Azure-Computers in die Verwaltung durch Azure Automation DSC mithilfe einer unserer Methoden für die direkte Integration kann es bis zu einer Stunde dauern, bis ein DSC-Knoten in Azure Automation angezeigt wird. Dies liegt an der Installation von Windows Management Framework 5.0 auf dem virtuellen Computer durch die Azure VM-Erweiterung für DSC, mit der der virtuelle Computer in Azure Automation DSC integriert werden muss.
* Nach der Registrierung handelt jeder Knoten automatisch ein eindeutiges Zertifikat für die Authentifizierung aus, die nach einem Jahr abläuft. Gegenwärtig kann das PowerShell DSC-Registrierungsprotokoll Zertifikate nicht automatisch erneuern, wenn sie sich dem Ablaufdatum nähern, weshalb Sie die Knoten nach einem Jahr erneut registrieren müssen. Stellen Sie vor einer erneuten Registrierung sicher, dass jeder Knoten Windows Management Framework 5.0 RTM ausführt. Wenn das Authentifizierungszertifikat eines Knotens abläuft und er nicht erneut registriert wird, schlägt die Kommunikation mit Azure Automation fehl und der Knoten wird als „Nicht reagierend“ markiert. Die erneute Registrierung erfolgt auf dieselbe Weise wie die ursprüngliche Registrierung des Knotens. Eine erneute Registrierung, die 90 Tage oder weniger vor Ablaufzeitpunkt des Zertifikats oder zu einem beliebigen Zeitpunkt nach dessen Ablaufzeitpunkt durchgeführt wird, hat zur Folge, dass ein neues Zertifikat generiert und verwendet wird.


## <a name="related-articles"></a>Verwandte Artikel

* [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md)
* [Kompilieren von Konfigurationen in Azure Automation DSC](automation-dsc-compile.md)
* [Azure Automation DSC-Cmdlets](https://msdn.microsoft.com/library/mt244122.aspx)
* [Azure Automation DSC – Preise](https://azure.microsoft.com/pricing/details/automation/)
* [Kontinuierliche Bereitstellung für IaaS-VMs mit Azure Automation DSC und Chocolatey](automation-dsc-cd-chocolatey.md)




<!--HONumber=Feb17_HO1-->


