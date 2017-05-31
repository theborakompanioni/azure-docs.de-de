---
title: "Erstellen einer Verwaltungslösung in OMS | Microsoft Docs"
description: "Verwaltungslösungen erweitern die Funktionalität der Operations Management Suite (OMS), indem sie Pakete mit Verwaltungsszenarien bereitstellen, die Kunden zu ihrem OMS-Arbeitsbereich hinzufügen können.  Diese Artikel beschreibt, wie Sie Verwaltungslösungen erstellen, die Sie in Ihrer Umgebung verwenden oder Ihren Kunden zur Verfügung stellen möchten."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 312f84fe2aaf3f63fe2b8d818786ef1c570ccfe1
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="design-and-build-a-management-solution-in-operations-management-suite-oms-preview"></a>Entwerfen und Erstellen einer Verwaltungslösung in der Operations Management Suite (OMS) (Vorschau)
> [!NOTE]
> Dies ist die vorläufige Dokumentation für das Erstellen von Verwaltungslösungen in der OMS, die sich derzeit in der Vorschau befinden. Jedes unten beschriebene Schema kann sich ändern.

[Verwaltungslösungen](operations-management-suite-solutions.md) erweitern die Funktionalität der Operations Management Suite (OMS), indem sie Pakete mit Verwaltungsszenarien bereitstellen, die Kunden zu ihrem OMS-Arbeitsbereich hinzufügen können.  Dieser Artikel stellt ein grundlegendes Verfahren zum Entwerfen und Erstellen einer Verwaltungslösung vor, die die häufigsten Anforderungen erfüllt.  Wenn Sie keine Erfahrung in der Erstellung von Verwaltungslösungen haben, können Sie dieses Verfahren als Ausgangspunkt verwenden und mit steigenden Anforderungen die Konzepte für komplexere Lösungen nutzen.

## <a name="what-is-a-management-solution"></a>Was ist eine Verwaltungslösung?

Verwaltungslösungen enthalten OMS- und Azure-Ressourcen, die zusammenarbeiten, um ein bestimmtes Überwachungsszenario herzustellen.  Sie werden auch als [Ressourcenverwaltungsvorlagen](../azure-resource-manager/resource-manager-template-walkthrough.md) Implementiert, die Details zum Installieren und Konfigurieren der enthaltenen Ressourcen bei Installation der Lösung umfassen.

Die grundlegende Strategie besteht darin, Ihre Verwaltungslösung damit zu beginnen, dass Sie die einzelnen Komponenten in der Azure-Umgebung erstellen.  Sobald die Funktionalität Ihren Vorstellungen entspricht, können Sie damit beginnen, sie in eine [Verwaltungslösungsdatei](operations-management-suite-solutions-solution-file.md) zu verpacken. 


## <a name="design-your-solution"></a>Entwerfen der Lösung
Das bekannteste Muster für eine Verwaltungslösung ist im folgenden Diagramm abgebildet.  Die verschiedenen Komponenten in diesem Muster werden weiter unten behandelt.

![OMS-Lösungsübersicht](media/operations-management-suite-solutions/solution-overview.png)


### <a name="data-sources"></a>Datenquellen
Der erste Schritt beim Entwerfen einer Lösung besteht darin, die benötigten Daten aus dem Log Analytics-Repository zu bestimmen.  Diese Daten können durch eine [Datenquelle](../log-analytics/log-analytics-data-sources.md) oder [eine andere Lösung](operations-management-suite-solutions.md) erfasst werden, oder Ihre Lösung muss ggf. den Prozess zum Sammeln der Daten enthalten.

Es gibt zahlreiche Möglichkeiten zum Erfassen von Datenquellen im Log Analytics-Repository, wie unter [Datenquellen in Log Analytics](../log-analytics/log-analytics-data-sources.md) beschrieben.  Neben Leistungsindikatoren für Windows- und Linux-Clients schließt dies im Windows-Ereignisprotokoll enthaltene oder von Syslog generierte Ereignisse ein.  Sie können auch von Azure Monitor erfasste Daten aus Azure-Ressourcen sammeln.  

Können bestimmte benötigte Daten nicht über eine der verfügbaren Datenquellen abgerufen werden, können Sie die [HTTP-Datensammler-API](../log-analytics/log-analytics-data-collector-api.md) verwenden, die Ihnen ermöglicht, von jedem beliebigen Client aus, der eine REST-API aufrufen kann, Daten in das Log Analytics-Repository zu schreiben.  Die geläufigste Methode der Sammlung benutzerdefinierter Daten in einer Verwaltungslösung besteht darin, ein [Runbook in Azure Automation](../automation/automation-runbook-types.md) zu erstellen, mit dem die erforderlichen Daten aus Azure oder externen Ressourcen gesammelt und mithilfe der Datensammler-API in das Repository geschrieben werden.  

### <a name="log-searches"></a>Protokollsuchvorgänge
[Protokollsuchen](../log-analytics/log-analytics-log-searches.md) dienen dazu, Daten aus dem Log Analytics-Repository zu extrahieren und zu analysieren.  Sie werden von Ansichten und Warnungen verwendet und ermöglichen Benutzern außerdem, Ad-hoc-Analysen von Daten im Repository auszuführen.  

Sie sollten alle Abfragen definieren, die Sie für den Benutzer als hilfreich erachten, auch wenn sie nicht von Ansichten oder Warnungen verwendet werden.  Sie stehen den Benutzern im Portal als gespeicherte Suchvorgänge zur Verfügung, und Sie können sie auch einer [Visualisierungskomponente „Liste der Abfragen“](../log-analytics/log-analytics-view-designer-parts.md#list-of-queries-part) in Ihrer benutzerdefinierte Ansicht hinzufügen.

### <a name="alerts"></a>Warnungen
[Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md) identifizieren Probleme über [Protokollsuchvorgänge](#log-searches) in den Daten des Repositorys.  Entweder benachrichtigen sie den Benutzer oder führen automatisch eine Aktion als Antwort aus. Sie sollten verschiedene Warnungsbedingungen für Ihre Anwendung definieren und entsprechende Warnungsregeln zu Ihrer Lösungsdatei hinzufügen.

Wenn ein Problem prinzipiell mit einem automatisierten Prozess behoben werden kann, empfiehlt es sich in der Regel, ein Runbook in Azure Automation zu erstellen, mit dem die entsprechende Korrektur durchgeführt wird.  Die meisten Azure-Dienste lassen sich mit [Cmdlets](/powershell/azure/overview) verwalten, die vom Runbook zum Ausführen solcher Funktionen verwendet werden können.

Wenn Ihre Lösung externe Funktionen als Antwort auf eine Warnung erfordert, können Sie eine [Webhookantwort](../log-analytics/log-analytics-alerts-actions.md) verwenden.  Dadurch können Sie einen externen Webdienst aufrufen, der Informationen aus der Warnung sendet.

### <a name="views"></a>Ansichten
Ansichten in Log Analytics dienen zur Visualisierung von Daten aus dem Log Analytics-Repository.  Jede Lösung enthält in der Regel eine einzige Ansicht mit einer [Kachel](../log-analytics/log-analytics-view-designer-tiles.md), die auf dem Hauptdashboard des Benutzers angezeigt wird.  Die Ansicht kann eine beliebige Anzahl von [Visualisierungskomponenten](../log-analytics/log-analytics-view-designer-parts.md) enthalten, um dem Benutzer unterschiedliche Visualisierungen der gesammelten Daten bereitzustellen.

Sie [Erstellen benutzerdefinierte Ansichten mit dem Ansicht-Designer](../log-analytics/log-analytics-view-designer.md). Diese können Sie später exportieren und Ihrer Lösungsdatei hinzufügen.  


## <a name="create-solution-file"></a>Erstellen einer Lösungsdatei
Nachdem Sie die Komponenten, die Teil der Lösung werden sollen, getestet und konfiguriert haben, können Sie [die Lösungsdatei erstellen](operations-management-suite-solutions-solution-file.md).  Implementieren Sie die Komponenten der Lösung in eine [Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md), die eine [Lösungsressource](operations-management-suite-solutions-solution-file.md#solution-resource) mit Beziehungen zu den anderen Ressourcen in der Datei umfasst.  


## <a name="test-your-solution"></a>Testen Ihrer Lösung
Während der Entwicklung Ihrer Lösung müssen Sie diese in Ihrem Arbeitsbereich installieren und testen.  Hierzu können Sie jede der verfügbaren Methoden zum [Testen und Installieren der Resource Manager-Vorlagen installiert](../azure-resource-manager/resource-group-template-deploy.md) verwenden.

## <a name="publish-your-solution"></a>Veröffentlichen Ihrer Lösung
Sobald Sie Ihre Lösung fertig gestellt und getestet haben, können Sie sie Kunden über die folgenden Quellen zur Verfügung stellen.

- **Azure-Schnellstartvorlagen**.  [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) ist ein Satz aus von der Community über GitHub bereitgestellten Resource Manager-Vorlagen.  Informationen zur Bereitstellung Ihrer Lösung finden Sie im [Contribution Guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Marketplace**.  Der [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) ermöglicht Ihnen, Ihre Lösung an andere Entwickler, unabhängige Softwareanbieter (ISVs) und IT-Experten zu verteilen und zu verkaufen.  Unter [Veröffentlichen und Verwalten eines Angebots im Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) erfahren Sie, wie Sie Ihre Lösung im Azure Marketplace veröffentlichen können.



## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Erstellen einer Lösungsdatei ](operations-management-suite-solutions-solution-file.md) für Ihre Verwaltungslösung.
* Erfahren Sie Näheres zum [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).
* Durchsuchen Sie die [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates), um Beispiele für verschiedene Resource Manager-Vorlagen zu finden.

