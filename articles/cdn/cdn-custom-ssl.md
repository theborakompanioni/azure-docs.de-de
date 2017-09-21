---
title: "Aktivieren oder Deaktivieren von HTTPS in einer benutzerdefinierten Azure Content Delivery Network-Domäne | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie HTTPS für Ihren Azure CDN-Endpunkt mit einer benutzerdefinierten Domäne aktivieren oder deaktivieren."
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
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c92f1e20acf55b8bd791fad43f17e162a5cb3847
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="enable-or-disable-https-on-an-azure-content-delivery-network-custom-domain"></a>Aktivieren oder Deaktivieren von HTTPS in einer benutzerdefinierten Azure Content Delivery Network-Domäne

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Dank der HTTPS-Unterstützung für benutzerdefinierte CDN-Domänen (Microsoft Azure Content Delivery Network) können Sie unter Verwendung Ihres eigenen Domänennamens sichere Inhalte über SSL bereitstellen und so die Datensicherheit bei der Übertragung verbessern. Der End-to-End-Workflow zur Aktivierung von HTTPS für Ihre benutzerdefinierte Domäne wird durch die Möglichkeit zur Aktivierung mit nur einem Klick sowie durch eine umfassende Zertifikatverwaltung vereinfacht – alles ohne zusätzliche Kosten.

Während der Datenübertragung müssen Schutz und Integrität sämtlicher sensibler Webanwendungsdaten unbedingt gewährleistet sein. Die Verwendung des HTTPS-Protokolls stellt sicher, dass Ihre sensible Daten verschlüsselt sind, wenn sie über das Internet gesendet werden. Das Protokoll sorgt für Vertrauen und bietet Authentifizierung und Angriffsschutz für Ihre Webanwendungen. Azure CDN unterstützt HTTPS derzeit für einen CDN-Endpunkt. Wenn Sie also etwa über Azure CDN einen CDN-Endpunkt erstellen (beispielsweise „https://contoso.azureedge.net“), wird HTTPS standardmäßig aktiviert. Mit HTTPS für benutzerdefinierte Domänen können Sie nun auch eine sichere Bereitstellung für eine benutzerdefinierte Domäne (z.B. „https://www.contoso.com“) aktivieren. 

Zu den zentralen Attributen des HTTPS-Features zählen unter anderem folgende:

- Keine zusätzliche Kosten: Zertifikatabruf und -verlängerung sind kostenlos, und für HTTPS-Datenverkehr fallen keine zusätzlichen Kosten an. Sie zahlen nur für den ausgehenden Datenverkehr des CDNs (nach GB).

- Unkomplizierte Aktivierung: Über das [Azure-Portal](https://portal.azure.com) ist die Bereitstellung mit einem einzelnen Klick möglich. Das Feature kann aber auch per REST-API oder mithilfe anderer Entwicklertools aktiviert werden.

- Umfassende Zertifikatverwaltung: Die gesamte Zertifikatbeschaffung und -verwaltung wird für Sie erledigt. Zertifikate werden automatisch bereitgestellt und vor Ablauf verlängert. Dadurch ist gewährleistet, dass keine Dienstunterbrechungen aufgrund eines abgelaufenen Zertifikats auftreten.

>[!NOTE] 
>Vor der Aktivierung der HTTPS-Unterstützung muss bereits eine [benutzerdefinierte Azure CDN-Domäne](./cdn-map-content-to-custom-domain.md) eingerichtet worden sein.

## <a name="enabling-https"></a>Aktivieren von HTTPS

Gehen Sie folgendermaßen vor, um HTTPS zu aktivieren:

### <a name="step-1-enable-the-feature"></a>Schritt 1: Aktivieren des Features 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Standard- oder Premium-Verizon-CDN-Profil.

2. Klicken Sie in der Liste mit den Endpunkten auf den Endpunkt, die Ihre benutzerdefinierte Domäne enthält.

3. Klicken Sie auf die benutzerdefinierte Domäne, für die Sie HTTPS aktivieren möchten.

    ![Blatt „Endpunkt“](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klicken Sie auf **Ein**, um HTTPS zu aktivieren, und klicken Sie dann auf **Anwenden**.

    ![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>Schritt 2: Überprüfen der Domäne

>[!IMPORTANT] 
>HTTPS wird für Ihre benutzerdefinierte Domäne erst nach Abschluss der Domänenüberprüfung aktiviert. Die Genehmigung der Domäne muss innerhalb von sechs Werktagen erfolgen. Anforderungen, die nicht innerhalb von sechs Arbeitstagen genehmigt werden, werden automatisch abgebrochen. 

Nach der Aktivierung von HTTPS für Ihre benutzerdefinierte Domäne überprüft DigiCert (unser HTTPS-Zertifikatanbieter), ob die Domäne wirklich Ihnen gehört. Hierzu setzt sich DigiCert mit dem Registrant für Ihre Domäne, der in den [WHOIS](http://whois.domaintools.com/)-Registranteninformationen angegeben ist, in Verbindung. Der Kontakt wird über die E-Mail-Adresse (Standardverfahren) oder die Telefonnummer in der WHOIS-Registrierung hergestellt. 

![WHOIS-Datensatz](./media/cdn-custom-ssl/whois-record.png)

Außerdem sendet DigiCert die Bestätigungs-E-Mail an folgende Adressen. Falls die WHOIS-Registranteninformationen privat sind, müssen Sie überprüfen, ob Sie die Bestätigung direkt über eine dieser Adressen vornehmen können:

admin@&lt;Ihr-Domänenname.com&gt;  
administrator@&lt;Ihr-Domänenname.com&gt;  
webmaster@&lt;Ihr-Domänenname.com&gt;  
hostmaster@&lt;Ihr-Domänenname.com&gt;  
postmaster@&lt;Ihr-Domänenname.com&gt;  

Sie sollten ähnlich wie im folgenden Beispiel nach wenigen Minuten eine E-Mail-Nachricht erhalten, in der Sie aufgefordert werden, die Anforderung zu genehmigen. Wenn Sie einen Spamfilter verwenden, fügen Sie admin@digicert.com der Positivliste hinzu. Sollten Sie nach 24 Stunden noch keine E-Mail erhalten haben, setzen Sie sich mit dem Microsoft-Support in Verbindung.
    
![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/domain-validation-email.png)

Wenn Sie auf den Genehmigungslink klicken, werden Sie auf das folgende Onlineformular für die Genehmigung weitergeleitet: 
    
![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/domain-validation-form.png)

Befolgen Sie die Anweisungen im Formular. Sie haben zwei Möglichkeiten zur Genehmigung:

- Sie können alle zukünftigen Aufträge genehmigen, die über dasselbe Konto und für dieselbe Stammdomäne (beispielsweise „contoso.com“) getätigt werden. Diese Vorgehensweise empfiehlt sich, wenn Sie später weitere benutzerdefinierte Domänen für die gleiche Stammdomäne hinzufügen möchten.

- Sie können lediglich den spezifischen Hostnamen aus dieser Anforderung genehmigen. Für nachfolgende Anforderungen wird eine zusätzliche Genehmigung benötigt.

Nach der Genehmigung fügt DigiCert Ihren benutzerdefinierten Domänennamen dem SAN-Zertifikat (Subject Alternative Names, alternativer Antragstellername) hinzu. Das Zertifikat ist ein Jahr lang gültig und wird automatisch verlängert, bevor es abläuft.

### <a name="step-3-wait-for-propagation"></a>Schritt 3: Warten auf die Weitergabe

Nach der Überprüfung des Domänennamens dauert es maximal 6 bis 8 Stunden, bis das HTTPS-Feature für die benutzerdefinierte Domäne aktiviert wird. Wenn der Prozess abgeschlossen ist, wird der Status „Benutzerdefiniertes HTTPS“ im Azure-Portal auf „Aktiviert“ festgelegt, und die vier Schritte auf dem Blatt „HTTPS für benutzerdefinierte Domänen“ werden als abgeschlossen markiert. Die benutzerdefinierte Domäne ist jetzt bereit zur Verwendung von HTTPS.

![Dialogfeld „HTTPS aktivieren“](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Vorgangsstatus

Die folgende Tabelle zeigt den Status des Vorgangs zum Aktivieren von HTTPS. Nach der Aktivierung von HTTPS werden auf dem Blatt „HTTPS für benutzerdefinierte Domänen“ vier Vorgangsschritte angezeigt. Beim Durchlaufen der einzelnen Schritte werden weitere Details zum jeweiligen Schritt angezeigt. Nachdem ein Schritt erfolgreich abgeschlossen wurde, wird neben diesem ein grünes Häkchen angezeigt. 

| Vorgangsschritt | Details zum Vorgangsschritt | 
| --- | --- |
| 1: Übermitteln der Anforderung | Übermitteln der Anforderung |
| | Die HTTPS-Anforderung wird übermittelt. |
| | Die HTTPS-Anforderung wurde erfolgreich übermittelt. |
| 2: Überprüfen der Domäne | Wir haben Ihnen eine E-Mail-Nachricht gesendet, in der Sie aufgefordert werden, den Besitz der Domäne zu bestätigen. Es wird auf Ihre Bestätigung gewartet. |
| | Der Besitz der Domäne wurde erfolgreich bestätigt. |
| | Die Überprüfungsanforderung für den Domänenbesitz ist abgelaufen (Kunde hat wahrscheinlich nicht innerhalb von 6 Tagen reagiert). HTTPS wird für Ihre Domäne nicht aktiviert. * |
| | Die Überprüfungsanforderung für den Domänenbesitz wurde vom Kunden zurückgewiesen. HTTPS wird für Ihre Domäne nicht aktiviert. * |
| 3: Zertifikatbereitstellung | Die Zertifizierungsstelle stellt zurzeit das Zertifikat aus, das zum Aktivieren von HTTPS für Ihre Domäne erforderlich ist. |
| | Das Zertifikat wurde ausgegeben und wird derzeit im CDN-Netzwerk bereitgestellt. Dies kann bis zu sechs Stunden dauern. |
| | Das Zertifikat wurde erfolgreich im CDN-Netzwerk bereitgestellt. |
| 4: Abschließen | HTTPS wurde in Ihrer Domäne erfolgreich aktiviert. |

\* Diese Meldung wird nicht bei einem Fehler angezeigt. 

Wenn vor dem Übermitteln der Anforderung ein Fehler auftritt, wird die folgende Fehlermeldung angezeigt:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>Deaktivieren von HTTPS

Wenn Sie HTTPS aktiviert haben, können Sie es später deaktivieren. Gehen Sie folgendermaßen vor, um HTTPS zu deaktivieren:

### <a name="step-1-disable-the-feature"></a>Schritt 1: Deaktivieren des Features 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Standard- oder Premium-Verizon-CDN-Profil.

2. Klicken Sie in der Liste mit den Endpunkten auf den Endpunkt, die Ihre benutzerdefinierte Domäne enthält.

3. Klicken Sie auf die benutzerdefinierte Domäne, für die Sie HTTPS deaktivieren möchten.

    ![Blatt „Endpunkt“](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Klicken Sie auf **Aus**, um HTTPS zu deaktivieren, und klicken Sie dann auf **Anwenden**.

    ![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>Schritt 2: Warten auf die Weitergabe

Nachdem das HTTPS-Feature für die benutzerdefinierte Domäne deaktiviert wurde, dauert es maximal 6 bis 8 Stunden, bis die Änderung wirksam wird. Wenn der Prozess abgeschlossen ist, wird der Status „Benutzerdefiniertes HTTPS“ im Azure-Portal auf „Deaktiviert“ festgelegt, und die drei Schritte auf dem Blatt „HTTPS für benutzerdefinierte Domänen“ werden als abgeschlossen markiert. Die benutzerdefinierte Domäne kann HTTPS nicht mehr verwenden.

![Dialogfeld „HTTPS deaktivieren“](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Vorgangsstatus

Die folgende Tabelle zeigt den Status des Vorgangs zum Deaktivieren von HTTPS. Nach der Deaktivierung von HTTPS werden auf dem Blatt „HTTPS für benutzerdefinierte Domänen“ drei Vorgangsschritte angezeigt. Beim Durchlaufen der einzelnen Schritte werden weitere Details zum jeweiligen Schritt angezeigt. Nachdem ein Schritt erfolgreich abgeschlossen wurde, wird neben diesem ein grünes Häkchen angezeigt. 

| Vorgangsstatus | Vorgangsdetails | 
| --- | --- |
| 1: Übermitteln der Anforderung | Ihre Anforderung wird übermittelt. |
| 2: Aufheben der Zertifikatbereitstellung | Das Zertifikat wird gelöscht. |
| 3: Abschließen | Das Zertifikat wurde gelöscht. |

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. *Wer ist der Zertifikatanbieter, und welche Art von Zertifikat wird verwendet?*

    Wir verwenden SAN-Zertifikate (Subject Alternative Names; alternative Antragstellernamen) von DigiCert. Ein SAN-Zertifikat kann mehrere vollqualifizierte Domänennamen mit einem einzelnen Zertifikat schützen.

2. *Kann ich mein dediziertes Zertifikat verwenden?*
    
    Noch nicht, die Option ist aber geplant.

3. *Was passiert, wenn ich die Domänenüberprüfungs-E-Mail von DigiCert nicht erhalte?*

    Sollten Sie nach 24 Stunden noch keine E-Mail erhalten haben, setzen Sie sich mit dem Microsoft-Support in Verbindung.

4. *Ist ein SAN-Zertifikat weniger sicher als ein dediziertes Zertifikat?*
    
    Ein SAN-Zertifikat bietet die gleichen Verschlüsselungs- und Sicherheitsstandards wie ein dediziertes Zertifikat. Alle ausgestellten SSL-Zertifikate verwenden SHA-256 für verbesserte Serversicherheit.

5. *Kann ich HTTPS für benutzerdefinierte Domänen mit Azure CDN von Akamai verwenden?*

    Dieses Feature ist derzeit nur mit Azure CDN von Verizon verfügbar. Wir arbeiten daran, dieses Feature in den nächsten Monaten auch für Azure CDN von Akamai zu unterstützen.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, wie Sie eine [benutzerdefinierte Domäne für Ihren Azure CDN-Endpunkt](./cdn-map-content-to-custom-domain.md) einrichten.



