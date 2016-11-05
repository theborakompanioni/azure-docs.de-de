---
title: Übersicht über den Azure-Ressourcen-Manager | Microsoft Docs
description: Es wird beschrieben, wie Sie den Azure-Ressourcen-Manager für die Bereitstellung, Verwaltung und Zugriffssteuerung von Ressourcen unter Azure verwenden.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: tomfitz

---
# Übersicht über den Azure-Ressourcen-Manager
Die Infrastruktur für Ihre Anwendung besteht normalerweise aus vielen Komponenten: womöglich ein virtueller Computer, ein Speicherkonto und ein virtuelles Netzwerk oder eine Web-App, eine Datenbank, ein Datenbankserver und Drittanbieterdienste. Sie sehen diese Komponenten nicht als separate Entitäten, sondern als verwandte und voneinander abhängige Teile einer einzelnen Entität. Diese möchten Sie als Gruppe bereitstellen, verwalten und überwachen. Mit dem Azure-Ressourcen-Manager können Sie als Gruppe mit den Ressourcen in Ihrer Lösung arbeiten. Sie können alle Ressourcen für Ihre Lösung in einem einzigen koordinierten Vorgang bereitstellen, aktualisieren oder löschen. Sie verwenden eine Vorlage für die Bereitstellung, die für unterschiedliche Umgebungen geeignet sein kann, z.B. Testing, Staging und Produktion. Der Ressourcen-Manager bietet Sicherheits-, Überwachungs- und Kennzeichnungsfunktionen, mit denen Sie Ihre Ressourcen nach der Bereitstellung verwalten können.

## Terminologie
Wenn Sie mit dem Azure Resource Manager noch nicht vertraut sind, kennen Sie unter Umständen einige Begriffe noch nicht.

* **Ressource**: Ein verwaltbares Element, das über Azure verfügbar ist. Beispiele für häufig verwendete Ressourcen sind virtueller Computer, Speicherkonto, Web-App, Datenbank und virtuelles Netzwerk, aber es sind noch viele weitere Ressourcen vorhanden.
* **Ressourcengruppe**: Ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Die Ressourcengruppe kann alle Ressourcen für die Lösung oder nur die Ressourcen enthalten, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten. Weitere Informationen finden Sie unter [Ressourcengruppen](#resource-groups).
* **Ressourcenanbieter**: Ein Dienst zum Beschaffen der Ressourcen, die Sie über den Resource Manager bereitstellen und verwalten können. Jeder Ressourcenanbieter bietet Vorgänge zum Arbeiten mit den bereitgestellten Ressourcen. Beispiele für häufig verwendete Ressourcenanbieter sind „Microsoft.Compute“ zum Bereitstellen der Ressource für den virtuellen Computer, „Microsoft.Storage“ zum Bereitstellen der Speicherkontoressource und „Microsoft.Web“ zum Bereitstellen von Ressourcen für Web-Apps. Weitere Informationen finden Sie unter [Ressourcenanbieter](#resource-providers).
* **Resource Manager-Vorlage**: Eine JSON-Datei (JavaScript Object Notation), mit der eine oder mehrere Ressourcen zum Bereitstellen einer Ressourcengruppe definiert werden. Außerdem werden damit die Abhängigkeiten zwischen den bereitgestellten Ressourcen definiert. Die Vorlage kann zum konsistenten und wiederholten Bereitstellen der Ressourcen verwendet werden. Weitere Informationen finden Sie unter [Bereitstellung von Vorlagen](#template-deployment).
* **Deklarative Syntax**: Bei dieser Syntax können Sie beispielsweise „Here is what I intend to create“ (Dies möchte ich erstellen) eingeben, ohne dafür die Folge der Programmierbefehle für die Erstellung schreiben zu müssen. Die Resource Manager-Vorlage ist ein Beispiel für die deklarative Syntax. In der Datei definieren Sie die Eigenschaften für die Infrastruktur zum Bereitstellen für Azure.

## Vorteile der Verwendung des Ressourcen-Managers
Der Ressourcen-Manager bietet mehrere Vorteile:

* Sie können alle Ressourcen für Ihre Lösung als Gruppe bereitstellen, verwalten und überwachen, anstatt diese Ressourcen einzeln zu verarbeiten.
* Sie können die Lösung während des gesamten Entwicklungslebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.
* Sie können zum Verwalten Ihrer Infrastruktur anstelle von Skripts auch deklarative Vorlagen verwenden.
* Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.
* Sie können die Zugriffssteuerung auf alle Dienste in der Ressourcengruppe anwenden, da die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) standardmäßig in die Verwaltungsplattform integriert ist.
* Sie können Tags auf Ressourcen anwenden, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren.
* Indem Sie die Kosten für eine Gruppe mit Ressourcen anzeigen, für die das gleiche Tag verwendet wird, erhalten Sie die Abrechnungsinformationen für Ihre Organisation.

Der Ressourcen-Manager stellt eine neue Möglichkeit zur Bereitstellung und Verwaltung Ihrer Lösungen bereit. Weitere Informationen zu den Änderungen an dem von Ihnen verwendeten früheren Bereitstellungsmodell finden Sie unter [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](resource-manager-deployment-model.md).

## Anleitungen
Die folgenden Vorschläge helfen Ihnen, bei der Arbeit mit Lösungen die Vorteile von Resource Manager zu nutzen.

1. Definieren Sie die Infrastruktur mithilfe der deklarativen Syntax in Ressourcen-Manager-Vorlagen anstatt mit imperativen Befehlen, und stellen Sie sie so bereit.
2. Definieren Sie alle Bereitstellungs- und Konfigurationsschritte in der Vorlage. Sie sollten keine manuellen Schritte zum Einrichten der Lösung durchführen müssen.
3. Führen Sie imperative Befehle aus, um Ihre Ressourcen zu verwalten, z. B. Starten oder Beenden einer App oder des Computers.
4. Gruppieren Sie Ressourcen mit dem gleichen Lebenszyklus in einer Ressourcengruppe. Verwenden Sie Tags für die weitere Organisation von Ressourcen.

Weitere Empfehlungen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](resource-manager-template-best-practices.md).

## Ressourcengruppen
Beim Definieren der Ressourcengruppe sind einige wichtige Faktoren zu beachten:

1. Alle Ressourcen einer Gruppe sollten über den gleichen Lebenszyklus verfügen. Sie werden von Ihnen zusammen bereitgestellt, aktualisiert und gelöscht. Falls eine Ressource, z. B. ein Datenbankserver, in einem anderen Entwicklungszyklus vorhanden sein muss, sollte er in einer anderen Ressourcengruppe enthalten sein.
2. Jede Ressource kann nur in einer Ressourcengruppe vorhanden sein.
3. Sie können eine Ressource einer Ressourcengruppe jederzeit hinzufügen bzw. die Ressource daraus entfernen.
4. Sie können eine Ressource aus einer Ressourcengruppe in eine andere Gruppe verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).
5. Eine Ressourcengruppe kann Ressourcen enthalten, die sich in unterschiedlichen Regionen befinden.
6. Eine Ressourcengruppe kann zum Festlegen der Zugriffssteuerung für administrative Aktionen verwendet werden.
7. Eine Ressource kann mit Ressourcen in anderen Ressourcengruppen interagieren. Diese Interaktion tritt häufig auf, wenn die beiden Ressourcen zusammengehören, aber nicht den gleichen Lebenszyklus aufweisen (z.B. Web-Apps, die eine Verbindung mit einer Datenbank herstellen).

Beim Erstellen einer Ressourcengruppe müssen Sie einen Standort für die Ressourcengruppe angeben. Vielleicht stellen Sie sich hierbei die folgende Frage: „Warum wird für eine Ressourcengruppe ein Standort benötigt? Und wenn die Ressourcen andere Standorte als die Ressourcengruppe aufweisen können, warum ist der Standort der Ressourcengruppe dann überhaupt wichtig?“ In der Ressourcengruppe werden Metadaten zu den Ressourcen gespeichert. Wenn Sie einen Standort für die Ressourcengruppe angeben, legen Sie also fest, wo die Metadaten gespeichert werden. Aus Compliance-Gründen müssen Sie unter Umständen sicherstellen, dass Ihre Daten in einer bestimmten Region gespeichert werden.

## Ressourcenanbieter
Jeder Ressourcenanbieter stellt einen Satz mit Ressourcen und Vorgängen für den technischen Bereich bereit. Wenn Sie beispielsweise Schlüssel und geheime Schlüssel speichern möchten, verwenden Sie den Ressourcenanbieter **Microsoft.KeyVault**. Der Ressourcenanbieter verfügt über einen Ressourcentyp namens **vaults** zum Erstellen des Schlüsseltresors und einen Ressourcentyp namens **vaults/secrets** zum Erstellen eines geheimen Schlüssels im Schlüsseltresor. Außerdem stellt er Vorgänge über [Schlüsseltresor-REST-API-Vorgänge](https://msdn.microsoft.com/library/azure/dn903609.aspx) bereit. Sie können die REST-API direkt aufrufen oder [Key Vault-PowerShell-Cmdlets](https://msdn.microsoft.com/library/dn868052.aspx) und die [Key Vault-Azure-CLI](key-vault/key-vault-manage-with-cli.md) zum Verwalten des Schlüsseltresors verwenden. Sie können für die meisten Ressourcen auch verschiedene Programmiersprachen verwenden. Weitere Informationen finden Sie unter [SDKs und Beispiele](#sdks-and-samples).

Zum Bereitstellen und Verwalten Ihrer Infrastruktur müssen Sie mit den Details zum Ressourcenanbieter vertraut sein. Sie müssen die Ressourcentypen, die Versionsnummern für REST-API-Vorgänge, die unterstützten Vorgänge und das Schema zum Erstellen von Ressourcen kennen. Informationen zu den unterstützten Ressourcenanbietern finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für den Ressourcen-Manager](resource-manager-supported-services.md).

## Bereitstellung von Vorlagen
Mit dem Resource Manager können Sie eine Vorlage (im JSON-Format) erstellen, mit der die Bereitstellung und Konfiguration Ihrer Anwendung definiert wird. Mit einer Vorlage können Sie die Anwendung während des gesamten App-Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden. Azure Resource Manager analysiert Abhängigkeiten, um sicherzustellen, dass Ressourcen in der richtigen Reihenfolge erstellt werden. Weitere Informationen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](resource-group-define-dependencies.md).

Wenn Sie eine Lösung über das Portal erstellen, enthält sie automatisch eine Bereitstellungsvorlage. Sie müssen die Vorlage nicht völlig neu erstellen, weil Sie mit der Vorlage für Ihre Lösung beginnen und sie dann an die speziellen Anforderungen anpassen können. Sie können eine Vorlage für eine vorhandene Ressourcengruppe abrufen, indem Sie entweder den aktuellen Zustand der Ressourcengruppe exportieren oder die Vorlage anzeigen, die für eine bestimmte Bereitstellung verwendet wurde. Das Anzeigen der exportierten Vorlage ist hilfreich, um sich über die Vorlagensyntax zu informieren. Weitere Informationen zur Verwendung von exportierten Vorlagen finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md).

Sie müssen nicht die gesamte Infrastruktur in einer einzelnen Vorlage definieren. Oftmals ist es sinnvoll, die Bereitstellungsanforderungen in eine Gruppe von spezifischen, zweckgebundenen Vorlagen zu unterteilen. Sie können diese Vorlagen mühelos für verschiedene Lösungen erneut verwenden. Um eine bestimmte Lösung bereitzustellen, erstellen Sie eine Mastervorlage, die alle erforderlichen Vorlagen verknüpft. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

Sie können die Vorlage auch für Aktualisierungen der Infrastruktur verwenden. Beispielsweise können Sie Ihrer Lösung eine Ressource sowie Konfigurationsregeln für die Ressourcen hinzufügen, die bereits bereitgestellt wurden. Wenn in der Vorlage die Erstellung einer Ressource angegeben ist, diese aber bereits vorhanden ist, führt der Azure Resource Manager anstelle der Erstellung einer neuen Ressource eine Aktualisierung durch. Der Azure-Ressourcen-Manager aktualisiert die vorhandene Ressource auf den Zustand, der für eine neue Ressource gelten würde. Oder Sie können angeben, dass Ressourcen-Manager Ressourcen löschen, die nicht in der Vorlage angegeben werden. Informationen über die verschiedenen Optionen bei der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

Sie können Parameter in Ihrer Vorlage angeben, um in Bezug auf die Bereitstellung für Anpassung und Flexibilität zu sorgen. Beispielsweise können Sie Parameterwerte übergeben, mit denen die Bereitstellung für Ihre Testumgebung angepasst wird. Indem Sie die Parameter angeben, können Sie dieselbe Vorlage nutzen, um die Lösung in unterschiedlichen Umgebungen bereitzustellen.

Der Ressourcen-Manager verfügt über Erweiterungen für Szenarien, in denen zusätzliche Vorgänge, wie z. B. das Installieren bestimmter Software, die nicht Teil des Setups ist, erforderlich sind. Wenn Sie bereits einen Konfigurationsverwaltungsdienst verwenden, z. B. DSC, Chef oder Puppet, können Sie ihn weiter nutzen, indem Sie Erweiterungen einsetzen.

Die Vorlage wird schließlich zu einem Teil des Quellcodes für Ihre App. Sie können sie in das Quellcoderepository einchecken und im Verlauf der App-Entwicklung aktualisieren. Sie können die Vorlage mit Visual Studio bearbeiten.

Weitere Informationen zum Definieren der Vorlage finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).

Schritt-für-Schritt-Anweisungen zum Erstellen einer Vorlage finden Sie unter [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md).

Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).

## Tags
Der Ressourcen-Manager verfügt über eine Markierungsfunktion, mit der Sie Ressourcen gemäß Ihren Anforderungen für die Verwaltung oder Abrechnung kategorisieren können. Die Verwendung von Tags (Markierungen) kann ratsam sein, wenn Sie über eine komplexe Sammlung von Ressourcengruppen und Ressourcen verfügen und diese Ressourcen auf möglichst sinnvolle Weise visualisieren müssen. Beispielsweise können Sie Ressourcen markieren, die in Ihrer Organisation eine ähnliche Funktion haben oder zu derselben Abteilung gehören. Benutzer in Ihrer Organisation können mehrere Ressourcen erstellen, die ohne Tags später schwer zu identifizieren und zu verwalten sind. Es kann beispielsweise sein, dass Sie alle Ressourcen für ein bestimmtes Projekt löschen möchten. Wenn diese Ressourcen für das Projekt nicht mit Tags versehen sind, müssen Sie manuell danach suchen. Mit Tags lassen sich unter Umständen unnötige Kosten in Ihrem Abonnement vermeiden.

Ressourcen müssen sich nicht in derselben Ressourcengruppe befinden, um ein gemeinsames Tag aufzuweisen. Sie können Ihre eigene Tag-Taxonomie erstellen, um dafür zu sorgen, dass alle Benutzer in Ihrer Organisation die gleichen Tags verwenden. So wird verhindert, dass Benutzer versehentlich leicht unterschiedliche Tags nutzen (z. B. „dept“ anstelle von „department“).

Weitere Informationen zu Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md). Sie können eine [angepasste Richtlinie](#manage-resources-with-customized-policies) erstellen, die das Hinzufügen von Tags zu Ressourcen während der Bereitstellung erforderlich macht.

## Zugriffssteuerung
Mit dem Ressourcen-Manager können Sie steuern, wer Zugriff auf spezielle Aktionen für Ihre Organisation hat. OAuth und die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sind standardmäßig in die Verwaltungsplattform integriert, und diese Zugriffssteuerung wird auf alle Dienste in der Ressourcengruppe angewendet. Sie können Benutzer vordefinierten Plattformrollen und ressourcenspezifischen Rollen hinzufügen und diese Rollen auf ein Abonnement, eine Ressourcengruppe oder eine Ressource anwenden, um den Zugriff zu beschränken. Beispielsweise können Sie die vordefinierte Rolle „SQL DB Contributor“ verwenden, mit der Benutzer Datenbanken verwalten können, aber keine Datenbankserver oder Sicherheitsrichtlinien. Sie fügen Benutzer Ihrer Organisation, die diese Art von Zugriff benötigen, der Rolle „SQL-DB-Mitwirkender“ hinzu und wenden die Rolle auf das Abonnement, die Ressourcengruppe oder die Ressource an.

Der Ressourcen-Manager protokolliert automatisch Benutzeraktionen zu Überwachungszwecken. Informationen zum Arbeiten mit den Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit dem Ressourcen-Manager](resource-group-audit.md).

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](active-directory/role-based-access-control-configure.md). Das Thema [RBAC: Integrierte Rollen](active-directory/role-based-access-built-in-roles.md) enthält eine Liste mit den integrierten Rollen und zulässigen Aktionen. Integrierte Rollen sind allgemeine Rollen, wie z. B. Besitzer, Leser und Mitwirkender, sowie servicespezifische Rollen, wie z. B. Virtual Machine Contributor, Virtual Network Contributor und SQL Security Manager (um nur einige der verfügbaren Rollen zu nennen).

Sie können kritische Ressourcen auch explizit sperren, um zu verhindern, dass sie von Benutzern gelöscht oder geändert werden. Weitere Informationen finden Sie unter [Sperren von Ressourcen mit dem Azure-Ressourcen-Manager](resource-group-lock-resources.md).

Bewährte Methoden finden Sie unter [Sicherheitsaspekte für Azure-Ressourcen-Manager](best-practices-resource-manager-security.md).

## Verwalten von Ressourcen mit benutzerdefinierten Richtlinien
Mit dem Ressourcen-Manager können Sie benutzerdefinierte Richtlinien zum Verwalten Ihrer Ressourcen erstellen. Die Arten der von Ihnen erstellten Richtlinien können unterschiedliche Szenarien umfassen. Sie können eine Namenskonvention für Ressourcen erzwingen, begrenzen, welche Arten und Instanzen von Ressourcen bereitgestellt werden können, oder begrenzen, in welchen Regionen eine Ressourcenart gehostet werden kann. Um die Abrechnung nach Abteilungen zu organisieren, können Sie die Verwendung eines Tagwerts für Ressourcen obligatorisch machen. Sie erstellen Richtlinien, um Kosten zu senken und die Konsistenz in Ihrem Abonnement zu wahren. Weitere Informationen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).

## Einheitliche Verwaltungsebene
Der Resource Manager bietet kompatible Vorgänge für Azure PowerShell, die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows sowie das Azure-Portal und die REST-API. Sie können die Schnittstelle verwenden, die für Sie am besten geeignet ist, und schnell zwischen Schnittstellen wechseln, ohne dass Verwirrung entsteht.

Weitere Informationen zu PowerShell finden Sie unter [Verwenden von Azure PowerShell mit dem Ressourcen-Manager](powershell-azure-resource-manager.md) und [Azure-Ressourcen-Manager-Cmdlets](https://msdn.microsoft.com/library/azure/dn757692.aspx).

Informationen zur Azure-Befehlszeilenschnittstelle finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung](xplat-cli-azure-resource-manager.md).

Informationen zur REST-API finden Sie unter [Referenz zur REST-API des Azure-Ressourcen-Managers](https://msdn.microsoft.com/library/azure/dn790568.aspx). Informationen zum Anzeigen von REST-Vorgängen für Ihre bereitgestellten Ressourcen finden Sie unter [Verwenden des Azure-Ressourcen-Explorer zum Anzeigen und Ändern von Ressourcen](resource-manager-resource-explorer.md).

Informationen zum Verwenden des Portals finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-Portal](resource-group-template-deploy-portal.md).

Der Azure-Ressourcen-Manager unterstützt Cross-Origin Resource Sharing (CORS). Mit CORS können Sie die Ressourcen-Manager-REST-API oder die REST-API eines Azure-Diensts über eine Webanwendung aufrufen, die sich in einer anderen Domäne befindet. Ohne CORS-Unterstützung verhindert der Webbrowser, dass eine App in einer Domäne auf Ressourcen in einer anderen Domäne zugreifen kann. Der Ressourcen-Manager aktiviert CORS für alle Anforderungen mit gültigen Anmeldeinformationen für die Authentifizierung.

## SDKs
Azure-SDKs sind für mehrere Sprachen und Plattformen verfügbar. Jede dieser Sprachimplementierungen ist jeweils über die entsprechenden Ökosystem-Paket-Manager und GitHub verfügbar.

Der Code in diesen SDKs wird jeweils über Azure RESTful-API-Spezifikationen generiert. Dies sind Open-Source-Spezifikationen, die auf der Swagger 2.0-Spezifikation basieren. Der SDK-Code wird über ein Open-Source-Projekt mit dem Namen „AutoRest“ generiert. AutoRest transformiert diese RESTful-API-Spezifikationen in Clientbibliotheken in mehreren Sprachen. Falls Sie Aspekte des generierten Codes in den SDKs verbessern möchten, können Sie den gesamten Satz der Tools zum Erstellen der SDKs verwenden. Sie sind frei zugänglich und verfügbar und basieren auf dem gängigen API-Spezifikationsformat.

Im Anschluss finden Sie unsere Open Source-SDK-Repositorys. Wir freuen uns über Feedback, Hinweise zu Problemen und Pull Requests.

[.NET](https://github.com/Azure/azure-sdk-for-net) | [Java](https://github.com/Azure/azure-sdk-for-java) | [Node.js](https://github.com/Azure/azure-sdk-for-node) | [PHP](https://github.com/Azure/azure-sdk-for-php) | [Python](https://github.com/Azure/azure-sdk-for-python) | [Ruby](https://github.com/Azure/azure-sdk-ruby)

> [!NOTE]
> Falls die benötigte Funktion durch das SDK nicht bereitstellt wird, können Sie die [Azure-REST-API](https://msdn.microsoft.com/library/azure/dn790568.aspx) auch direkt aufrufen.
> 
> 

## Beispiele
### .NET
* [Manage Azure resources and resource groups (Verwalten von Azure-Ressourcen und -Ressourcengruppen)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)
* [Deploy an SSH Enabled VM with a Template (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)

### Java
* [Manage Azure resources (Verwalten von Azure-Ressourcen)](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource/)
* [Manage Azure resource groups (Verwalten von Azure-Ressourcengruppen)](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group/)
* [Deploy an SSH Enabled VM with a Template (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage)](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)

### Node.js
* [Manage Azure resources and resource groups (Verwalten von Azure-Ressourcen und -Ressourcengruppen)](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)
* [Deploy an SSH Enabled VM with a Template (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage)](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)

### Python
* [Manage Azure resources and resource groups (Verwalten von Azure-Ressourcen und -Ressourcengruppen)](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)
* [Deploy an SSH Enabled VM with a Template (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage)](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)

### Ruby
* [Manage Azure resources and resource groups (Verwalten von Azure-Ressourcen und -Ressourcengruppen)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)
* [Deploy an SSH Enabled VM with a Template (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)

Neben diesen Beispielen können Sie auch die Katalogbeispiele durchsuchen.

[.NET](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=dotnet) | [Java](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=java) | [Node.js](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=nodejs) | [Python](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=python) | [Ruby](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=ruby)

## Nächste Schritte
* Eine einfache Einführung in die Verwendung von Vorlagen finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md).
* Eine ausführlichere exemplarische Vorgehensweise zum Erstellen einer Vorlage finden Sie unter [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md).
* Grundlegende Informationen zu den Funktionen, die in einer Vorlage verwendet werden können, finden Sie unter [Vorlagenfunktionen](resource-group-template-functions.md).
* Informationen zum Verwenden von Visual Studio mit Resource Manager finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
* Informationen zur Verwendung von VS Code mit dem Resource Manager finden Sie unter [Verwenden von Azure Resource Manager-Vorlagen in Visual Studio Code](resource-manager-vs-code.md).

Hier sehen Sie eine Videodemonstration dieser Übersicht:

[!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Azure-Resource-Manager-Overview/player]


<!---HONumber=AcomDC_0921_2016-->