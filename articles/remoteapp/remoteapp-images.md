---
title: Was enthalten Azure RemoteApp-Vorlagenimages? | Microsoft Docs
description: "Erfahren Sie mehr über die in Azure RemoteApp enthaltenen Vorlagenimages."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 348306795fd3c8275b21e4ec6dceae408916bf72
ms.lasthandoff: 03/31/2017


---
# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Was enthalten Azure RemoteApp-Vorlagenimages?
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Ihr Azure RemoteApp-Abonnement beinhaltet drei Vorlagenimages:

* Windows Server 2012
* Microsoft Office 365 ProPlus (Office 365-Abonnement erforderlich)
* Microsoft Office 2013 Professional Plus (nur Testversion)

> [!IMPORTANT]
> Ihr Azure RemoteApp-Abonnement ermöglicht den Zugriff auf die Software in den Images (mit Ausnahme von Office 365 ProPlus, für das ein eigenes Abonnement benötigt wird, und Office 2013, das nicht in der Produktionsumgebung verwendet werden kann). Dies bedeutet, dass Sie die Programme oder Apps auf den Vorlagenimages für Ihre Benutzer freigeben können. Wenn Sie eine Sammlung erstellen, die das Windows Server 2012 R2-Image verwendet, können Sie z. B. System Center Endpoint Protection für den Zugriff auf über RemoteApp veröffentlichen.
> 
> Weitere Informationen finden Sie unter [Details zur Lizenzierung in RemoteApp](remoteapp-licensing.md) . Informationen zur Office-Lizenzierung finden Sie unter [Verwenden von Office mit Azure RemoteApp](remoteapp-o365.md) .
> 
> 

Im Folgenden erhalten Sie ausführliche Informationen zum Inhalt der einzelnen Images.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 („einfaches Image“)
Dieses Image basiert auf dem Betriebssystem Microsoft Windows Server 2012 R2 Datacenter und weist die folgenden Rollen und Features auf, die zum Erfüllen der Anforderungen an Azure RemoteApp-Vorlagenimages installiert wurden:

* .NET Framework 4.5, 3.5.1 und 3.5
* Desktopdarstellung
* Freihand- und Handschriftdienste
* Media Foundation
* Remotedesktop-Sitzungshost
* Windows PowerShell 4.0
* Windows PowerShell ISE
* WoW64-Unterstützung

Auf diesem Image sind außerdem die folgenden Anwendungen installiert:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (Abonnement erforderlich)
Office 365 ist die am häufigsten angeforderte Anwendung; daher haben wir für Sie ein "benutzerdefiniertes" Image erstellt, mit dem Sie arbeiten können.

Dieses Image ist eine Erweiterung des einfachen Images und enthält neben den Komponenten, die für das Windows Server 2012 R2-Image beschrieben wurden, die folgenden Komponenten von Microsoft Office 365 ProPlus:

* Access
* Excel
* Lync
* OneNote
* OneDrive for Business (Synchronisierungs-Agent wird für die Verwendung mit Azure RemoteApp nicht unterstützt)
* Outlook
* PowerPoint
* Word
* Microsoft Office-Korrekturhilfen

Das Image umfasst außerdem Visio Pro und Project Pro.

Sowie die folgenden Anwendungen:

* SQL Native Client
* ODBC-Treiber
* SQL Server Data Mining-Client
* MasterDataServices-Client
* Microsoft Publisher
* PowerQuery
* PowerMap

Die vollständige Funktionalität der Office 365 ProPlus-Anwendungen ist nur für Benutzer mit einem Office 365 ProPlus-Plan verfügbar. Weitere Informationen zu Office 365-Abonnementplänen finden Sie unter [Office 365-Servicepläne](http://technet.microsoft.com/library/office-365-plan-options.aspx). Haben Sie noch Fragen? Prüfen Sie die Informationen unter [Office 365 + RemoteApp](remoteapp-o365.md) . Lesen Sie auch den neuen Artikel [Verwenden Ihres Office 365-Abonnements mit Azure RemoteApp](remoteapp-officesubscription.md).

Beachten Sie, dass Sie Office 365 ProPlus, Visio Pro und Project Pro separat lizenzieren müssen – sie verfügen jeweils über eine eigene Lizenz.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (nur Testversion)
Während des kostenlosen Testzeitraums können Sie den Dienst mit dem Office 2013-Image testen.

Dieses Image ist eine Erweiterung des einfachen Images und enthält neben den Komponenten, die für das Windows Server 2012 R2-Image beschrieben wurden, die folgenden Komponenten von Microsoft Office 2013 Professional Plus:

* Access
* Excel
* Lync
* OneNote
* OneDrive for Business (Synchronisierungs-Agent wird für die Verwendung mit Azure RemoteApp nicht unterstützt)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Microsoft Office-Korrekturhilfen

> [!IMPORTANT]
> **Rechtliche Hinweise:** Dieses Image enthält keine Microsoft Office-Lizenz und *kann nicht für die Produktion verwendet werden*. Das Image von Office 2013 Professional Plus dient nur zu Testzwecken. Wenn Sie Office-Apps in Azure RemoteApp für die Produktion verwenden möchten, müssen Sie das Office 365 ProPlus-Image verwenden. Weitere Informationen zur Office-Lizenzierung finden Sie unter [Verwenden von Office 365 mit Azure RemoteApp](remoteapp-o365.md)
> 
> 


