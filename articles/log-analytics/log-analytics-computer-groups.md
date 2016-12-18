---
title: Computergruppen in Log Analytics-Protokollsuchen | Microsoft Docs
description: "Mit Computergruppen in Log Analytics können Sie Protokollsuchvorgänge auf eine bestimmte Gruppe von Computern eingrenzen.  In diesem Artikel werden die verschiedenen Methoden beschrieben, mit denen Sie Computergruppen erstellen, sowie wie sie diese in einer Suche verwenden."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6c0affd0f5ea600f979cfcc87e2435658c8dab14


---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Computergruppen in Log Analytics-Protokollsuchen
Mit Computergruppen in Log Analytics können Sie [Protokollsuchvorgänge](log-analytics-log-searches.md) auf eine bestimmte Gruppe von Computern eingrenzen.  Die einzelnen Gruppen werden über eine von Ihnen definierte Abfrage mit Computern aufgefüllt oder indem Sie Gruppen aus verschiedenen Quellen importieren.  Wenn die Gruppe in eine Protokollsuche eingeschlossen wird, sind die Ergebnisse auf Datensätze beschränkt, die Computern in der Gruppe entsprechen.

## <a name="creating-a-computer-group"></a>Erstellen einer Computergruppe
Sie können eine Computergruppe in Log Analytics mithilfe einer der Methoden in der folgenden Tabelle erstellen.  Einzelheiten zu den einzelnen Methoden finden Sie in den Abschnitten unten. 

| Methode | Beschreibung |
|:--- |:--- |
| Protokollsuche |Erstellen Sie eine Protokollsuche, die eine Liste der Computer zurückgibt, und speichern Sie die Ergebnisse als eine Computergruppe. |
| Protokollsuch-API |Verwenden Sie die Protokollsuch-API, um programmgesteuert eine Computergruppe basierend auf den Ergebnissen einer Protokollsuche zu erstellen. |
| Active Directory |Scannen Sie automatisch die Gruppenmitgliedschaft aller Agent-Computer, die Mitglieder einer Active Directory-Domäne sind, und erstellen Sie für die einzelnen Sicherheitsgruppen jeweils eine Gruppe in Log Analytics. |
| WSUS |Scannen Sie automatisch WSUS-Server oder -Clients auf Zielgruppen, und erstellen Sie jeweils eine Gruppe in Log Analytics. |

### <a name="log-search"></a>Protokollsuche
Computergruppen, die aus einer Protokollsuche erstellt wurden, enthalten alle Computer die bei der von Ihnen definierten Suchabfrage zurückgegeben wurden.  Diese Abfrage wird jedes Mal ausgeführt, wenn die Computergruppe verwendet wird. Daher werden sämtliche Änderungen seit der Erstellung der Gruppe berücksichtigt.

Gehen Sie zum Erstellen einer Computergruppe aus einer Protokollsuche wie folgt vor.

1. [Erstellen Sie eine Protokollsuche](log-analytics-log-searches.md), die eine Liste mit Computern zurückgibt.  Die Suche muss eine bestimmten Gruppe von Computern zurückgeben, indem Sie in der Abfrage z.B. **distinct Computer** oder **measure count() by Computer** verwenden.  
2. Klicken Sie oben auf der Seite auf die Schaltfläche **Speichern**.
3. Wählen Sie für **Speichern Sie diese Abfrage als Computergruppe:** die Option **Ja** aus.
4. Geben Sie einen **Namen** und eine **Kategorie** für die Gruppe ein.  Wenn bereits eine Suche mit demselben Namen und derselben Kategorie vorhanden ist, werden Sie gefragt, ob diese überschrieben werden soll.  Sie können über mehrere Suchvorgänge mit demselben Namen in unterschiedlichen Kategorien verfügen. 

Nachstehend finden Sie Beispielsuchen, die Sie als eine Computergruppe speichern können.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Protokollsuch-API
Computergruppen, die mit der Protokoll-API erstellt wurden, sind mit denen, die mit einer Protokollsuche erstellt wurden, identisch.

Weitere Informationen zum Erstellen einer Computergruppe mithilfe der Protokollsuch-API finden Sie unter [Computergruppen in der Log Analytics-REST-API für die Protokollsuche](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Wenn Sie Log Analytics für das Importieren von Active Directory-Gruppenmitgliedschaften konfigurieren, werden dabei die Gruppenmitgliedschaften von in eine Domäne eingebundenen Computern mit dem OMS-Agent analysiert.  Für jede Sicherheitsgruppe in Active Directory wird in Log Analytics eine Computergruppe erstellt, und jeder Computer wird der Computergruppe hinzugefügt, die den Sicherheitsgruppen entspricht, in denen er Mitglied ist.  Diese Mitgliedschaft wird kontinuierlich alle 4 Stunden aktualisiert.  

Sie konfigurieren Log Analytics für das Importieren von Active Directory-Sicherheitsgruppen im Menü **Computergruppen** in den **Einstellungen** von Log Analytics.  Wählen Sie **Automatisierung** und anschließend **Active Directory-Gruppenmitgliedschaften von Computern importieren** aus.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wenn Gruppen importiert wurden, werden im Menü die Anzahl der Computer, für die eine Gruppenmitgliedschaft erkannt wurde, und die Anzahl der importierten Gruppen aufgeführt.  Klicken Sie auf einen dieser Links, um die **ComputerGroup**-Datensätze mit diesen Informationen zurückzugeben.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Wenn Sie Log Analytics für das Importieren von WSUS-Gruppenmitgliedschaften konfigurieren, werden dabei die Zielgruppenmitgliedschaften aller Computer mit dem OMS-Agent analysiert.  Bei Verwendung clientseitiger Zielgruppen wird die Gruppenmitgliedschaft aller Computer, die mit OMS verbunden und Teil einer WSUS-Zielgruppe sind, in Log Analytics importiert. Bei Verwendung serverseitiger Zielgruppen sollte der OMS-Agent auf dem WSUS-Server installiert sein, damit die Informationen zur Gruppenmitgliedschaft in OMS importiert werden können.  Diese Mitgliedschaft wird kontinuierlich alle 4 Stunden aktualisiert. 

Sie konfigurieren Log Analytics für das Importieren von Active Directory-Sicherheitsgruppen im Menü **Computergruppen** in den **Einstellungen** von Log Analytics.  Wählen Sie **Active Directory** und anschließend **Active Directory-Gruppenmitgliedschaften von Computern importieren** aus.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Wenn Gruppen importiert wurden, werden im Menü die Anzahl der Computer, für die eine Gruppenmitgliedschaft erkannt wurde, und die Anzahl der importierten Gruppen aufgeführt.  Klicken Sie auf einen dieser Links, um die **ComputerGroup**-Datensätze mit diesen Informationen zurückzugeben.

## <a name="managing-computer-groups"></a>Verwalten von Computergruppen
Sie können die Computergruppen, die über eine Protokollsuche oder die Protokollsuch-API erstellt wurden, im Menü **Computergruppen** in den **Einstellungen** von Log Analytics anzeigen.  Klicken Sie auf das **x** in der Spalte **Entfernen**, um die Computergruppe zu löschen.  Klicken Sie auf das Symbol **Mitglieder anzeigen** für eine Gruppe, um die Protokollsuche für die Gruppe ausführen, mit der die Elemente zurückgegeben werden. 

![Gespeicherte Computergruppen](media/log-analytics-computer-groups/configure-saved.png)

Um die Gruppe zu ändern, erstellen Sie eine neue Gruppe mit derselben **Kategorie** und demselben **Namen**, um die ursprüngliche Gruppe zu überschrieben.

## <a name="using-a-computer-group-in-a-log-search"></a>Verwenden einer Computergruppe in einer Protokollsuche
Verwenden Sie die folgende Syntax, um in einer Protokollsuche auf eine Computergruppe zu verweisen.  Die Angabe der **Kategorie** ist optional und nur erforderlich, wenn Sie über mehrere Computergruppen mit demselben Namen in verschiedenen Kategorien verfügen. 

    $ComputerGroups[Category: Name]

Wenn eine Suche ausgeführt wird, werden zuerst die Mitglieder aller in der Suche enthaltenen Computergruppen aufgelöst.  Wenn die Gruppe auf einer Protokollsuche basiert, wird diese Suche vor der eigentlichen Protokollsuche ausgeführt, um die Mitglieder der Gruppe zurückzugeben.

Computergruppen werden häufig mit der **IN**-Klausel in der Protokollsuche verwendet, wie im folgenden Beispiel gezeigt.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Computergruppen-Datensätze
Im OMS-Repositorys wird für jede Mitgliedschaft in einer Computergruppe, die aus Active Directory oder WSUS erstellt wurde, ein Datensatz erstellt.  Diese Datensätze sind vom Typ **ComputerGroup** und weisen die in der folgenden Tabelle aufgeführten Eigenschaften auf.  Es werden keine Datensätze für Computergruppen basierend auf der Protokollsuche erstellt.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Computer |Der Name des Mitgliedscomputers |
| Group |Der Anzeigename der Gruppe |
| GroupFullName |Der vollständige Pfad zur Gruppe, einschließlich Quelle und Quellname |
| GroupSource |Die Quelle, aus der die Gruppe zusammengestellt wurde <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Der Name der Quelle, aus der die Gruppen zusammengestellt wurden.  Für Active Directory ist dies der Domänenname. |
| ManagementGroupName |Name der Verwaltungsgruppe für SCOM-Agents.  Bei anderen Agents lautet dieser „AOI-\<Arbeitsbereich-ID\>“. |
| TimeGenerated |Das Datum und die Uhrzeit der Erstellung oder letzten Aktualisierung der Computergruppe |

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  




<!--HONumber=Nov16_HO3-->


