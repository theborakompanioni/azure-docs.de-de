---
title: "Veröffentlichen von Remotedesktops mit dem Azure AD-App-Proxy | Microsoft-Dokumentation"
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxyconnectors.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: e45d704e68c17d36fd5b195179730b80d0f53e0c
ms.lasthandoff: 04/20/2017


---

# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Veröffentlichen des Remotedesktops per Azure AD-Anwendungsproxy

In diesem Artikel wird beschrieben, wie Remotedesktopdienste (RDS) mit dem Anwendungsproxy bereitgestellt werden, damit Remotebenutzer ihre Produktivität aufrechterhalten können. 

Dieser Artikel richtet sich an folgende Zielgruppe:
- Bestehende Kunden des Azure AD-Anwendungsproxys, die durch Veröffentlichung von lokalen Anwendungen über Remotedesktopdienste mehrere Anwendungen für ihre Endbenutzer anbieten möchten. 
- Bestehende Kunden der Remotedesktopdienste, die die Angriffsfläche ihrer Bereitstellung mit dem Azure AD-Anwendungsproxy reduzieren möchten. Dieses Szenario bietet eine begrenzte Anzahl von Steuerelementen zur Überprüfung in zwei Schritten und für den bedingten Zugriff auf RDS.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Informationen zur Rolle des Anwendungsproxys bei standardmäßigen RDS-Bereitstellungen

Eine standardmäßige RDS-Bereitstellung umfasst verschiedene Remotedesktop-Rollendienste, die auf Windows Server ausgeführt werden. Die [Architektur der Remotedesktopdienste](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) bietet mehrere Bereitstellungsoptionen. Der größte Unterschied zwischen der [RDS-Bereitstellung mit Azure AD-Anwendungsproxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (in der folgenden Abbildung dargestellt) und den anderen Bereitstellungsoptionen besteht darin, dass das Anwendungsproxy-Szenario eine permanente ausgehende Verbindung vom Server aufweist, auf dem der Connectordienst ausgeführt wird. Bei anderen Bereitstellungen werden eingehende Verbindungen durch einen Lastenausgleich offen gelassen. 

![Der Anwendungsproxy befindet sich zwischen der RDS-VM und dem öffentlichen Internet.](./media/application-proxy-publish-remote-desktop/rds-with-app-proxy.png)

Bei einer RDS-Bereitstellung werden die Rollen „RD-Web“ und „RD-Gateway“ auf Computern mit Internetzugriff ausgeführt. Diese Endpunkte werden aus folgenden Gründen zur Verfügung gestellt:
- Durch RD-Web wird ein öffentlicher Endpunkt für den Benutzer bereitgestellt, um sich bei den verschiedenen lokalen Anwendungen und Desktops, auf die sie zugreifen können, anzumelden und diese anzuzeigen. Nach Auswahl einer Ressource wird eine RDP-Verbindung mithilfe der systemeigenen App im Betriebssystem erstellt.
- RD-Gateway kommt ins Spiel, sobald ein Benutzer die RDP-Verbindung startet. Das RD-Gateway verarbeitet den verschlüsselten RDP-Datenverkehr, der über das Internet eingeht, und übersetzt diesen auf dem lokalen Server, mit dem der Benutzer eine Verbindung herstellt. In diesem Szenario stammt der im RD-Gateway eingehende Datenverkehr vom Azure AD-Anwendungsproxy.

>[!TIP]
>Wenn Sie RDS zum ersten Mal bereitstellen oder weitere Informationen wünschen, bevor Sie beginnen, lesen Sie den Abschnitt [Nahtlose Bereitstellung von RDS mit Azure Resource Manager und dem Azure Marketplace](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Anforderungen

Die RD-Web- und RD-Gateway-Endpunkte müssen sich auf demselben Computer befinden und ein gemeinsames Stamm verwenden. RD-Web und RD-Gateway werden als einzelne Anwendung veröffentlicht, damit Sie über die Möglichkeit für einmaliges Anmelden zwischen den beiden Anwendungen verfügen. 

Sie sollten bereits [RDS bereitgestellt](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure) und das [Anwendungsproxy aktiviert](active-directory-application-proxy-enable.md) haben. 

Ihre Endbenutzer können nur über Windows 7- und Windows 10-Desktops, die über das RD-Webseite eine Verbindung herstellen, auf dieses Szenario zugreifen. Dieses Szenario wird nicht in anderen Betriebssystemen unterstützt, selbst bei solchen mit Microsoft-Remotedesktopanwendungen.

Ihre Endbenutzer müssen beim Herstellen einer Verbindung mit ihren Ressourcen Internet Explorer verwenden und das Add-On RDS ActiveX aktivieren. 

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Bereitstellen des gemeinsamen RDS- und Anwendungsproxy-Szenarios

Führen Sie nach der Einrichtung von RDS und des Azure AD-Anwendungsproxys für Ihre Umgebung die Schritte durch, um beide Lösungen zu kombinieren. Diese Schritte führen Sie durch die Veröffentlichung der zwei RDS-Endpunkte mit Webzugriff (RD-Web und RD-Gateway) als Anwendungen und anschließenden Weiterleitung des Datenverkehrs in RDS an das Anwendungsproxy.

### <a name="publish-the-rd-host-endpoint"></a>Veröffentlichen des RD-Hostendpunkts

1. [Veröffentlichen Sie anhand der folgenden Werten eine neue Anwendung mit Anwendungsproxy](application-proxy-publish-azure-portal.md):
   - Interne URL: https://<rdhost>.com/, wobei <rdhost> das gemeinsame Stammverzeichnis ist, den RD-Web und RD-Gateway gemeinsam nutzen. 
   - Externe URL: Dieses Feld wird automatisch basierend auf dem Namen der Anwendung ausgefüllt, den Sie jedoch ändern können. Ihre Benutzer werden beim Zugriff auf RDS an diese URL weitergeleitet. 
   - Präauthentifizierungsmethode: Azure Active Directory
   - URL-Header übersetzen: Nein
2. Weisen Sie Benutzern die veröffentlichte RD-Anwendung zu. Stellen Sie sicher, dass alle Benutzer auch Zugriff auf RDS haben.
3. Legen Sie für die Methode zur einmaligen Anmeldung für die Anwendung **Azure AD-SSO deaktiviert** fest. Ihre Benutzer werden aufgefordert, sich einmal bei Azure AD und einmal bei RD-Web zu authentifizieren, verfügen jedoch über die Möglichkeit zur einmaligen Anmeldung beim RD-Gateway. 
4. Navigieren Sie zu **Azure Active Directory** > **App-Registrierungen** > *Ihre Anwendung* > **Einstellungen**. 
5. Wählen Sie **Eigenschaften** und aktualisieren Sie das Feld **URL der Startseite**, sodass auf Ihren RD-Web-Endpunkt verwiesen wird (z.B. https://<rdhost>.com/RDWeb).

### <a name="direct-rds-traffic-to-application-proxy"></a>Weiterleiten des RDS-Datenverkehrs an den Anwendungsproxy

Stellen Sie eine Verbindung mit der RDS-Bereitstellung als Administrator her und ändern Sie den Remotedesktop-Gatewayservernamen für die Bereitstellung. Dadurch wird sichergestellt, dass Verbindungen über den Azure AD-Anwendungsproxy hergestellt werden.

1. Stellen Sie eine Verbindung zum RDS-Server her, auf dem die Rolle „Remotedesktop-Verbindungsbroker“ ausgeführt wird.
2. Starten Sie den **Server-Manager**.
3. Wählen Sie im Bereich auf der linken Seite **Remotedesktopdienste**.
4. Wählen Sie **Übersicht**.
5. Klicken Sie im Abschnitt „Bereitstellungsübersicht“ auf das Dropdownmenü und wählen Sie **Bereitstellungseigenschaften bearbeiten**.
6. Ändern Sie auf der Registerkarte „RD-Gateway“ das Feld **Servername** in die externe URL, die Sie für den RD-Hostendpunkt im Anwendungsproxy festgelegt haben. 
7. Ändern Sie das Feld **Anmeldemethode** in **Kennwortauthentifizierung**.

  ![Bildschirm „Bereitstellungseigenschaften“ in RDS](./media/application-proxy-publish-remote-desktop/rds-deployment-properties.png)

8. Führen Sie für jede Sammlung den folgenden Befehl aus. Ersetzen Sie *<yourcollectionname>* und *<proxyfrontendurl>* durch Ihre eigenen Angaben. Dieser Befehl ermöglicht eine einmalige Anmeldung zwischen RD-Web und RD-Gateway und optimiert die Leistung:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <proxyfrontendurl> `n require pre-authentication:i:1"
   ```

Nachdem Sie nun den Remotedesktop konfiguriert haben, fungiert der Azure AD-Anwendungsproxy als RDS-Komponente mit Internetzugriff. Sie können die anderen Endpunkte mit öffentlichem Internetzugriff auf Ihren RD-Web- und RD-Gateway-Computern entfernen. 

## <a name="test-the-scenario"></a>Prüfen des Szenarios

Prüfen Sie das Szenario mit Internet Explorer auf einem Windows 7- oder 10-Computer.

1. Navigieren Sie zur externen URL, die Sie eingerichtet haben, oder suchen Sie im Bereich [Meine Apps](https://myapps.microsoft.com) nach Ihrer Anwendung.
2. Sie werden aufgefordert, sich bei Azure Active Directory zu authentifizieren. Melden Sie sich mit einem Konto an, das Sie der Anwendung zugewiesen haben.
3. Sie werden aufgefordert, sich bei RD-Web zu authentifizieren. 
4. Nach Ihrer erfolgreichen RDS-Authentifizierung können Sie den gewünschten Desktop oder die gewünschte Anwendung auswählen und loslegen. 

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy](application-proxy-enable-remote-access-sharepoint.md)  
[Sicherheitsaspekte beim Remotezugriff auf Apps mit dem Azure AD-Anwendungsproxy](application-proxy-security-considerations.md)
