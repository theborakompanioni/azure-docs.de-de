---
title: "Aktivieren von HTTPS für eine benutzerdefinierte Azure CDN-Domäne | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie HTTPS für Ihren Azure CDN-Endpunkt mit einer benutzerdefinierten Domäne aktivieren."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.lasthandoff: 03/04/2017


---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Aktivieren von HTTPS für eine benutzerdefinierte Azure CDN-Domäne

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Dank der HTTPS-Unterstützung für benutzerdefinierte Azure CDN-Domänen können Sie unter Verwendung Ihres eigenen Domänennamens sichere Inhalte über SSL bereitstellen und so die Datensicherheit bei der Übertragung verbessern. Der End-to-End-Workflow zur Aktivierung von HTTPS für Ihre benutzerdefinierte Domäne wird durch die Möglichkeit zur Aktivierung mit nur einem Klick sowie durch eine umfassende Zertifikatverwaltung vereinfacht – alles ohne zusätzliche Kosten.

Während der Datenübertragung müssen Schutz und Integrität sämtlicher sensibler Webanwendungsdaten unbedingt gewährleistet sein. Die Verwendung des HTTPS-Protokolls stellt sicher, dass Ihre sensible Daten verschlüsselt sind, wenn sie über das Internet gesendet werden. Das Protokoll sorgt für Vertrauen und bietet Authentifizierung und Angriffsschutz für Ihre Webanwendungen. Azure CDN unterstützt HTTPS derzeit für einen CDN-Endpunkt. Wenn Sie also etwa über Azure CDN einen CDN-Endpunkt erstellen (beispielsweise „https://contoso.azureedge.net“), wird HTTPS standardmäßig aktiviert. Mit HTTPS für benutzerdefinierte Domänen können Sie nun auch eine sichere Bereitstellung für eine benutzerdefinierte Domäne (etwa „https://www.contoso.com“) aktivieren. 

Zu den zentralen Attributen des HTTPS-Features zählen unter anderem folgende:

- Keine zusätzliche Kosten: Zertifikatabruf und -verlängerung sind kostenlos, und für HTTPS-Datenverkehr fallen keine zusätzlichen Kosten an. Sie zahlen nur für den ausgehenden Datenverkehr des CDNs (nach GB).

- Unkomplizierte Aktivierung: Über das [Azure-Portal](https://portal.azure.com) ist die Bereitstellung mit einem einzelnen Klick möglich. Das Feature kann aber auch per REST-API oder mithilfe anderer Entwicklertools aktiviert werden.

- Umfassende Zertifikatverwaltung: Die gesamte Zertifikatbeschaffung und -verwaltung wird für Sie erledigt. Zertifikate werden automatisch bereitgestellt und vor Ablauf verlängert. Dadurch ist gewährleistet, dass keine Dienstunterbrechungen aufgrund eines abgelaufenen Zertifikats auftreten.

>[!NOTE] 
>Vor der Aktivierung der HTTPS-Unterstützung muss bereits eine [benutzerdefinierte Azure CDN-Domäne](./cdn-map-content-to-custom-domain.md) eingerichtet worden sein.

## <a name="step-1-enabling-the-feature"></a>Schritt 1: Aktivieren des Features 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Standard- oder Premium-Verizon-CDN-Profil.

2. Klicken Sie in der Liste mit den Endpunkten auf den Endpunkt, die Ihre benutzerdefinierte Domäne enthält.

3. Klicken Sie auf die benutzerdefinierte Domäne, für die Sie HTTPS aktivieren möchten.

    ![Blatt „Endpunkt“](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klicken Sie auf **Ein**, um HTTPS zu aktivieren, und speichern Sie die Änderung.

    ![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>Schritt 2: Überprüfen der Domäne

>[!IMPORTANT] 
>HTTPS wird für Ihre benutzerdefinierte Domäne erst nach Abschluss der Domänenüberprüfung aktiviert. Die Genehmigung der Domäne muss innerhalb von sechs Werktagen erfolgen. Liegt nach sechs Werktagen keine Genehmigung vor, wird die Anforderung storniert.  

Nach der Aktivierung von HTTPS für Ihre benutzerdefinierte Domäne überprüft DigiCert (unser HTTPS-Zertifikatanbieter), ob die Domäne wirklich Ihnen gehört. Hierzu setzt sich DigiCert entweder per E-Mail (Standardverfahren) oder telefonisch mit dem Registrant für Ihre Domäne in Verbindung, der in den WHOIS-Registranteninformationen angegeben ist. DigiCert sendet die Bestätigungs-E-Mail an unten stehende Adressen. Falls die WHOIS-Registranteninformationen privat sind, müssen Sie sicherstellen, dass Sie die Bestätigung direkt aus einer dieser Adressen vornehmen können.

>admin@<ihr-domänenname.com> administrator@<ihr-domänenname.com>  
>webmaster@<ihr-domänenname.com>  
>hostmaster@<ihr-domänenname.com>  
>postmaster@<ihr-domänenname.com>


Nach Erhalt der E-Mail haben Sie zwei Optionen:

1. Sie können alle zukünftigen Aufträge genehmigen, die über das gleiche Konto für die gleiche Stammdomäne (beispielsweise „contoso.com“) getätigt werden. Diese Vorgehensweise empfiehlt sich, wenn Sie später weitere benutzerdefinierte Domänen für die gleiche Stammdomäne hinzufügen möchten.
 
2. Sie können lediglich den spezifischen Hostnamen aus dieser Anforderung genehmigen. Für nachfolgende Anforderungen wird eine zusätzliche Genehmigung benötigt.

    Beispiel-E-Mail:
    
    ![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/domain-validation-email-example.png)

Nach der Genehmigung fügt DigiCert Ihren benutzerdefinierten Domänennamen dem SAN-Zertifikat hinzu. Das Zertifikat ist ein Jahr lang gültig und wird automatisch verlängert, bevor es abläuft.

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a>Schritt 3: Verwenden des Features nach Abschluss der Verteilung

Nach der Überprüfung des Domänennamens dauert es bis zu sechs bis acht Stunden, bis das HTTPS-Feature für die benutzerdefinierte Domäne aktiv ist. Nach Abschluss des Prozesses wird der Status „HTTPS für benutzerdefinierte Domänen“ im Azure-Portal auf „Aktiviert“ festgelegt. Ab diesem Zeitpunkt können Sie HTTPS für Ihre benutzerdefinierte Domäne verwenden.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. *Wer ist der Zertifikatanbieter, und welche Art von Zertifikat wird verwendet?*

    Wir verwenden SAN-Zertifikate (Subject Alternative Names; alternative Antragstellernamen) von DigiCert. Ein SAN-Zertifikat kann mehrere vollqualifizierte Domänennamen mit einem einzelnen Zertifikat schützen.

2. *Kann ich mein dediziertes Zertifikat verwenden?*
    
    Noch nicht, die Option ist aber geplant.

3. *Was passiert, wenn ich die Domänenüberprüfungs-E-Mail von DigiCert nicht erhalte?*

    Sollten Sie nach 24 Stunden noch keine E-Mail erhalten haben, setzen Sie sich mit Microsoft in Verbindung.

4. *Ist ein SAN-Zertifikat weniger sicher als ein dediziertes Zertifikat?*
    
    Ein SAN-Zertifikat bietet die gleichen Verschlüsselungs- und Sicherheitsstandards wie ein dediziertes Zertifikat. Alle ausgestellten SSL-Zertifikate verwenden SHA-256 für verbesserte Serversicherheit.

5. *Kann ich HTTPS für benutzerdefinierte Domänen mit Azure CDN von Akamai verwenden?*

    Dieses Feature ist derzeit nur mit Azure CDN von Verizon verfügbar. Wir arbeiten daran, dieses Feature in den nächsten Monaten auch für Azure CDN von Akamai zu unterstützen.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, wie Sie eine [benutzerdefinierte Domäne für Ihren Azure CDN-Endpunkt](./cdn-map-content-to-custom-domain.md) einrichten.



