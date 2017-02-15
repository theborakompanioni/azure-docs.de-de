---
title: 'Gewusst wie: Skalieren von Apps in einer App Service-Umgebung'
description: Skalierung einer Web-App in einer App Service-Umgebung
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 240c2486c23b7cd84e2471bf5b2170e08ee1f150


---
# <a name="scaling-apps-in-an-app-service-environment"></a>Skalieren von Apps in einer App Service-Umgebung
In Azure App Service können üblicherweise drei Faktoren skaliert werden:

* Tarif
* Workergröße 
* Anzahl von Instanzen

In einer App Service-Umgebung ist es nicht erforderlich, den Tarif auszuwählen oder zu ändern.  Diese wird im Hinblick auf die Funktionalität bereits im Premium-Plan ausgeführt.  

Im Hinblick auf Workergrößen kann der Administrator der App Service-Umgebung die Größe der Computeressource zuweisen, die für jeden Workerpool verwendet wird.  Dies bedeutet, dass Sie P4-Compute-Ressourcen für Workerpool 1 und P1-Compute-Ressourcen für Workerpool 2 zuweisen können, wenn dies gewünscht ist.  Es ist keine Reihenfolge nach Größe erforderlich.  Ausführliche Informationen zu Größen und Preisen finden Sie im Dokument [Azure App Service-Preise][AppServicePricing].  Damit stehen für die Skalierung von Web-Apps und App Service-Plänen in einer App Service-Umgebung die folgenden Optionen zur Verfügung:

* Auswahl der Workerpools
* Anzahl von Instanzen

Das Ändern dieser Elemente erfolgt über die entsprechende Benutzeroberfläche in Ihrem in der App Service-Umgebung gehosteten App Service-Plan.  

![][1]

Beachten Sie, dass Ihr ASP nicht über die Anzahl der verfügbaren Computeressourcen, die im Workerpool Ihres ASP enthalten sind, hinaus zentral hochskaliert werden kann.  Wenn Sie in diesem Workerpool Compute-Ressourcen benötigen, bitten Sie den Administrator Ihrer App Service-Umgebung, diese hinzuzufügen.  Weitere Informationen rund um die Neukonfiguration der App Service-Umgebung finden Sie unter [Konfigurieren einer App Service-Umgebung][HowtoConfigureASE].  Sie sollten auch die Features für das automatische Skalieren der App Service-Umgebung nutzen, um nach Zeitplan oder Metriken Kapazitäten hinzuzufügen.  Weitere Informationen zum Konfigurieren der automatischen Skalierung für die App Service-Umgebung selbst finden Sie unter [Skalieren von Apps in einer App Service-Umgebung][ASEAutoscale].

Sie können mehrere App Service-Pläne mit Computeressourcen aus verschiedenen Workerpools erstellen. Sie können aber auch den gleichen Workerpool verwenden.  Wenn in Workerpool 1 beispielsweise (10) Computeressourcen verfügbar sind, können Sie einen App Service-Plan mit (6) Computeressourcen und einen zweiten mit (4) Computeressourcen erstellen. 

### <a name="scaling-the-number-of-instances"></a>Skalieren der Anzahl von Instanzen
Wenn Sie Ihre Web-App in einer App Service-Umgebung erstellen, beginnt sie mit einer Instanz.  Sie können dann auf zusätzliche Instanzen horizontal hochskalieren, um zusätzliche Computeressourcen für Ihre App bereitzustellen .   

Wenn Ihre App Service-Umgebung über ausreichend Kapazität verfügt, ist dies ganz einfach.  Wechseln Sie zum App Service-Plan, der die zu skalierenden Websites umfasst, und wählen Sie die Option zum Skalieren.  Daraufhin wird die Benutzeroberfläche geöffnet, auf der Sie manuell die Skalierung für Ihren ASP festlegen oder Regeln für das automatische Skalieren Ihres ASP konfigurieren können.  Zum manuellen Skalieren der App legen Sie einfach ***Skalieren nach*** auf ***Eine Anzahl von Instanzen, die ich manuell festlege*** fest.  Von hier aus können Sie entweder den Schieberegler auf die gewünschte Menge einstellen oder diese in das Feld neben dem Schieberegler eingeben.  

![][2] 

Die Regeln für das automatische Skalieren eines ASPs in einer App Service-Umgebung funktionieren wie gewohnt.  Sie können ***CPU-Prozentsatz*** unter ***Skalieren nach*** auswählen und Regeln für das automatische Skalieren des ASPs basierend auf einem CPU-Prozentsatz erstellen, oder Sie können komplexere Regeln erstellen, die ***Zeitpläne und Leistungsregeln*** verwenden.  Ausführlichere Informationen zur Konfiguration der automatischen Skalierung finden Sie in der Anleitung unter [Skalieren einer App in Azure App Service][AppScale]. 

### <a name="worker-pool-selection"></a>Auswahl der Workerpools
Wie bereits erwähnt erfolgt die Auswahl des Workerpools über die Benutzeroberfläche des App Service-Plans.  Öffnen Sie das Blatt für den App Service-Plan, den Sie skalieren und für den Sie einen Workerpool auswählen möchten.  Es werden alle Workerpools angezeigt, die Sie in der App Service-Umgebung konfiguriert haben.  Wenn Sie nur über einen Workerpool verfügen, wird nur dieser Pool aufgeführt.  Um den Workerpool für Ihren ASP zu ändern, wählen Sie einfach den Workerpool aus, in den Sie Ihren App Service-Plan verschieben möchten.  

![][3]

Vor dem Verschieben des ASPs aus einem Workerpool in einen anderen sollten Sie sicherstellen, dass Sie über eine ausreichende Kapazität für den ASP verfügen.  In der Liste der Workerpools wird nicht nur der Name des Workerpools aufgeführt, sondern Sie können auch sehen, wie viele Worker im Workerpool zur Verfügung stehen.  Stellen Sie sicher, dass eine ausreichende Anzahl von Instanzen für Ihren App Service-Plan vorhanden ist.  Wenn Sie mehr Compute-Ressourcen im Workerpool benötigen, in den Sie Ihren App Service-Plan verschieben möchten, bitten Sie den Administrator Ihrer App Service-Umgebung, weitere Ressourcen hinzuzufügen.  

> [!NOTE]
> Das Verschieben eines ASPs aus einem Workerpool verursacht Kaltstarts der Apps in diesem ASP.  Dadurch werden Anforderungen langsamer verarbeitet, da Ihre App auf den neuen Computeressourcen kaltgestartet wurde.  Der Kaltstart kann mithilfe der [Anwendungsfunktion „Aufwärmen“][AppWarmup] in Azure App Service vermieden werden.  Das in diesem Artikel beschriebene Modul zum Initialisieren der Anwendung funktioniert auch bei Kaltstarts, da der Initialisierungsprozess auch aufgerufen wird, wenn Apps auf neuen Computeressourcen kaltgestartet werden. 
> 
> 

## <a name="getting-started"></a>Erste Schritte
Informationen zum Einstieg in App Service-Umgebungen finden Sie unter [Erstellen einer App Service-Umgebung][HowtoCreateASE].

Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/



<!--HONumber=Nov16_HO3-->


