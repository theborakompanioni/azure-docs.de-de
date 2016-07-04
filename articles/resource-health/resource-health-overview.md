<properties
   pageTitle="Übersicht über Azure Resource Health | Microsoft Azure"
   description="Übersicht über Azure Resource Health"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# Übersicht über Azure Resource Health

Azure Resource Health (Ressourcenintegrität) ist ein Dienst, der die Integrität von einzelnen Azure-Ressourcen offenlegt und wertvolle Hinweise zur Behandlung von Problemen bietet. In einer Cloudumgebung, in der der direkte Zugriff auf Server oder Infrastrukturelemente nicht möglich ist, besteht das Ziel der Ressourcenintegrität darin, die Behebung von Problemen durch Kunden zu beschleunigen. Kunden benötigen insbesondere weniger Zeit, um zu ermitteln, ob die Problemursache in der Anwendung selbst zu finden ist oder ob das Problem durch ein Ereignis auf der Azure-Plattform verursacht wird.

## Was wird als Ressource angesehen, und wie wird in Bezug auf die Ressourcenintegrität entschieden, ob die Ressource fehlerfrei ist? 
Eine Ressource ist eine vom Benutzer erstellte Instanz eines Ressourcentyps, der von einem Dienst bereitgestellt wird, z.B. ein virtueller Computer, eine Web-App oder eine SQL-Datenbank.

Die Ressourcenintegrität basiert auf Signalen, die von der Ressource und/oder dem Dienst ausgesendet werden. So kann ermittelt werden, ob eine Ressource fehlerfrei („integer“) ist. Folgender Hinweis ist wichtig: Derzeit gilt die Ressourcenintegrität nur für die Integrität einer bestimmten Ressource. Es werden keine anderen Elemente berücksichtigt, die ggf. zur Gesamtintegrität beitragen. Wenn beispielsweise der Status eines virtuellen Computers gemeldet wird, wird nur der Computeteil der Infrastruktur berücksichtigt. Probleme im Netzwerk werden unter Resource Health also nur dann angezeigt, wenn ein Dienstausfall deklariert wurde. In diesem Fall wird dies über das Banner oben auf dem Blatt angegeben. Weitere Informationen zu Dienstausfällen erhalten Sie weiter unten in diesem Artikel.

## Inwiefern unterscheidet sich Resource Health vom Dashboard zum Dienststatus?

Die von Resource Health bereitgestellten Informationen sind präziser als die vom Dashboard zum Dienststatus bereitgestellten Informationen. Vom Dashboard werden Ereignisse gemeldet, die sich auf die Verfügbarkeit eines Diensts in einer Region auswirken. Resource Health macht Informationen verfügbar, die für eine bestimmte Ressource relevant sind. Beispielsweise werden Ereignisse gemeldet, die eine Beeinträchtigung der Verfügbarkeit eines virtuellen Computers, einer Web-App oder einer SQL-Datenbank darstellen. Wenn ein Knoten beispielsweise unerwartet neu gestartet wird, können Kunden, deren virtuelle Computer auf dem Knoten ausgeführt wurden, zum Beispiel den Grund erfahren, warum die VM für eine bestimmte Zeit nicht verfügbar war.

## Zugreifen auf Resource Health
Es gibt zwei Möglichkeiten, um auf die Dienste von Resource Health zuzugreifen.

### Azure-Portal
Das Blatt „Ressourcenintegrität“ im Azure-Portal enthält ausführliche Informationen zur Integrität der Ressource sowie empfohlene Aktionen, die je nach der derzeitigen Integrität der Ressource variieren. Dieses Blatt eignet sich am besten zum Abfragen der Ressourcenintegrität, da es den Zugriff auf andere Ressourcen im Portal erlaubt. Wie bereits erwähnt, variieren die empfohlenen Aktionen auf dem Blatt „Ressourcenintegrität“ je nach der derzeitigen Integrität:

* Fehlerfreie Ressourcen: Da kein Problem erkannt wurde, das sich auf die Integrität der Ressource auswirken könnte, geht es bei den Aktionen vor allem um die Unterstützung der Problembehandlung. Beispielsweise ist der direkte Zugriff auf das Blatt „Problembehandlung“ möglich, auf dem Kunden eine Anleitung zur Behebung häufiger Probleme finden.
* Fehlerhafte Ressource: Für Probleme, die von Azure verursacht werden, werden auf dem Blatt Aktionen angezeigt, die von Microsoft zum Wiederherstellen der Ressource durchgeführt wurden bzw. werden. Für Probleme, die durch vom Benutzer initiierte Aktionen verursacht werden, wird auf dem Blatt eine Liste mit Aktionen aufgeführt, mit denen Kunden das Problem beheben und die Ressource wiederherstellen können.  

Nachdem Sie sich am Azure-Portal angemeldet haben, haben Sie zwei Möglichkeiten zum Zugreifen auf das Blatt „Ressourcenintegrität“:

####Öffnen des Blatts „Ressource“
Öffnen Sie das Blatt „Ressource“ für eine bestimmte Ressource. Klicken Sie auf dem Blatt „Einstellungen“, das neben dem Blatt „Ressource“ geöffnet wird, auf „Ressourcenintegrität“, um das Blatt „„Ressourcenintegrität““ zu öffnen.

![Blatt „Ressourcenintegrität“](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### Blatt „Hilfe und Support“
Öffnen Sie das Blatt „Hilfe und Support“, indem Sie oben rechts auf das Fragezeichen klicken und dann die Option „Hilfe und Support“ wählen.

**In der oberen Navigationsleiste**

![Hilfe und Support](./media/resource-health-overview/HelpAndSupport.png)

Wenn Sie auf die Kachel klicken, wird das Blatt „Ressourcenintegrität“ für das Abonnement geöffnet, und alle Ressourcen Ihres Abonnements werden aufgelistet. Neben jeder Ressource wird mit einem Symbol die Integrität angegeben. Wenn Sie auf eine Ressource klicken, wird jeweils das entsprechende Blatt „Ressourcenintegrität“ geöffnet.

**Kachel „Ressourcenintegrität“**

![Kachel „Ressourcenintegrität“](./media/resource-health-overview/resourceHealthTile.png)

### API für Ressourcenintegrität
Zusätzlich zur Benutzeroberfläche des Azure-Portals gibt es auch eine Gruppe von APIs, die zum Abfragen der Ressourcenintegrität verwendet werden kann. Die verfügbaren APIs ermöglichen Benutzern das Abfragen der aktuellen Integrität aller Ressourcen in einem Abonnement oder in einer Ressourcengruppe bzw. der Integrität einer einzelnen Ressource.

Eine andere API ermöglicht Benutzern das Abfragen des Integritätsverlaufs einer einzelnen Ressource. Die Antwort ist eine Sammlung der Ressourcenintegritätstatus der letzten 14 Tage. Wenn die Ressource ggf. von einem Ausfall beeinträchtigt wurde, enthält der Integritätsstatus eine Anmerkung namens „serviceImpactingEvents“ mit weiteren Details zum Ausfall.

Bevor die API zum Abfragen der Ressourcenintegrität verwendet werden kann, muss das Abonnement beim Dienst registriert werden, indem eine POST-Anforderung an die folgende URL gesendet wird: volume
 
        //Register the subscription with the Resource health resource provider
        https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ResourceHealth/register?api-version=2015-01-01
        
Unten sind Beispiele zum Aufrufen der API für die Ressourcenintegrität angegeben.

        // GET health of all resources in a subscription:
        https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ResourceHealth/availabilityStatuses?api-version=2015-01-01
        
        //GET health of all resources in a resource group:
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.ResourceHealth/availabilityStatuses?api-version=2015-01-01
        
        //GET the health of a single resource:
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/providers/Microsoft.ResourceHealth/availabilityStatuses/current?api-version=2015-01-01
        
        //GET the historical health of a single resource:
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/providers/Microsoft.ResourceHealth/availabilityStatuses?api-version=2015-01-01


## Was bedeutet der angezeigte Status der Ressourcenintegrität?
Es gibt vier verschiedene Integritätsstatus, die für die Ressource angezeigt werden können.

### Verfügbar
Der Dienst hat auf der Plattform keine Probleme erkannt, die sich auf die Verfügbarkeit der Ressource auswirken können. Dies wird durch ein grünes Häkchen angezeigt.

![Ressource ist verfügbar](./media/resource-health-overview/Available.png)

### Nicht verfügbar

In diesem Fall hat der Dienst ein fortlaufendes Problem auf der Plattform erkannt, das sich auf die Verfügbarkeit der Ressource auswirkt. Ein Beispiel hierfür ist, dass der Knoten, auf dem die VM ausgeführt wurde, unerwartet neu gestartet wurde. Dies wird durch ein rotes Warnsymbol angezeigt. Zusätzliche Informationen zum Problem werden im mittleren Bereich des Blatts angezeigt, z.B.:

1.	Aktionen von Microsoft zum Wiederherstellen der Ressource 
2.	Ausführliche Zeitachse des Problems, einschließlich der erwarteten Behebungsdauer
3.	Liste mit empfohlenen Aktionen für Benutzer 

![Ressource ist nicht verfügbar](./media/resource-health-overview/Unavailable.png)

### Nicht verfügbar – Vom Kunden initiiert
Die Ressource ist aufgrund einer Kundenanforderung, z.B. dem Beenden einer Ressource oder dem Anfordern eines Neustarts, nicht verfügbar. Dies wird durch ein blaues Infosymbol angezeigt.

![Ressource ist aufgrund einer vom Benutzer initiierten Aktion nicht verfügbar](./media/resource-health-overview/userInitiated.png)

### Unknown
Der Dienst hat länger als fünf Minuten keine Informationen mehr zu dieser Ressource erhalten. Dies wird durch ein graues Fragezeichensymbol angezeigt.

Es ist wichtig zu beachten, dass dies kein definitiver Hinweis darauf ist, dass die Ressource einen Fehler aufweist. Kunden sollten sich daher an die folgenden Empfehlungen halten:

* Wenn die Ressource wie erwartet ausgeführt wird, die Integrität unter „Ressourcenintegrität“ aber als „Unbekannt“ angegeben wird, liegen keine Probleme vor. Sie können davon ausgehen, dass der Status der Ressource nach einigen Minuten wieder als „Fehlerfrei“ angezeigt wird.
* Falls es Probleme beim Zugreifen auf die Ressource gibt und die Integrität unter „Ressourcenintegrität“ als „Unbekannt“ angegeben wird, kann dies ein erster Hinweis auf ein Problem sein. Es ist ratsam, weitere Untersuchungen durchzuführen, bis der Status der Integrität entweder auf „Fehlerfrei“ oder „Fehlerhaft“ aktualisiert wird.

![Integrität der Ressource ist unbekannt](./media/resource-health-overview/unknown.png)

## Ereignisse mit Beeinträchtigung von Diensten
Falls die Ressource durch ein dauerhaftes Ereignis beeinträchtigt wird, wird oben auf dem Blatt „Ressourcenintegrität“ ein Banner angezeigt. Wenn Sie auf das Banner klicken, wird das Blatt „Überwachungsereignisse“ geöffnet, auf dem Sie weitere Informationen zum Ausfall finden.

![Ressourcenintegrität ggf. durch ein vom Dienst initiiertes Ereignis beeinträchtigt](./media/resource-health-overview/serviceImpactingEvent.png)

## Was muss ich sonst noch zur Ressourcenintegrität wissen?

### Signal-Latenz
Die Signale für die Ressourcenintegrität können um bis zu 15 Minuten verzögert sein. Dies kann zu Diskrepanzen zwischen dem aktuellen Integritätsstatus der Ressource und der tatsächlichen Verfügbarkeit führen. Behalten Sie dies im Hinterkopf, um zu verhindern, dass unnötig Zeit für die Untersuchung möglicher Probleme aufgewendet wird.

### Sonderfall für SQL 
Die Ressourcenintegrität meldet den Status der SQL-Datenbank, nicht von SQL Server. Dieser Ansatz liefert zwar ein realistischeres Bild der Integrität, aber es müssen auch mehrere Komponenten und Dienste berücksichtigt werden, um die Integrität der Datenbank zu ermitteln. Das aktuelle Signal ist von Anmeldungen bei der Datenbank abhängig. Dies bedeutet, dass der Integritätsstatus für Datenbanken, die regelmäßige Anmeldungen erhalten (also unter anderem den Empfang von Anforderungen zur Ausführung von Abfragen), auch regelmäßig angezeigt wird. Wenn auf die Datenbank mindestens zehn Minuten lang nicht zugegriffen wurde, wird sie in den Zustand „Unbekannt“ versetzt. Dies bedeutet nicht, dass die Datenbank nicht verfügbar ist. Es bedeutet nur, dass kein Signal gesendet wurde, weil keine Anmeldungen durchgeführt wurden. Beim Herstellen einer Verbindung mit der Datenbank und Ausführen einer Abfrage werden die Signale ausgegeben, die zum Ermitteln und Aktualisieren des Integritätsstatus der Datenbank erforderlich sind.

## Feedback
Wir sind stets offen für Feedback und Vorschläge. Sie können uns gern Ihre [Vorschläge](https://feedback.azure.com/forums/266794-support-feedback) senden. Außerdem erreichen Sie uns über [Twitter](https://twitter.com/azuresupport) oder die [MSDN-Foren](https://social.msdn.microsoft.com/Forums/azure).

<!---HONumber=AcomDC_0622_2016-->