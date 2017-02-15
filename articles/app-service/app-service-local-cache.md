---
title: "Übersicht über den lokalen Cache von Azure App Service | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie den lokalen Cache von Azure App Service aktivieren, dessen Größe ändern und seinen Status abrufen."
services: app-service
documentationcenter: app-service
author: SyntaxC4
manager: yochayk
editor: 
tags: optional
keywords: 
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 385eb87ec32f5f605b28cc8c76b1c89c7e90bfec
ms.openlocfilehash: 09ec6d1aae5dc893e92b7c4ca1c30a251d02443d


---
# <a name="azure-app-service-local-cache-overview"></a>Übersicht über den lokalen Cache von Azure App Service
Der Inhalt von Azure-Web-Apps wird in Azure Storage gespeichert und dauerhaft als Inhaltsfreigabe bereitgestellt. Dieses Design ist auf den Einsatz mit einer Vielzahl von Apps ausgelegt und weist die folgenden Merkmale auf:  

* Der Inhalt wird über mehrere VM-Instanzen der Web-App hinweg freigegeben.
* Der Inhalt wird dauerhaft bereitgestellt und kann durch ausgeführte Web-Apps geändert werden.
* Protokolldateien und Diagnosedatendateien stehen unterhalb desselben freigegebenen Inhaltsordners zur Verfügung.
* Beim Veröffentlichen neuer Inhalte wird der Inhaltsordner direkt aktualisiert. Der Inhalt kann umgehend über die SCM-Website und die ausgeführte Web-App angezeigt werden (einige Technologien wie z. B. ASP.NET initiieren bei einigen Dateiänderungen üblicherweise einen Web-App-Neustart, um die aktuellen Inhalte abzurufen).

Während viele Web-Apps einzelne oder alle dieser Features nutzen, benötigen einige Web-Apps lediglich einen hoch leistungsfähigen schreibgeschützten Inhaltsspeicher, aus dem sie mit hoher Verfügbarkeit ausgeführt werden können. Diese Apps können von einer VM-Instanz eines bestimmten lokalen Caches profitieren.

Der lokale Cache von Azure App Service bietet eine Webrollenansicht Ihrer Inhalte. Dabei handelt es sich um einen Cache Ihres Speicherinhalts mit „write but discard“-Prinzip, der beim Sitestart asynchron erstellt wird. Wenn der Cache bereit ist, wird die Site zur Ausführung mit dem zwischengespeicherten Inhalt umgeschaltet. Web-Apps, die mit lokalem Cache ausgeführt werden, bieten die folgenden Vorteile:

* Sie werden nicht durch Latenzen beeinträchtigt, die beim Zugriff auf Inhalt in Azure Storage auftreten.
* Sie werden nicht durch geplante Upgrades oder ungeplante Downtimes der Server beeinträchtigt, die die Inhaltsfreigabe verwalten, und sie werden nicht durch andere Unterbrechungen im Zusammenhang mit Azure Storage beeinträchtigt.
* Es sind weniger App-Neustarts aufgrund von Änderungen an der Speicherfreigabe erforderlich.

## <a name="how-local-cache-changes-the-behavior-of-app-service"></a>Auswirkung des lokalen Caches auf das Verhalten von App Service
* Der lokale Cache ist eine Kopie der Ordner „/site“ und „/siteextensions“ der Web-App. Er wird beim Start der Web-App auf der lokalen VM-Instanz erstellt. Die Größe des lokalen Caches ist pro Web-App standardmäßig auf 300 MB beschränkt, kann aber auf bis zu 2 GB erhöht werden.
* Der lokale Cache bietet Lese- und Schreibzugriff. Änderungen werden jedoch verworfen, wenn die Web-App zwischen virtuellen Computern verschoben oder neu gestartet wird. Der lokale Cache sollte nicht für Apps verwendet werden, die unternehmenskritische Daten im Inhaltsspeicher speichern.
* Web-Apps können wie bisher Protokolldateien und Diagnosedaten schreiben. Protokolldateien und Daten werden jedoch lokal auf der VM gespeichert. Diese Daten werden dann in regelmäßigen Abständen in den freigegebenen Inhaltsspeicher kopiert. Der Kopiervorgang in den freigegebenen Inhaltsspeicher erfolgt nach dem „Best Case“-Prinzip, und zurückgeschriebene Daten können bei einem plötzlichen Absturz einer VM-Instanz verloren gehen.
* Für Web-Apps, die den lokalen Cache verwenden, ergibt sich eine Änderung in Bezug auf die Ordnerstruktur für die LogFiles- und Data-Ordner. Es sind jetzt Unterordner in den Speicherordnern „LogFiles“ und „Data“ vorhanden, die das folgende Benennungsmuster verwenden: „eindeutiger Bezeichner“ + Zeitstempel Jeder dieser Unterordner entspricht einer VM-Instanz, auf der die Web-App ausgeführt wird oder wurde.  
* Das Veröffentlichen von Änderungen an der Web-App über einen der Veröffentlichungsmechanismen führt zu einer Veröffentlichung im freigegebenen Inhaltsspeicher. Dieses Verhalten wurde umgesetzt, weil der veröffentlichte Inhalt dauerhaft bereitgestellt werden soll. Zum Aktualisieren des lokalen Caches der Web-App ist ein Neustart erforderlich. Dies erscheint überzogen? Informationen zu einem nahtlosen Lebenszyklus finden Sie weiter unten in diesem Artikel.
* „D:\Home“ zeigt auf den lokalen Cache. „D:\local“ zeigt weiterhin auf den temporären VM-spezifischen Speicher.
* Die standardmäßige Inhaltsansicht der SCM-Site ist weiterhin die des freigegebenen Inhaltsspeichers.

## <a name="enable-local-cache-in-app-service"></a>Aktivieren des lokalen Caches in App Service
Der lokale Cache wird mithilfe einer Kombination aus reservierten App-Einstellungen konfiguriert. Diese App-Einstellungen können über die folgenden Methoden konfiguriert werden:

* [Azure-Portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Konfigurieren des lokalen Caches über das Azure-Portal
<a name="Configure-Local-Cache-Portal"></a>

Der lokale Cache wird für jede Web-App über die folgende App-Einstellung aktiviert: `WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![App-Einstellungen im Azure-Portal: lokaler Cache](media/app-service-local-cache/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Konfigurieren des lokalen Caches mit Azure Resource Manager
<a name="Configure-Local-Cache-ARM"></a>

```
...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],
    "properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-the-size-setting-in-local-cache"></a>Ändern der Größeneinstellung im lokalen Cache
Standardmäßig ist der lokale Cache **300 MB** groß. Dies schließt die Ordner „/site“ und „/siteextensions“ ein, die aus dem Inhaltspeicher kopiert werden, sowie alle lokal erstellten Protokolle und Datenordner. Um dieses Limit zu erhöhen, verwenden Sie die folgende App-Einstellung: `WEBSITE_LOCAL_CACHE_SIZEINMB`. Die Größe kann auf bis zu **2 GB** (2.000 MB) pro Web-App erhöht werden.

## <a name="best-practices-for-using-app-service-local-cache"></a>Bewährte Methoden für die Verwendung des lokalen Caches von App Service
Es wird empfohlen, den lokalen Cache gemeinsam mit dem Feature [Stagingumgebungen](../app-service-web/web-sites-staged-publishing.md) zu verwenden.

* Fügen Sie die *persistente* App-Einstellung `WEBSITE_LOCAL_CACHE_OPTION` mit dem Wert `Always` zu Ihrem **Produktionsslot** hinzu. Wenn Sie `WEBSITE_LOCAL_CACHE_SIZEINMB`verwenden, fügen Sie diese Einstellung ebenfalls als persistente Einstellung zu Ihrem Produktionsslot hinzu.
* Erstellen Sie einen **Stagingslot**, und führen Sie eine Veröffentlichung in Ihrem Stagingslot durch. Für den Stagingslot wird üblicherweise nicht die Verwendung des lokalen Caches festgelegt. So wird ein nahtloser Lebenszyklus für das Erstellen/Bereitstellen/Testen für das Staging ermöglicht, während die Vorteile des lokalen Caches für den Produktionsslot genutzt werden.
* Testen Sie Ihre Site mit dem Stagingslot.  
* Wenn Sie bereit dazu sind, wechseln Sie über einen [swap-Vorgang](../app-service-web/web-sites-staged-publishing.md#Swap) zwischen Ihrem Staging- und Ihrem Produktionsslot.  
* Persistente Einstellungen umfassen einen Namen und gelten dauerhaft für einen Slot. Wenn also der Stagingslot auf den Produktionsslot umgeschaltet wird, erbt dieser die App-Einstellungen für den lokalen Cache. Der neue Produktionsslot wird nach ein paar Minuten mit dem lokalen Cache ausgeführt und im Rahmen der Slotaufwärmung nach dem Wechsel aufgewärmt. Wenn der Slotwechsel abgeschlossen ist, wird Ihr Produktionsslot mit dem lokalen Cache ausgeführt.

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)
### <a name="how-can-i-tell-if-local-cache-applies-to-my-web-app"></a>Ist der lokale Cache für meine Web-App geeignet?
Wenn Ihre Web-App einen zuverlässigen Hochleistungs-Inhaltsspeicher benötigt, den Inhaltsspeicher nicht zum Schreiben von unternehmenskritischen Daten zur Laufzeit nutzt und eine Gesamtgröße von maximal 2 GB erforderlich ist, dann lautet die Antwort: Ja, Ihre Anwendung ist für den lokalen Cache geeignet! Um die Gesamtgröße der Ordner „/site“ und „/siteextensions“ zu ermitteln, können Sie die Siteerweiterung „Azure Web Apps Disk Usage“ verwenden.  

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Woher weiß ich, dass meine Site auf die Verwendung des lokalen Caches umgeschaltet hat?
Wenn Sie das Feature für den lokalen Cache mit Stagingumgebungen verwenden, findet der Wechsel erst dann statt, wenn der lokale Cache aufgewärmt wurde. Um zu prüfen, ob Ihre Site mit dem lokalen Cache ausgeführt wird, können Sie die Workerprozess-Umgebungsvariable `WEBSITE_LOCALCACHE_READY`überprüfen. Befolgen Sie die auf der Seite für die [Workerprozess-Umgebungsvariable](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) bereitgestellten Anweisungen, um auf mehreren Instanzen auf die Workerprozess-Umgebungsvariable zuzugreifen.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-to-have-them-why"></a>Ich habe soeben neue Änderungen veröffentlicht, aber in meiner Web-App scheinen diese nicht verfügbar zu sein. Warum?
Wenn Ihre Web-App den lokalen Cache verwendet, müssen Sie Ihre Site neu starten, um die neuesten Änderungen abzurufen. Sie möchten Änderungen nicht für eine Produktionssite veröffentlichen? Relevante Informationen finden Sie bei den Slotoptionen im oben stehenden Abschnitt zu den bewährten Methoden.

### <a name="where-are-my-logs"></a>Wo sind meine Protokolle?
Bei Verwendung des lokalen Caches sehen Ihre Protokolle und Datenordner etwas anders aus. Die Struktur Ihrer Unterordner bleibt jedoch erhalten, mit der Ausnahme, dass sie unterhalb eines Unterordners mit folgendem Format geschachtelt sind: „eindeutiger VM-Bezeichner“ + Zeitstempel

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Ich habe den lokalen Cache aktiviert, aber meine Web-App wird weiterhin neu gestartet. Warum? Ich dachte, durch den lokalen Cache werden App-Neustarts verringert.
Der lokale Cache trägt dazu bei, speicherbezogene Neustarts von Web-Apps zu vermeiden. Ihre Web-App kann jedoch aufgrund von geplanten Upgrades der VM-Infrastruktur weiterhin neu gestartet werden. Die App sollte bei aktiviertem lokalen Cache insgesamt jedoch seltener neu gestartet werden.



<!--HONumber=Feb17_HO2-->


