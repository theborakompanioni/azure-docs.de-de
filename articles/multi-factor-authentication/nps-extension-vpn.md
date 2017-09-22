---
title: VPN-Integration in Azure MFA mit NPS-Erweiterung | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt die Integration Ihrer VPN-Infrastruktur in Azure MFA mit der Netzwerkrichtlinienserver-Erweiterung (Network Policy Server, NPS) für Microsoft Azure."
services: active-directory
keywords: Azure MFA, Integrieren von VPN, Azure Active Directory, Netzwerkrichtlinienserver-Erweiterung
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: b47e9b321b2fd0d0db9762003531b0fe9f045f07
ms.contentlocale: de-de
ms.lasthandoff: 09/21/2017

---

# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-the-network-policy-server-nps-extension-for-azure"></a>Integration Ihrer VPN-Infrastruktur in Azure MFA (Multi-Factor Authentication, mehrstufige Authentifizierung) mit der Netzwerkrichtlinienserver-Erweiterung (Network Policy Server, NPS) für Azure.

## <a name="overview"></a>Übersicht

Die Netzwerkrichtlinienserver-Erweiterung (NPS) für Azure ermöglicht Unternehmen, ihre RADIUS-Clientauthentifizierung (Remote Authentication Dial-in User Service) mit cloudbasierter [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md) zu schützen, die eine Überprüfung in zwei Schritten bietet.

Dieser Artikel enthält Anweisungen für die Integration der NPS-Infrastruktur in Azure MFA mit der NPS-Erweiterung für Azure, um sichere zweistufige Überprüfung für Benutzer zu aktivieren, die versuchen, eine VPN-Verbindung mit Ihrem Netzwerk herzustellen. 

Die Netzwerkrichtlinien- und Zugriffsdienste (Network Policy and Access Services, NPS) bieten Organisationen folgende Möglichkeiten:

* Festlegen zentraler Orte für die Verwaltung und Steuerung von Netzwerkanforderungen, um Folgendes anzugeben: wer eine Verbindung herstellen kann, zu welchen Tageszeiten Verbindungen zugelassen sind, die Dauer der Verbindungen und die Sicherheitsstufe, die Clients verwenden müssen, um eine Verbindung herstellen zu können usw. Anstatt diese Richtlinien auf jedem VPN oder Remotedesktopgateway-Server (RD) anzugeben, können diese Richtlinien einmal an einem zentralen Ort angegeben werden. Das RADIUS-Protokoll wird verwendet, um die zentralisierte Authentifizierung, Autorisierung und Ressourcenerfassung (Authentication, Authorization, Accounting – AAA) bereitzustellen. 
* Richten Sie Netzwerkzugriffsschutz-Clientintegritätsrichtlinien (Network Access Protection, NAP) ein, die bestimmen, ob Geräten uneingeschränkter oder eingeschränkter Zugriff auf Netzwerkressourcen gewährt wird, und erzwingen Sie deren Durchsetzung.
* Bieten Sie eine Möglichkeit zum Erzwingen der Authentifizierung und Autorisierung für den Zugriff auf 802.1x-fähige Drahtloszugriffspunkte und Ethernet-Switches.    

Weitere Informationen finden Sie unter [Network Policy Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) (Netzwerkrichtlinienserver). 

Um die Sicherheit zu erhöhen und ein hohes Maß an Kompatibilität zu bieten, können Organisationen NPS in Azure MFA integrieren, um sicherzustellen, dass Benutzer die Überprüfung in zwei Schritten verwenden, um mit dem virtuellen Port auf dem VPN-Server eine Verbindung herstellen zu können. Damit Benutzern Zugriff gewährt wird, müssen sie die von ihnen festgelegte Kombination aus Benutzername und Kennwort angeben. Diese Informationen müssen vertrauenswürdig und dürfen nicht problemlos duplizierbar sein, z.B. eine Mobiltelefonnummer, Festnetznummer, Anwendung auf einem mobilen Gerät usw.

Vor der Verfügbarkeit der NPS-Erweiterung für Azure mussten Kunden, die die Überprüfung in zwei Schritten für integrierte NPS- und Azure MFA-Umgebungen implementieren wollten, einen separaten MFA-Server in der lokalen Umgebung konfigurieren und verwalten, wie in „Remotedesktopgateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS“ dokumentiert.

Die Verfügbarkeit der NPS-Erweiterung für Azure bietet Organisationen jetzt die Möglichkeit, entweder eine lokal basierte oder eine cloudbasierte MFA-Lösung zum Schützen der RADIUS-Clientauthentifizierung bereitzustellen.
 
## <a name="authentication-flow"></a>Authentifizierungsfluss
Wenn ein Benutzer eine Verbindung mit einem virtuellen Port auf einem VPN-Server herstellt, muss er sich zunächst mit einer Vielzahl von Protokollen authentifizieren, die die Verwendung einer Kombination aus Benutzername und Kennwort und zertifikatbasierte Authentifizierungsmethoden ermöglichen. 

Zusätzlich zur Authentifizierung und Identitätsüberprüfung müssen Benutzer über die entsprechenden Einwahlberechtigungen verfügen. In einfachen Implementierungen werden diese Einwahlberechtigungen, die Zugriff erlauben, direkt auf den Active Directory-Benutzerobjekten festgelegt. 

 ![Benutzereigenschaften](./media/nps-extension-vpn/image1.png)

In einfachen Implementierungen gewährt oder verweigert jeder VPN-Server den Zugriff basierend auf Richtlinien, die auf jedem lokalen VPN-Server definiert werden.

In größeren und besser skalierbaren Implementierungen befinden sich die Richtlinien, die VPN-Zugriff gewähren oder verweigern, zentral auf RADIUS-Servern. In diesem Fall fungiert der VPN-Server als Zugriffsserver (RADIUS-Client), der Verbindungsanfragen und Ressourcenerfassungsnachrichten an einen RADIUS-Server weiterleitet. Um eine Verbindung mit dem virtuellen Port auf dem VPN-Server herzustellen, müssen Benutzer authentifiziert werden und die zentral auf RADIUS-Servern definierten Bedingungen erfüllen. 

Wenn die NPS-Erweiterung für Azure in NPS integriert ist, lautet der erfolgreiche Authentifizierungsablauf wie folgt:

1. Der VPN-Server empfängt eine Authentifizierungsanforderung von einem VPN-Benutzer, die den Benutzernamen und das Kennwort zum Herstellen der Verbindung mit einer Ressource, z.B. einer Remotedesktopsitzung, enthält. 
2. Als RADIUS-Client fungierend konvertiert der VPN-Server die Anforderung in eine RADIUS-Zugriffsanforderungsnachricht und sendet die Nachricht (Kennwort ist verschlüsselt) an den RADIUS-Server (NPS), auf dem die NPS-Erweiterung installiert ist. 
3. Die Kombination aus Benutzername und Kennwort wird in Active Directory überprüft. Wenn Benutzername oder Kennwort falsch ist, sendet der RADIUS-Server eine Zugriffsverweigerungsnachricht. 
4. Wenn alle Bedingungen entsprechend den Angaben in der NPS-Verbindungsanforderung und den Netzwerkrichtlinien erfüllt sind (z.B. Uhrzeit- oder Gruppenmitgliedschaftseinschränkungen), löst die NPS-Erweiterung eine Anforderung der sekundären Authentifizierung mit Azure MFA aus. 
5. Azure MFA kommuniziert mit Azure Active Directory, ruft die Details des Benutzers ab und führt die sekundäre Authentifizierung mithilfe der Überprüfungsmethode aus, die vom Benutzer konfiguriert ist (Textnachricht, mobile App usw.). 
6. Bei erfolgreicher Authentifizierung durch MFA kommuniziert Azure MFA das Ergebnis an die NPS-Erweiterung.
7. Nach Authentifizierung und Autorisierung des Verbindungsversuchs sendet der NPS-Server, auf dem die Erweiterung installiert ist, eine RADIUS-Zugriffsakzeptierungsnachricht an den VPN-Server (RADIUS-Client).
8. Der Benutzer erhält Zugriff auf den virtuellen Port des VPN-Servers und richtet einen verschlüsselten VPN-Tunnel ein.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt werden die erforderlichen Voraussetzungen zur Integration von Azure MFA in das Remotedesktopgateway erörtert. Bevor Sie beginnen, müssen folgende Voraussetzungen erfüllt sein:

* VPN-Infrastruktur
* Netzwerkrichtlinien- und Zugriffsdienste-Rolle (Network Policy and Access Services, NPS)
* Azure MFA-Lizenz
* Windows Server-Software
* Bibliotheken
* Azure AD synchronisiert mit lokalem AD 
* GUID-ID von Azure Active Directory

### <a name="vpn-infrastructure"></a>VPN-Infrastruktur
In diesem Artikel wird davon ausgegangen, dass Sie über eine funktionierende VPN-Infrastruktur mit Microsoft Windows Server 2016 verfügen, und dass der VPN-Server zurzeit nicht zum Weiterleiten von Verbindungsanforderungen an einen RADIUS-Server konfiguriert ist. In diesem Handbuch konfigurieren Sie die VPN-Infrastruktur zur Verwendung eines zentralen RADIUS-Servers.

Wenn Sie nicht über eine funktionierende Infrastruktur verfügen, können Sie diese Infrastruktur schnell anhand der Anleitungen in zahlreichen VPN-Setuptutorials erstellen, die Sie auf den Websites von Microsoft und Drittanbietern finden. 

### <a name="network-policy-and-access-services-nps-role"></a>Netzwerkrichtlinien- und Zugriffsdienste-Rolle (Network Policy and Access Services, NPS)

Der NPS-Rollendienst bietet die RADIUS-Funktionen für Server und Client. In diesem Artikel wird davon ausgegangen, dass Sie die NPS-Rolle auf einem Mitgliedsserver oder Domänencontroller in Ihrer Umgebung installiert haben. In diesem Handbuch konfigurieren Sie RADIUS für eine VPN-Konfiguration. Installieren Sie die NPS-Rolle auf einem _anderen_ Server als Ihrem VPN-Server.

Informationen zum Installieren des NPS-Rollendiensts für Windows Server 2012 oder höher finden Sie unter [Install a NAP Health Policy Server](https://technet.microsoft.com/library/dd296890.aspx) (Installieren eines NAP-Integritätsrichtlinienservers). Die Netzwerkzugriffsrichtlinie (Network Access Policy, NAP) ist in Windows Server 2016 veraltet. Eine Beschreibung der bewährten Methoden für NPS einschließlich der Empfehlung zum Installieren von NPS auf einem Domänencontroller finden Sie unter [Best Practices for NPS](https://technet.microsoft.com/library/cc771746) (Bewährte Methoden für NPS).

### <a name="licenses"></a>Lizenzen

Eine Lizenz für Azure MFA ist erforderlich, die über ein Azure AD Premium-, Enterprise Mobility plus Security-Abonnement (EMS) oder MFA-Abonnement verfügbar ist. Weitere Informationen finden Sie unter [Beziehen von Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). Zu Testzwecken können Sie ein Testabonnement verwenden.

### <a name="software"></a>Software

Die NPS-Erweiterung erfordert Windows Server 2008 R2 SP1 oder höher mit installiertem NPS-Rollendienst. Alle Schritte in diesem Handbuch wurden mit Windows Server 2016 ausgeführt.

### <a name="libraries"></a>Bibliotheken

Diese Bibliotheken werden automatisch mit der Erweiterung installiert.

-   [Visual C++ Redistributable Packages für Visual Studio 2013 (x64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-Modul für Windows PowerShell, Version 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Das Microsoft Azure Active Directory-Modul für Windows PowerShell wird, sofern es nicht bereits vorhanden ist, über ein Konfigurationsskript installiert, das Sie als Teil des Installationsvorgangs ausführen. Es ist nicht erforderlich, dieses Modul vorab zu installieren, wenn es nicht bereits installiert ist.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory synchronisiert mit der lokalen Active Directory-Instanz 

Um die NPS-Erweiterung zu verwenden, müssen lokale Benutzer mit Azure Active Directory synchronisiert und für Multi-Factor Authentication aktiviert werden. Dieses Handbuch setzt voraus, dass lokale Benutzer mithilfe von AD Connect mit Azure Active Directory synchronisiert werden. Anweisungen zum Aktivieren von Benutzern für MFA siehe unten.
Informationen zu Azure AD Connect finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>GUID-ID von Azure Active Directory 
Um den NPS zu installieren, müssen Sie die GUID von Azure Active Directory kennen. Anweisungen zum Ermitteln der GUID von Azure Active Directory finden Sie im nächsten Abschnitt.

## <a name="configure-radius-for-vpn-connections"></a>Konfigurieren von RADIUS für VPN-Verbindungen

Wenn Sie die NPS-Serverrolle auf einem Mitgliedsserver installiert haben, müssen Sie sie zum Authentifizieren und Autorisieren von VPN-Clients konfigurieren, die VPN-Verbindungen anfordern. 

In diesem Abschnitt wird davon ausgegangen, dass Sie die Netzwerkrichtlinienserver-Rolle installiert, aber noch nicht für die Verwendung in Ihrer Infrastruktur konfiguriert haben.

>[!NOTE]
>Wenn Sie bereits über einen funktionierenden VPN-Server verfügen, der einen zentralisierten RADIUS-Server für die Authentifizierung verwendet, können Sie diesen Abschnitt überspringen.
>

### <a name="register-server-in-active-directory"></a>Registrieren des Servers in Active Directory
Die ordnungsgemäße Funktionsweise des NPS-Servers in diesem Szenario setzt seine Registrierung in Active Directory voraus.

1. Öffnen Sie den Server-Manager.
2. Klicken Sie im Server-Manager auf **Tools** und dann auf **Netzwerkrichtlinienserver**. 
3. Klicken Sie in der Netzwerkrichtlinienserver-Konsole mit der rechten Maustaste auf **NPS (Lokal)**, und klicken Sie dann auf **Server in Active Directory registrieren**. Klicken Sie zweimal auf **OK**.

 ![Netzwerkrichtlinienserver](./media/nps-extension-vpn/image2.png)

4. Lassen Sie die Konsole für den nächsten Vorgang geöffnet.

### <a name="use-wizard-to-configure-radius-server"></a>Verwenden des Assistenten zum Konfigurieren des RADIUS-Servers
Sie können eine standardmäßige (assistentenbasierte) oder die erweiterte Konfigurationsoption zum Konfigurieren des RADIUS-Servers verwenden. In diesem Abschnitt wird die Verwendung der Option zur assistentenbasierten Standardkonfiguration vorausgesetzt.

1. Klicken Sie in der Netzwerkrichtlinienserver-Konsole auf **NPS (Lokal)**.
2. Wählen Sie unter „Standardkonfiguration“ **RADIUS-Server für Einwähl- oder VPN-Verbindungen**, und klicken Sie dann auf **VPN oder DFÜ konfigurieren**.

 ![Konfigurieren von VPN](./media/nps-extension-vpn/image3.png)

3. Wählen Sie auf der Seite „Auswählen des Einwähl- oder VPN-Verbindungstyps“ die Option **VPN-Verbindungen**, und klicken Sie auf **Weiter**.

 ![Virtuelles privates Netzwerk](./media/nps-extension-vpn/image4.png)

4. Klicken Sie auf der Seite „Angeben des Einwähl- oder VPN-Servers“ auf **Hinzufügen**.
5. Geben Sie im Dialogfeld **Neuer RADIUS-Client** einen Anzeigenamen ein, und geben Sie den auflösbaren Namen oder die IP-Adresse des VPN-Servers und ein freigegebenes geheimes Kennwort ein. Dieses freigegebene geheime Kennwort sollte lang und komplex sein. Notieren Sie dieses Kennwort, da Sie es für die Schritte im nächsten Abschnitt benötigen.

 ![Neuer RADIUS-Client](./media/nps-extension-vpn/image5.png)

6. Klicken Sie auf **OK** und dann auf **Weiter**.
7. Akzeptieren Sie auf der Seite **Authentifizierungsmethoden konfigurieren** die Standardauswahl „Microsoft-verschlüsselte Authentifizierung, Version 2 (MS-CHAPv2)“ oder eine andere Option, und klicken Sie auf **Weiter**.

  >[!NOTE]
  >Wenn Sie Extensible Authentication-Protokoll (EAP) konfigurieren, müssen Sie entweder MS-CHAPv2 oder PEAP verwenden. Kein anderes EAP wird unterstützt.
 
8. Klicken Sie auf der Seite „Benutzergruppen angeben“ auf **Hinzufügen** , und wählen Sie eine entsprechende Gruppe aus, falls vorhanden. Andernfalls lassen Sie die Auswahl leer, um allen Benutzern Zugriff zu gewähren.

 ![Benutzergruppen angeben](./media/nps-extension-vpn/image7.png)

9. Klicken Sie auf **Weiter**.

10. Klicken Sie auf der Seite „Angeben von IP-Filtern“ auf **Weiter**.
11. Akzeptieren Sie auf der Seite „Angeben von Verschlüsselungseinstellungen“ die Standardeinstellungen, und klicken Sie auf **Weiter**.

 ![Angeben von Verschlüsselungseinstellungen](./media/nps-extension-vpn/image8.png)

12. Lassen Sie auf der Seite „Bereichsname angeben“ den Namen leer, und klicken Sie auf **Weiter**.

 ![Bereichsname angeben](./media/nps-extension-vpn/image9.png)

13. Klicken Sie auf der Seite „Abschließen der neuen Einwähl- oder VPN-Verbindungen und RADIUS-Clients“ auf **Fertig stellen**.

 ![Abschließen der Verbindungen](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>Überprüfen der RADIUS-Konfiguration
In diesem Abschnitt wird die Konfiguration erörtert, die Sie mithilfe des Assistenten erstellt haben.

1. Erweitern Sie auf dem NPS-Server in der Konsole „NPS (Lokal)“ „RADIUS-Clients“, und wählen Sie **RADIUS-Clients**.
2. Klicken Sie im Detailbereich mit der rechten Maustaste auf den RADIUS-Client, den Sie mithilfe des Assistenten erstellt haben, und klicken Sie auf **Eigenschaften**. Die Eigenschaften für den RADIUS-Client (der VPN-Server) sollten ähnlich wie unten dargestellt sein.

 ![VPN-Eigenschaften](./media/nps-extension-vpn/image11.png)

3. Klicken Sie auf **Abbrechen**.
4. Erweitern Sie auf dem NPS-Server in der Konsole „NPS (Lokal)“ **Richtlinien**, und wählen Sie **Verbindungsanforderungsrichtlinien**. Die Richtlinie für VPN-Verbindungen sollte angezeigt werden und der Abbildung unten ähneln.

 ![Verbindungsanforderungen](./media/nps-extension-vpn/image12.png)

5. Wählen Sie unter „Richtlinien“ **Netzwerkrichtlinien**. Eine Richtlinie für VPN-Verbindungen sollte angezeigt werden und der Abbildung unten ähneln.

 ![Netzwerkeigenschaften](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-to-use-radius-authentication"></a>Konfigurieren von VPN-Servern zur Verwendung der RADIUS-Authentifizierung
In diesem Abschnitt konfigurieren Sie die VPN-Server zur Verwendung der RADIUS-Authentifizierung. Dieser Abschnitt setzt voraus, dass Sie über eine funktionsfähige VPN-Serverkonfiguration verfügen, jedoch nicht den VPN-Server zur Verwendung der RADIUS-Authentifizierung konfiguriert haben. Nach dem Konfigurieren des VPN-Servers bestätigen Sie, dass die Konfiguration wie erwartet funktioniert.

>[!NOTE]
>Wenn Sie bereits über eine funktionierende VPN-Serverkonfiguration verfügen, die die RADIUS-Authentifizierung verwendet, können Sie diesen Abschnitt überspringen.
>

### <a name="configure-authentication-provider"></a>Konfigurieren des Authentifizierungsanbieters
1. Öffnen Sie auf dem VPN-Server den Server-Manager.
2. Klicken Sie im Server-Manager auf **Tools** und dann auf **Routing und RAS**.
3. Klicken Sie in der „Routing und RAS“-Konsole mit der rechten Maustaste auf **\[Servername\] (Lokal)**, und klicken Sie dann auf **Eigenschaften**.

 ![Routing und RAS](./media/nps-extension-vpn/image14.png)
 
4. Klicken Sie im Dialogfeld**[Servername] (Lokal) Eigenschaften** auf die Registerkarte **Sicherheit**. 
5. Klicken Sie auf der Registerkarte **Sicherheit** unter „Authentifizierungsanbieter“ auf **RADIUS-Authentifizierung** und dann auf **Konfigurieren**.

 ![RADIUS-Authentifizierung](./media/nps-extension-vpn/image15.png)
 
6. Klicken Sie im Dialogfeld „RADIUS-Authentifizierung“ auf **Hinzufügen**.
7. Fügen Sie in „RADIUS-Server hinzufügen“ in „Servername“ den Namen oder die IP-Adresse des RADIUS-Servers hinzu, den Sie im vorherigen Abschnitt konfiguriert haben.
8. Klicken Sie in „Gemeinsamer geheimer Schlüssel“ auf **Änderung**, und fügen Sie das gemeinsame geheime Kennwort hinzu, das Sie zuvor erstellt und notiert haben.
9. Ändern Sie in „Timeout (Sekunden)“ den Wert in einen Wert zwischen **30** und **60**. Dies ist erforderlich, damit genügend Zeit für den Abschluss des zweiten Authentifizierungsfaktors bleibt.
 
 ![RADIUS-Server hinzufügen](./media/nps-extension-vpn/image16.png)
 
10. Klicken Sie auf **OK**, bis alle Dialogfelder geschlossen sind.

### <a name="test-vpn-connectivity"></a>Testen der VPN-Konnektivität
In diesem Abschnitt bestätigen Sie, dass der VPN-Client vom RADIUS-Server authentifiziert und autorisiert ist, wenn Sie versuchen, eine Verbindung mit dem virtuellen VPN-Port herzustellen. Dieser Abschnitt setzt voraus, dass Sie Windows 10 als VPN-Client verwenden. 

>[!NOTE]
>Wenn Sie bereits einen VPN-Client zum Herstellen einer Verbindung mit dem VPN-Server konfiguriert und die Einstellungen gespeichert haben, können Sie die Schritte zum Konfigurieren und Speichern eines VPN-Verbindungsobjekts überspringen.
>

1. Klicken Sie auf dem VPN-Clientcomputer auf **Starten** und dann auf **Einstellungen** (Zahnradsymbol).
2. Klicken Sie im Fenster „Einstellungen“ auf **Netzwerk und Internet**.
3. Klicken Sie auf **VPN**.
4. Klicken Sie auf **VPN-Verbindung hinzufügen**.
5. Geben Sie in „VPN-Verbindung hinzufügen“ als VPN-Anbieter „Windows (integriert)“ an, füllen Sie dann die verbleibenden Felder nach Bedarf aus, und klicken Sie auf **Speichern**. 

 ![VPN-Verbindung hinzufügen](./media/nps-extension-vpn/image17.png)
 
6. Öffnen Sie **Netzwerk- und Freigabecenter** in der Systemsteuerung.
7. Klicken Sie auf **Adaptereinstellungen ändern**.

 ![Adaptereinstellungen ändern](./media/nps-extension-vpn/image18.png)

8. Klicken Sie mit der rechten Maustaste auf die VPN-Netzwerkverbindung, und klicken Sie auf „Eigenschaften“. 

 ![VPN-Netzwerkeigenschaften](./media/nps-extension-vpn/image19.png)

9. Klicken Sie im Dialogfeld „VPN-Eigenschaften“ auf die Registerkarte **Sicherheit**. 
10. Stellen Sie auf der Registerkarte „Sicherheit“ sicher, dass nur **Microsoft CHAP, Version 2 (MS-CHAP v2)** ausgewählt ist, und klicken Sie auf „OK“.

 ![Zulassen von Protokollen](./media/nps-extension-vpn/image20.png)

11. Klicken Sie mit der rechten Maustaste auf die VPN-Verbindung, und klicken Sie auf **Verbinden**.
12. Klicken Sie auf der Seite „Einstellungen“ auf **Verbinden**.

Eine erfolgreiche Verbindung wird im Sicherheitsprotokoll auf dem RADIUS-Server als Ereignis-ID 6272 angezeigt, wie unten dargestellt.

 ![Ereigniseigenschaften](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Leitfaden zur Problembehandlung
Angenommen, Ihre VPN-Konfiguration funktionierte vor der Konfiguration des VPN-Servers zur Verwendung eines zentralisierten RADIUS-Servers für die Authentifizierung und Autorisierung ordnungsgemäß. In diesem Fall wurde das Problem wahrscheinlich durch eine fehlerhafte Konfiguration des RADIUS-Servers oder die Verwendung eines ungültigen Benutzernamens oder Kennworts verursacht. Wenn Sie beispielsweise das alternative UPN-Suffix im Benutzernamen verwenden, könnte beim Anmeldeversuch ein Fehler auftreten (Sie sollten den gleichen Kontonamen verwenden, um beste Ergebnisse zu erzielen). 

Um diese Probleme zu beheben, ist die Untersuchung der Sicherheitsereignisprotokolle auf dem RADIUS-Server ein idealer Ausgangspunkt. Um bei der Suche nach Ereignissen Zeit zu sparen, können Sie, wie unten gezeigt, die rollenbasierte benutzerdefinierte Netzwerkrichtlinien- und Zugriffsserveransicht in der Ereignisanzeige verwenden. Ereignis-ID 6273 zeigt Ereignisse an, wo der Netzwerkrichtlinienserver einem Benutzer den Zugriff verweigert hat. 

 ![Ereignisanzeige](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Konfigurieren der Multi-Factor Authentication
Dieser Abschnitt enthält Anweisungen zum Aktivieren von Benutzern für MFA und zur Einrichtung von Konten für die Überprüfung in zwei Schritten. 

### <a name="enable-multi-factor-authentication"></a>Aktivieren der mehrstufigen Authentifizierung
In diesem Abschnitt aktivieren Sie Azure AD-Konten für MFA. Verwenden Sie das **klassische Portal**, um Benutzer für MFA zu aktivieren. 

1. Öffnen Sie einen Browser, und navigieren Sie zu [https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Melden Sie sich als Administrator an.
3. Klicken Sie im Portal im linken Navigationsbereich auf **ACTIVE DIRECTORY**.

 ![Standardverzeichnis](./media/nps-extension-vpn/image23.png)

4. Klicken Sie in der Spalte „NAME“ auf **Standardverzeichnis** (oder ein anderes Verzeichnis, falls zutreffend).
5. Klicken Sie auf der Seite „Schnellstart“ auf **Konfigurieren**.

 ![Standardkonfiguration](./media/nps-extension-vpn/image24.png)

6. Scrollen Sie auf der Seite „KONFIGURIEREN“ im Abschnitt „Multi-Factor Authentication“ auf **Diensteinstellungen verwalten**.

 ![Verwalten von MFA-Einstellungen](./media/nps-extension-vpn/image25.png)
 
7. Prüfen Sie auf der Seite „Multi-Factor Authentication“ die Standardeinstellungen für den Dienst, und klicken Sie dann auf **Benutzer**. 

 ![MFA-Benutzer](./media/nps-extension-vpn/image26.png)
 
8. Wählen Sie auf der Seite „Benutzer“ die Benutzer aus, die Sie für MFA aktivieren möchten, und klicken Sie dann auf **Aktivieren**.

 ![Eigenschaften](./media/nps-extension-vpn/image27.png)
 
9. Klicken Sie nach Aufforderung auf **Multi-Factor Auth aktivieren**.

 ![MFA aktivieren](./media/nps-extension-vpn/image28.png)
 
10. Klicken Sie auf **Schließen**. 
11. Aktualisieren Sie die Seite. Der MFA-Status wird in „Aktiviert“ geändert.

Informationen zum Aktivieren von Benutzern für Multi-Factor Authentication finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurieren von Konten für die Überprüfung in zwei Schritten
Sobald ein Konto für MFA aktiviert ist, können sich Benutzer erst dann bei Ressourcen anmelden, die der MFA-Richtlinie unterliegen, wenn sie erfolgreich mit der Überprüfung in zwei Schritten ein vertrauenswürdiges Gerät für den zweiten Authentifizierungsfaktor konfiguriert haben.

In diesem Abschnitt konfigurieren Sie ein vertrauenswürdiges Gerät für die Verwendung mit der Überprüfung in zwei Schritten. Sie können mehrere Optionen inklusive der folgenden konfigurieren:

* **Mobile App**. Sie installieren die Microsoft-Authenticator-App auf einem Windows Phone, Android- oder iOS-Gerät. Je nach den Richtlinien Ihres Unternehmens müssen Sie die App in einem von zwei Modi verwenden: „Benachrichtigungen für Überprüfungen empfangen“ (eine Benachrichtigung wird mit Push auf Ihr Gerät übertragen) oder „Prüfcode verwenden“ (Sie müssen einen Prüfcode eingeben, der alle 30 Sekunden aktualisiert wird). 
* **Mobiltelefonanruf oder SMS**. Sie können entweder einen automatisierten Telefonanruf oder eine Textnachricht erhalten. Mit der Telefonanrufoption können Sie den Anruf beantworten und die #-Taste zum Authentifizieren drücken. Mit der Textnachrichtenoption können Sie entweder auf die SMS antworten oder den Überprüfungscode im Anmeldungsfenster eingeben.
* **Anruf beim Bürotelefon**. Dieser Prozess ist mit dem oben beschriebenen für automatische Telefonanrufe identisch.

Befolgen Sie diese Anweisungen zum Einrichten eines Geräts, um die mobile App zum Empfangen von Pushbenachrichtigungen für die Überprüfung zu verwenden.

1. Melden Sie sich bei [https://aka.ms/mfasetup](https://aka.ms/mfasetup) oder einer beliebigen Site wie z.B. [https://portal.azure.com](https://portal.azure.com) an, wo Sie sich mit den MFA-fähigen Anmeldeinformationen authentifizieren müssen. 
2. Beim Anmelden mit Ihrem Benutzernamen und Kennwort wird ein Bildschirm angezeigt, in dem Sie zum Einrichten des Kontos für die zusätzliche Sicherheitsüberprüfung aufgefordert werden.

 ![Zusätzliche Sicherheit](./media/nps-extension-vpn/image29.png)

3. Klicken Sie auf **Jetzt einrichten**.
4. Wählen Sie auf der Seite „Zusätzliche Sicherheitsüberprüfung“ einen Kontakttyp (Authentifizierungstelefon, Bürotelefon oder mobile App). Wählen Sie dann ein Land oder eine Region sowie eine Methode. Die Methode variiert je nach ausgewähltem Kontakttyp. Wenn Sie z.B. „Mobile App“ wählen, können Sie wahlweise entweder Benachrichtigungen für die Überprüfung empfangen oder einen Überprüfungscode verwenden. Die folgenden Schritte gehen davon aus, dass Sie **Mobile App** als Kontakttyp wählen.

 ![Telefonauthentifizierung](./media/nps-extension-vpn/image30.png)

5. Wählen Sie „Mobile App“, klicken Sie auf **Benachrichtigungen zur Überprüfung empfangen** und dann auf **Einrichten**. 

 ![Überprüfung mit der mobilen App](./media/nps-extension-vpn/image31.png)
 
6. Falls nicht bereits geschehen, installieren Sie die mobile Authenticator-App auf Ihrem Gerät. 
7. Befolgen Sie die Anweisungen in der mobilen App, um den dargestellten Barcode zu scannen oder die Informationen manuell einzugeben, und klicken Sie dann auf **Fertig**.

 ![Konfigurieren der mobilen App](./media/nps-extension-vpn/image32.png)

8. Klicken Sie auf der Seite „Zusätzliche Sicherheitsüberprüfung“ auf **Nehmen Sie Kontakt mit mir auf**, und beantworten Sie die Benachrichtigung, die an Ihr Gerät gesendet wird.
9. Geben Sie auf der Seite „Zusätzliche Sicherheitsüberprüfung“ eine Kontaktnummer ein, falls Sie den Zugriff auf die mobile App verlieren, und klicken Sie auf **Weiter**.

 ![Mobiltelefonnummer](./media/nps-extension-vpn/image33.png)
 
10. Klicken Sie auf der Seite „Zusätzliche Sicherheitsüberprüfung“ auf **Fertig**.

Das Gerät ist jetzt konfiguriert, um eine zweite Methode der Überprüfung bereitzustellen. Informationen zum Einrichten der Konten für die Überprüfung in zwei Schritten finden Sie unter [Einrichten meines Kontos für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Installieren und Konfigurieren der NPS-Erweiterung

Dieser Abschnitt enthält Anweisungen zum Konfigurieren von VPN zur Verwendung der Azure MFA für die Clientauthentifizierung mit dem VPN-Server.

Sobald Sie die NPS-Erweiterung installiert und konfiguriert haben, muss alle von diesem Server verarbeitete RADIUS-basierte Clientauthentifizierung Azure MFA verwenden. Wenn nicht alle VPN-Benutzer bei Azure MFA registriert sind, können Sie einen anderen RADIUS-Server einrichten, um Benutzer zu authentifizieren, die nicht zur Verwendung von MFA konfiguriert sind. Alternativ können Sie einen Registrierungseintrag erstellen, der herausgeforderten Benutzern nur dann, wenn sie bei MFA registriert sind, ermöglicht, einen zweiten Authentifizierungsfaktor bereitzustellen. 

Erstellen Sie einen neuen Zeichenfolgenwert mit dem Namen _REQUIRE_USER_MATCH in HKLM\SOFTWARE\Microsoft\AzureMfa_, und legen Sie den Wert auf TRUE oder FALSE fest. 

 ![Benutzerübereinstimmung erfordern](./media/nps-extension-vpn/image34.png)
 
Wenn der Wert auf TRUE festgelegt oder nicht festgelegt ist, unterliegen alle Authentifizierungsanforderungen einer MFA-Herausforderung. Wenn der Wert auf FALSE festgelegt ist, werden MFA-Herausforderungen nur an Benutzer ausgegeben, die in MFA registriert sind. Verwenden Sie die FALSE-Einstellung nur während einer Onboardingperiode in Test- oder Produktionsumgebungen.

### <a name="acquire-azure-active-directory-guid-id"></a>Abrufen der GUID-ID von Azure Active Directory

Im Rahmen der Konfiguration der NPS-Erweiterung müssen Sie Administratoranmeldeinformationen und Azure Active Directory-ID für Ihren Azure AD-Mandanten angeben. Die folgenden Schritte veranschaulichen das Abrufen der Mandanten-ID.

1. Melden Sie sich beim Azure-Portal unter [https://portal.azure.com](https://portal.azure.com) als globaler Administrator des Azure-Mandanten an.
2. Klicken Sie im linken Navigationsbereich auf das **Azure Active Directory**-Symbol.
3. Klicken Sie auf **Eigenschaften**.
4. Um Ihre Verzeichnis-ID in die Zwischenablage zu kopieren, wählen Sie das **Kopieren**-Symbol.
 
 ![Verzeichnis-ID](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>Installieren der NPS-Erweiterung
Die NPS-Erweiterung muss auf einem Server installiert sein, auf dem die Netzwerkrichtlinien- und Zugriffsdienste-Rolle (NPS) installiert ist, und der in Ihrem Entwurf als RADIUS-Server dient. Installieren Sie die NPS-Erweiterung nicht auf Ihrem Remotedesktopserver.

1. Laden Sie die NPS-Erweiterung von [https://aka.ms/npsmfa](https://aka.ms/npsmfa) herunter. 
2. Kopieren Sie die ausführbare Setupdatei (NpsExtnForAzureMfaInstaller.exe) auf den NPS-Server.
3. Doppelklicken Sie auf dem NPS-Server auf **NpsExtnForAzureMfaInstaller.exe**. Klicken Sie nach Aufforderung auf **Ausführen**.
4. Überprüfen Sie im Dialogfeld „NPS-Erweiterung für Azure MFA“ die Softwarelizenzbedingungen, aktivieren Sie **Ich stimme den Lizenzbedingungen zu**, und klicken Sie auf **Installieren**.

 ![NPS-Erweiterung](./media/nps-extension-vpn/image36.png)
 
5. Klicken Sie im Dialogfeld „NPS-Erweiterung für Azure MFA“ auf **Schließen**.  

 ![Setup erfolgreich](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Konfigurieren von Zertifikaten für die Verwendung mit der NPS-Erweiterung mithilfe eines PowerShell-Skripts
Um die sichere Kommunikation zu gewährleisten, müssen Sie Zertifikate für die Verwendung durch die NPS-Erweiterung konfigurieren. Die NPS-Komponenten umfassen ein Windows PowerShell-Skript, das ein selbstsigniertes Zertifikat zur Verwendung mit NPS konfiguriert. 

Dieses Skript führt folgende Aktionen aus:

* Erstellen eines selbstsignierten Zertifikats
* Zuordnen des öffentlichen Schlüssels des Zertifikats zum Dienstprinzipal in Azure AD
* Speichern des Zertifikats im Speicher des lokalen Computers
* Gewähren des Zugriffs auf den privaten Schlüssel des Zertifikats für den Netzwerkbenutzer
* Neustart des Netzwerkrichtlinienserver-Diensts

Wenn Sie Ihre eigenen Zertifikate verwenden möchten, müssen Sie den öffentlichen Teil Ihres Zertifikats dem Dienstprinzipal in Azure AD zuordnen usw.
Um das Skript zu verwenden, geben Sie die Erweiterung mit Ihren Azure Active Directory-Administratoranmeldeinformationen und die Azure Active Directory-Mandanten-ID ein, die Sie zuvor kopiert haben. Führen Sie das Skript auf jedem NPS-Server aus, auf dem Sie die NPS-Erweiterung installieren.

1. Öffnen Sie eine administrative Windows PowerShell-Eingabeaufforderung.
2. Geben Sie an der PowerShell-Eingabeaufforderung _cd ‘c:\Programme\Microsoft\AzureMfa\Config’_ ein, und drücken Sie die **EINGABETASTE**.
3. Geben Sie _.\AzureMfsNpsExtnConfigSetup.ps1_ ein, und drücken Sie die **EINGABETASTE**. 
 * Das Skript überprüft, ob das Azure Active Directory PowerShell-Modul installiert ist. Wenn es nicht installiert ist, installiert das Skript das Modul für Sie.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. Nachdem das Skript die Installation des PowerShell-Moduls überprüft hat, wird das Dialogfeld des Azure Active Directory PowerShell-Moduls angezeigt. Geben Sie im Dialogfeld Ihre Azure AD-Administratoranmeldeinformationen und das Kennwort ein, und klicken Sie auf **Anmelden**. 
 
 ![PowerShell-Anmeldung](./media/nps-extension-vpn/image39.png)
 
5. Wenn Sie aufgefordert werden, fügen Sie die Mandanten-ID ein, die Sie vorher in die Zwischenablage kopiert haben, und drücken Sie die **EINGABETASTE**. 

 ![Mandanten-ID](./media/nps-extension-vpn/image40.png)

6. Das Skript erstellt ein selbstsigniertes Zertifikat und führt andere Änderungen an der Konfiguration durch. Die Ausgabe ähnelt dem folgenden Bild.

 ![Selbstsigniertes Zertifikat](./media/nps-extension-vpn/image41.png)

7. Starten Sie den Server neu.
 
### <a name="verify-configuration"></a>Überprüfen der Konfiguration
Um die Konfiguration zu überprüfen, müssen Sie eine neue VPN-Verbindung mit dem VPN-Server herstellen. Nach erfolgreicher Eingabe Ihrer Anmeldeinformationen zur primären Authentifizierung wartet die VPN-Verbindung auf die erfolgreiche sekundäre Authentifizierung, bevor die Verbindung hergestellt wird, wie unten dargestellt. 

 ![Überprüfen der Konfiguration](./media/nps-extension-vpn/image42.png)

Nach Ihrer erfolgreichen Authentifizierung mit der sekundären Überprüfungsmethode, die Sie zuvor in Azure MFA konfiguriert haben, werden Sie mit der Ressource verbunden. Falls die sekundäre Authentifizierung jedoch nicht erfolgreich ist, wird Ihnen der Zugriff auf die Ressource verweigert. 

Im folgenden Beispiel wird die Authenticator-App auf einem Windows Phone verwendet, um die sekundäre Authentifizierung bereitzustellen.

 ![Überprüfen des Kontos](./media/nps-extension-vpn/image43.png)

Nachdem Sie erfolgreich mit der zweiten Methode authentifiziert wurden, erhalten Sie Zugriff auf den virtuellen Port des VPN-Servers. Da Sie jedoch aufgefordert wurden, eine sekundäre Authentifizierungsmethode mit einer mobilen App auf einem vertrauenswürdigen Gerät zu verwenden, ist das Protokoll im Prozess sicherer, als nur bei Verwendung einer Kombination von Benutzernamen und Kennwort.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Anzeigen der Protokolle der Ereignisanzeige für erfolgreiche Anmeldeereignisse
Um die erfolgreichen Anmeldeereignisse in den Protokollen der Windows-Ereignisanzeige anzuzeigen, können Sie den folgenden Windows PowerShell-Befehl zum Abfragen der Windows-Sicherheitsprotokolle auf dem NPS-Server ausgeben.

Verwenden Sie zum Abfragen erfolgreicher Anmeldeereignisse in den Protokollen der Sicherheitsereignisanzeige den folgenden Befehl:
* _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL 

 ![Sicherheitsereignisanzeige](./media/nps-extension-vpn/image44.png)
 
Sie können auch das Sicherheitsprotokoll oder die benutzerdefinierte Ansicht der Netzwerkrichtlinien- und Zugriffsdienste anzeigen, wie unten dargestellt:

 ![Netzwerkrichtlinienzugriff](./media/nps-extension-vpn/image45.png)

Auf dem Server, auf dem Sie die NPS-Erweiterung für Azure MFA installiert haben, finden Sie spezifische Ereignisanzeige-Anwendungsprotokolle für die Erweiterung unter **Anwendungs- und Dienstprotokolle\Microsoft\AzureMfa**. 

* _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL

 ![Anzahl der Ereignisse](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Leitfaden zur Problembehandlung
Wenn die Konfiguration nicht wie erwartet funktioniert, sollten Sie die Problembehandlung idealerweise mit der Überprüfung beginnen, ob der Benutzer für die Verwendung von Azure MFA konfiguriert ist. Bitten Sie den Benutzer, eine Verbindung mit [https://portal.azure.com](https://portal.azure.com) herzustellen. Wenn er zur sekundären Authentifizierung aufgefordert wird und sich erfolgreich authentifizieren kann, können Sie eine fehlerhafte Azure MFA-Konfiguration ausschließen.

Wenn Azure MFA für den Benutzer funktioniert, prüfen Sie die relevanten Ereignisprotokolle. Dazu gehören Sicherheitsereignis-, Gatewaybetriebs- und Azure MFA-Protokolle, die im vorherigen Abschnitt erläutert wurden. 

Es folgt eine Beispielausgabe des Sicherheitsprotokolls mit einem fehlerhaften Anmeldeereignis (Ereignis-ID 6273):

 ![Sicherheitsprotokoll](./media/nps-extension-vpn/image47.png)

Es folgt ein verwandtes Ereignis aus den Azure MFA-Protokollen:

 ![Azure MFA-Protokolle](./media/nps-extension-vpn/image48.png)

Zum Ausführen erweiterter Problembehandlungsoptionen nutzen Sie die NPS-Datenbankformat-Protokolldateien dort, wo der NPS-Dienst installiert ist. Diese Protokolldateien werden im Ordner _%SystemRoot%\System32\Logs_ als durch Trennzeichen getrennte Textdateien erstellt. Eine Beschreibung dieser Protokolldateien finden Sie unter [Interpret NPS Database Format Log Files](https://technet.microsoft.com/library/cc771748.aspx) (Interpretieren von NPS-Datenbankformat-Protokolldateien). 

Die Einträge in diesen Protokolldateien sind schwierig zu interpretieren, ohne sie in eine Tabelle oder eine Datenbank zu importieren. Sie finden online eine Anzahl von IAS-Parsern, die Ihnen die Interpretation der Protokolldateien erleichtern. Im Folgenden finden Sie die Ausgabe einer solchen herunterladbaren [Sharewareanwendung](http://www.deepsoftware.com/iasviewer): 

 ![Sharewareanwendung](./media/nps-extension-vpn/image49.png)

Schließlich können Sie für zusätzliche Problembehandlungsoptionen ein Protokollanalyseprogramm wie Wireshark oder [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx) verwenden. Die folgende Abbildung aus Wireshark zeigt die zwischen VPN-Server und NPS-Server gesendeten RADIUS-Nachrichten.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Weitere Informationen finden Sie unter [Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Nächste Schritte
[Beziehen von Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Remotedesktop-Gateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)


