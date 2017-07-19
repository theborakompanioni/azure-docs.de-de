---
title: Erstellen einer externen Azure App Service-Umgebung
description: "Es wird beschrieben, wie Sie beim Erstellen einer App oder bei einem eigenständigen Vorgang eine Azure App Service-Umgebung erstellen."
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 3f1418b71a7327264e29e3f08ef97b254ee9930d
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="create-an-external-app-service-environment"></a>Erstellen einer externen App Service-Umgebung #

Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz in Ihrem Azure Virtual Network (VNet). Es gibt zwei Möglichkeiten, eine ASE bereitzustellen:

- Mit einer VIP unter einer externen IP-Adresse, die häufig als _externe ASE_ bezeichnet wird.
- Mit der VIP unter einer internen IP-Adresse, die häufig als _ILB-ASE_ bezeichnet wird, da der interne Endpunkt ein interner Load Balancer (ILB) ist.

In diesem Artikel wird gezeigt, wie Sie eine externe ASE erstellen. Eine erste Übersicht über die ASE können Sie sich unter [Introduction to the App Service Environment][Intro] (Einführung in die App Service-Umgebung) verschaffen. Ausführliche Informationen zur Erstellung einer ILB-ASE finden Sie unter [Create and Use an Internal Load Balancer with an App Service Environment][MakeILBASE] (Erstellen und Verwenden eines internen Load Balancers mit einer App Service-Umgebung).

## <a name="before-you-create-your-ase"></a>Bevor Sie Ihre ASE erstellen ##

Es ist wichtig, dass Sie wissen, welche Dinge Sie nach der ASE-Erstellung nicht ändern können. Dies ist Folgendes:

- Ort
- Abonnement
- Ressourcengruppe
- Verwendetes virtuelles Netzwerk
- Verwendetes Subnetz
- Subnetzgröße

> [!NOTE]
> Stellen Sie beim Auswählen eines virtuellen Netzwerks und Angeben eines Subnetzes sicher, dass die Größe in Bezug auf das zukünftige Wachstum ausreichend hoch gewählt ist. Die empfohlene Größe ist `/25` mit 128 Adressen.
>

## <a name="three-ways-to-create-an-ase"></a>Drei Möglichkeiten zum Erstellen einer ASE ##

Es gibt drei Möglichkeiten, wie Sie eine ASE erstellen können. Sie können eine ASE wie folgt erstellen:

- Beim Erstellen eines App Service-Plans, wobei die ASE und der App Service-Plan in einem Schritt erstellt werden.
- Über die eigenständige Benutzeroberfläche für die ASE-Erstellung, wobei eine ASE ohne Inhalte erstellt wird. Dies ist eine erweiterte Benutzeroberfläche für die ASE-Erstellung, die Sie verwenden, um eine ASE mit einem internen Load Balancer (ILB) zu erstellen.
- Mit einer Resource Manager-Vorlage. Dieser Ansatz ist für fortgeschrittene Benutzer bestimmt. Informationen hierzu finden Sie unter [How To Create an ASE Using Azure Resource Manager Templates][MakeASEfromTemplate] (Erstellen einer ASE mit Azure Resource Manager-Vorlagen).

Eine ASE, die ohne ILB erstellt wird, verfügt über eine öffentliche VIP. Dies bedeutet, dass der gesamte HTTP-Datenverkehr an die Apps in der ASE über eine IP-Adresse verläuft, die über das Internet zugänglich ist. Eine ASE mit einem ILB verfügt über einen Endpunkt unter der IP-Adresse des virtuellen Netzwerks. Diese Apps werden nicht direkt im Internet verfügbar gemacht.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Gemeinsames Erstellen einer ASE und eines App Service-Plans ##

Der App Service-Plan ist ein Container mit Apps. Wenn Sie im App Service eine App erstellen, müssen Sie immer einen App Service-Plan auswählen oder erstellen. Das Containermodell lautet: Umgebungen enthalten App Service-Pläne, und App Service-Pläne enthalten Apps.

Gehen Sie wie folgt vor, um während der Erstellung eines App Service-Plans eine ASE zu erstellen:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu &gt; Web und mobil &gt; Web-App**.

    ![][1]
1. Wählen Sie Ihr Abonnement aus. Beachten Sie bei mehreren Abonnements, dass Sie zum Erstellen einer App in Ihrer ASE dasselbe Abonnement verwenden müssen, das Sie beim Erstellen der ASE verwendet haben.
1. Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie sie. *Ressourcengruppen* ermöglichen Ihnen, verwandte Azure-Ressourcen als Einheit zu verwalten, und sind hilfreich beim Einrichten von *RBAC-Regeln* (Role-Based Access Control, rollenbasierte Zugriffssteuerung) für Ihre Apps. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager][ARMOverview].
1. Klicken Sie auf den App Service-Plan, und wählen Sie anschließend die Option **Neu erstellen**.

    ![][2]
1. Wählen Sie in der Dropdownliste **Standort** die Region aus, in der die ASE erstellt werden soll. Wenn Sie eine vorhandene ASE wählen, ist keine Erstellung einer neuen ASE möglich, sondern der App Service-Plan wird einfach in der gewählten ASE erstellt.
1. Klicken Sie auf die Benutzeroberfläche für den **Tarif**, und wählen Sie eine der Preis-SKUs vom Typ **Isoliert**. Die Auswahl einer SKU-Karte vom Typ **Isoliert** und eines Standorts, bei dem es sich nicht um eine ASE handelt, bedeutet, dass Sie an diesem Standort eine neue ASE erstellen möchten. Bei diesem Vorgang wird die Benutzeroberfläche für die ASE-Erstellung angezeigt, nachdem Sie auf der Seite mit den Preisen auf **Auswählen** geklickt haben. Die SKU **Isoliert** ist nur zusammen mit einer ASE verfügbar. Es ist außerdem nicht möglich, in einer ASE eine andere Preis-SKU als **Isoliert** zu verwenden.

    ![][3]
1. Geben Sie den Namen für Ihre ASE ein. Der Name Ihrer ASE wird im aufrufbaren Namen für Ihre Apps verwendet. Wenn der Name der App Service-Umgebung _appsvcenvdemo_ ist, lautet der Unterdomänenname *.appsvcenvdemo.p.azurewebsites.net*. Wenn Sie dann eine App namens *mytestapp* erstellen, kann sie unter der Adresse *mytestapp.appsvcenvdemo.p.azurewebsites.net* aufgerufen werden. Sie dürfen im Namen Ihrer ASE keine Leerzeichen verwenden. Bei Verwendung von Großbuchstaben im Namen wird der entsprechende Domänenname dennoch vollständig in Kleinbuchstaben geschrieben.

    ![][4]
1. Wählen Sie entweder **Neu erstellen** oder **Vorhandene auswählen**. Die Option zum Auswählen eines vorhandenen virtuellen Netzwerks ist nur verfügbar, wenn in der gewählten Region ein virtuelles Netzwerk vorhanden ist. Bei Auswahl von **Neu erstellen** geben Sie einen Namen für das virtuelle Netzwerk an. Ein neues virtuelles Resource Manager-Netzwerk mit diesem Namen und dem Adressraum `192.168.250.0/23` wird in der ausgewählten Region erstellt. Bei Auswahl von **Vorhandene auswählen** gehen Sie wie folgt vor:
    1. Wählen Sie den Adressblock des virtuellen Netzwerks aus, falls Sie mehr als einen verwenden.
    2. Geben Sie einen neuen Subnetznamen an.
    3. Wählen Sie die Größe des Subnetzes aus. **Erinnerung: Dieser Wert sollte hoch genug gewählt werden, um das zukünftige Wachstum Ihrer ASE abzudecken.** Die empfohlene Größe ist `/25` mit 128 Adressen zur Verarbeitung einer ASE maximaler Größe. `/28` ist beispielsweise nicht zu empfehlen, da nur 16 Adressen verfügbar sind. Für Infrastrukturanforderungen werden mindestens fünf Adressen benötigt, sodass Sie in einem Subnetz vom Typ `/28` nur noch über einen maximalen Skalierungsbereich von elf Instanzen verfügen.
    4. Wählen Sie den IP-Bereich des Subnetzes aus.

Der ASE-Erstellungsprozess beginnt, nachdem Sie die Option **Erstellen** gewählt haben. Außerdem werden der App Service-Plan und die App erstellt. Die ASE, der App Service-Plan und die App sind alle unter demselben Abonnement und auch in derselben Ressourcengruppe angeordnet. Falls es für Sie erforderlich ist, dass Ihre ASE in einer anderen Ressourcengruppe als der App Service-Plan und die App angeordnet sind, oder falls Sie eine ILB-ASE benötigen, sollten Sie die Optionen zum Erstellen einer eigenständigen ASE verwenden.

## <a name="create-an-ase-by-itself"></a>Erstellen einer eigenständigen ASE ##

Wenn der Ablauf zum Erstellen der eigenständigen ASE durchlaufen wird, wird eine ASE ohne Inhalte erstellt. Für eine leere ASE fällt trotzdem eine monatliche Gebühr für die Infrastruktur an. Die Hauptgründe zum Durchlaufen dieses Workflows sind die Erstellung einer ASE mit einem ILB oder einer ASE in ihrer eigenen Ressourcengruppe. Nach dem Erstellen Ihrer ASE können Sie Apps in der ASE erstellen, indem Sie die normale Benutzeroberfläche für die App-Erstellung verwenden und Ihre neue ASE als Standort wählen.

Greifen Sie auf die Benutzeroberfläche für die ASE-Erstellung zu, indem Sie den Azure Marketplace nach ***App Service-Umgebung*** durchsuchen oder „Neu“ &gt; „Web und mobil“ &gt; „App Service-Umgebung“ verwenden. Gehen Sie wie folgt vor, um eine ASE mit der Benutzeroberfläche für die eigenständige Erstellung zu erstellen:

1. Geben Sie den Namen Ihrer ASE an. Der Name, der für die ASE angegeben wird, wird für die in der ASE erstellten Apps verwendet. Wenn der Name der ASE *mynewdemoase* lautet, ist der Name der Unterdomäne *.mynewdemoase.p.azurewebsites.net*. Wenn Sie dann eine App namens *mytestapp* erstellen, kann sie unter der Adresse *mytestapp.mynewdemoase.p.azurewebsites.net* aufgerufen werden. Sie dürfen im Namen Ihrer ASE keine Leerzeichen verwenden. Bei Verwendung von Großbuchstaben im Namen wird der entsprechende Domänenname dennoch vollständig in Kleinbuchstaben geschrieben. Bei Verwendung eines ILB wird Ihr ASE-Name nicht in Ihrer Unterdomäne verwendet, sondern stattdessen explizit während der ASE-Erstellung angegeben.

    ![][5]
1.  Wählen Sie Ihr Abonnement aus. Das für Ihre ASE verwendete Abonnement ist auch dasjenige, in dem alle Apps in dieser ASE erstellt werden. Sie können Ihre ASE nicht in einem virtuellen Netzwerk platzieren, das sich in einem anderen Abonnement befindet.
1.  Wählen eine neue Ressourcengruppe aus, oder geben Sie eine an. Die Ressourcengruppe, die für Ihre ASE verwendet wird, muss identisch mit derjenigen sein, die für das virtuelle Netzwerk verwendet wird. Wenn Sie ein vorhandenes virtuelles Netzwerk auswählen, wird die Auswahl der Ressourcengruppe für Ihre ASE entsprechend den Angaben für Ihr virtuelles Netzwerk aktualisiert.

    ![][6]
1. Treffen Sie Ihre Auswahl für „Virtuelles Netzwerk“ und „Speicherort“. Sie können wählen, ob Sie ein neues virtuelles Netzwerk oder ein vorhandenes virtuelles Netzwerk erstellen möchten. Wenn Sie ein neues virtuelles Netzwerk auswählen, können Sie einen Namen und Speicherort angeben. Das neue virtuelle Netzwerk hat den Adressbereich 192.168.250.0/23 und ein Subnetz namens **default**, das als 192.168.250.0/24 definiert ist. Sie können nur ein virtuelles Resource Manager-Netzwerk auswählen. Die Auswahl des **VIP-Typs** bestimmt, ob auf Ihre ASE ein direkter Zugriff aus dem Internet möglich ist (extern) oder ob ein interner·Load Balancer (ILB) verwendet wird. Mehr hierzu erfahren Sie unter [Create and Use an Internal Load Balancer with an App Service Environment][MakeILBASE] (Erstellen und Verwenden eines internen Load Balancers mit einer App Service-Umgebung). Wenn Sie einen VIP-Adresstyp des Typs **Extern** auswählen, können Sie angeben, mit wie vielen externen IP-Adressen das System für IP-basierte SSL-Zwecke erstellt wird. Bei Auswahl von **Intern** müssen Sie die Unterdomäne angeben, die Ihre ASE verwenden soll. ASEs können in virtuellen Netzwerken bereitgestellt werden, die *entweder* öffentliche Adressbereiche *oder* RFC1918-Adressräume (d.h. private Adressen) verwenden. Um ein virtuelles Netzwerk mit einem öffentlichen Adressbereich zu verwenden, müssen Sie das virtuelle Netzwerk vorab erstellen. Bei Auswahl eines vorhandenen virtuellen Netzwerks müssen Sie während der Erstellung der ASE ein neues Subnetz erstellen. **Eine vorab erstelltes Subnetz kann nicht im Portal verwendet werden. Sie können eine ASE mit einem vorhandenen Subnetz erstellen, wenn Sie Ihre ASE mithilfe einer Resource Manager-Vorlage erstellen.** Informationen zum Erstellen einer ASE mit einer Vorlage finden Sie unter [Erstellen einer ASE mit Azure Resource Manager-Vorlagen][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service-Umgebung v1 ##

Sie können weiterhin Instanzen der ersten Version des ASE-Features (ASEv1) erstellen. Sie gelangen zu dieser Benutzeroberfläche, indem Sie im Marketplace nach **App Service-Umgebung v1** suchen. Die Benutzeroberfläche für die Erstellung entspricht der Benutzeroberfläche für die Erstellung der eigenständigen ASE. Nach Abschluss des Vorgangs wird Ihre ASEv1 mit zwei Front-Ends und zwei Workern erstellt. Bei ASEv1 müssen Sie die Front-Ends und Worker verwalten. Diese werden beim Erstellen Ihrer App Service-Pläne nicht automatisch hinzugefügt. Die Front-Ends agieren als HTTP/HTTPS-Endpunkte und senden Datenverkehr an die Workers. Dies sind die Rollen, die Ihre Apps hosten. Sie können die Menge nach der ASE-Erstellung anpassen. Weitere Informationen zu ASEv1 finden Sie unter [Einführung in die App Service-Umgebung v1][ASEv1Intro]. Weitere Informationen zur Skalierung, Verwaltung und Überwachung von ASEv1 finden Sie unter [Konfigurieren einer App Service-Umgebung][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png



<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md

