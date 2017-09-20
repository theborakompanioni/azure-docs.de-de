---
title: "Versionshinweise zu Azure SDK für .NET 2.8"
description: "Versionshinweise zu Azure SDK für .NET 2.8"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: 81772fb5801ec620d1c1996509a275e4bb29fb12


---
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK für .NET 2.8, 2.8.1 und 2.8.2
## <a name="overview"></a>Übersicht
Dieser Artikel enthält die Versionshinweise (einschließlich bekannter Probleme und wichtiger Änderungen) für das Azure SDK für .NET 2.8, 2.8.1 und 2.8.2. 

Eine vollständige Liste der neuen Funktionen und in dieser Version vorgenommenen Aktualisierungen finden Sie in der Ankündigung zum [Azure SDK 2.8 für Visual Studio 2013 und Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) (in englischer Sprache). 

## <a name="azure-sdk-for-net-28"></a>Azure SDK für .NET 2.8
### <a name="download-azure-sdk-for-net-28"></a>Herunterladen von Azure SDK für .NET 2.8
[Azure SDK für .NET 2.8 für Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK für .NET 2.8 für Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>Unterstützung von .NET 4.5.2
#### <a name="known-issues"></a>Bekannte Probleme
Mit Azure .NET SDK 2.8 können Sie .NET 4.5.2-Clouddienstpakete erstellen. Das .NET 4.5.2-Framework wird jedoch bis zur Gastbetriebssystemversion im Januar 2016 nicht in den Gastbetriebssystem-Standardimages installiert. Bis zu dieser Version wird das .NET 4.5.2-Framework über eine separate Gastbetriebssystemversion (November 2015-02) zur Verfügung gestellt. Informationen zur Veröffentlichung des Images finden Sie auf der Seite [Azure-Gastbetriebssystemreleases und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md).  Wenn das Image „November 2015-02“ veröffentlicht wurde, können Sie dieses verwenden, indem Sie die Clouddienst-Konfigurationsdatei (CSCFG-Datei) entsprechend aktualisieren. Legen Sie in der Dienstkonfigurationsdatei das osVersion-Attribut des ServiceConfiguration-Elements auf die Zeichenfolge „WA-GUEST-OS-4.26_201511-02“ fest. Wenn Sie sich für die Verwendung dieses Images anmelden, erhalten Sie keine automatischen Updates für das Gastbetriebssystem mehr. Um automatische Updates zu erhalten, muss osVersion auf „*“ festgelegt sein. .NET 4.5.2 ist ab Januar 2016 nur noch über automatische Updates verfügbar.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Bekannte Probleme
Während der Projekterstellung einer **Data Factory-Vorlage** mit Beispieldaten kann das Azure PowerShell-Skript fehlschlagen, wenn auf dem Computer eine höhere Azure PowerShell-Version als Version 0.9.8 installiert ist.

Um diese Art Projekt erfolgreich zu erstellen, müssen Sie [Azure PowerShell 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)installieren.

### <a name="azure-resource-manager-tools"></a>Azure-Ressourcen-Manager-Tools
#### <a name="breaking-changes"></a>Wichtige Änderungen
Das mit dem Projekt „Azure-Ressourcengruppe“ bereitgestellte PowerShell-Skript wurde in dieser Version so aktualisiert, dass es mit den neuen Azure PowerShell-Cmdlets, Version 1.0, verwendet werden kann.  Dieses neue Skript kann nicht in Visual Studio ausgeführt werden, wenn eine frühere Version des SDK als Version 2.8 verwendet wird.  

Skripts aus Projekten, die in früheren Versionen des SDK erstellt wurden, können bei Verwendung der Version 2.8 des SDK nicht ausgeführt werden.  Alle Skripts funktionieren weiterhin außerhalb von Visual Studio mit der entsprechenden Version der Azure PowerShell-Cmdlets.  

Für das SDK 2.8 ist Version 1.0 der Azure PowerShell-Cmdlets erforderlich.  Alle anderen Versionen des SDK erfordern Version 0.9.8 der Azure PowerShell-Cmdlets.  Weitere Informationen finden Sie in [diesem](http://go.microsoft.com/fwlink/?LinkID=623011) Blog.

### <a name="web-tools-extensions"></a>Erweiterungen für Webtools
#### <a name="known-issues"></a>Bekannte Probleme
Die folgenden bekannten Probleme werden in der folgenden Version behoben.

* Bewegungen in Cloud Explorer und Server-Explorer in Bezug auf App Service für Nichtproduktionsumgebungen (z. B. Kunden von Azure China und Azure Stack) funktionieren nicht. Kunden in diesen betroffenen Bereichen können das Veröffentlichungsprofil über das Azure-Portal herunterladen, um die Veröffentlichungsfunktion zu ermöglichen. In einer zukünftigen Version werden Probleme bei Gesten wie „Attach Debugger“ und „View Streaming Logs“ für Kunden von Azure China und Azure Stack behoben. 
* Kunden erhalten möglicherweise Fehlermeldungen bei der App Service-Erstellung, wenn sich die App Insights-Instanz, für die die Bereitstellung erfolgt, in einer anderen Region als im Osten der USA befindet. In diesen Fällen können die Veröffentlichungsszenarios durch Erstellen eines App Service im Portal und Herunterladen des Veröffentlichungsprofils ermöglicht werden. 

### <a name="azure-hdinsight-tools"></a>Azure HDInsight-Tools
#### <a name="new-updates"></a>Neue Updates
* Sie können die Hive-Abfrage im Cluster über HiveServer2 fast ohne Mehraufwand ausführen und die Auftragsprotokolle in Echtzeit anzeigen.
* Mithilfe der neuen Aufgabenausführungsansicht für Hive können Sie Aufträge eingehender betrachten, mehr Details anzeigen und mögliche Probleme ermitteln.

Weitere Informationen finden Sie unter [Azure SDK 2.8 für Visual Studio 2013 und Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK für .NET 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekannte Probleme bei Visual Studio 2013 und Visual Studio 2015
1. Für ausgelöste Webaufträge, die in Slots veröffentlicht werden, wird eine Fehlermeldung angezeigt und es wird kein Zeitplan festgelegt. Der Webauftrag wird allerdings an Azure weitergegeben. Kunden, die einen geplanten Auftrag benötigen, können dann das Azure-Portal zum Einrichten des Webauftrags verwenden. 
2. Bei Python-Kunden treten möglicherweise Debugger-Probleme auf. Das Service-Team führt dafür ein Update ein. Wenn Kunden betroffen sind, informieren Sie Microsoft dennoch in den Foren, im Ankündigungsblog oder im Kommentarbereich der Versionshinweise. 
3. In bestimmten Regionen (z. B. Südindien) treten Fehler bei der App Service-Bereitstellung auf. Dies gilt auch für das Portal, und Kunden, bei denen dieses Problem auftritt, können das Azure-Portal nutzen, um den Zugriff zum Veröffentlichen in diesen Georegionen anzufordern. Nachdem die Kunden den Zugriff auf diese Regionen angefordert haben, sollte die Bereitstellung des Azure-Portals funktionieren. 

## <a name="azure-sdk-for-net-282"></a>Azure SDK für .NET 2.8.2
Nach der Installation von Version 2.8.2 dieses Tools kann es vorkommen, dass Sie folgende Probleme feststellen.         

* Wenn Sie Windows 10 verwenden und Internet Explorer nicht installiert ist, kann es vorkommen, dass Ihnen die Fehlermeldung „Internet Explorer konnte nicht gefunden werden“ angezeigt wird.
  Installieren Sie Internet Explorer über das Dialogfeld „Windows-Komponenten hinzufügen/entfernen“.

Tritt dieses Problem bei Ihnen auf, teilen Sie uns dies bitte über die Funktion „Lächeln senden“ mit.

Weitere Informationen finden Sie in [diesem Beitrag](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) .

## <a name="other-updates"></a>Andere Updates
Andere Updates finden Sie im [Ankündigungsartikel zu Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Siehe auch
[Ankündigungsbeitrag zu Azure SDK 2.8 (in englischer Sprache)](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Unterstützungs- und Deaktivierungsinformationen zum Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx)




<!--HONumber=Dec16_HO2-->


