---
title: "Veröffentlichen des Remotedesktops per Azure Active Directory-Anwendungsproxy | Microsoft-Dokumentation"
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
ms.date: 01/12/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 40ddf7d6648561eab855b41e76a5ca509c6ca4ef
ms.openlocfilehash: 05130d728c579e0b778bdc8ec49e01f2223c2bba


---

# <a name="publish-remote-desktop-with-azure-active-directory-application-proxy"></a>Veröffentlichen des Remotedesktops per Azure Active Directory-Anwendungsproxy

In diesem Artikel erfahren Sie, wie Sie Remotebenutzern den Zugriff auf Remotedesktopbereitstellungen ermöglichen. Solche Remotedesktopbereitstellungen können sich in der lokalen Umgebung oder in privaten Netzwerken befinden (Beispiel: IaaS-Bereitstellungen). 

> [!NOTE]
> Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 
 

Der Datenverkehr des Remotedesktopprotokolls kann per Anwendungsproxy als Pass-Through-Proxyanwendung veröffentlicht werden. Diese Lösung behebt das Verbindungsproblem und bietet grundlegenden Schutz wie Netzwerkpufferung, Internet-Front-End mit verstärkter Sicherheit und DDoS-Schutz. 

##<a name="remote-desktop-deployment"></a>Remotedesktopbereitstellung

Im Rahmen der Remotedesktopbereitstellung wird das Remotedesktopgateway so veröffentlicht, dass es den RPC-über-HTTPS-Datenverkehr in RDP-über-UDP-Datenverkehr konvertieren kann.

Sie können Ihre Clients so konfigurieren, dass sie über Remotedesktopclients wie „MSTSC.exe“ auf den Azure AD-Anwendungsproxy zugreifen. Dadurch können Sie unter Verwendung der entsprechenden Connectors eine neue HTTPS-Verbindung mit dem Remotedesktopgateway herstellen. Dadurch ist das Remotedesktopgateway nicht direkt mit dem Internet verbunden, und sämtliche HTTPS-Anforderungen enden zunächst in der Cloud. 

Die entsprechende Topologie ist im folgenden Diagramm dargestellt:

 ![Lokale Azure AD-Dienste](./media/application-proxy-publish-remote-desktop/remote-desktop-topology.png)

## <a name="configure-the-remote-desktop-gateway-url"></a>Konfigurieren der Remotedesktopgateway-URL

Solange Ihre Benutzer die Remotedesktopgateway-URL konfigurieren gilt: Wenn die Benutzer wie in den meisten Fällen üblich RDP-Datenverkehr auslösen, haben sie Zugriff auf Dateien und andere Methoden.

Zur Veröffentlichung kann entweder der vom App-Proxy bereitgestellte Domänenname (msappproxy.net) oder ein benutzerdefinierter, in Azure AD konfigurierter Domänenname (beispielsweise „rdg.contoso.com“) verwendet werden. 

Falls Ihre Clientgeräte und die RDP-Datei bereits mit einer Remotedesktopgateway-URL konfiguriert sind, können Sie den gleichen Domänennamen verwenden und so die Änderung vermeiden. In diesem Fall sollte das Zertifikat für diese Domäne für den App-Proxy bereitgestellt werden, und die dazugehörige Zertifikatsperrliste muss über das Internet zugänglich sein.

Ohne konfigurierte Remotedesktopgateway-URL können Benutzer oder Administratoren die URL in Remotedesktopclients (MSTSC) mithilfe des Felds „Remotedesktopverbindung“ angeben, wie im Anschluss gezeigt.

 ![Lokale Azure AD-Dienste](./media/application-proxy-publish-remote-desktop/remote-desktop-connection-advanced.png)

Das Feld „Verbindungseinstellungen“ wird angezeigt, wenn Sie auf der Registerkarte **Erweitert** auf **Einstellungen** klicken.

 ![Lokale Azure AD-Dienste](./media/application-proxy-publish-remote-desktop/remote-desktop-connection-settings.png)

## <a name="remote-desktop-web-access"></a>Web Access für Remotedesktop

Wenn Ihre Organisation das RDWA-Portal (Web Access für Remotedesktop) verwendet, können Sie für die Veröffentlichung auch den Azure AD-Anwendungsproxy verwenden. Bei der Veröffentlichung für dieses Portal stehen Vorauthentifizierung und einmaliges Anmelden (SSO) zur Verfügung.

Die Topologie für dieses Szenario ist im folgenden Diagramm dargestellt:

 ![Lokale Azure AD-Dienste](./media/application-proxy-publish-remote-desktop/remote-desktop-web-access-portal1.png)

Im obigen Fall werden Ihre Benutzer vor dem Zugriff auf RDWA in Azure AD authentifiziert. Falls sie bereits in Azure AD authentifiziert wurden (beispielsweise bei Verwendung von Office 365), ist keine erneute Authentifizierung für RDWA erforderlich.

Wenn Ihre Benutzer die RDP-Sitzung starten, müssen sie sich über den RDP-Kanal erneut authentifizieren. Der Grund: Zum einmaligen Anmelden zwischen RDWA und Remotedesktopgateway müssen die Endbenutzer-Anmeldeinformationen per ActiveX auf dem Client gespeichert werden. Dieser Prozess wird von der formularbasierten RDWA-Authentifizierung ausgelöst. Wenn die RDWA-Authentifizierung Kerberos verwendet, erfolgt keine formularbasierte Authentifizierung. Folglich funktioniert das einmalige Anmelden von RDWA für RDP nicht.

Falls RDWA einmaliges Anmelden für den RDP-Datenverkehr benötigt oder die formularbasierte RDWA-Authentifizierung stark angepasst wurde, kann RDWA ohne Vorauthentifizierung veröffentlicht werden.

Die Topologie für dieses Szenario ist im folgenden Diagramm dargestellt:

 ![Lokale Azure AD-Dienste](./media/application-proxy-publish-remote-desktop/remote-desktop-web-access-portal2.png)

Im obigen Fall müssen sich Ihre Benutzer mittels formularbasierter Authentifizierung bei RDWA authentifizieren, die Authentifizierung muss jedoch nicht per RDP erfolgen. 

Wichtig: In beiden Fällen ist für den RDP-Datenverkehr keine Vorauthentifizierung erforderlich. Daher können Benutzer darauf zugreifen, ohne zuvor RDWA zu durchlaufen.

##<a name="next-steps"></a>Nächste Schritte

[Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy](application-proxy-enable-remote-access-sharepoint.md)<br>
[Aktivieren des Anwendungsproxys über das Azure-Portal](https://github.com/Microsoft/azure-docs-pr/blob/master/articles/active-directory/active-directory-application-proxy-enable.md)



<!--HONumber=Feb17_HO1-->


