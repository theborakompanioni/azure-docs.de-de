---
title: IIS-Authentifizierung und Azure MFA-Server | Microsoft-Dokumentation
description: "Dies ist die Azure Multi-Factor Authentication-Seite, die bei der Bereitstellung der IIS-Authentifizierung und des Azure Multi-Factor Authentication-Servers Unterstützung bietet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d1bf1c8a-2c10-4ae6-9f4b-75f0c3df43eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: b1841aea07140a855a1978e7c3141f76aa2d8862
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>Konfigurieren des Azure Multi-Factor Authentication-Servers für IIS-Web-Apps

Verwenden Sie den Bereich „IIS-Authentifizierung“ des Azure Multi-Factor Authentication-Servers (MFA), um die IIS-Authentifizierung für die Integration mit Microsoft IIS-Webanwendungen zu aktivieren und zu konfigurieren. Vom Azure MFA-Server wird ein Plug-In installiert, mit dem an den IIS-Webserver gesendete Anforderungen gefiltert werden können, um Azure Multi-Factor Authentication hinzuzufügen. Das IIS-Plug-In bietet Unterstützung für die formularbasierte Authentifizierung und die integrierte Windows-HTTP-Authentifizierung. Vertrauenswürdige IP-Adressen können auch konfiguriert werden, um interne IP-Adressen der zweistufigen Authentifizierung auszunehmen.

![IIS-Authentifizierung](./media/multi-factor-authentication-get-started-server-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Verwenden formularbasierter IIS-Authentifizierung mit dem Azure Multi-Factor Authentication-Server
Um eine IIS-Webanwendung zu sichern, die die formularbasierte Authentifizierung verwendet, installieren Sie den Azure Multi-Factor Authentication-Server auf dem IIS-Webserver, und konfigurieren Sie den Server wie im folgenden Verfahren dargestellt:

1. Klicken Sie auf dem Azure Multi-Factor Authentication-Server im linken Menü auf das Symbol „IIS-Authentifizierung“.
2. Klicken Sie auf die Registerkarte **Formularbasiert**.
3. Klicken Sie auf **Hinzufügen**.
4. Damit Benutzername, Kennwort und Domänenvariablen automatisch erkannt werden, geben Sie die Anmelde-URL (z.B. „https://localhost/contoso/auth/login.aspx“) im Dialogfeld „Formularbasierte Website automatisch konfigurieren“ ein, und klicken Sie auf **OK**.
5. Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication-Benutzerabgleich erfordern**, wenn alle Benutzer in den Server importiert wurden oder werden und die Multi-Factor Authentication verwenden. Wenn eine große Anzahl von Benutzern noch nicht in den Server importiert wurde und/oder von der Multi-Factor Authentication ausgenommen werden soll, lassen Sie das Kontrollkästchen deaktiviert.
6. Wenn die Seitenvariablen nicht automatisch erkannt werden können, klicken Sie im Dialogfeld „Formularbasierte Website automatisch konfigurieren“ auf **Manuell angeben**.
7. Geben Sie im Dialogfeld "Formularbasierte Website hinzufügen" im Feld "Sende-URL" die URL der Anmeldeseite und optional einen Anwendungsnamen ein. Der Anwendungsname wird in Azure Multi-Factor Authentication-Berichten und möglicherweise auch in Authentifizierungsnachrichten in SMS oder der mobilen App angezeigt.
8. Wählen Sie das richtige Anforderungsformat. Dieses ist für die meisten Webanwendungen auf **POST oder GET** festgelegt.
9. Geben Sie die Variablen für den Benutzernamen, das Kennwort und die Domäne ein (sofern auf der Anmeldeseite angezeigt). Navigieren Sie in einem Webbrowser zur Anmeldeseite, klicken Sie mit der rechten Maustaste auf die Seite, und wählen Sie **Quelltext anzeigen** aus, um die Namen der Eingabefelder zu ermitteln.
10. Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication-Benutzerabgleich erfordern**, wenn alle Benutzer in den Server importiert wurden bzw. werden und die Multi-Factor Authentication verwenden. Wenn eine große Anzahl von Benutzern noch nicht in den Server importiert wurde und/oder von der Multi-Factor Authentication ausgenommen werden soll, lassen Sie das Kontrollkästchen deaktiviert.
11. Klicken Sie auf **Erweitert**, um die erweiterten Einstellungen anzuzeigen, z.B.:

  - Sie können eine Datei für eine benutzerdefinierte Verweigerungsseite auswählen.
  - Sie können erfolgreiche Authentifizierungen für die Website mithilfe von Cookies für einen bestimmten Zeitraum zwischenspeichern.
  - Geben Sie an, ob die Authentifizierung der primären Anmeldeinformationen für eine Windows-Domäne, ein LDAP-Verzeichnis oder einen RADIUS-Server durchgeführt werden soll.

12. Klicken Sie auf **OK**, um zum Dialogfeld „Formularbasierte Website hinzufügen“ zurückzukehren.
13. Klicken Sie auf **OK**.
14. Sobald die URL und die Seitenvariablen erkannt bzw. eingegeben wurden, werden die Websitedaten im Bereich „Formularbasiert“ angezeigt.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Verwenden der integrierten Windows-Authentifizierung mit dem Azure Multi-Factor Authentication-Server
Um eine IIS-Webanwendung zu sichern, für die die integrierte Windows-HTTP-Authentifizierung verwendet wird, installieren Sie den Azure MFA-Server auf dem IIS-Webserver und konfigurieren den Server dann mit den folgenden Schritten:

1. Klicken Sie auf dem Azure Multi-Factor Authentication-Server im linken Menü auf das Symbol „IIS-Authentifizierung“.
2. Klicken Sie auf die Registerkarte **HTTP**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie im Dialogfeld „Basis-URL hinzufügen“ im Feld „Basis-URL“ die URL für die Website, auf der die HTTP-Authentifizierung erfolgt (z.B. „http://localhost/owa“), und einen Anwendungsnamen (optional) ein. Der Anwendungsname wird in Azure Multi-Factor Authentication-Berichten und möglicherweise auch in Authentifizierungsnachrichten in SMS oder der mobilen App angezeigt.
5. Passen Sie die Leerlaufzeitüberschreitung und die maximale Sitzungsdauer an, wenn die Standardwerte nicht ausreichend sind.
6. Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication-Benutzerabgleich erfordern**, wenn alle Benutzer in den Server importiert wurden oder werden und die Multi-Factor Authentication verwenden. Wenn eine große Anzahl von Benutzern noch nicht in den Server importiert wurde und/oder von der Multi-Factor Authentication ausgenommen werden soll, lassen Sie das Kontrollkästchen deaktiviert.
7. Aktivieren Sie bei Bedarf das Kontrollkästchen für das **Cookie zur** Zwischenspeicherung.
8. Klicken Sie auf **OK**.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Aktivieren des IIS-Plug-Ins für den Azure Multi-Factor Authentication-Server
Nachdem Sie die URLs und Einstellungen für die formularbasierte oder die HTTP-Authentifizierung konfiguriert haben, wählen Sie die Speicherorte aus, an denen die IIS-Plug-Ins von Azure Multi-Factor Authentication in IIS geladen und aktiviert werden sollen. Gehen Sie dazu wie folgt vor:

1. Klicken Sie bei Ausführung unter IIS 6 auf die Registerkarte **ISAPI**. Wählen Sie die Website aus, auf der die Webanwendung läuft (z.B. die Standardwebsite), um das ISAPI-Filter-Plug-In von Azure Multi-Factor Authentication für diese Website zu aktivieren.
2. Klicken Sie bei Ausführung unter IIS 7 oder höher auf die Registerkarte **Systemeigenes Modul**. Wählen Sie den Server, die Websites oder die Anwendungen aus, um das IIS-Plug-In auf den gewünschten Ebenen zu aktivieren.
3. Aktivieren Sie im oberen Bildschirmbereich das Kontrollkästchen **IIS-Authentifizierung aktivieren**. Die ausgewählte IIS-Anwendung wird jetzt mit Azure Multi-Factor Authentication gesichert. Stellen Sie sicher, dass die Benutzer in den Server importiert wurden.

## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen
Über vertrauenswürdige IP-Adressen können Benutzer Azure Multi-Factor Authentication bei Websiteanforderungen umgehen, die von bestimmten IP-Adressen oder Subnetzen stammen. Beispielsweise können Benutzer, die sich vom Büro aus anmelden, von Azure Multi-Factor Authentication ausschließen. Dazu geben Sie das Bürosubnetz als Eintrag unter „Vertrauenswürdige IPs“ an. Verwenden Sie zum Konfigurieren von vertrauenswürdigen IP-Adressen das folgende Verfahren:

1. Klicken Sie im Abschnitt „IIS-Authentifizierung“ auf die Registerkarte **Vertrauenswürdige IPs**.
2. Klicken Sie auf **Hinzufügen**.
3. Im daraufhin angezeigten Dialogfeld „Vertrauenswürdige IP hinzufügen“ wählen Sie das Optionsfeld **Eine IP**, **IP-Bereich** oder **Subnetz** aus.
4. Geben Sie die IP-Adresse, den IP-Adressbereich oder das Subnetz ein, das der Positivliste hinzugefügt werden soll. Wählen Sie im Falle eines Subnetzes die entsprechende Netzmaske aus, und klicken Sie auf **OK**. Der Eintrag wird der Positivliste hinzugefügt.

