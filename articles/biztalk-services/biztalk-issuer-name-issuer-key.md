---
title: "Ausstellername und Ausstellerschlüssel in BizTalk Services | Microsoft Docs"
description: "Erfahren Sie, wie Sie den Ausstellernamen und -schlüssel für Service Bus oder Access Control (ACS) in BizTalk Services abrufen. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 71f9dd111ebdbe885f33d162b2ea320dfaa167bb
ms.openlocfilehash: 4fb13a158c660105a5fc8f79a92c67ba65c5356d


---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Name und Schlüssel des Ausstellers
Azure BizTalk Services verwendet Ausstellername und -schlüssel von Service Bus und Access Control. Dies gilt insbesondere in folgenden Fällen:

| Task | Welcher Ausstellername und -schlüssel? |
| --- | --- |
| Bereitstellen einer Anwendung in Visual Studio |Access Control-Ausstellername und -schlüssel |
| Konfigurieren des Azure BizTalk Services-Portals |Access Control-Ausstellername und -schlüssel |
| Erstellen von LOB-Relays mit den BizTalk Adapterdiensten in Visual Studio |Service Bus-Ausstellername und -schlüssel |

Dieses Thema beschreibt die notwendigen Schritte zum Abrufen von Ausstellername und -schlüssel. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Access Control-Ausstellername und -schlüssel
Access Control-Ausstellername und -schlüssel werden von den folgenden Komponenten verwendet:

* Ihre in Visual Studio erstellte Azure BizTalk Service-Anwendung: Um Ihre BizTalk Service-Anwendung in Visual Studio für Azure bereitzustellen, müssen Sie Access Control-Ausstellername und -schlüssel eingeben. 
* Das Azure BizTalk Services-Portal: Wenn Sie einen BizTalk Service erstellen und das BizTalk Services-Portal öffnen, werden Ihr Access Control-Ausstellername und -schlüssel automatisch für Ihre Bereitstellungen mit den gleichen Access Control-Werten registriert.

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a>Abrufen des Access Control-Ausstellernamens und -schlüssels

Um ACS für die Authentifizierung zu verwenden und die Werte für Ausstellernamen und Ausstellerschlüssel abzurufen, müssen folgende Schritte ausgeführt werden:

1. Installieren Sie die [Azure PowerShell-Cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Fügen Sie Ihr Azure-Konto hinzu: `Add-AzureAccount`
3. Geben Sie den Abonnementnamen an zurück: `get-azuresubscription`
4. Wählen Sie Ihr Abonnement aus: `select-azuresubscription <name of your subscription>` 
5. Erstellen Sie einen neuen Namespace: `new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Beispiel:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. Wenn der neue ACS-Namespace erstellt wird (dies kann mehrere Minuten dauern), sind die Werte für Ausstellername und Ausstellerschlüssel in der Verbindungszeichenfolge aufgeführt: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

Zusammenfassung:  
Ausstellername = SharedSecretIssuer  
Ausstellerschlüssel = SharedSecretKey

Weitere Informationen zum [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx)-Cmdlet. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Service Bus-Ausstellername und -schlüssel
Service Bus-Ausstellername und -schlüssel werden von BizTalk-Adapterdiensten verwendet. In Ihrem BizTalk Services-Projekt in Visual Studio können Sie die BizTalk-Adapterdienste verwenden, um sich mit einem lokalen LOB-System (Line of Business) zu verbinden. Erstellen Sie dazu das LOB-Relay und geben Sie Ihre LOB-Systemdetails ein. Bei diesem Prozess geben Sie ebenfalls Ihren Service Bus-Ausstellernamen und -schlüssel ein.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Abrufen von Service Bus-Ausstellername und -schlüssel
1. Melden Sie sich am [klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Klicken Sie im linken Navigationsbereich auf **Service Bus**.
3. Wählen Sie Ihren Namespace aus. Klicken Sie in der Taskleiste auf **Verbindungsinformationen**. Daraufhin werden **Standardaussteller** (Ausstellername) und **Standardschlüssel** (Ausstellerschlüssel) angezeigt. Diese Werte können kopiert werden.  

Zusammenfassung:  
Ausstellername = Standardaussteller  
Ausstellerschlüssel = Standardschlüssel

## <a name="next"></a>Weiter
Zusätzliche Azure BizTalk Services-Themen:

* [Installieren des Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Lernprogramme: Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Wie verwende ich das Azure BizTalk Services SDK?](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [BizTalk Services-Dokumentation](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Weitere Informationen
* [Verwenden des ACS-Verwaltungsdiensts zum Konfigurieren von Dienstidentitäten](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk Services: Editionsübersicht](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Bereitstellen mithilfe des klassischen Azure-Portals](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Bereitstellungsstatusübersicht](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Registerkarten "Dashboard", "Überwachen" und "Skalieren"](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Sichern und Wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Drosselung](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>




<!--HONumber=Nov16_HO3-->


