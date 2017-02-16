---
title: 'Azure AD Connect: Passthrough-Authentifizierung | Microsoft-Dokumentation'
description: "In diesem Thema erfahren Sie, wie die Azure AD-Passthrough-Authentifizierung mit einer lokalen Active Directory-Instanz (AD) funktioniert, um Zugriff auf Azure Active Directory (Azure AD) und damit verbundene Dienste zu gewähren."
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
ms.date: 01/04/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: c692d552a649ce194591e23b30d39f434ef853eb
ms.openlocfilehash: f87be3753f1eb5a01945cd154b0ee9132e9bb43c


---

# <a name="what-is-azure-ad-pass-through-authentication"></a>Was ist die Azure AD-Passthrough-Authentifizierung?
Durch Verwendung der gleichen Anmeldeinformationen (Benutzername und Kennwort) für den Zugriff auf Ihre Unternehmensressourcen und Ihre cloudbasierten Dienste können Sie dafür sorgen, dass Ihre Benutzer sich nicht mehrere Anmeldeinformationen merken müssen. So sinkt die Wahrscheinlichkeit, dass ein Benutzer seine Anmeldedaten vergisst.  Gleichzeitig reduzieren Sie den Aufwand des Helpdesks für Kennwortzurücksetzungen.  

In vielen Organisationen ist es ausreichend, die Azure AD-[Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) zu verwenden, um Benutzern einen einzigen Satz Anmeldeinformationen für den Zugriff auf lokale und cloudbasierte Ressourcen bereitzustellen. In anderen Organisationen dagegen dürfen Kennwörter selbst in Hashformat die internen Organisationsgrenzen nicht verlassen.  

Die Azure AD-Passthrough-Authentifizierung bietet diesen Kunden eine einfache Lösung, da sie sicherstellt, dass die Kennwortüberprüfung für Azure AD-Dienste im lokalen Active Directory ausgeführt wird. Hierbei ist keine komplexe Netzwerkinfrastruktur notwendig, und die lokalen Kennwörter müssen in keiner Form in der Cloud gespeichert werden.  

In Kombination mit dem [einmaligen Anmelden](active-directory-aadconnect-sso.md) müssen die Benutzer ihr Kennwort auch nicht mehr eingeben, um sich bei Azure AD oder anderen Clouddiensten anzumelden – eine wirklich integrierte Lösung für Unternehmenscomputer.

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Die Passthrough-Authentifizierung kann über AAD Connect konfiguriert werden und nutzt einen einfachen lokalen Agent, der auf Anforderungen für eine Kennwortüberprüfung lauscht.  Der Agent lässt sich problemlos auf mehreren Computern bereitstellen, um für hohe Verfügbarkeit und Lastenausgleich zu sorgen.  Da die gesamte Kommunikation ausgehend ist, ist weder eine DMZ erforderlich, noch muss der Connector in einer DMZ installiert werden.  Folgende Computeranforderungen gelten für den Connector:

- Windows Server 2012 R2 oder höher
- Mitglied einer Domäne in der Gesamtstruktur, in der die Benutzer überprüft werden

>[!NOTE]
>Umgebungen mit mehreren Gesamtstrukturen können unterstützt werden, wenn Vertrauensstellungen zwischen den Gesamtstrukturen bestehen und das Namensuffixrouting ordnungsgemäß konfiguriert ist.

## <a name="supported-clients-in-the-preview"></a>Unterstützte Clients in der Vorschau
Die Passthrough-Authentifizierung ist in browserbasierten Clients und Office-Clients möglich, die [moderne Authentifizierungsmechanismen](https://aka.ms/modernauthga) unterstützen.  Im Fall von Clients, die eine solche Unterstützung nicht bieten, wie z.B. ältere Office-Clients oder Exchange Active Sync (also native E-Mail-Clients auf mobilen Geräten), wird Kunden empfohlen, die entsprechende äquivalente Methode zur modernen Authentifizierung zu verwenden.  Dadurch wird nicht nur die Passthrough-Authentifizierung ermöglicht, sondern auch die Anwendung eines bedingten Zugriffs, beispielsweise durch Multi-Factor Authentication.

Für Windows 10 in Azure AD wird die Passthrough-Authentifizierung zurzeit nicht unterstützt.  Kunden können jedoch für die oben erwähnten älteren Clients zusätzlich die Synchronisierung von Kennworthashs als automatisches Fallback für Windows 10 verwenden.

>[!NOTE]
>Während der Vorschau ist die Kennworthashsynchronisierung standardmäßig aktiviert, wenn die Passthrough-Authentifizierung als Anmeldeoption in Azure AD Connect ausgewählt wird. Diese Einstellung kann auf der Optionsseite von Azure AD Connect deaktiviert werden.

## <a name="how-azure-ad-pass-through-authentication-works"></a>So funktioniert die Azure AD-Passthrough-Authentifizierung
Wenn ein Benutzer seinen Benutzernamen und sein Kennwort auf der Azure AD-Anmeldeseite eingibt, stellt Azure AD diesen Namen und das Kennwort zur Überprüfung in die geeignete lokale Connectorwarteschlange.  Einer der verfügbaren lokalen Connectors ruft den Benutzernamen und das Kennwort ab und überprüft die Informationen in Active Directory.  Die Überprüfung erfolgt über Windows-Standard-APIs auf ähnliche Weise wie die Kennwortüberprüfung durch die Active Directory-Verbunddienste.

Der lokale Domänencontroller wertet die Anforderung aus und gibt eine Antwort an den Connector zurück, der diese wiederum an Azure AD zurückgibt. Azure AD wertet die Antwort aus und antwortet dem Benutzer entsprechend, z.B. durch Ausgeben eines Tokens oder Anfordern einer Multi-Factor Authentication.  Das folgende Diagramm zeigt die verschiedenen Schritte.


![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="azure-ad-pass-through-prerequisites"></a>Voraussetzungen für Azure AD-Passthrough-Authentifizierung
Bevor Sie die Azure AD-Passthrough-Authentifizierung aktivieren und verwenden können, benötigen Sie Folgendes:

- Azure AD Connect-Software.
- Ein Azure AD-Verzeichnis, in dem Sie globaler Administrator sind.  

>[!NOTE]
>Es empfiehlt sich, hierfür ein rein cloudbasiertes Administratorkonto zu verwenden, damit Sie die Konfiguration Ihres Mandanten auch dann verwalten können, wenn Ihr lokaler Dienst ausfällt oder nicht erreichbar ist.

- Ein Server unter Windows Server 2012 R2 oder höher, auf dem das AAD Connect-Tool ausgeführt wird.  Dieser Computer muss Mitglied der gleichen Gesamtstruktur sein wie der Benutzer, der überprüft werden soll.
- Beachten Sie Folgendes: Wenn Sie über mehrere Gesamtstrukturen mit Benutzern verfügen, die mit Azure AD synchronisiert werden sollen, müssen Vertrauensstellungen zwischen den Gesamtstrukturen bestehen.
- Der lokale UserPrincipalName muss als der Azure AD-Benutzername verwendet werden.
- Ein zweiter Server unter Windows Server 2012 R2 oder höher, auf dem ein zweiter Connector für Hochverfügbarkeit und Lastenausgleich ausgeführt wird.  Anweisungen zum Bereitstellen dieses Connectors finden Sie weiter unten in diesem Artikel.
- Wenn zwischen dem Connector und Azure AD eine Firewall vorhanden ist, stellen Sie Folgendes sicher:
    - Wenn die URL-Filterung aktiviert ist, stellen Sie sicher, dass der Connector mit den folgenden URLs kommunizieren kann:
        -  *.msappproxy.net
        -  *.servicebus.windows.net.  
        -  Der Connector stellt auch direkte IP-Verbindungen mit den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=41653) her.
    - Stellen Sie sicher, dass die Firewall keine SSL-Überprüfung ausführt, da der Connector Clientzertifikate zur Kommunikation mit Azure AD verwendet.
    - Stellen Sie sicher, dass der Connector über die unten stehenden Ports Anforderungen über HTTPS (TCP)an Azure AD senden kann.

|Portnummer|Beschreibung
| --- | ---
|80|Ermöglicht ausgehenden HTTP-Datenverkehr für die Sicherheitsüberprüfung, z.B. SSL-Zertifikatssperrlisten.
|443|   Ermöglicht die Benutzerauthentifizierung über Azure AD.
|8080/443|  Aktiviert die Bootstrapsequenz und die automatische Aktualisierung des Connectors.
|9090|  Ermöglicht die Connectorregistrierung (nur für den Connectorregistrierungsprozess erforderlich).
|9091|  Ermöglicht die automatische Verlängerung des Vertrauenszertifikats für den Connector.
|9352, 5671|    Ermöglicht die Kommunikation zwischen Connector und Azure AD-Dienst für eingehende Anforderungen.
|9350|  (Optional) Verbessert die Leistung für eingehende Anforderungen.
|10100 -&10120;|   Ermöglicht die Rückgabe von Antworten vom Connector an Azure AD.

Wenn Ihre Firewall Datenverkehr gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden. Stellen Sie außerdem sicher, dass Port 8080 für "NT Authority\System" aktiviert ist.

## <a name="enabling-pass-through-authentication"></a>Aktivieren der Passthrough-Authentifizierung
Die Azure AD-Passthrough-Authentifizierung wird über Azure AD Connect aktiviert.  Durch Aktivieren der Passthrough-Authentifizierung wird der erste Connector auf dem gleichen Server wie Azure AD Connect bereitgestellt.  Wählen Sie beim Installieren von Azure AD Connect eine benutzerdefinierte Installation aus, und wählen Sie auf der Seite mit den Anmeldeoptionen die Passthrough-Authentifizierung aus. Weitere Informationen finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso3.png)

Standardmäßig wird der erste Connector für die Passthrough-Authentifizierung auf dem Azure AD Connect-Server installiert.  Sie sollten einen zweiten Connector auf einem anderen Server installieren, um sicherzustellen, dass Sie Hochverfügbarkeit und Lastenausgleich für Authentifizierungsanforderungen bieten können.

Befolgen Sie die nachstehenden Anweisungen, um den zweiten Connector bereitzustellen:

### <a name="step-1-install-the-connector-without-registration"></a>Schritt 1: Installieren des Connectors ohne Registrierung

1.  Laden Sie den aktuellen [Connector](https://go.microsoft.com/fwlink/?linkid=837580) herunter.
2.  Öffnen Sie eine Eingabeaufforderung als Administrator.
3.  Führen Sie den folgenden Befehl aus, in dem die Option „/q“ für die Installation im Hintergrund steht. Das bedeutet, dass Sie bei der Installation nicht aufgefordert werden, den Endbenutzer-Lizenzvertrag zu akzeptieren.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

### <a name="step-2-register-the-connector-with-azure-ad-for-pass-through-authentication"></a>Schritt 2: Registrieren des Connectors bei Azure AD für die Passthrough-Authentifizierung

1.  Öffnen Sie ein PowerShell-Fenster als Administrator.
2.  Navigieren Sie zu „C:\Programme\Microsoft AAD App Proxy Connector“, und führen Sie das Skript aus.
.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Feature PassthroughAuthentication
3.  Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des Administratorkontos Ihres Azure AD-Mandanten ein.

## <a name="troubleshooting-pass-through-authentication"></a>Problembehandlung bei der Passthrough-Authentifizierung
Es gibt verschiedene Kategorien von Problemen, die bei der Passthrough-Authentifizierung auftreten können.  Je nachdem, welche Art von Problem auftritt, müssen Sie möglicherweise an verschiedenen Orten suchen.

Bei Fehlern in Zusammenhang mit dem Connector können Sie das Ereignisprotokoll des Connectorcomputers unter „Application and Service Logs\Microsoft\AadApplicationProxy\Connector\Admin“ prüfen.  Bei Bedarf sind ausführlichere Protokolle verfügbar, indem Sie die Analyse- und Debugprotokolle anzeigen und das Sitzungsprotokoll des Connectors aktivieren.  Beachten Sie, dass nicht empfohlen wird, dieses Protokoll standardmäßig zu aktivieren, und dass der Inhalt erst sichtbar wird, nachdem das Protokoll deaktiviert wurde.

Weitere Informationen finden Sie auch in den Ablaufprotokollen für den Connector unter „C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace“. Diese Protokolle enthalten auch den Grund dafür, dass bei der Passthrough-Authentifizierung für einen bestimmten Benutzer ein Fehler aufgetreten ist. Folgender Eintrag beispielsweise enthält den Fehlercode 1328:

    ApplicationProxyConnectorService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ

Sie können die Details zu diesem Fehler abrufen, indem Sie eine Eingabeaufforderung öffnen und folgenden Befehl ausführen. (Ersetzen Sie „1328“ durch den jeweiligen Fehlercode in der Anforderung.)

Net helpmsg 1328

Das Ergebnis sieht etwa wie folgt aus.

![Passthrough-Authentifizierung](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

Weitere Informationen finden Sie auch in den Sicherheitsprotokollen der Domänencontroller, sofern die Überwachungsprotokollierung aktiviert ist.  Eine einfache Abfrage nach Authentifizierungsanforderungen durch den Connector sieht etwa folgendermaßen aus:

    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe')]]</Select>
    </Query>
    </QueryList>

Im Folgenden finden Sie weitere Fehler, die auf dem Azure AD-Anmeldebildschirm angezeigt werden können, sowie die zugehörigen Lösungsschritte.

|Error|Beschreibung|Lösung
| --- | --- | ---
|AADSTS80001|Verbindung mit Active Directory kann nicht hergestellt werden.|Stellen Sie sicher, dass die Connectorcomputer in die Domäne eingebunden sind und eine Verbindung mit Active Directory herstellen können.  
|AADSTS8002|Bei der Verbindung mit Active Directory ist ein Timeout aufgetreten.|Überprüfen Sie, ob Active Directory verfügbar ist und auf Anforderungen des Connector antwortet.
|AADSTS80004|Der an den Connector übergebene Benutzername war ungültig.|Stellen Sie sicher, dass der Benutzer den richtigen Benutzernamen für die Anmeldung verwendet.
|AADSTS80005|Bei der Überprüfung ist eine unvorhersehbare WebException aufgetreten.|Dies ist ein vorübergehendes Problem. Versuchen Sie die Anforderung erneut.  Sollte der Fehler weiterhin auftreten, wenden Sie sich an den Microsoft-Support.
|AADSTS80007|Bei der Kommunikation mit Active Directory ist ein Fehler aufgetreten.|Suchen Sie in den Connectorprotokollen nach weiteren Informationen, und überprüfen Sie, ob Active Directory erwartungsgemäß funktioniert.



<!--HONumber=Jan17_HO1-->


