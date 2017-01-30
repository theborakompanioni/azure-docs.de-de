---
title: 'Azure AD Connect: einmaliges Anmelden | Microsoft-Dokumentation'
description: In diesem Thema erfahren Sie, wie das einmalige Anmelden aus einem lokalen Active Directory (AD) bei einem cloudbasierten Azure Active Directory (Azure AD) und damit verbundenen Diensten funktioniert.
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: bddd581b5164b03ccba75370cd89a7831101cf37
ms.openlocfilehash: f0133aaf735533159d72b1e63b952258707ad8b9


---

# <a name="what-is-single-sign-on-sso-preview"></a>Was ist das einmalige Anmelden (Vorschau)?
Das einmalige Anmelden (Single Sign-On, SSO) ist eine Option, die in Azure Active Directory Connect entweder über die [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) oder die [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) aktiviert werden kann.  Wenn diese Option aktiviert ist, müssen Benutzer kein Kennwort eingeben, um sich bei Azure Active Directory (Azure AD) oder anderen Clouddiensten anzumelden, sofern sie an unternehmenseigenen Computern arbeiten und mit dem Unternehmensnetzwerk verbunden sind.  

Durch Aktivieren des einmaligen Anmeldens müssen Benutzer beim Zugriff auf Azure Active Directory-Dienste (z.B. Office 365, Microsoft Intune, CRM Online oder SaaS-Dienste) kein Kennwort eingeben, sondern verwenden stattdessen ein Kerberos-Ticket, das automatisch aus Ihrem lokalen Active Directory abgerufen wird.

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso1.png)

Mit dem einmaligen Anmelden können Benutzer auf vertraute Weise auf cloudbasierte Dienste zugreifen, und die Organisation profitiert von einem sicheren und einfachen Prozess, der keinerlei zusätzlichen lokalen Komponenten erfordert.

Dieses Feature wird über AAD Connect aktiviert und funktioniert mit der Kennworthashsynchronisierung oder der Passthrough-Authentifizierung und Ihrem lokalen Active Directory.  Damit Ihre Endbenutzer das einmalige Anmelden in Ihrer Umgebung verwenden können, müssen Sie Folgendes sicherstellen:


- Die Benutzer müssen in die Domäne eingebundene Computer verwenden.
- Es muss eine direkte Verbindung mit einem Domänencontroller bestehen, beispielsweise über das Unternehmensnetzwerk (Kabel- oder Funknetzwerk) oder per Remotezugriff z.B. über eine VPN-Verbindung.
- Definieren Sie die Kerberos-Endpunkte in der Cloud als Teil der Intranetzone.

Wenn eine der obigen Anforderungen nicht erfüllt ist (wenn sich z.B. der Computer nicht im Unternehmensnetzwerk befindet und Active Directory nicht verfügbar ist), wird der Benutzer einfach aufgefordert, sein Kennwort einzugeben, genauso, als wäre das einmalige Anmelden nicht aktiviert.

## <a name="supported-clients"></a>Unterstützte Clients
Das einmalige Anmelden ist in browserbasierten Clients und Office-Clients möglich, die moderne Authentifizierungsmechanismen auf Computern unterstützen, die eine Kerberos-Authentifizierung ausführen können, beispielsweise auf Windows-Computern.  Die folgende Matrix zeigt Informationen zu den browserbasierten Clients unter verschiedenen Betriebssystemen.

| Betriebssystem/Browser |Internet Explorer|Chrome|Firefox|Edge
| --- | --- |--- | --- |--- |
|Windows 10|Ja|Ja|Ja*|Nein
|Windows 8.1|Ja|Ja|Ja*|–
|Windows 8|Ja|Ja|Ja*|N/V
|Windows 7|Ja|Ja|Ja*|–
|Mac|N/V|N/V|N/V|–

*Erfordert separate Konfiguration.

>[!NOTE]
>Bei Windows 10-basierten Clients wird empfohlen, [Azure AD Join](../active-directory-azureadjoin-overview.md) zu verwenden, um für eine optimale Funktionsweise mit Azure AD zu sorgen. 

## <a name="how-single-sign-on-works"></a>So funktioniert das einmalige Anmelden

Wenn Sie das einmalige Anmelden in Azure AD Connect aktivieren, wird im lokalen Active Directory ein Computerkonto namens AZUREADSSOACCT erstellt, und der Kerberos-Entschlüsselungsschlüssel wird sicher in Azure AD freigegeben.  Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen erstellt, um die Cloud-URLs darzustellen, die während der Authentifizierung zwischen Client und Azure AD verwendet werden.  

Sobald das Setup abgeschlossen ist, erfolgt die Authentifizierung auf die gleiche Weise wie bei jeder anderen auf der integrierten Windows-Authentifizierung (IWA) basierenden Anwendung.  Wenn Sie mit der Funktionsweise von IWA vertraut sind, wissen Sie bereits, wie das einmalige Anmelden mit Azure AD funktioniert.  Wenn Ihnen die Funktionsweise nicht bekannt ist, finden Sie hier Informationen zum Prozess für IWA:

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso2.png)

Zuerst versucht der Benutzer, auf die Ressource zuzugreifen.  Eine Ressource kann ein Computer oder eine URL sein.  Bei Azure AD handelt es sich um eine URL für einen Dienst, wie z.B. SharePoint Online – im Folgenden als „Azure AD-Ressource“ bezeichnet.

1.  Die Azure AD-Ressource fordert über eine „401 – Nicht autorisiert“-Antwort den Client auf, ein Kerberos-Ticket bereitzustellen.
2.  Der Client fordert bei Active Directory ein Ticket für die Azure AD-Ressource an. 
3.  Active Directory sucht das Computerkonto, das der Azure AD-Ressource zugeordnet ist, und gibt ein Kerberos-Ticket an den Client zurück, das mit dem Geheimnis des Computerkontos verschlüsselt ist. Das Ticket enthält die Identität des Benutzers, der zurzeit am Computer angemeldet ist.
4.  Der Client sendet das von Active Directory abgerufene Kerberos-Ticket an Azure AD.
5.  Azure AD entschlüsselt das Kerberos-Ticket mit einem zuvor freigegebenen Schlüssel. Dann gibt Azure AD dann entweder ein Token an den Benutzer zurück oder fordert den Benutzer auf, zusätzliche Identitätsnachweise z.B. per Multi-Factor Authentication bereitzustellen – je nachdem, was die Ressource erfordert.

Das einmalige Anmelden ist eine opportunistische Funktion, d.h., wenn die Funktion aus irgendeinem Grund nicht verfügbar ist, können Benutzer einfach wie üblich ihr Kennwort auf der Anmeldeseite eingeben.

## <a name="enabling-sso-with-pass-through-authentication-or-password-hash-sync"></a>Aktivieren des einmaligen Anmeldens mit Passthrough-Authentifizierung oder Kennworthashsynchronisierung
Azure AD Connect stellt einen einfachen Prozess zum Aktivieren des einmaligen Anmeldens mit Passthrough-Authentifizierung oder Kennworthashsynchronisierung bereit.  Sie müssen sicherstellen, dass Sie in jeder zu synchronisierenden Gesamtstruktur in einer Domäne über Domänenadministratorrechte verfügen, um die Konfiguration der Kerberos-Dienstprinzipalnamen im Computerkonto zu ermöglichen.  Der Benutzername und das Kennwort werden nicht in Azure AD Connect oder Azure AD gespeichert und nur für diesen Vorgang verwendet.

Wählen Sie beim Installieren von Azure AD Connect eine benutzerdefinierte Installation aus, damit Sie das einmalige Anmelden auf der Benutzeranmeldeseite konfigurieren können. Weitere Informationen finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso3.png)

Sobald das einmalige Anmelden aktiviert ist, können Sie mit dem Assistenten fortfahren, bis Sie zur Seite „Einmaliges Anmelden“ gelangen.

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso4.png)

Geben Sie für jede aufgeführte Gesamtstruktur die entsprechenden Kontodetails ein. Das einmalige Anmelden wird für Ihr Azure-Verzeichnis aktiviert.

## <a name="ensuring-clients-sign-in-automatically"></a>Sicherstellen der automatischen Anmeldung von Clients
Standardmäßig versuchen Browser nicht, Anmeldeinformationen an Webserver zu senden, sofern die URL nicht als in der Intranetzone befindlich definiert wurde.  Im Allgemeinen kann der Browser die richtige Zone anhand der URL ermitteln.  Wenn die URL z.B. „http://intranet/“ lautet, sendet der Browser automatisch Anmeldeinformationen, weil die URL der Intranetzone zugeordnet ist.  Wenn die URL jedoch einen Punkt enthält, z.B. „http://intranet.contoso.com/“, sendet der Server nicht automatisch Anmeldeinformationen und behandelt die URL so, als wäre sie eine beliebige Internetwebsite.

Da es sich bei den für das einmalige Anmelden in Azure AD verwendeten URLs um global routingfähige Hostnamen handelt, enthalten sie einen Punkt. Aus diesem Grund müssen sie explizit zur Intranetzone des Computers hinzugefügt werden, damit der Browser die Anmeldeinformationen des aktuell angemeldeten Benutzers automatisch in Form eines Kerberos-Tickets an Azure AD sendet.  Die einfachste Möglichkeit, die erforderlichen URLs zur Intranetzone hinzuzufügen, ist das Erstellen einer Gruppenrichtlinie in Active Directory.

1.  Öffnen Sie die Gruppenrichtlinien-Verwaltungstools.
2.  Bearbeiten Sie die Gruppenrichtlinie, die auf alle Benutzer angewendet wird.  Beispiel: Standardrichtlinie der Domäne.
3.  Navigieren Sie zu **Benutzerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Internet Explorer\Internetsystemsteuerung\Sicherheitsseite**, und wählen Sie die Option **Liste der Site zu Zonenzuweisungen**.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso6.png) </br>
4.  Aktivieren Sie die Richtlinie, und geben Sie die folgenden Werte bzw. Daten in das Dialogfeld ein.</br>
        
        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net 
        Data: 1'
5.  Das Ergebnis sollte in etwa wie folgt aussehen: ![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso7.png)

6.  Klicken Sie zweimal nacheinander auf OK.

Ihre Benutzer können das einmalige Anmelden jetzt verwenden.

>[!NOTE]
>Standardmäßig verwendet Chrome den gleichen Satz vertrauenswürdiger Website-URLs wie Internet Explorer.  Wenn Sie andere Einstellungen für Chrome konfiguriert haben, müssen Sie diese separat aktualisieren.

## <a name="troubleshooting-single-sign-on-issues"></a>Behandeln von Problemen beim einmaligen Anmelden
Es muss unbedingt sichergestellt werden, dass der Client ordnungsgemäß für das einmalige Anmelden konfiguriert ist. Dazu gehört Folgendes:

1.  Sowohl „https://autologon.microsoftazuread-sso.com“ als auch „https://aadg.windows.net.nsatc.net“ sind innerhalb der Intranetzone definiert.  
2.  Stellen Sie sicher, dass die Arbeitsstation in die Domäne eingebunden ist.
3.  Stellen Sie sicher, dass der Benutzer mit einem Domänenkonto angemeldet ist.
4.  Stellen Sie sicher, dass der Computer mit dem Unternehmensnetzwerk verbunden ist.
5.  Stellen Sie sicher, dass die Uhrzeit des Computers mit der Uhrzeit von Active Directory und den Domänencontrollern synchronisiert ist. Eine Abweichung von 5 Minuten ist zulässig.

Wenn die oben genannten Anforderungen erfüllt sind, überprüfen Sie die Konsolenprotokolle des Browsers, um weitere Informationen zu erhalten.  Sie finden die Konsolenprotokolle bei den Entwicklertools.  Auf diese Weise können Sie das potenzielle Problem ermitteln.

## <a name="event-log-entries"></a>Einträge des Ereignisprotokolls
Jedes Mal, wenn ein Benutzer sich per einmaligem Anmelden anmeldet, wird ein Eintrag in das Ereignisprotokoll des Domänencontrollers geschrieben, sofern die Erfolgsüberwachung aktiviert ist.  Diese Ereignisse finden Sie in den Ereignisprotokollen für das Sicherheitsereignis 4769 für das Computerkonto AzureADSSOAcc$.  Mit folgendem Filter können Sie alle Sicherheitsereignisse anzeigen, die dem Computerkonto zugeordnet sind:

    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>






<!--HONumber=Dec16_HO3-->


