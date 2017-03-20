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
ms.sourcegitcommit: 0d9f2f5c9ad5c47eb5e1a8a8ec3fee2952caef63
ms.openlocfilehash: 5c01f89c335a6c85b5b50a4c8491ae1b02740578
ms.lasthandoff: 02/27/2017


---

# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Veröffentlichen des Remotedesktops per Azure AD-Anwendungsproxy

In diesem Artikel erfahren Sie, wie Sie Remotebenutzern den Zugriff auf Windows-Remotedesktopbereitstellungen ermöglichen. Remotedesktopbereitstellungen können sich in der lokalen Umgebung oder in privaten Netzwerken befinden (Beispiel: IaaS-Bereitstellungen).

> [!NOTE]
> Das Anwendungsproxy-Feature ist nur verfügbar, wenn Sie Azure Active Directory (Azure AD) auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Der Datenverkehr des Remotedesktopprotokolls (RDP) kann per Azure AD-Anwendungsproxy als Pass-Through-Proxyanwendung veröffentlicht werden. Diese Lösung behebt das Verbindungsproblem und bietet grundlegenden Schutz wie Netzwerkpufferung, Internet-Front-End mit verstärkter Sicherheit und verteiltem Denial-of-Service-Schutz (DDoS).

## <a name="remote-desktop-deployment"></a>Remotedesktopbereitstellung

Im Rahmen der Remotedesktopbereitstellung wird das Remotedesktopgateway veröffentlicht, sodass es den RPC (Remoteprozeduraufruf)-über-HTTPS-Datenverkehr in RDP-über-UDP (User Datagram-Protokoll)-Datenverkehr konvertieren kann.

Sie können die Clients so konfigurieren, dass sie über Remotedesktopclients wie „MSTSC.exe“ auf den Azure AD-Anwendungsproxy zugreifen. So erstellen Sie unter Verwendung der entsprechenden Connectors eine neue HTTPS-Verbindung mit dem Remotedesktopgateway. Dadurch ist das Gateway nicht direkt mit dem Internet verbunden, und sämtliche HTTPS-Anforderungen enden zunächst in der Cloud.

Die Topologie wird im folgenden Diagramm veranschaulicht:

 ![Lokale Azure AD-Dienste – Diagramm](./media/application-proxy-publish-remote-desktop/remote-desktop-topology.png)

## <a name="configure-the-remote-desktop-gateway-url"></a>Konfigurieren der Remotedesktopgateway-URL

Wenn Benutzer die Remotedesktopgateway-URL konfigurieren und wie in den meisten Fällen üblich RDP-Datenverkehr auslösen, haben sie Zugriff auf Dateien und andere Methoden.

Zur Veröffentlichung können Sie entweder den vom Anwendungsproxy bereitgestellten Domänennamen (msappproxy.net) oder einen benutzerdefinierteren, in Azure AD konfigurierten Domänenname (beispielsweise „rdg.contoso.com“) verwenden.

Falls die Clientgeräte und die RDP-Datei bereits mit einer Remotedesktopgateway-URL konfiguriert sind, können Sie den gleichen Domänennamen verwenden und so die Änderung vermeiden. In diesem Fall sollte das Zertifikat für diese Domäne für den Anwendungsproxy bereitgestellt werden, und die dazugehörige Zertifikatsperrliste (Certificate Revocation List, CRL) muss über das Internet zugänglich sein.

Ohne konfigurierte Remotedesktopgateway-URL können Benutzer oder Administratoren wie hier gezeigt die URL in Remotedesktopclients (MSTSC) mithilfe des Felds „Remotedesktopverbindung“ angeben.

 ![Dialogfeld „Remotedesktopverbindung“](./media/application-proxy-publish-remote-desktop/remote-desktop-connection-advanced.png)

Das Dialogfeld **Verbindungseinstellungen** wird angezeigt, wenn Sie auf der Registerkarte **Erweitert** auf **Einstellungen** klicken.

 ![Fenster „Verbindungseinstellungen“ im Dialogfeld „Remotedesktopverbindung“](./media/application-proxy-publish-remote-desktop/remote-desktop-connection-settings.png)

## <a name="remote-desktop-web-access"></a>Web Access für Remotedesktop

Wenn Ihre Organisation das RDWA-Portal (Web Access für Remotedesktop) verwendet, können Sie für die Veröffentlichung auch den Azure AD-Anwendungsproxy verwenden. Bei der Veröffentlichung für dieses Portal stehen Vorauthentifizierung und einmaliges Anmelden (SSO) zur Verfügung.

Die Topologie des RDWA-Szenarios wird im folgenden Diagramm veranschaulicht:

 ![Diagramm des RDWA-Szenarios](./media/application-proxy-publish-remote-desktop/remote-desktop-web-access-portal1.png)

Im oben genannten Fall werden Benutzer vor dem Zugriff auf RDWA in Azure AD authentifiziert. Falls sie bereits in Azure AD authentifiziert wurden (beispielsweise bei Verwendung von Office 365), ist keine erneute Authentifizierung für RDWA erforderlich.

Wenn Benutzer die RDP-Sitzung starten, müssen sie sich über den RDP-Kanal erneut authentifizieren. Der Grund: Zum einmaligen Anmelden zwischen RDWA und Remotedesktopgateway müssen die Benutzeranmeldeinformationen mithilfe von ActiveX auf dem Client gespeichert werden. Dieser Prozess wird von der formularbasierten RDWA-Authentifizierung ausgelöst. Wenn die RDWA-Authentifizierung Kerberos verwendet, erfolgt keine formularbasierte Authentifizierung. Folglich funktioniert das einmalige Anmelden von RDWA für RDP nicht.

Falls RDWA einmaliges Anmelden für den RDP-Datenverkehr benötigt oder die formularbasierte RDWA-Authentifizierung stark angepasst wurde, kann können Sie RDWA ohne Vorauthentifizierung veröffentlichen.

Die Topologie dieses Szenarios wird im folgenden Diagramm veranschaulicht:

 ![Diagramm des RDWA-Szenarios](./media/application-proxy-publish-remote-desktop/remote-desktop-web-access-portal2.png)

Im oben genannten Fall müssen sich Benutzer mittels formularbasierter Authentifizierung bei RDWA authentifizieren, die Authentifizierung muss jedoch nicht per RDP erfolgen.

>[!NOTE]
>In beiden oben genannten Fällen ist keine Vorauthentifizierung für den RDP-Datenverkehr erforderlich. Daher können Benutzer darauf zugreifen, ohne zuvor RDWA zu durchlaufen.

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy](application-proxy-enable-remote-access-sharepoint.md)  
[Aktivieren des Anwendungsproxys über das Azure-Portal](https://github.com/Microsoft/azure-docs-pr/blob/master/articles/active-directory/active-directory-application-proxy-enable.md)

