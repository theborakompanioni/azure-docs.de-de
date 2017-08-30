---
title: "Azure AD-Passthrough-Authentifizierung – Schnellstart | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie die ersten Schritte für die Azure AD-Passthrough-Authentifizierung (Azure Active Directory) ausführen."
services: active-directory
keywords: "Azure AD Connect-Passthrough-Authentifizierung, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 07063ea53e96c6467e40e8a7ca70e5c03ce53284
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory-Passthrough-Authentifizierung: Schnellstart

## <a name="how-to-deploy-azure-ad-pass-through-authentication"></a>Bereitstellen der Azure AD-Passthrough-Authentifizierung

Mit der Azure AD-Passthrough-Authentifizierung (Azure Active Directory) können sich Benutzer mit denselben Kennwörtern sowohl an lokalen als auch an cloudbasierten Anwendungen anmelden. Benutzer werden angemeldet, indem sie ihre Kennwörter für Ihre lokale Active Directory-Instanz direkt angeben.

>[!IMPORTANT]
>Die Azure AD-Passthrough-Authentifizierung befindet sich derzeit in der Vorschauphase. Wenn Sie dieses Feature bereits in der Vorschau verwendet haben, sollten Sie sicherstellen, dass Sie die Vorschauversionen der Authentifizierungs-Agents mithilfe der [hier](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md) bereitgestellten Anweisungen aktualisieren.

Befolgen Sie diese Anweisungen, um die Passthrough-Authentifizierung bereitzustellen:

## <a name="step-1-check-prerequisites"></a>Schritt 1: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

### <a name="on-the-azure-active-directory-admin-center"></a>Im Azure Active Directory-Admin Center

1. Erstellen Sie auf Ihrem Azure AD-Mandanten ein auf die Cloud beschränktes globales Administratorkonto. Auf diese Weise können Sie die Konfiguration Ihres Mandanten verwalten, falls Ihre lokalen Dienste ausfallen oder nicht verfügbar sind. Erfahren Sie, wie Sie ein [rein cloudbasiertes Konto für den globalen Administrator hinzufügen](../active-directory-users-create-azure-portal.md). Die Ausführung dieses Schritts ist sehr wichtig, damit sichergestellt ist, dass Sie für Ihren Mandanten nicht gesperrt werden.
2. Fügen Sie Ihrem Azure AD-Mandanten mindestens einen [benutzerdefinierten Domänennamen](../active-directory-add-domain.md) hinzu. Ihre Benutzer melden sich mit einem dieser Domänennamen an.

### <a name="in-your-on-premises-environment"></a>In Ihrer lokalen Umgebung

1. Identifizieren Sie einen Server mit Windows Server 2012 R2 oder höher, auf dem Azure AD Connect ausgeführt werden soll. Fügen Sie den Server derselben AD-Gesamtstruktur wie die Benutzer hinzu, deren Kennwörter überprüft werden müssen.
2. Installieren Sie die [aktuelle Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) auf dem Server, den Sie im vorherigen Schritt ausgewählt haben. Wenn Azure AD Connect bereits ausgeführt wird, vergewissern Sie sich, dass die Version 1.1.557.0 oder höher lautet.
3. Identifizieren Sie einen weiteren Server mit Windows Server 2012 R2 oder höher, auf dem der eigenständige Authentifizierungs-Agent ausgeführt werden soll. Die Version des Authentifizierungs-Agents muss 1.5.193.0 oder höher sein. Dieser Server wird benötigt, um die hohe Verfügbarkeit von Anmeldeanforderungen sicherzustellen. Fügen Sie den Server derselben AD-Gesamtstruktur wie die Benutzer hinzu, deren Kennwörter überprüft werden müssen.
4. Wenn zwischen Ihren Servern und Azure AD eine Firewall eingerichtet wurde, müssen Sie die folgenden Elemente konfigurieren:
   - Stellen Sie sicher, dass Authentifizierung-Agents **ausgehende** Anforderungen an Azure AD über die folgenden Ports senden können:
   
   | Portnummer | Wie diese verwendet wird |
   | --- | --- |
   | **80** | Herunterladen der Zertifikatssperrlisten (CRL) bei der Überprüfung des SSL-Zertifikats |
   | **443** | Gesamte mit unserem Dienst ausgehende Kommunikation |
   
   Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
   - Wenn Ihre Firewall oder ihr Proxy DNS-Whitelisting zulässt, beschränken Sie Verbindungen mit **\*.msappproxy.net** und **\*.servicebus.windows.net** mittels Whitelist. Ist dies nicht der Fall, aktivieren Sie den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden.
   - Ihre Authentifizierungs-Agents benötigen für den anfänglichen Registrierungsprozess Zugriff auf **login.windows.net** und **login.microsoftonline.com**, daher müssen Sie Ihre Firewall auch für diese URLs öffnen.
   - Geben Sie für die Überprüfung des Zertifikats folgende URLs frei: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** und **www.microsoft.com:80**. Da diese URLs für die Überprüfung des Zertifikats in Verbindung mit anderen Microsoft-Produkten verwendet werden, haben Sie diese möglicherweise bereits freigegeben.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Schritt 2: Aktivieren der Exchange ActiveSync-Unterstützung (optional)

Um Exchange ActiveSync-Unterstützung zu aktivieren, gehen Sie wie folgt vor:

1. Führen Sie mit [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) folgenden Befehl aus:
```
Get-OrganizationConfig | fl per*
```

2. Überprüfen Sie den Wert der `PerTenantSwitchToESTSEnabled`-Einstellung. Wenn der Wert **true** ist, ist Ihr Mandant richtig konfiguriert – dies ist im Allgemeinen bei den meisten Kunden der Fall. Wenn der Wert **false** ist, führen Sie folgenden Befehl aus:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Überprüfen Sie, ob der Wert der `PerTenantSwitchToESTSEnabled`-Einstellung nun auf **true** festgelegt ist. Warten Sie eine Stunde, bevor Sie mit dem nächsten Schritt fortfahren.

Wenn während dieses Schritts Probleme auftreten, entnehmen Sie unserem [Handbuch zur Problembehandlung](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) weitere Informationen.

## <a name="step-3-enable-the-feature"></a>Schritt 3: Aktivieren des Features

Die Passthrough-Authentifizierung kann mit [Azure AD Connect](active-directory-aadconnect.md) aktiviert werden.

>[!IMPORTANT]
>Sie können die Passthrough-Authentifizierung auf dem primären Server oder dem Stagingserver von Azure AD Connect aktivieren. Es wird dringend empfohlen, die Aktivierung über den primären Server durchzuführen.

Wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md), wenn Sie Azure AD Connect zum ersten Mal installieren. Wählen Sie auf der Seite **Benutzeranmeldung** die Option **Passthrough-Authentifizierung** als Anmeldemethode aus. Nach erfolgreicher Durchführung wird auf demselben Server wie Azure AD Connect ein Passthrough-Authentifizierungs-Agent installiert. Darüber hinaus wird die Passthrough-Authentifizierungsfunktion auf Ihrem Mandanten aktiviert.

![Azure AD Connect – Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso3.png)

Wenn Sie Azure AD Connect bereits installiert haben (per [Expressinstallation](active-directory-aadconnect-get-started-express.md) oder [benutzerdefinierter Installation](active-directory-aadconnect-get-started-custom.md)), wählen Sie in Azure AD Connect die Option **Benutzeranmeldeseite ändern** aus, und klicken Sie auf **Weiter**. Wählen Sie anschließend **Passthrough-Authentifizierung** als Anmeldemethode aus. Nach erfolgreicher Durchführung wird auf demselben Server wie Azure AD Connect ein Passthrough-Authentifizierungs-Agent installiert, und das Feature wird auf Ihrem Mandanten aktiviert.

![Azure AD Connect – Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Die Passthrough-Authentifizierung ist ein Feature auf Mandantenebene. Wenn Sie es aktivieren, wirkt sich dies auf die Anmeldung der Benutzer in _allen_ verwalteten Domänen Ihres Mandanten aus. Wenn Sie von AD FS zur Passthrough-Authentifizierung wechseln, sollten Sie mindestens 12 Stunden warten, bevor Sie Ihre AD FS-Infrastruktur herunterfahren. Diese Wartezeit soll sicherstellen, dass Benutzer sich während des Übergangs weiterhin bei Exchange ActiveSync anmelden können.

## <a name="step-4-test-the-feature"></a>Schritt 4: Testen des Features

Befolgen Sie diese Anweisungen, um zu überprüfen, ob die Passthrough-Authentifizierung ordnungsgemäß aktiviert ist:

1. Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an.
2. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.
3. Wählen Sie **Azure AD Connect** aus.
4. Überprüfen Sie, ob das Feature **Passthrough-Authentifizierung** als **aktiviert** angezeigt wird.
5. Wählen Sie **Passthrough-Authentifizierung** aus. Auf diesem Blatt werden die Server aufgelistet, auf denen Ihre Authentifizierungs-Agents installiert sind.

![Azure Active Directory-Admin Center – Azure AD Connect-Blatt](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory-Admin Center – Passthrough-Authentifizierungs-Blatt](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Jetzt können sich Benutzer aus allen verwalteten Domänen Ihres Mandanten mit der Passthrough-Authentifizierung anmelden. Benutzer von Verbunddomänen melden sich aber weiterhin mithilfe der Active Directory-Verbunddienste (AD FS) oder einem anderen Verbundanbieter an, den Sie zuvor konfiguriert haben. Wenn Sie eine Verbunddomäne in eine verwaltete Domäne konvertieren, melden sich alle Benutzer dieser Domäne automatisch mit der Passthrough-Authentifizierung an. Benutzer, die auf die Cloud beschränkt sind, sind von der Passthrough-Authentifizierungsfunktion nicht betroffen.

## <a name="step-5-ensure-high-availability"></a>Schritt 5: Sicherstellen der hohen Verfügbarkeit

Wenn Sie die Bereitstellung der Passthrough-Authentifizierung in einer Produktionsumgebung planen, sollten Sie einen eigenständigen Authentifizierungs-Agent installieren. Installieren Sie diesen zweiten Authentifizierungs-Agent auf einem _anderen_ Server – also nicht auf dem Server, auf dem Azure AD Connect und der erste Authentifizierungs-Agent ausgeführt werden. Mit diesem Setup erzielen Sie für Anmeldeanforderungen eine hohe Verfügbarkeit. Befolgen Sie diese Anweisungen zum Bereitstellen eines eigenständigen Authentifizierungs-Agents:

1. **Laden Sie die neueste Version des Authentifizierungs-Agents (Versionen 1.5.193.0 oder höher) herunter**: Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an.
2. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.
3. Wählen Sie **Azure AD Connect** und dann **Passthrough-Authentifizierung** aus. Wählen Sie dann **Agent herunterladen** aus.
4. Klicken Sie auf die Schaltfläche **Bedingungen akzeptieren und herunterladen**.
5. **Installieren Sie die neueste Version des Authentifizierungs-Agents**: Führen Sie die im vorherigen Schritt heruntergeladene ausführbare Datei aus. Stellen Sie die Anmeldeinformationen des globalen Administrators Ihres Mandanten bereit, wenn Sie dazu aufgefordert werden.

![Azure Active Directory-Admin Center – Schaltfläche zum Herunterladen des Authentifizierungs-Agents](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory-Admin Center – Blatt „Agent herunterladen“](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Sie können den Authentifizierungs-Agent auch [hier](https://aka.ms/getauthagent) herunterladen. _Ehe_ Sie den Authentifizierungs-Agent installieren, müssen Sie die [Nutzungsbedingungen](https://aka.ms/authagenteula) lesen und akzeptieren.

## <a name="next-steps"></a>Nächste Schritte
- [**Aktuelle Einschränkungen**](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Diese Funktion befindet sich derzeit in der Vorschauphase. Informieren Sie sich darüber, welche Szenarien unterstützt werden.
- [**Ausführliche technische Betrachtung:**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informieren Sie sich über die Funktionsweise dieses Features.
- [**Häufig gestellte Fragen**](active-directory-aadconnect-pass-through-authentication-faq.md) – Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen

