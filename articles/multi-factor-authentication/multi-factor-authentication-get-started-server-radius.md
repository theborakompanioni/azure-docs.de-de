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
ms.date: 02/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 20afeb3ba290ddf728d2b52c076c7a57fadc77c6
ms.openlocfilehash: e696b95c9db86b062440f0c4fd788bf97223317a
ms.contentlocale: de-de
ms.lasthandoff: 02/28/2017

---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>Integration der RADIUS-Authentifizierung mit dem Azure Multi-Factor Authentication-Server
Verwenden Sie den Abschnitt „RADIUS-Authentifizierung“ des Azure MFA-Servers, um die RADIUS-Authentifizierung zu aktivieren und zu konfigurieren. RADIUS ist ein Standardprotokoll, um Authentifizierungsanforderungen zu akzeptieren und diese Anforderungen zu verarbeiten. Der Azure Multi-Factor Authentication-Server dient als RADIUS-Server. Fügen Sie ihn zwischen Ihrem RADIUS-Client (VPN-Appliance) und Ihrem Authentifizierungsziel ein. Dies kann eine Active Directory-Instanz (AD), ein LDAP-Verzeichnis oder ein anderer RADIUS-Server sein, um Azure Multi-Factor Authentication hinzuzufügen. Damit Azure Multi-Factor Authentication (MFA) funktioniert, müssen Sie den Azure MFA-Server so konfigurieren, dass er sowohl mit den Clientservern als auch mit dem Authentifizierungsziel kommunizieren kann. Der Azure MFA-Server akzeptiert Anforderungen von einem RADIUS-Client, überprüft die Anmeldeinformationen für das Authentifizierungsziel, fügt Azure Multi-Factor Authentication hinzu, und sendet eine Antwort zurück an den RADIUS-Client. Die Authentifizierungsanforderung ist nur erfolgreich, wenn die primäre Authentifizierung und die mehrstufige Authentifizierung per Azure Multi-Factor Authentication erfolgreich verlaufen sind.

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

  Der Anwendungsname wird in Azure Multi-Factor Authentication-Berichten und unter Umständen auch in Authentifizierungsnachrichten in SMS oder der mobilen App angezeigt.

  Das gemeinsame Geheimnis muss auf dem Azure Multi-Factor Authentication-Server und in der Appliance bzw. auf dem Server identisch sein.

6. Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication-Benutzerabgleich erfordern**, wenn alle Benutzer in den Server importiert wurden oder werden und die Multi-Factor Authentication verwenden. Wenn eine hohe Anzahl von Benutzern noch nicht in den Server importiert wurde und/oder von der Prüfung in zwei Schritten ausgenommen werden soll, lassen Sie das Kontrollkästchen deaktiviert.
7. Aktivieren Sie das Kontrollkästchen **Alternatives OATH-Token aktivieren**, wenn Sie OATH-Kennungen von Apps für die mobile Verifizierung als Fallback für den Out-of-Band-Telefonanruf, die SMS oder die Pushbenachrichtigung verwenden möchten.
8. Klicken Sie auf **OK**.

Wiederholen Sie die Schritte 4 bis 8, um so viele zusätzliche RADIUS-Clients wie erforderlich hinzuzufügen.

## <a name="configure-your-radius-client"></a>Konfigurieren des RADIUS-Clients

1. Klicken Sie auf die Registerkarte **Ziel**.
2. Wenn der Azure MFA-Server auf einem in die Domäne eingebundenen Server in einer Active Directory-Umgebung installiert ist, wählen Sie die Windows-Domäne.
3. Wenn der Benutzer mit einem LDAP-Verzeichnis authentifiziert werden sollen, wählen Sie die **LDAP-Bindung**.

  Zum Verwenden von LDAP-Bindungen müssen Sie auf das Symbol „Verzeichnisintegration“ klicken und die LDAP-Konfiguration auf der Registerkarte „Einstellungen“ bearbeiten, damit der Server die Bindung mit Ihrem Verzeichnis herstellen kann. Anweisungen zum Konfigurieren von LDAP finden Sie im [Konfigurationshandbuch zum LDAP-Proxy](multi-factor-authentication-get-started-server-ldap.md).

4. Wenn Benutzer für einen anderen RADIUS-Server authentifiziert werden sollen, wählen Sie den/die RADIUS-Server.
5. Klicken Sie auf **Hinzufügen**, um den Server zu konfigurieren, an den der Azure MFA-Server die RADIUS-Anforderungen per Proxy sendet.
6. Geben Sie im Dialogfeld „RADIUS-Server hinzufügen“ die IP-Adresse des RADIUS-Servers und ein gemeinsames Geheimnis ein.

  Das gemeinsame Geheimnis muss auf dem Azure Multi-Factor Authentication-Server und dem RADIUS-Server identisch sein. Ändern Sie den Authentifizierungs- und Kontoführungsport, wenn der RADIUS-Server andere Ports verwendet.

7. Klicken Sie auf **OK**.
8. Fügen Sie den Azure MFA-Server als RADIUS-Client dem anderen RADIUS-Server hinzu, damit dieser die Zugriffsanforderungen verarbeiten kann, die er vom Azure MFA-Server erhält. Verwenden Sie das gleiche gemeinsame Geheimnis, das im Azure Multi-Factor Authentication-Server konfiguriert ist.

Wiederholen Sie diese Schritte, um weitere RADIUS-Server hinzuzufügen, und konfigurieren Sie die Reihenfolge, in der der Azure MFA-Server diese mit den Schaltflächen **Nach oben** und **Nach unten** aufrufen soll.

Dadurch wird die Konfiguration des Azure Multi-Factor Authentication-Servers abgeschlossen. Der Server wartet jetzt auf den konfigurierten Ports auf RADIUS-Zugriffsanforderungen von den konfigurierten Clients.   

## <a name="radius-client-configuration"></a>RADIUS-Clientkonfiguration
Verwenden Sie die folgenden Richtlinien, um den RADIUS-Client zu konfigurieren:

* Konfigurieren Sie Ihre Appliance/Ihren Server zur Authentifizierung über RADIUS an die IP-Adresse des Azure Multi-Factor Authentication-Servers, die als RADIUS-Server fungiert.
* Verwenden Sie das gleiche gemeinsame Geheimnis, das weiter oben konfiguriert wurde.
* Konfigurieren Sie den RADIUS-Timeout auf 30 bis 60 Sekunden, damit genug Zeit ist, die Anmeldeinformationen des Benutzers zu überprüfen, die zweistufige Authentifizierung durchzuführen, die Antwort zu empfangen und dann auf die RADIUS-Zugriffsanforderung zu reagieren.

