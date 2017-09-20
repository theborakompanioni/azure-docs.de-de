---
title: RADIUS-Authentifizierung und Azure MFA-Server | Microsoft-Dokumentation
description: "Dies ist die Azure Multi-Factor Authentication-Seite, die bei der Bereitstellung der RADIUS-Authentifizierung und des Azure Multi-Factor Authentication-Servers Unterstützung bietet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: kgremban
ms.reviewer: 
ms.custom: H1Hack27Feb2017, it-pro
ms.translationtype: HT
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: 4dfa56ba6f80193e643965b97b6439c62f7873e0
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>Integration der RADIUS-Authentifizierung mit dem Azure Multi-Factor Authentication-Server

RADIUS ist ein Standardprotokoll, um Authentifizierungsanforderungen zu akzeptieren und diese Anforderungen zu verarbeiten. Der Azure Multi-Factor Authentication-Server kann als RADIUS-Server fungieren. Fügen Sie ihn zwischen Ihrem RADIUS-Client (VPN-Appliance) und Ihrem Authentifizierungsziel ein, um die Überprüfung in zwei Schritten hinzuzufügen. Mögliche Authentifizierungsziele sind Active Directory, ein LDAP-Verzeichnis oder ein anderer RADIUS-Server. Damit Azure Multi-Factor Authentication (MFA) funktioniert, müssen Sie den Azure MFA-Server so konfigurieren, dass er sowohl mit den Clientservern als auch mit dem Authentifizierungsziel kommunizieren kann. Der Azure MFA-Server akzeptiert Anforderungen von einem RADIUS-Client, überprüft die Anmeldeinformationen für das Authentifizierungsziel, fügt Azure Multi-Factor Authentication hinzu, und sendet eine Antwort zurück an den RADIUS-Client. Die Authentifizierungsanforderung ist nur erfolgreich, wenn die primäre Authentifizierung und die mehrstufige Authentifizierung per Azure Multi-Factor Authentication erfolgreich verlaufen sind.

> [!NOTE]
> Der MFA-Server unterstützt die RADIUS-Protokolle PAP (Kennwortauthentifizierungsprotokoll) und MSCHAPv2 (Microsoft Challenge-Handshake Authentication-Protokoll) nur, wenn er als RADIUS-Server agiert.  Andere Protokolle, z.B. EAP (Extensible Authentication-Protokoll), können verwendet werden, wenn der MFA-Server als RADIUS-Proxy zu einem anderen RADIUS-Server fungiert, der dieses Protokoll unterstützt.
>
> Bei dieser Konfiguration funktionieren unidirektionale SMS- und OATH-Token nicht, da der MFA-Server mit alternativen Protokollen keinen erfolgreichen RADIUS-Abfrage/Rückmeldung-Vorgang initiieren kann.

![RADIUS-Authentifizierung](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>Hinzufügen eines RADIUS-Clients
Zum Konfigurieren der RADIUS-Authentifizierung installieren Sie den Azure Multi-Factor Authentication-Server auf einem Windows-Server. Wenn Sie über eine Active Directory-Umgebung verfügen, sollte der Server der Domäne innerhalb des Netzwerks angehören. Verwenden Sie das folgende Verfahren, um den Azure Multi-Factor Authentication-Server zu konfigurieren:

1. Klicken Sie auf dem Azure Multi-Factor Authentication-Server im linken Menü auf das Symbol „RADIUS-Authentifizierung“.
2. Aktivieren Sie das Kontrollkästchen **RADIUS-Authentifizierung aktivieren**.
3. Ändern Sie auf der Registerkarte „Clients“ die Ports für Authentifizierung und Abrechnung, wenn der Azure MFA-RADIUS-Dienst über andere Ports als Standardports auf RADIUS-Anforderungen lauschen muss.
4. Klicken Sie auf **Hinzufügen**.
5. Geben Sie IP-Adresse der Appliance/des Servers, die bzw. der auf dem Azure Multi-Factor Authentication-Server authentifiziert wird, sowie einen Anwendungsnamen (optional) und ein gemeinsames Geheimnis ein.

  Der Anwendungsname wird in Berichten und ggf. auch in Authentifizierungsnachrichten (SMS oder mobile App) angezeigt.

  Das gemeinsame Geheimnis muss auf dem Azure Multi-Factor Authentication-Server und in der Appliance bzw. auf dem Server identisch sein.

6. Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication-Benutzerabgleich erfordern**, wenn alle Benutzer in den Server importiert wurden und die Multi-Factor Authentication verwenden. Wenn viele Benutzer noch nicht in den Server importiert wurden oder von der Überprüfung in zwei Schritten ausgenommen sind, lassen Sie das Kontrollkästchen deaktiviert.
7. Aktivieren Sie das Kontrollkästchen **Alternatives OATH-Token aktivieren**, wenn Sie OATH-Kennungen aus Apps für die mobile Verifizierung als Ausweichmethode verwenden möchten.
8. Klicken Sie auf **OK**.

Wiederholen Sie die Schritte 4 bis 8, um so viele zusätzliche RADIUS-Clients wie erforderlich hinzuzufügen.

## <a name="configure-your-radius-client"></a>Konfigurieren des RADIUS-Clients

1. Klicken Sie auf die Registerkarte **Ziel**.
2. Wenn der Azure MFA-Server auf einem in die Domäne eingebundenen Server in einer Active Directory-Umgebung installiert ist, wählen Sie die Windows-Domäne.
3. Wenn der Benutzer mit einem LDAP-Verzeichnis authentifiziert werden sollen, wählen Sie die **LDAP-Bindung**.

  Wählen Sie das Symbol für die Verzeichnisintegration aus, und bearbeiten Sie auf der Registerkarte „Einstellungen“ die LDAP-Konfiguration, damit der Server die Bindung mit Ihrem Verzeichnis herstellen kann. Anweisungen zum Konfigurieren von LDAP finden Sie im [Konfigurationshandbuch zum LDAP-Proxy](multi-factor-authentication-get-started-server-ldap.md).

4. Wenn Benutzer für einen anderen RADIUS-Server authentifiziert werden sollen, wählen Sie den/die RADIUS-Server.
5. Klicken Sie auf **Hinzufügen**, um den Server zu konfigurieren, an den der Azure MFA-Server die RADIUS-Anforderungen per Proxy sendet.
6. Geben Sie im Dialogfeld „RADIUS-Server hinzufügen“ die IP-Adresse des RADIUS-Servers und ein gemeinsames Geheimnis ein.

  Das gemeinsame Geheimnis muss auf dem Azure Multi-Factor Authentication-Server und dem RADIUS-Server identisch sein. Ändern Sie den Authentifizierungs- und Kontoführungsport, wenn der RADIUS-Server andere Ports verwendet.

7. Klicken Sie auf **OK**.
8. Fügen Sie den Azure MFA-Server als RADIUS-Client dem anderen RADIUS-Server hinzu, damit dieser die Zugriffsanforderungen verarbeiten kann, die er vom Azure MFA-Server erhält. Verwenden Sie das gleiche gemeinsame Geheimnis, das im Azure Multi-Factor Authentication-Server konfiguriert ist.

Wiederholen Sie diese Schritte, um weitere RADIUS-Server hinzuzufügen. Konfigurieren Sie mithilfe der Schaltflächen **Nach oben** und **Nach unten** die Reihenfolge, in der der Azure MFA-Server die Server aufrufen soll.

Der Azure Multi-Factor Authentication-Server ist nun konfiguriert. Der Server wartet jetzt auf den konfigurierten Ports auf RADIUS-Zugriffsanforderungen von den konfigurierten Clients.   

## <a name="radius-client-configuration"></a>RADIUS-Clientkonfiguration
Verwenden Sie die folgenden Richtlinien, um den RADIUS-Client zu konfigurieren:

* Konfigurieren Sie Ihre Appliance/Ihren Server zur Authentifizierung über RADIUS mit der IP-Adresse des Azure Multi-Factor Authentication-Servers, der als RADIUS-Server fungiert.
* Verwenden Sie das gleiche gemeinsame Geheimnis, das weiter oben konfiguriert wurde.
* Konfigurieren Sie den RADIUS-Timeout auf 30 bis 60 Sekunden, damit genug Zeit ist, die Anmeldeinformationen des Benutzers zu überprüfen, die zweistufige Authentifizierung durchzuführen, die Antwort zu empfangen und dann auf die RADIUS-Zugriffsanforderung zu reagieren.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die [Integration in die RADIUS-Authentifizierung](multi-factor-authentication-nps-extension.md), wenn Sie über Azure Multi-Factor Authentication in der Cloud verfügen. 
