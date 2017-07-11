---
title: Erste Schritte mit Azure Multi-Factor Authentication-Server | Microsoft-Dokumentation
description: Auf dieser Seite zur Azure Multi-Factor Authentication werden die ersten Schritte mit dem Azure MFA-Server beschrieben.
services: multi-factor-authentication
keywords: Authentifizierungsserver, Aktivierungsseite der Azure Multi-Factor Authentication-App, Download Authentifizierungsserver
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 4235dfd0e17b9892787dd86d807b8f1f6e360675
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

<a id="getting-started-with-the-azure-multi-factor-authentication-server" class="xliff"></a>

# Erste Schritte mit Azure Multi-Factor Authentication-Server

<center>![Lokale MFA](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nachdem wir nun geklärt haben, dass wir den lokalen Multi-Factor Authentication-Server verwenden möchten, können wir mit der Einrichtung beginnen. Auf dieser Seite werden eine Neuinstallation des Servers und die Einrichtung mit einer lokalen Active Directory-Instanz behandelt. Falls Sie den MFA-Server bereits installiert haben und ein Upgrade vornehmen möchten, lesen Sie unter [Upgrade to the latest Azure Multi-Factor Authentication Server](multi-factor-authentication-server-upgrade.md) (Upgraden auf den neuesten Azure Multi-Factor Authentication-Server) weiter. Falls Sie nur den Webdienst installieren möchten, lesen Sie unter [Aktivieren der Mobile App-Authentifizierung mit dem Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-server-webservice.md) weiter.
 
<a id="plan-your-deployment" class="xliff"></a>

## Planen der Bereitstellung

Bevor Sie den Azure Multi-Factor Authentication-Server herunterladen, sollten Sie überlegen, wie Ihre Last- und Hochverfügbarkeitsanforderungen sind. Verwenden Sie diese Informationen, um zu entscheiden, wie und wo Sie bereitstellen möchten. 

Ein guter Anhaltspunkt für den benötigten Speicherplatz ist die Anzahl der Benutzer, von denen Sie erwarten, dass sie sich regelmäßig authentifizieren. 

| Benutzer | RAM |
| ----- | --- |
| 1-10,000 | 4 GB |
| 10,001-50,000 | 8 GB |
| 50,001-100,000 | 12 GB |
| 100,000-200,001 | 16 GB |
| 200,001+ | 32 GB |

Müssen Sie mehrere Server für hohe Verfügbarkeit oder Lastenausgleich einrichten? Es gibt zahlreiche Möglichkeiten, um diese Konfiguration mit Azure MFA-Server einzurichten. Wenn Sie Ihren ersten Azure MFA-Server installieren, wird dieser der Master. Alle weiteren Server werden untergeordnet und synchronisieren Benutzer und die Konfiguration automatisch mit dem Master. Anschließend können Sie einen primären Server konfigurieren und die übrigen als Sicherung verwenden, oder Lastenausgleich zwischen allen Servern einrichten. 

Wenn ein Azure MFA-Masterserver offline geschaltet wird, können die untergeordnete Server weiterhin Anforderungen für die Überprüfung in zwei Schritten verarbeiten. Sie können jedoch keine neuen Benutzer hinzufügen, und vorhandene Benutzer können ihre Einstellungen nicht aktualisieren, bis der Master wieder online geschaltet oder ein untergeordneter Server höher gestuft wird. 

<a id="prepare-your-environment" class="xliff"></a>

## Vorbereiten der Umgebung

Stellen Sie sicher, dass der Server, den Sie für Azure Multi-Factor Authentication verwenden, die folgenden Anforderungen erfüllt:

| Anforderungen an den Azure Multi-Factor Authentication-Server | Beschreibung |
|:--- |:--- |
| Hardware |<li>200 MB Festplattenspeicher</li><li>x32- oder x64-fähiger Prozessor</li><li>1 GB oder mehr RAM</li> |
| Software |<li>Windows Server 2008 oder höher, wenn der Host ein Serverbetriebssystem ist</li><li>Windows 7 oder höher, wenn der Host ein Clientbetriebssystem ist</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 oder höher bei Installation des Benutzerportals oder des Webdienst-SDK</li> |

<a id="azure-multi-factor-authentication-server-firewall-requirements" class="xliff"></a>

### Anforderungen an die Azure Multi-Factor Authentication-Server-Firewall
Für jeden MFA-Server muss die Kommunikation über den ausgehenden Port 443 zu folgenden Adressen möglich sein:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Öffnen Sie die folgenden IP-Adressbereiche, wenn Firewalls auf den ausgehenden Port 443 beschränkt sind:

| IP-Subnetz | Netzmaske | IP-Bereich |
|:--- |:--- |:--- |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1–134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1–134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129–70.37.154.254 |

Wenn Sie das Ereignisbestätigungsfeature nicht verwenden und Ihre Benutzer keine mobilen Apps nutzen, um eine Verifizierung über Geräte im Unternehmensnetzwerk durchzuführen, benötigen Sie lediglich folgende Bereiche:

| IP-Subnetz | Netzmaske | IP-Bereich |
|:--- |:--- |:--- |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72–134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72–134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201–70.37.154.206 |

<a id="download-the-azure-multi-factor-authentication-server" class="xliff"></a>

## Herunterladen des Azure Multi-Factor Authentication-Servers
Es gibt zwei Möglichkeiten, den Azure Multi-Factor Authentication-Server herunterzuladen. Bei beiden wird das Azure-Portal verwendet. Die erste besteht darin, den Multi-Factor Authentication-Anbieter direkt zu verwalten. Die zweite erfolgt über die Diensteinstellungen. Für die zweite Option ist entweder ein Multi-Factor Authentication-Anbieter oder eine Azure MFA-, Azure AD Premium- oder Enterprise Mobility Suite-Lizenz erforderlich.

> [!Important]
> Diese beiden scheinen Ähnlichkeiten aufzuweisen, es ist jedoch wichtig zu wissen, welche Option verwendet werden soll. Erstellen Sie keinen Multi-Factor Authentication-Anbieter, um den Serverdownload zu erhalten, falls Ihre Benutzer über Lizenzen verfügen, die in MFA (Azure MFA, Azure AD Premium oder Enterprise Mobility + Security) enthalten sind. Verwenden Sie stattdessen Option 2, um den Server von der Seite mit den Diensteinstellungen herunterzuladen. 

<a id="option-1-download-azure-multi-factor-authentication-server-from-the-azure-classic-portal" class="xliff"></a>

### Option 1: Herunterladen des Azure Multi-Factor Authentication-Servers im klassischen Azure-Portal

Verwenden Sie diese Downloadoption, falls Sie bereits über einen Multi-Factor Authentication-Anbieter verfügen, da Sie für MFA pro aktiviertem Benutzer oder pro Authentifizierung bezahlen. 

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Klicken Sie auf der Active Directory-Seite auf **Anbieter für mehrstufige Authentifizierung** ![Anbieter für mehrstufige Authentifizierung](./media/multi-factor-authentication-get-started-server/authproviders.png).
4. Klicken Sie unten auf **Verwalten**. Eine neue Seite wird geöffnet.
5. Klicken Sie auf **Downloads**.
6. Klicken Sie auf den **Download**-Link.
   ![Download](./media/multi-factor-authentication-get-started-server/download4.png)
7. Speichern Sie den Download.

<a id="option-2-download-azure-multi-factor-authentication-server-from-the-service-settings" class="xliff"></a>

### Option 2: Herunterladen des Azure Multi-Factor Authentication-Server über die Diensteinstellungen

Verwenden Sie diese Downloadoption, wenn Sie Enterprise Mobility Suite-, Azure AD Premium- oder Enterprise Cloud Suite-Lizenzen besitzen. 

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Doppelklicken Sie auf Ihre Instanz von Azure AD.
4. Klicken Sie oben auf **Konfigurieren**
5. Scrollen Sie nach unten zum Abschnitt **Multi-Factor Authentication**, und wählen Sie die Option **Diensteinstellungen verwalten**.
6. Klicken Sie auf der Seite "Diensteinstellungen" am unteren Rand des Bildschirms auf **Portal aufrufen**. Eine neue Seite wird geöffnet.
   ![Download](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Klicken Sie auf **Downloads**
8. Klicken Sie auf den **Download**-Link.
    ![Download](./media/multi-factor-authentication-get-started-server/download4.png)
9. Speichern Sie den Download.

<a id="install-and-configure-the-azure-multi-factor-authentication-server" class="xliff"></a>

## Installieren und Konfigurieren des Azure Multi-Factor Authentication-Servers
Nachdem Sie den Server heruntergeladen haben, können Sie ihn installieren und konfigurieren.  Achten Sie darauf, dass der Computer, auf dem Sie den Server installieren möchten, die im Abschnitt „Planung“ aufgeführten Anforderungen erfüllt. 

Bei diesen Schritten wurde eine Expresseinrichtung mit dem Konfigurations-Assistenten verwendet. Falls der Assistent nicht angezeigt wird oder Sie den Assistenten erneut ausführen möchten, können Sie ihn auf dem Server im Menü **Tools** auswählen.

1. Doppelklicken Sie auf die ausführbare Datei. 
2. Vergewissern Sie sich, dass auf dem Bildschirm „Installationsordner auswählen“ der richtige Ordner ausgewählt ist, und klicken Sie auf **Weiter**.
3. Klicken Sie nach Abschluss der Installation auf **Fertig stellen**.  Der Konfigurations-Assistent wird gestartet.
4. Aktivieren Sie auf der Willkommensseite des Konfigurations-Assistenten das Kontrollkästchen **Verwendung des Authentifizierungskonfigurations-Assistenten überspringen**, und klicken Sie auf **Weiter**.  Der Assistent wird geschlossen, und der Server startet.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Wechseln Sie zurück zu der Seite, von der Sie den Server heruntergeladen haben, und klicken Sie auf die Schaltfläche **Anmeldeinformationen für Aktivierung generieren** . Kopieren Sie diese Informationen auf dem Azure MFA-Server in die angezeigten Felder, und klicken Sie auf **Aktivieren**.

<a id="import-users-from-active-directory" class="xliff"></a>

## Importieren von Benutzern aus Active Directory
Nachdem Sie den Server installiert und konfiguriert haben, können Sie Benutzer schnell in den Azure MFA-Server importieren.

1. Wählen Sie auf dem Azure MFA-Server auf der linken Seite **Benutzer**aus.
2. Wählen Sie unten **Aus Active Directory importieren**aus.
3. Sie können jetzt entweder nach einzelnen Benutzern suchen oder im AD-Verzeichnis nach Organisationseinheiten suchen, die die gewünschten Benutzer enthalten.  In diesem Beispiel wird die Benutzer-Organisationseinheit angegeben.
4. Markieren Sie alle Benutzer auf der rechten Seite, und klicken Sie auf **Importieren**.  Es wird ein Popupfenster angezeigt, wenn der Vorgang erfolgreich ausgeführt wurde.  Schließen Sie das Fenster "Importieren".
   ![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

<a id="send-users-an-email" class="xliff"></a>

## Senden einer E-Mail an Benutzer
Senden Sie den Benutzern nach dem Importieren in den MFA-Server eine E-Mail mit dem Hinweis, dass sie für die Überprüfung in zwei Schritten registriert wurden.

Die gesendete E-Mail sollte sich danach richten, wie Sie die Benutzer für die Überprüfung in zwei Schritten konfiguriert haben. Falls Sie beispielsweise die Telefonnummern aus dem Telefonverzeichnis Ihres Unternehmens importieren können, sollte die E-Mail die Standardnummern enthalten, damit die Benutzer wissen, was sie erwartet. Falls Sie keine Telefonnummern importiert haben oder Ihre Benutzer die mobile App verwenden, senden Sie ihnen eine E-Mail, in der sie aufgefordert werden, ihre Kontoregistrierung abzuschließen. Schließen Sie in die E-Mail einen Link zum Benutzerportal für Azure Multi-Factor Authentication ein.

Der Inhalt der E-Mail variiert auch abhängig von der Überprüfungsmethode, die für den Benutzer festgelegt wurde (Telefonanruf, SMS oder mobile App).  Wenn der Benutzer beispielsweise bei der Authentifizierung eine PIN verwenden muss, wird er per E-Mail informiert, wie seine anfängliche PIN festgelegt wurde.  Benutzer müssen ihre PIN während der ersten Überprüfung ändern.


<a id="configure-email-and-email-templates" class="xliff"></a>

### Konfigurieren von E-Mails und E-Mail-Vorlagen
Klicken Sie auf das E-Mail-Symbol auf der linken Seite, um die Einstellungen zum Senden dieser E-Mails einzurichten. Auf dieser Seite können Sie die SMTP-Informationen Ihres E-Mailservers eingeben und E-Mails senden, indem Sie das Kontrollkästchen **E-Mails an Benutzer senden** aktivieren.

![E-Mail-Einstellungen](./media/multi-factor-authentication-get-started-server/email1.png)

Auf der Registerkarte „E-Mail-Inhalt“ werden alle E-Mail-Vorlagen angezeigt, die Sie auswählen können. Je nachdem, wie Sie Ihre Benutzer für die Überprüfung in zwei Schritten konfiguriert haben, können Sie also die am besten geeignete Vorlage auswählen.

![E-Mail-Vorlagen](./media/multi-factor-authentication-get-started-server/email2.png)

<a id="how-the-azure-multi-factor-authentication-server-handles-user-data" class="xliff"></a>

## Verarbeitung von Benutzerdaten durch den Azure Multi-Factor Authentication-Server
Bei lokaler Verwendung des Multi-Factor Authentication (MFA)-Servers werden die Daten eines Benutzers auf den lokalen Servern gespeichert. Daten werden nicht dauerhaft in der Cloud gespeichert. Wenn der Benutzer eine Überprüfung in zwei Schritten durchführt, sendet der MFA-Server Daten an den Azure MFA-Clouddienst, um die Überprüfung durchzuführen. Wenn diese Authentifizierungsanforderungen an den Clouddienst gesendet werden, werden in der Anforderung und den Protokollen die folgenden Felder gesendet, damit sie in den Authentifizierungs-/Verwendungsberichten des Kunden verfügbar sind. Einige Felder sind optional und können für den Multi-Factor Authentication-Server aktiviert oder deaktiviert werden. Für die Kommunikation zwischen MFA-Server und MFA-Clouddienst wird SSL/TLS über den ausgehenden Port 443 verwendet. Die Felder lauten:

* Eindeutige ID: Benutzername oder interne ID des MFA-Servers
* Vor- und Nachname (optional)
* E-Mail-Adresse (optional)
* Rufnummer: für eine Anruf- oder SMS-Authentifizierung
* Gerätetoken: für die Authentifizierung einer mobilen App
* Authentifizierungsmodus
* Authentifizierungsergebnis
* Name des MFA-Servers
* MFA-Server-IP
* Client-IP – falls verfügbar

Zusätzlich zu den obigen Feldern werden auch das Überprüfungsergebnis (Erfolg/Ablehnung) und der Grund für etwaige Ablehnungen mit den Authentifizierungsdaten gespeichert. Sie stehen in den Authentifizierungs- bzw. Verwendungsberichten zur Verfügung.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

- Einrichten und Konfigurieren des [Benutzerportals](multi-factor-authentication-get-started-portal.md) für Self-Service

- Einrichten und Konfigurieren des Azure MFA-Servers mit [Active Directory-Verbunddienste](multi-factor-authentication-get-started-adfs.md), [RADIUS-Authentifizierung](multi-factor-authentication-get-started-server-radius.md) oder [LDAP-Authentifizierung](multi-factor-authentication-get-started-server-ldap.md)

- Einrichten und Konfigurieren von [Remotedesktopgateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS](multi-factor-authentication-get-started-server-rdg.md) 

- [Bereitstellen des mobilen App-Webdiensts für den Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-server-webservice.md)

- [Erweiterte Szenarien mit Azure Multi-Factor Authentication und Drittanbieter-VPNs](multi-factor-authentication-advanced-vpn-configurations.md)

