---
title: "Lösungen in der Operations Management Suite (OMS) | Microsoft Docs"
description: "Lösungen erweitern die Funktionalität der Operations Management Suite (OMS), indem sie Pakete mit Verwaltungsszenarien bereitstellen, die Kunden zu ihrem OMS-Arbeitsbereich hinzufügen können.  Dieser Artikel enthält Details, wie benutzerdefinierte Lösungen von Kunden und Partnern erstellt werden können."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ede557e1678aa6c24f50bd120552345dcd00cdc3


---
# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Verwaltungslösungen in der Operations Management Suite (OMS) (Vorschau)
> [!NOTE]
> Dies ist die vorläufige Dokumentation für Verwaltungslösungen in OMS, die sich derzeit in der Vorschau befinden.    
> 
> 

Lösungen erweitern die Funktionalität der Operations Management Suite (OMS), indem sie Pakete mit Verwaltungsszenarien bereitstellen, die Kunden zu ihrer Umgebung hinzufügen können.  Zusätzlich zu den [von Microsoft bereitgestellten Lösungen](../log-analytics/log-analytics-add-solutions.md) können Partner und Kunden Verwaltungslösungen erstellen, die sie in deren eigener Umgebung verwenden oder sie für Kunden über die Community verfügbar machen.

## <a name="finding-and-installing-management-solutions"></a>Suchen und Installieren von Verwaltungslösungen
Es gibt mehrere Methoden zum Suchen und Installieren von Verwaltungslösungen, wie in den folgenden Abschnitten beschrieben.

### <a name="azure-marketplace"></a>Azure Marketplace
Von Microsoft und vertrauenswürdigen Partnern bereitgestellte Verwaltungslösungen können aus dem Azure Marketplace in das Azure-Portal installiert werden.

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie im linken Bereich **Weitere Dienste** aus.
3. Scrollen Sie entweder hinunter zu **Lösungen**, oder tippen Sie *Lösungen* in das Dialogfeld **Filter** ein.
4. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.
5. Suchen Sie nach für Sie interessanten Lösungen, indem Sie entweder zu dieser navigieren, auf die Schaltfläche **Filter** klicken, oder etwas in das Feld **Alles durchsuchen** eingeben.
6. Klicken Sie auf ein Marketplace-Element, um dessen ausführliche Informationen anzuzeigen.
7. Klicken Sie auf **Erstellen**, um den Bereich **Lösung hinzufügen** zu öffnen.
8. Sie werden aufgefordert, erforderliche Informationen wie z.B. den [OMS Arbeitsbereich und das Automation-Konto](#oms-workspace-and-automation-account) zusätzlich zu den Werten für Parameter in der Lösung anzugeben.
9. Klicken Sie auf **Erstellen**, um die Lösung zu installieren.

### <a name="oms-portal"></a>OMS-Portal
Von Microsoft bereitgestellte Verwaltungslösungen können aus dem Lösungskatalog in das OMS-Portal installiert werden.

1. Melden Sie sich beim OMS-Portal an.
2. Klicken Sie auf die Kachel **Lösungskatalog**.
3. Auf der Seite des OMS-Lösungskatalogs finden Sie Informationen zu jeder der verfügbaren Lösungen. Klicken Sie auf den Namen der Lösung, die Sie OMS hinzufügen möchten.
4. Auf der Seite für die ausgewählte Lösung werden ausführliche Informationen zur Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
5. Auf der Übersichtsseite in OMS wird eine neue Kachel für die hinzugefügte Lösung angezeigt, und Sie können die Lösung verwenden, nachdem der OMS-Dienst Ihre Daten verarbeitet hat.

### <a name="azure-quickstart-templates"></a>Azure-Schnellstartvorlagen
Mitglieder der Community können Verwaltungslösungen für Azure-Schnellstartvorlagen senden.  Sie können diese Vorlagen für eine spätere Installation herunterladen oder sie untersuchen, um zu erfahren, wie Sie [Ihre eigenen Lösungen erstellen](#creating-a-solution).

1. Führen Sie den unter [OMS-Arbeitsbereich und Automation-Konto](#oms-workspace-and-automation-account) beschriebenen Prozess aus, um einen Arbeitsbereich und ein Konto zu verknüpfen.
2. Gehen Sie zu [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).  
3. Suchen Sie nach einer Lösung, an der Sie interessiert sind.
4. Wählen Sie die Lösung aus den Ergebnissen aus, um ihre Details anzuzeigen.
5. Klicken Sie auf die Schaltfläche **Bereitstellen in Azure**.
6. Sie werden aufgefordert, Informationen wie z.B. die Ressourcengruppe und den Speicherort zusätzlich zu den Werten für Parameter in der Lösung anzugeben.
7. Klicken Sie auf **Kaufen**, um die Lösung zu installieren.

### <a name="deploy-azure-resource-manager-template"></a>Bereitstellen der Azure Resource Manager-Vorlage
Lösungen, die Sie von der Community erhalten oder die Sie [selbst erstellen](#creating-a-solution), werden als Resource Manager-Vorlage implementiert, und Sie können die Standardmethoden für das [Bereitstellen einer Vorlage](../resource-group-template-deploy-portal.md) verwenden.  Beachten Sie, dass Sie vor der Installation der Lösung den [OMS-Arbeitsbereich und das Automation-Konto](#oms-workspace-and-automation-account) erstellen und verknüpfen müssen.

## <a name="oms-workspace-and-automation-account"></a>OMS-Arbeitsbereich und Automation-Konto
Die meisten Verwaltungslösungen erfordern einen [OMS-Arbeitsbereich](../log-analytics/log-analytics-manage-access.md) für Ansichten und ein [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview) für Runbooks und zugehörige Ressourcen. Der Arbeitsbereich und das Konto müssen den folgenden Anforderungen entsprechen.

* Eine Lösung kann nur einen OMS-Arbeitsbereich und nur ein Automation-Konto verwenden.  
* Der OMS-Arbeitsbereich und das Automation-Konto, die von einer Lösung verwendet werden, müssen miteinander verknüpft sein. Ein OMS-Arbeitsbereich kann nur mit einem Automation-Konto verknüpft werden, und ein Automation-Konto kann nur mit einem OMS-Arbeitsbereich verknüpft werden.
* Der OMS-Arbeitsbereich und das Automation-Konto müssen sich in der gleichen Ressourcengruppe und im gleichen Bereich befinden, um verknüpft zu werden.  Die Ausnahme ist ein OMS-Arbeitsbereich in der Region USA, Osten und ein Automation-Konto in der Region USA, Osten 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Erstellen einer Verknüpfung zwischen einem OMS-Arbeitsbereich und einem Automation-Konto
Wie Sie den OMS-Arbeitsbereich und das Automation-Konto angeben, hängt von der Installationsmethode Ihrer Lösung ab.

* Wenn Sie eine Microsoft-Lösung über das OMS-Portal installieren, wird sie im aktuellen OMS-Arbeitsbereich installiert und es wird kein Automation-Konto benötigt.
* Wenn Sie die Lösung über den Azure Marketplace installieren, erhalten Sie für einen OMS-Arbeitsbereich und ein Automation-Konto eine Aufforderung, und die Verknüpfung zwischen beiden wird für Sie erstellt.  
* Für Lösungen außerhalb des Azure Marketplace müssen Sie den OMS-Arbeitsbereich und das Automation-Konto miteinander verknüpfen, bevor Sie die Lösung installieren.  Hierzu wählen Sie eine Lösung im Azure Marketplace aus und dann den OMS-Arbeitsbereich und das Automation-Konto.  Sie müssen die Lösung nicht wirklich installieren, da die Verknüpfung erstellt wird, sobald der OMS-Arbeitsbereich und das Automation-Konto ausgewählt wurden.  Nachdem die Verknüpfung erstellt wurde, können Sie diesen OMS-Arbeitsbereich und dieses Automation-Konto für jede Lösung verwenden. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Überprüfen der Verknüpfung zwischen einem OMS-Arbeitsbereich und einem Automation-Konto
Sie können die Verknüpfung zwischen einem OMS-Arbeitsbereich und einem Automation-Konto mithilfe des folgenden Verfahrens überprüfen.

1. Wählen Sie im Azure-Portal das Automation-Konto aus.
2. Scrollen Sie zum Ende des Bereichs **Einstellungen**.
3. Falls ein Abschnitt mit dem Namen **OMS-Ressourcen** im Bereich **Einstellungen** existiert, wird dieses Konto an einen OMS-Arbeitsbereich angefügt.
4. Wählen Sie **Arbeitsbereich**, um die Details des OMS-Arbeitsbereichs anzuzeigen, der mit diesem Automation-Konto verknüpft ist.

## <a name="listing-management-solutions"></a>Auflisten von Verwaltungslösungen
Verwenden Sie das folgende Verfahren zum Anzeigen der Verwaltungslösungen in den Arbeitsbereichen, die mit Ihrem Azure-Abonnement verknüpft sind.

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie im linken Bereich **Weitere Dienste** aus.
3. Scrollen Sie entweder hinunter zu **Lösungen**, oder tippen Sie *Lösungen* in das Dialogfeld **Filter** ein.
4. Es werden Lösungen aufgelistet, die in all Ihren Arbeitsbereichen installiert sind.

Beachten Sie, dass Sie nur die im aktuellen Arbeitsbereich installierten Microsoft-Lösungen mithilfe des OMS-Portals anzeigen können.

## <a name="removing-a-management-solution"></a>Entfernen einer Verwaltungslösung
Wenn eine Verwaltungslösung entfernt wird, werden alle Ressourcen in der Lösung ebenfalls entfernt.  

1. Suchen sie im Azure-Portal die Lösung mithilfe der Prozedur in der [Auflistung von Lösungen](#listing-solutions).
2. Wählen Sie die Lösung aus, die Sie entfernen möchten.
3. Klicken Sie auf die Schaltfläche **Löschen** .

## <a name="creating-a-management-solution"></a>Erstellen einer Verwaltungslösung
Vollständige Anleitungen zur Erstellung von Verwaltungslösungen finden Sie unter [Creating solutions in Operations Management Suite (OMS) (Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS))](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Nächste Schritte
* Durchsuchen Sie die [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates), um Beispiele für verschiedene Resource Manager-Vorlagen zu finden.
* Erstellen Sie Ihre eigenen [Verwaltungslösungen](operations-management-suite-solutions-creating.md).




<!--HONumber=Nov16_HO3-->


