---
title: "Migrieren einer aktiven benutzerdefinierten Domäne zu Azure App Service | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine benutzerdefinierte Domäne, die bereits einer Livewebsite zugewiesen ist, ohne Ausfallzeiten zu Ihrer App in Azure App Service migrieren."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: 5f6537a45bcb092b5ef463e8069b9fc5582c14c2
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---
# <a name="migrate-an-active-custom-domain-to-azure-app-service"></a>Migrieren einer aktiven benutzerdefinierten Domäne zu Azure App Service

In diesem Artikel wird erläutert, wie Sie eine aktive benutzerdefinierte Domäne ohne Ausfallzeiten zu [Azure App Service](../app-service/app-service-value-prop-what-is.md) migrieren.

Wenn Sie eine Livewebsite und deren Domänenname zu App Service migrieren, wird über diesen Domänennamen bereits Livedatenverkehr abgewickelt, und Sie möchten während des Migrationsprozesses natürlich keine Ausfallzeiten bei der DNS-Auflösung. In diesem Fall müssen Sie den Domänennamen bereits im Vorfeld zur Verifizierung der Domäne an Ihre Azure-App binden.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie bereits wissen, wie [eine benutzerdefinierte Domäne manuell App Service zugeordnet wird](app-service-web-tutorial-custom-domain.md).

## <a name="bind-the-domain-name-preemptively"></a>Binden des Domänennamens bereits im Vorfeld

Wenn Sie eine benutzerdefinierte Domäne bereits im Vorfeld binden, haben Sie die folgenden beiden Vorgänge ausgeführt, bevor Sie Änderungen an Ihren DNS-Einträgen vornehmen:

- Überprüfen des Domänenbesitzes
- Aktivieren des Domänennamens für Ihre App

Wenn Sie schließlich den DNS-Eintrag so ändern, dass er auf Ihre App Service-App verweist, werden Clients ohne Ausfallzeiten bei der DNS-Auflösung von Ihrer alten Website zu Ihrer App Service-App umgeleitet.

Führen Sie die folgenden Schritte aus:

1. Erstellen Sie zunächst einen TXT-Überprüfungseintrag mit Ihrer DNS-Registrierung, wie unter [Erstellen von DNS-Einträgen](app-service-web-tutorial-custom-domain.md) beschrieben.
Für Ihren zusätzlichen TXT-Eintrag wird die Konvention der Zuordnung von „&lt;*Unterdomäne*>.&lt;*Stammdomäne*>“ zu „&lt;*App-Name*>.azurewebsites.net“ übernommen.
Die folgende Tabelle enthält einige Beispiele:  

    <table cellspacing="0" border="1">
    <tr>
    <th>FQDN-Beispiel</th>
    <th>TXT-Host</th>
    <th>TXT-Wert</th>
    </tr>
    <tr>
    <td>contoso.com (Stammdomäne)</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
    </tr>
    <tr>
    <td>www.contoso.com (Unterdomäne)</td>
    <td>awverify.www.contoso.com</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
    </tr>
    <tr>
    <td>\*.contoso.com (Platzhalter)</td>
    <td>awverify\*.contoso.com</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
    </tr>
    </table>

2. Fügen Sie anschließend Ihrer Azure-App den benutzerdefinierten Domänennamen hinzu, wie unter [Aktivieren des benutzerdefinierten Domänennamens für Ihre App](app-service-web-tutorial-custom-domain.md#enable-a) beschrieben.

    Ihre benutzerdefinierte Domäne ist nun in Ihrer Azure-App aktiviert. Nun müssen Sie nur noch den DNS-Eintrag bei Ihrer Domänenregistrierungsstelle aktualisieren.

3. Aktualisieren Sie abschließend den DNS-Eintrag Ihrer Domäne, sodass dieser auf Ihre Azure-App verweist, wie in [Erstellen von DNS-Einträgen](app-service-web-tutorial-custom-domain.md) beschrieben.

    Der Benutzerdatenverkehr sollte umgehend nach der DNS-Verteilung an Ihre Azure-App umgeleitet werden.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie Ihren benutzerdefinierten Domänennamen mit HTTPS sichern, indem Sie [ein SSL-Zertifikat in Azure erwerben](web-sites-purchase-ssl-web-site.md) oder [ein SSL-Zertifikat von einer anderen Stelle verwenden](app-service-web-tutorial-custom-ssl.md).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
>
>

[Erste Schritte mit Azure DNS](../dns/dns-getstarted-create-dnszone.md)  
[Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](../dns/dns-web-sites-custom-domain.md)  
[Delegieren von Domänen an Azure DNS](../dns/dns-domain-delegation.md)

