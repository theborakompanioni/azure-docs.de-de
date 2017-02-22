---
title: Erstellen einer App Service-Umgebung
description: Beschreibung der Erstellung von App Service-Umgebungen
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2016
ms.author: ccompy
translationtype: Human Translation
ms.sourcegitcommit: af98ae3b425e2e0584da53721249dbcd605a17d1
ms.openlocfilehash: 8d8e1ce7758691a392fdb6d6459e04c6b4f7f5f6


---
# <a name="how-to-create-an-app-service-environment"></a>Erstellen einer App Service-Umgebung
### <a name="overview"></a>Übersicht
App Service-Umgebungen (App Service Environments, ASEs) sind eine Premium-Dienstoption von Azure App Service mit erweiterten Konfigurationsmöglichkeiten, die in den mehrinstanzenfähigen Verwaltungseinheiten nicht verfügbar sind.  Mit dem ASE-Feature wird im Wesentlichen Azure App Service im virtuellen Netzwerk eines Kunden bereitgestellt.  Um ein besseres Verständnis der Möglichkeiten von App Service-Umgebungen zu erhalten, lesen Sie die Dokumentation [Was ist eine App Service-Umgebung?][WhatisASE].

### <a name="before-you-create-your-ase"></a>Bevor Sie Ihre ASE erstellen
Es ist wichtig, die Aspekte zu berücksichtigen, die Sie ändern können.  Die folgenden Aspekte Ihrer ASE können Sie nicht ändern, nachdem sie erstellt wurde:

* Ort
* Abonnement
* Ressourcengruppe
* Verwendetes VNET
* Verwendetes Subnetz 
* Subnetzgröße

Stellen Sie bei der Auswahl eines VNET und Angabe eines Subnetzes sicher, dass es auf künftiges Wachstum ausgelegt ist.  

### <a name="creating-an-app-service-environment"></a>Erstellen einer App Service-Umgebung
Es gibt zwei Möglichkeiten, auf die Benutzeroberfläche zur Erstellung der App Service-Umgebung zuzugreifen.  Durchsuchen Sie Azure Marketplace nach ***App Service Environment***, oder wählen Sie „Neu -> Web + Mobil -> App Service Environment“.  So erstellen Sie eine ASE:

1. Geben Sie den Namen Ihrer ASE an.  Der Name, der für die ASE angegeben wird, wird für die in der ASE erstellten Apps verwendet.  Wenn der Name der App Service-Umgebung „appsvcenvdemo“ ist, lautet der Unterdomänenname „.*appsvcenvdemo.p.azurewebsites.net*“.  Wenn Sie also eine App namens *mytestapp* erstellt haben, wäre diese unter der Adresse *mytestapp.appsvcenvdemo.p.azurewebsites.net* aufrufbar.  Sie dürfen im Namen Ihrer ASE keine Leerzeichen verwenden.  Bei Verwendung von Großbuchstaben im Namen wird der entsprechende Domänenname dennoch vollständig in Kleinbuchstaben geschrieben.  Bei Verwendung eines ILB wird Ihr ASE-Name nicht in Ihrer Unterdomäne verwendet, jedoch stattdessen explizit während der ASE-Erstellung angegeben.
   
    ![][1]
2. Wählen Sie Ihr Abonnement aus.  Das für Ihre ASE verwendete Abonnement ist auch dasjenige, in dem alle Apps in dieser ASE erstellt werden.  Sie können Ihre ASE nicht in einem VNET platzieren, das sich in einem anderen Abonnement befindet.
3. Wählen eine neue Ressourcengruppe aus, oder geben Sie eine an.  Die Ressourcengruppe, die für Ihre ASE verwendet wird, muss identisch mit derjenigen sein, die für das VNET verwendet wird.  Wenn Sie ein bereits vorhandenes VNET auswählen, wird die Auswahl der Ressourcengruppe für Ihre ASE entsprechend derjenigen Ihres VNET aktualisiert.
   
    ![][2]
4. Treffen Sie Ihre Auswahl für „Virtuelles Netzwerk“ und „Speicherort“.  Sie können ein neues VNET erstellen oder ein bereits vorhandenes VNET auswählen.  Wenn Sie ein neues VNET auswählen, können Sie einen Namen und Speicherort angeben. Das neue VNET hat den Adressbereich 192.168.250.0/23 und ein Subnetz namens **default**, das als 192.168.250.0/24 definiert ist.  Sie können auch einfach ein bereits vorhandenes klassisches oder Resource Manager-VNET auswählen.  Die Auswahl des VIP-Adresstyps bestimmt, ob auf Ihre ASE ein direkter Zugriff aus dem Internet möglich ist (extern) oder ob ein interner·Load Balancer (ILB) verwendet wird.  Mehr hierzu erfahren Sie unter [Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung][ILBASE].  Wenn Sie einen VIP-Adresstyp des Typs „Extern“ auswählen, können Sie auswählen, mit wie vielen externen IP-Adressen das System für IPSSL-Zwecke erstellt wird.  Bei Auswahl von „Intern“ müssen Sie die Unterdomäne angeben, die Ihrer ASE verwenden soll.  ASEs können in virtuellen Netzwerken bereitgestellt werden, die *entweder* öffentliche Adressbereiche *oder* RFC1918-Adressräume (d.h. private Adressen) verwenden.  Um ein virtuelles Netzwerk mit einem öffentlichen Adressbereich zu verwenden, müssen Sie das VNET vorab erstellen.  Bei Auswahl eines bereits bestehenden VNET müssen Sie während der Erstellung der ASE ein neues Subnetz erstellen.  **Eine vorab erstelltes Subnetz kann nicht im Portal verwendet werden.  Sie können eine ASE mit einem bereits vorhandenen Subnetz erstellen, wenn Sie Ihre ASE mithilfe einer Resource Manager-Vorlage erstellen.**  Zum Erstellen einer App anhand einer Vorlage befolgen Sie die Angaben unter [Erstellen einer App Service-Umgebung aus einer Vorlage][ILBAseTemplate] und [Erstellen einer ILB-ASE mit Azure Resource Manager-Vorlagen][ASEfromTemplate].

### <a name="details"></a>Details
Eine ASE wird mit zwei Front-Ends und zwei Workern erstellt.  Die Front-Ends agieren als HTTP/HTTPS-Endpunkte und senden Datenverkehr an die Workers, d.h. die Rollen, die Ihre Apps hosten.   Sie können die Menge nach der Erstellung der ASE anpassen und sogar automatische Skalierungsregeln für diese Ressourcenpools einrichten.  Weitere Informationen zur manuellen Skalierung, Verwaltung und Überwachung von App Service-Umgebungen finden Sie unter [Konfigurieren einer App Service-Umgebung][ASEConfig]. 

Nur die eine ASE darf im Subnetz vorhanden sein, das von der ASE verwendet wird.  Das Subnetz kann für nichts anderes als die ASE verwendet werden.

### <a name="after-app-service-environment-creation"></a>Nach Erstellung einer App Service-Umgebung
Nach der Erstellung einer App Service-Umgebung können Sie die folgenden Einstellungen anpassen:

* Anzahl der Front-Ends (Minimum: 2)
* Anzahl der Worker (Minimum: 2)
* Menge der für IP-SSL verfügbaren IP-Adressen
* Größen der Compute-Ressourcen, die von den Front-Ends oder den Workern verwendet werden (Mindestgröße für Front-Ends ist P2)

Weitere Informationen zur manuellen Skalierung, Verwaltung und Überwachung von App Service-Umgebungen finden Sie unter [Konfigurieren einer App Service-Umgebung][ASEConfig]. 

Weitere Informationen zur automatischen Skalierung finden Sie in diesem Leitfaden: [Konfigurieren der automatischen Skalierung für eine App Service-Umgebung][ASEAutoscale].

Es gibt zusätzliche Abhängigkeiten, die nicht zur Anpassung zur Verfügung stehen, wie beispielsweise Datenbank und Speicher.  Diese werden von Azure verwaltet und sind im System enthalten.  Der Systemspeicher unterstützt bis zu 500 GB für die gesamte App Service-Umgebung, und die Datenbank wird von Azure nach Bedarf an die Staffelung des Systems angepasst.

## <a name="getting-started"></a>Erste Schritte
Alle Artikel und Anleitungen zu App Service-Umgebungen stehen in der [Dokumentation zur App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md)zur Verfügung.

Informationen zu den ersten Schritten mit App Service-Umgebungen finden Sie unter [Einführung in App Service-Umgebungen][WhatisASE].

Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/



<!--HONumber=Dec16_HO3-->


