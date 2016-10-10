<properties
	pageTitle="Azure AD Connect: Ports | Microsoft Azure"
	description="Diese Seite ist eine Seite technische Referenz für Ports, die für Azure AD Connect geöffnet sein müssen."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="femila"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/25/2016"
	ms.author="billmath"/>

# Erforderliche Ports und Protokolle für die Hybrid-Identität
Das folgende Dokument ist eine technische Referenz mit Informationen zu den erforderlichen Ports und Protokollen, die für die Implementierung einer Hybrid-Identitätslösung benötigt werden. Sehen Sie sich die folgende Abbildung und die entsprechende Tabelle an.

![Was ist Azure AD Connect?](./media/active-directory-aadconnect-ports/required1.png)

## Tabelle 1: Azure AD Connect und lokales AD
In dieser Tabelle werden die Ports und Protokolle beschrieben, die für die Kommunikation zwischen dem Azure AD Connect-Server und der lokalen AD-Instanz erforderlich sind.

Protocol | Ports | Beschreibung
--------- | --------- |---------
DNS|53 (TCP/UDP)| DNS-Suchen in der Zielgesamtstruktur
Kerberos|88 (TCP/UDP)| Kerberos-Authentifizierung für die AD-Gesamtstruktur
MS-RPC |135 (TCP/UDP)| Wird bei der anfänglichen Konfiguration des Azure AD Connect-Assistenten beim Binden an die AD-Gesamtstruktur verwendet
LDAP|389 (TCP/UDP)| Wird zum Importieren von Daten aus AD verwendet. Daten werden per Kerberos Sign & Seal verschlüsselt.
LDAP/SSL|636 (TCP/UDP)| Wird zum Importieren von Daten aus AD verwendet. Die Datenübertragung ist signiert und verschlüsselt. Gilt nur bei Verwendung von SSL.
RPC |49152- 65535 (Random High RPC-Port) (TCP/UDP)| Wird bei der anfänglichen Konfiguration von Azure AD Connect beim Binden an AD-Gesamtstrukturen verwendet Weitere Informationen finden Sie unter [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017) und [KB224196](https://support.microsoft.com/kb/224196).

## Tabelle 2: Azure AD Connect und Azure AD
In dieser Tabelle werden die Ports und Protokolle beschrieben, die für die Kommunikation zwischen dem Azure AD Connect-Server und Azure AD erforderlich sind.

Protocol |Ports |Beschreibung
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Wird zum Herunterladen von Zertifikatsperrlisten zur Überprüfung von SSL-Zertifikaten verwendet
HTTPS|443 (TCP/UDP)| Wird zum Synchronisieren mit Azure AD verwendet

Eine Liste der URLs und IP-Adressen, die in der Firewall geöffnet sein müssen, finden Sie unter [URLs und IP-Adressbereiche von Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## Tabelle 3: Azure AD Connect und Verbund-/WAP-Server
In dieser Tabelle werden die Ports und Protokolle beschrieben, die für die Kommunikation zwischen dem Azure AD Connect-Server und Verbund-/WAP-Servern erforderlich sind.

Protocol |Ports |Beschreibung
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Wird zum Herunterladen von Zertifikatsperrlisten zur Überprüfung von SSL-Zertifikaten verwendet
HTTPS|443 (TCP/UDP)| Wird zum Synchronisieren mit Azure AD verwendet
WinRM|5985| WinRM-Listener

## Tabelle 4: WAP- und Verbundserver
In dieser Tabelle werden die Ports und Protokolle beschrieben, die für die Kommunikation zwischen den Verbundservern und WAP-Servern erforderlich sind.

Protocol |Ports |Beschreibung
--------- | --------- |---------
HTTPS|443 (TCP/UDP)| Wird für die Authentifizierung verwendet

## Tabelle 5: WAP und Benutzer
In dieser Tabelle werden die Ports und Protokolle beschrieben, die für die Kommunikation zwischen Benutzern und den WAP-Servern erforderlich sind.

Protocol |Ports |Beschreibung
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)| Wird für die Geräteauthentifizierung verwendet
TCP|49443 (TCP)| Wird für die Zertifikatauthentifizierung verwendet

## Tabelle 6a und 6b: Azure AD Connect Health-Agent für (AD FS/Sync) und Azure AD
In den folgenden Tabellen werden die Endpunkte, Ports und Protokolle beschrieben, die für die Kommunikation zwischen Azure AD Connect Health-Agents und Azure AD erforderlich sind.

### Tabelle 6a: Ports und Protokolle für den Azure AD Connect Health-Agent für (AD FS/Sync) und Azure AD
In dieser Tabelle werden die folgenden ausgehenden Ports und Protokolle beschrieben, die für die Kommunikation zwischen Azure AD Connect Health-Agents und Azure AD erforderlich sind.

Protocol |Ports |Beschreibung
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)| Ausgehend
Azure-Servicebus|5671 (TCP/UDP)| Ausgehend

### Tabelle 6b: Endpunkte für Azure AD Connect Health-Agent für (AD FS/Sync) und Azure AD
Eine Liste mit Endpunkten finden Sie im [Abschnitt mit den Anforderungen für den Azure AD Connect Health-Agent](active-directory-aadconnect-health-agent-install.md#requirements).

<!---HONumber=AcomDC_0928_2016-->