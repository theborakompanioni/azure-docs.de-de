<properties
	pageTitle="Zugreifen auf den Schlüsseltresor hinter der Firewall | Microsoft Azure"
	description="Es wird beschrieben, wie Sie aus einer Anwendung hinter einer Firewall auf den Schlüsseltresor zugreifen."
	services="key-vault"
	documentationCenter=""
	authors="amitbapat"
	manager="mbaldwin"
	tags="azure-resource-manager"/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="09/13/2016"
	ms.author="ambapat"/>

# Zugreifen auf den Schlüsseltresor hinter der Firewall
### F: Meine Schlüsseltresor-Clientanwendung muss hinter einer Firewall angeordnet sein. Welche Ports/Hosts/IP-Adressen muss ich öffnen, um den Zugriff auf den Schlüsseltresor zu ermöglichen?

Für den Zugriff auf einen Schlüsseltresor muss Ihre Schlüsseltresor-Clientanwendung auf mehrere Endpunkte für unterschiedliche Funktionalitäten zugreifen können.

- Authentifizierung (über Azure Active Directory)
- Verwaltung des Schlüsseltresors (z.B. Erstellen/Lesen/Aktualisieren/Löschen und auch Festlegen von Zugriffsrichtlinien) mit Azure Resource Manager
- Der Zugriff auf und die Verwaltung von Objekten (Schlüssel und geheime Schlüssel), die im Schlüsseltresor selbst gespeichert sind, verläuft über den schlüsseltresorspezifischen Endpunkt (z.B. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).

Je nach Konfiguration und Umgebung gibt es verschiedene Varianten.

## Ports

Der gesamte Datenverkehr an den Schlüsseltresor verläuft für alle drei Funktionen (Authentifizierung, Verwaltung und Datenebenenzugriff) über „HTTPS: Port 443“. Für die Zertifikatsperrliste fällt gelegentlich auch Datenverkehr über „HTTP (Port 80)“ an. Clients mit Unterstützung von OCSP sollten die Zertifikatsperrliste nicht erreichen, sondern nur gelegentlich [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).

## Authentifizierung

Die Schlüsseltresor-Clientanwendung muss zur Authentifizierung auf Azure Active Directory-Endpunkte zugreifen. Der verwendete Endpunkt richtet sich nach der AAD-Mandantenkonfiguration und dem Typ des Prinzipals (Benutzerprinzipal, Dienstprinzipal) sowie dem Kontotyp, z.B. Microsoft-Konto oder Organisations-ID.

| Prinzipaltyp | Endpunkt:Port |
|----------------|---------------|
| Benutzer mit Microsoft-Konto<br> (z.B. user@hotmail.com) | **Global:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure US Government:**<br> login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> login.microsoftonline.de:443<br><br> und <br>login.live.com:443 |
| Benutzer-/Dienstprinzipal mit Organisations-ID per AAD (z.B. user@contoso.com) | **Global:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure US Government:**<br> login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> login.microsoftonline.de:443 |
| Benutzer-/Dienstprinzipal mit Organisations-ID und AD FS oder einem anderen Verbundendpunkt (z.B. user@contoso.com) | Alle obigen Endpunkte für Organisations-ID plus AD FS oder andere Verbundendpunkte |

Es sind noch andere komplexe Szenarien möglich. Weitere Informationen finden Sie unter [Authentifizierungsszenarien für Azure AD](/documentation/articles/active-directory-authentication-scenarios/), [Integrieren von Anwendungen in Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) und [Entwicklerhandbuch zu Azure Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx).

## Schlüsseltresorverwaltung

Für die Schlüsseltresorverwaltung (CRUD und Festlegen der Zugriffsrichtlinie) benötigt die Schlüsseltresor-Clientanwendung Zugriff auf den Azure Resource Manager-Endpunkt.

| Typ des Vorgangs | Endpunkt:Port |
|----------------|---------------|
| Vorgänge auf der Steuerungsebene des Schlüsseltresors<br> mit Azure Resource Manager | **Global:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Graph-API | **Global:**<br> graph.windows.net:443<br><br> **Azure China:**<br> graph.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> graph.windows.net:443<br><br> **Azure Deutschland:**<br> graph.cloudapi.de:443 |

## Schlüsseltresorvorgänge

Für alle Schlüsseltresorvorgänge in Bezug auf die Objektverwaltung (Schlüssel und geheime Schlüssel) und Kryptografie benötigt der Schlüsseltresorclient Zugriff auf den Schlüsseltresor-Endpunkt. Das DNS-Suffix des Endpunkts unterscheidet sich je nach Standort des Schlüsseltresors. Der Schlüsseltresor-Endpunkt hat das folgende Format: <Tresorname>.<regionsspezifisches DNS-Suffix>. Dies ist in der Tabelle unten beschrieben.

| Typ des Vorgangs | Endpunkt:Port |
|----------------|---------------|
| Schlüsseltresorvorgänge wie kryptografische Vorgänge für Schlüssel, Erstellen/Lesen/Aktualisieren/Löschen von Schlüsseln und geheimen Schlüsseln, Festlegen/Abrufen von Tags und anderen Attributen für Schlüsseltresorobjekte (Schlüssel/geheime Schlüssel) | **Global:**<br> &lt;Tresorname&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;Tresorname&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;Tresorname&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Tresorname&gt;.vault.microsoftazure.de:443 |

## IP-Adressbereiche

Für den Schlüsseltresordienst werden dagegen andere Azure-Ressourcen wie die PaaS-Infrastruktur verwendet. Daher ist es nicht möglich, einen spezifischen Bereich mit IP-Adressen anzugeben, der jederzeit für Endpunkte des Schlüsseltresordiensts gilt. Wenn Ihre Firewall nur IP-Adressbereiche unterstützt, helfen Ihnen die Informationen im Dokument zu den [Microsoft Azure Datacenter-IP-Bereichen](https://www.microsoft.com/download/details.aspx?id=41653) weiter. Für die Authentifizierung und Identitätsverwaltung (Azure Active Directory) muss Ihre Anwendung eine Verbindung mit den Endpunkten herstellen können, die unter [URLs und IP-Adressbereiche von Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) beschrieben sind.

## Nächste Schritte

- Besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault), wenn Sie Fragen zum Schlüsseltresor haben.

<!---HONumber=AcomDC_0921_2016-->