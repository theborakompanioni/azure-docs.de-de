---
title: "Bewährte Methoden für die Azure-Datenbanksicherheit | Microsoft-Dokumentation"
description: "Dieser Artikel enthält bewährte Methoden zur Azure-Datenbanksicherheit."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: tomsh
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 5bd6fe0dd369b3bbc7ca0d697c964badda557cb8
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---

# <a name="azure-database-security-best-practices"></a>Bewährte Methoden für die Azure-Datenbanksicherheit

Die Sicherheit ist einer der wichtigsten Aspekte bei der Datenbankverwaltung und hat auch bei Azure SQL-Datenbank höchste Priorität. Ihre Datenbanken können umfassend geschützt werden, um die meisten gesetzlichen oder sicherheitsbezogenen Anforderungen (HIPAA, ISO 27001/27002, PCI DSS Level 1 und andere) zu erfüllen. Eine aktuelle Liste mit Sicherheitszertifizierungen finden Sie auf der [Microsoft Trust Center-Website](http://azure.microsoft.com/support/trust-center/services/). Sie können Ihre Datenbanken auch gemäß den jeweils geltenden rechtlichen Anforderungen in spezifischen Azure-Datencentern anordnen.

In diesem Artikel werden die bewährten Methoden für die Azure-Datenbanksicherheit beschrieben. Diese bewährten Methoden sind aus unseren Erfahrungen mit der Azure-Datenbanksicherheit und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Für jede bewährte Methode wird Folgendes beschrieben:

-   Wobei es bei der bewährten Methode geht
-   Warum Sie die bewährte Methode nutzen sollten
-   Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
-   Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für die Azure-Datenbanksicherheit basiert auf einer Konsensmeinung und den Fähigkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Zeitpunkt der Erstellung dieses Artikels existierten. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Bewährte Methoden zur Azure-Datenbanksicherheit, die in diesem Artikel beschrieben werden:

-   Verwenden von Firewallregeln zum Einschränken des Datenbankzugriffs
-   Aktivieren der Datenbankauthentifizierung
-   Schützen Ihrer Daten per Verschlüsselung
-   Schützen der Daten während der Übertragung
-   Aktivieren der Datenbanküberwachung
-   Aktivieren der Bedrohungserkennung für Datenbanken


## <a name="use-firewall-rules-to-restrict-database-access"></a>Verwenden von Firewallregeln zum Einschränken des Datenbankzugriffs

Die Microsoft Azure SQL-Datenbank bietet einen relationalen Datenbankdienst für Azure und andere internetbasierte Anwendungen. Zur Gewährleistung der Zugriffssicherheit steuert SQL-Datenbank den Zugriff mit Firewallregeln, die die Konnektivität nach IP-Adresse einschränken, mit Authentifizierungsmechanismen, die die Identitätsbestätigung durch Benutzer erfordern, und mit Autorisierungsmechanismen, die bestimmte Aktionen und Daten für Benutzer eingrenzen. Firewalls verhindern jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.

![Firewallregeln](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

Der Azure SQL-Datenbank-Dienst ist nur über TCP-Port 1433 verfügbar. Wenn Sie von Ihrem Computer auf eine SQL-Datenbank zugreifen möchten, sollten Sie sicherstellen, dass die Firewall Ihres Clientcomputers die ausgehende TCP-Kommunikation über TCP-Port 1433 zulässt. Sofern sie nicht für andere Anwendungen benötigt werden, sollten Sie eingehende Verbindungen über TCP-Port 1433 mithilfe von Firewallregeln blockieren.

Im Rahmen des Verbindungsprozesses werden Verbindungen von virtuellen Azure-Computern an eine andere IP-Adresse und einen anderen Port umgeleitet, wobei diese Angaben für jede Workerrolle jeweils eindeutig sind. Die Portnummer liegt im Bereich von 11000 bis 11999. Weitere Informationen zu TCP-Ports finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5 und SQL Database2](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12).

> [!Note]
> Weitere Informationen zu Firewallregeln in SQL-Datenbank finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="enable-database-authentication"></a>Aktivieren der Datenbankauthentifizierung
SQL-Datenbank unterstützt zwei Arten der Authentifizierung: SQL-Authentifizierung und Azure Active Directory-Authentifizierung (Azure AD-Authentifizierung).

**SQL-Authentifizierung** wird in folgenden Fällen empfohlen:

-   Ermöglicht für SQL Azure die Unterstützung von Umgebungen mit gemischten Betriebssystemen, bei denen alle Benutzer nicht von einer Windows-Domäne authentifiziert werden.
-   Ermöglicht SQL Azure die Unterstützung von älteren Anwendungen sowie von Drittanbieteranwendungen, für die eine SQL Server-Authentifizierung erforderlich ist.
-   Ermöglicht es Benutzern, von unbekannten oder nicht vertrauenswürdigen Domänen aus eine Verbindung herzustellen. Beispiel: Eine Anwendung, bei der etablierte Kunden eine Verbindung mit zugewiesenen SQL Server-Anmeldungen herstellen, um den Status ihrer Bestellungen zu erhalten.
-   Ermöglicht SQL Azure die Unterstützung von webbasierten Anwendungen, für die Benutzer ihre eigenen Identitäten erstellen.
-   Ermöglicht Softwareentwicklern die Verteilung ihrer Anwendungen mithilfe einer komplexen Berechtigungshierarchie, die auf bekannten, voreingestellten SQL Server-Anmeldungen basiert.

> [!Note]
> Für die SQL Server-Authentifizierung kann aber nicht das Kerberos-Sicherheitsprotokoll verwendet werden.

Bei Verwendung der **SQL-Authentifizierung** ist Folgendes erforderlich:

-   Sie verwalten die sicheren Anmeldeinformationen selbst.
-   Sie schützen die Anmeldeinformationen in der Verbindungszeichenfolge.
-   Sie schützen (falls zutreffend) die Anmeldeinformationen, die über das Netzwerk vom Webserver an die Datenbank übergeben werden. Weitere Informationen finden Sie unter [How To: Connect to SQL Server Using SQL Authentication in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx) (Gewusst wie: Herstellen einer Verbindung mit SQL Server per SQL-Authentifizierung in ASP.NET 2.0).

Die **Azure Active Directory-Authentifizierung** ist ein Mechanismus zum Herstellen einer Verbindung mit Microsoft Azure SQL-Datenbank und [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Mithilfe der Azure AD-Authentifizierung können Sie die Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten an einer zentralen Stelle verwalten. Die zentrale ID-Verwaltung ermöglicht eine einheitliche Verwaltung von Datenbankbenutzern und vereinfacht die Berechtigungsverwaltung. Daraus ergeben sich u. a. die folgenden Vorteile:

-   Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
-   Es wird einer unkontrollierten Ausbreitung von Benutzeridentitäten über Datenbankserver hinweg Einhalt geboten.
-   Über eine zentrale Stelle wird eine Kennwortrotation ermöglicht.
-   Kunden können Datenbankberechtigungen mithilfe von externen Gruppen (AAD) verwalten.
-   Durch das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen wird das Speichern von Kennwörtern überflüssig.
-   Die Azure Active Directory-Authentifizierung verwendet eigenständige Datenbankbenutzer zum Authentifizieren von Identitäten auf Datenbankebene.
-   Die Azure Active Directory-Authentifizierung unterstützt tokenbasierte Authentifizierung für Anwendungen, die sich mit der SQL-Datenbank verbinden.
-   Die Azure Active Directory-Authentifizierung unterstützt AD FS (Domänenverbund) sowie native Benutzer-/Kennwortauthentifizierung für ein lokales Azure Active Directory ohne Domänensynchronisierung.
-   Azure AD unterstützt Verbindungen von SQL Server Management Studio, bei denen universelle Active Directory-Authentifizierungsverfahren verwendet werden, zu denen auch die Multi-Factor Authentication (MFA) gehört. MFA bietet eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen – Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Weitere Informationen finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication).

Die Konfigurationsschritte schließen die folgenden Verfahren zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung ein:

-   Erstellen und Auffüllen von Azure AD
-   Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
-   Erstellen eines Azure Active Directory-Administrators für Azure SQL Server oder [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)
- Konfigurieren der Clientcomputer
-   Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind
-   Herstellen einer Verbindung mit Ihrer Datenbank unter Verwendung von Azure AD-Identitäten

Ausführliche Informationen finden Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="protect-your-data-using-encryption"></a>Schützen Ihrer Daten per Verschlüsselung

[Transparente Datenverschlüsselung in Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096.aspx) (Transparent Data Encryption, TDE) bietet Schutz vor der Bedrohung durch böswillige Aktivitäten. Hierzu werden die Datenbank, die dazugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt, ohne dass Änderungen der Anwendung erforderlich sind. TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird.

Auch wenn der gesamte Speicher verschlüsselt ist, ist es sehr wichtig, dass Sie Ihre Datenbank selbst ebenfalls verschlüsseln. Dies ist eine Implementierung des „Defense-in-Depth-Ansatzes“ (mehrschichtige Strategie) für den Datenschutz. Falls Sie die Azure SQL-Datenbank verwenden und sensible Daten wie beispielsweise Kreditkarteninformationen oder US-Sozialversicherungsnummern sichern möchten, können Sie Datenbanken mithilfe der FIPS 140-2-zertifizierten AES-256-Bit-Verschlüsselung verschlüsseln. Diese erfüllt die Anforderungen vieler Branchenstandards (z.B. HIPAA oder PCI).

Es ist wichtig zu verstehen, dass Dateien, die im Zusammenhang mit der [Pufferpoolerweiterung](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) (Buffer Pool Extension, BPE) stehen, nicht verschlüsselt werden, wenn eine Datenbank per TDE verschlüsselt wird. Sie müssen Dateisystemebenen-Verschlüsselungstools wie [BitLocker](https://technet.microsoft.com/library/cc732774) oder das [Verschlüsselnde Dateisystem (Encrypting File System, EFS)](https://technet.microsoft.com/library/cc700811.aspx) für Dateien verwenden, die mit der Pufferpoolerweiterung in Zusammenhang stehen.

Da ein autorisierter Benutzer, wie beispielsweise ein Sicherheits- oder Datenbankadministrator, auf die Daten zugreifen kann, selbst wenn Sie mit TDE verschlüsselt sind, sollten Sie auch die folgenden Empfehlungen befolgen:

-   Aktivieren Sie die SQL-Authentifizierung auf Datenbankebene.
-   Verwenden Sie die Azure AD-Authentifizierung mithilfe von [RBAC-Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).
-   Benutzer und Anwendungen sollten verschiedenen Konten verwenden, um sich zu authentifizieren. Auf diese Weise können Sie die Anzahl von an Benutzer und Anwendungen verliehenen Berechtigungen beschränken und das Risiko böswilliger Aktivitäten reduzieren.
-   Implementieren Sie Sicherheitsmaßnahmen auf Datenbankebene, indem Sie feste Datenbankrollen (z.B. „db_datareader“ oder „db_datawriter“) verwenden oder benutzerdefinierte Rollen für Ihre Anwendung erstellen, um explizite Berechtigungen für ausgewählte Datenbankobjekte zu gewähren.

Es gibt noch weitere Möglichkeiten zum Verschlüsseln Ihrer Daten:

-   [Verschlüsselung auf Zellenebene](https://msdn.microsoft.com/library/ms179331.aspx) werden bestimmte Spalten oder sogar Zellen, die Daten enthalten, mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsselt.
-   Verschlüsselung verwendeter Daten mit Always Encrypted: Mit [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) können Kunden sensible Daten in Clientanwendungen verschlüsseln, ohne die Verschlüsselungsschlüssel für das Datenbankmodul (SQL-Datenbank oder SQL Server) jemals offenzulegen. Zu diesem Zweck ermöglicht Always Encrypted eine Trennung zwischen den Besitzern der Daten (die diese anzeigen dürfen) und den Personen, die die Daten verwalten (aber ansonsten keinen Zugriff haben).
-   Verwendung der Sicherheit auf Zeilenebene: Bei der Sicherheit auf Zeilenebene können Kunden den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (z.B. Gruppenmitgliedschaft oder Ausführungskontext). Weitere Informationen finden Sie unter [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131).

## <a name="protect-data-in-transit"></a>Schützen der Daten während der Übertragung
Der Schutz von Daten während der Übertragung sollte ein wesentlicher Bestandteil Ihrer Datenschutzstrategie sein. Da die Daten zwischen vielen verschiedenen Speicherorten übertragen werden, empfiehlt es sich im Allgemeinen, immer SSL/TLS-Protokolle zu verwenden, um Daten zwischen verschiedenen Speicherorten auszutauschen. In einigen Fällen sollten Sie den gesamten Kommunikationskanal zwischen Ihrer lokalen und Ihrer Cloudinfrastruktur isolieren, indem Sie ein virtuelles privates Netzwerk (VPN) verwenden.

Für Daten, die sich zwischen Ihrer lokalen Infrastruktur und Azure bewegen, sollten Sie passende Sicherheitsmaßnahmen in Betracht ziehen, beispielsweise HTTPS oder VPN.

Verwenden Sie eine [Azure-Site-to-Site-VPN-Verbindung ](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create) für Organisationen, die den Zugriff von mehreren lokalen Arbeitsstationen auf Azure sichern müssen.

Für Organisationen, die den Zugriff von einzelnen lokalen oder nicht lokalen Arbeitsstationen auf Azure schützen müssen, empfiehlt sich die Verwendung eines [Punkt-zu-Site-VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Größere Datasets können über dedizierte Hochgeschwindigkeits-WAN-Verbindungen wie [ExpressRoute](https://azure.microsoft.com/services/expressroute/) verschoben werden. Falls Sie sich für die Verwendung von ExpressRoute entscheiden, können Sie die Daten auch auf Anwendungsebene verschlüsseln, indem Sie [SSL/TLS](https://support.microsoft.com/kb/257591) oder andere Protokolle für zusätzlichen Schutz verwenden.

Falls Sie mit Azure Storage über das Azure-Portal interagieren, werden alle Transaktionen über HTTPS durchgeführt. [Speicher-REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx) über HTTPS kann auch verwendet werden, um mit [Azure Storage](https://azure.microsoft.com/services/storage/) und [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) zu interagieren.

Organisationen, die die Daten während der Übertragung nicht schützen, sind anfälliger für [Man-in-the-Middle-Angriffe](https://technet.microsoft.com/library/gg195821.aspx), [Abhöraktionen](https://technet.microsoft.com/library/gg195641.aspx) und Session Hijacking. Bei diesen Angriffen kann es sich um den ersten Schritt zur Zugriffsgewinnung auf vertrauliche Daten handeln.

Weitere Informationen zur Azure-VPN-Option finden Sie im Artikel [Planung und Entwurf für VPN Gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design).

## <a name="enable-database-auditing"></a>Aktivieren der Datenbanküberwachung
Die Überprüfung einer Instanz des SQL Server-Datenbankmoduls oder einer einzelnen Datenbank umfasst das Nachverfolgen und Protokollieren von Ereignissen, die im Datenbankmodul auftreten. Bei der SQL Server-Überwachung können Sie Serverüberwachungen erstellen, die Serverüberwachungsspezifikationen für Ereignisse auf Serverebene und Datenbanküberwachungsspezifikationen für Ereignisse auf Datenbankebene enthalten. Überwachte Ereignisse können in die Ereignisprotokolle oder Überwachungsdateien geschrieben werden.

Es gibt mehrere Ebenen der Überwachung für SQL Server. Dies richtet sich nach den behördlichen Anforderungen bzw. den Standardvorgaben für Ihre Installation. SQL Server Audit enthält die Tools und Prozesse, die Sie benötigen, um Überwachungen für verschiedene Server- und Datenbankobjekte zu aktivieren, zu speichern und anzuzeigen.

Die [SQL-Datenbanküberwachung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) in Azure verfolgt Datenbankereignisse nach und schreibt sie in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto.

Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

Die Überwachung ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht.

Weitere Informationen zur Datenbanküberwachung und zur damit verbundenen Aktivierung finden Sie im Artikel [Aktivieren der Überwachung und Bedrohungserkennung für SQL-Server in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="enable-database-threat-detection"></a>Aktivieren der Bedrohungserkennung für Datenbanken
Mit der SQL-Bedrohungserkennung können Sie potenzielle Bedrohungen erkennen, sobald diese auftreten, und darauf reagieren, indem Sie Sicherheitswarnungen zu anomalen Aktivitäten einrichten. Sie erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbank-Zugriffsmustern. Warnungen der SQL-Bedrohungserkennung bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

Beispielsweise ist die Einschleusung von SQL-Befehlen ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet und wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können.

Weitere Informationen zum Einrichten der Bedrohungserkennung für Ihre Datenbank im Azure-Portal finden Sie unter [Bedrohungserkennung von SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Außerdem sind Warnungen bei der SQL-Bedrohungserkennung in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert. Sie sind herzlich eingeladen, dieses Feature 60 Tage lang kostenlos zu testen.

Weitere Informationen zur Bedrohungserkennung für Datenbanken und zur damit verbundenen Aktivierung finden Sie im Artikel [Aktivieren der Überwachung und Bedrohungserkennung für SQL-Server in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="conclusion"></a>Zusammenfassung
Azure-Datenbank ist eine stabile Datenbankplattform mit umfassenden Sicherheitsfunktionen, die viele Anforderungen des Unternehmens sowie behördliche Vorschriften erfüllen. Sie können zum Schutz von Daten beitragen, indem Sie den physischen Zugriff auf Ihre Daten steuern und eine Vielzahl von Optionen für die Sicherheit auf Datei-, Spalten- oder Zeilenebene per Transparent Data Encryption, Verschlüsselung auf Zellenebene oder Sicherheit auf Zeilenebene verwenden. Always Encrypted ermöglicht auch Vorgänge für verschlüsselte Daten, sodass der Prozess der Anwendungsaktualisierung vereinfacht wird. Durch den Zugriff auf Überwachungsprotokolle der SQL-Datenbankaktivität erhalten sie zudem Informationen dazu, wie und wann auf die Daten zugegriffen wird.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Firewallregeln finden Sie unter [Firewallregeln](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
- Weitere Informationen zu Benutzern und Anmeldungenen finden Sie unter [Verwalten von Anmeldungen](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).
- Ein Tutorial finden Sie unter [Schützen von Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).

