---
title: "Übersicht über die Sicherheit der Azure-Datenbank | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über die Sicherheitsfeatures der Azure-Datenbank."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: TomSh
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 036ce3dce28e7951bb39754c4351661fae85f06c
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="azure-database-security-overview"></a>Übersicht über die Sicherheit der Azure-Datenbank

Sicherheit ist einer der wichtigsten Aspekte bei der Datenbankverwaltung und hat auch bei Azure SQL-Datenbank höchste Priorität. Azure SQL-Datenbank unterstützt Verbindungssicherheit mit Firewallregeln und Verbindungsverschlüsselung. Die Lösung unterstützt sowohl die Authentifizierung mit Benutzername und Kennwort als auch die Azure Active Directory-Authentifizierung mit Identitäten, die von Azure Active Directory verwaltet werden. Für die Autorisierung wird eine rollenbasierte Zugriffssteuerung verwendet.

Zur Unterstützung der Verschlüsselung werden von Azure SQL-Datenbank Datenbanken, die dazugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt, ohne dass Änderungen an der Anwendung erforderlich sind.

Darüber hinaus bietet Microsoft noch weitere Verschlüsselungsmöglichkeiten für Unternehmensdaten:

-   Verschlüsselung auf Zellenebene, um bestimmte Datenspalten und sogar Datenzellen mit verschiedenen Verschlüsselungsschlüsseln zu verschlüsseln.
-   Wenn Sie ein Hardwaresicherheitsmodul oder eine zentrale Verwaltung Ihrer Verschlüsselungsschlüsselhierarchie benötigen, können Sie Azure Key Vault mit SQL Server auf einem virtuellen Azure-Computer verwenden.
-   Always Encrypted (derzeit in der Vorschauphase) macht die Verschlüsselung für Anwendungen transparent und ermöglicht es Clients, sensible Daten in Clientanwendungen zu verschlüsseln, ohne die Verschlüsselungsschlüssel mit SQL-Datenbank zu teilen.

Mit der Azure SQL-Datenbanküberwachung können Unternehmen Ereignisse in einem Überwachungsprotokoll in Azure Storage erfassen. SQL-Datenbanküberwachung kann auch in Microsoft Power BI integriert werden, um detaillierte Berichte und Analysen leichter zu ermöglichen.

 SQL Azure-Datenbanken können umfassend geschützt werden, um die meisten gesetzlichen oder sicherheitsbezogenen Anforderungen (HIPAA, ISO 27001/27002, PCI DSS Level 1 und andere) zu erfüllen. Eine aktuelle Liste mit Sicherheitszertifizierungen finden Sie im [Microsoft Azure Trust Center](http://azure.microsoft.com/support/trust-center/services/).

In diesem Artikel werden die Grundlagen für den Schutz von Microsoft Azure SQL-Datenbanken für strukturierte, tabellarische und relationale Daten behandelt. In diesem Artikel erhalten Sie vor allem die ersten Informationen zu den Ressourcen zum Schützen von Daten, zum Steuern des Zugriffs und für die proaktive Überwachung.

Dabei stehen folgende Themen im Mittelpunkt:

-   Schützen von Daten
-   Zugriffssteuerung
-   Proaktive Überwachung
-   Zentralisierte Sicherheitsverwaltung
-   Azure Marketplace

## <a name="protect-data"></a>Schützen von Daten

SQL-Datenbank schützt Ihre Daten, indem Daten während der Übertragung per [Transport Layer Security](https://support.microsoft.com/kb/3135244), ruhende Daten per [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242) und verwendete Daten mithilfe von [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) verschlüsselt werden.

In diesem Abschnitt behandeln wir Folgendes:

-   Verschlüsselung von Daten während der Übertragung
-   Verschlüsselung ruhender Daten
-   Verschlüsselung verwendeter Daten (Client)

Es gibt noch weitere Möglichkeiten zum Verschlüsseln Ihrer Daten:

-   [Verschlüsselung auf Zellenebene](https://msdn.microsoft.com/library/ms179331.aspx) werden bestimmte Spalten oder sogar Zellen, die Daten enthalten, mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsselt.
-   Wenn Sie ein Hardwaresicherheitsmodul oder eine zentrale Verwaltung Ihrer Verschlüsselungsschlüsselhierarchie benötigen, können Sie den [Azure-Schlüsseltresor mit SQL Server in einer Azure-VM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)verwenden.

### <a name="encryption-in-motion"></a>Verschlüsselung von Daten während der Übertragung

Ein häufiges Problem von Client-/Serveranwendungen ist der Datenschutz während der Übertragung über öffentliche und private Netzwerke. Unverschlüsselte Daten können bei der Übertragung über ein Netzwerk von nicht autorisierten Benutzern erfasst und gestohlen werden. Bei Verwendung von Datenbankdiensten müssen die Daten zwischen Datenbankclient und -server sowie zwischen Datenbankservern, die untereinander und mit Anwendungen der mittleren Ebene kommunizieren, verschlüsselt werden.

Ein Problem bei der Netzwerkverwaltung ist der Schutz von Daten, die zwischen Anwendungen über ein nicht vertrauenswürdiges Netzwerk gesendet werden. Sie können [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) zur Authentifizierung von Servern und Clients verwenden und damit anschließend Nachrichten zwischen den authentifizierten Parteien verschlüsseln.

Im Zuge der Authentifizierung sendet ein TLS/SSL-Client eine Nachricht an einen TLS/SSL-Server, der daraufhin mit den für die Serverauthentifizierung erforderlichen Informationen antwortet. Client und Server tauschen zusätzlich Sitzungsschlüssel aus, und der Authentifizierungsdialog wird beendet. Nach Abschluss der Authentifizierung kann die durch SSL geschützte Kommunikation zwischen Server und Client unter Verwendung der symmetrischen Verschlüsselungsschlüssel beginnen, die im Zuge der Authentifizierung eingerichtet wurden.

Für alle Verbindungen zur Azure SQL-Datenbank ist eine Verschlüsselung (SSL/TLS) erforderlich, und zwar zu jedem Zeitpunkt während der Übertragung von Daten in und aus der Datenbank. SQL Azure verwendet TLS/SSL zur Authentifizierung von Servern und Clients und verschlüsselt damit anschließend Nachrichten zwischen den authentifizierten Parteien. Sie müssen in der Verbindungszeichenfolge Ihrer Anwendung Parameter zum Verschlüsseln der Verbindung angeben, und dem Serverzertifikat darf nicht vertraut werden. (Das passiert, wenn Sie die Verbindungszeichenfolge aus dem klassischen Azure-Portal kopieren.) Andernfalls wird die Identität des Servers nicht überprüft, und die Verbindung ist anfällig für Man-in-the-Middle-Angriffe. Für den ADO.NET-Treiber lauten diese Verbindungszeichenfolgenparameter beispielsweise „Encrypt=True“ und „TrustServerCertificate=False“.

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten
Sie können verschiedene Vorsichtsmaßnahmen zum Schützen der Datenbank treffen, z.B. Entwerfen eines sicheren Systems, Verschlüsseln vertraulicher Datenbestände und Erstellen einer Firewall für die Datenbankserver. Im Falle eines Diebstahls physischer Medien (Festplatten, Sicherungsbänder oder Ähnliches) kann eine böswillige Partei die Datenbank jedoch einfach wiederherstellen oder anfügen und die Daten durchsuchen.

Eine Lösung wäre, die sensiblen Daten in der Datenbank zu verschlüsseln und die Schlüssel, die zum Verschlüsseln der Daten verwendet werden, mit einem Zertifikat zu schützen. Dadurch wird die Verwendung der Daten durch Personen ohne Schlüssel verhindert. Diese Art des Schutzes muss allerdings geplant werden.

Zur Lösung dieses Problems unterstützen SQL Server und SQL Azure [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). TDE verschlüsselt Datendateien für SQL Server und Azure SQL-Datenbank. Dies wird als Verschlüsselung ruhender Daten bezeichnet.

Transparent Data Encryption von Azure SQL-Datenbank schützt vor böswilligen Aktivitäten, indem die Datenbank, die dazugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt werden, ohne dass dazu Änderungen an der Anwendung erforderlich sind.  

TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird. In SQL-Datenbank wird der Datenbankverschlüsselungsschlüssel mit einem integrierten Serverzertifikat geschützt. Das integrierte Serverzertifikat ist für jeden SQL-Datenbank-Server eindeutig.

Wenn eine Datenbank Teil einer GeoDR-Beziehung ist, wird sie auf jedem Server durch einen anderen Schlüssel geschützt. Sind zwei Datenbanken mit dem gleichen Server verbunden, verwenden sie das gleiche integrierte Zertifikat. Microsoft führt für diese Zertifikate nach spätestens 90 Tagen automatisch eine Rotation durch. Eine allgemeine Beschreibung von TDE finden Sie unter [Transparente Datenverschlüsselung (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Verschlüsselung verwendeter Daten (Client)

Bei einer Verletzung der Datensicherheit werden meistens wichtige Daten wie Kreditkartennummern oder personenbezogene Informationen entwendet. Datenbanken können sich als wahre Goldgrube für vertrauliche Informationen erweisen. Sie können unter anderem persönlichen Daten von Kunden, vertrauliche wettbewerbsrelevante Informationen und geistiges Eigentum enthalten. Verloren gegangene oder gestohlene Daten können die eigene Marke beschädigen, zu Wettbewerbsnachteilen führen und hohe Geldstrafen und sogar Gerichtsverfahren nach sich ziehen. Das gilt insbesondere für Kundendaten.

![Always Encrypted](./media/azure-databse-security-overview/azure-database-fig1.png)

[Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) ist ein Feature zum Schutz sensibler Daten wie Kreditkartennummern oder nationaler Identifikationsnummern (etwa Sozialversicherungsnummern), die in Azure SQL-Datenbank oder SQL Server-Datenbanken gespeichert sind. Mit Always Encrypted können Kunden vertrauliche Daten in Clientanwendungen verschlüsseln, ohne die Verschlüsselungsschlüssel für das Datenbankmodul (SQL-Datenbank oder SQL Server) jemals offenzulegen.

Always Encrypted ermöglicht eine Trennung zwischen den Besitzern der Daten (die diese anzeigen dürfen) und den Personen, die die Daten verwalten (aber ansonsten keinen Zugriff haben). Dadurch wird sichergestellt, dass lokale Datenbankadministratoren, Clouddatenbankbetreiber und andere nicht autorisierte Benutzer mit umfangreichen Berechtigungen nicht auf die verschlüsselten Daten zugreifen können.

Darüber hinaus macht Always Encrypted die Verschlüsselung für Anwendungen transparent. Auf dem Clientcomputer wird ein Always Encrypted-fähiger Treiber installiert, um die automatische Ver- und Entschlüsselung sensibler Daten in der Clientanwendung zu ermöglichen. Der Treiber verschlüsselt die Daten in vertraulichen Spalten vor der Übergabe an das Datenbankmodul und schreibt Abfragen automatisch so um, dass die Semantik für die Anwendung erhalten bleibt. Analog dazu werden in verschlüsselten Datenbankspalten gespeicherte Daten aus Abfrageergebnissen durch den Treiber transparent entschlüsselt.

## <a name="access-control"></a>Zugriffssteuerung
Zur Gewährleistung der Sicherheit steuert SQL-Datenbank den Zugriff mit Firewallregeln, die die Konnektivität nach IP-Adresse einschränken, Authentifizierungsmechanismen, die die Identitätsbestätigung durch Benutzer erfordern, und Autorisierungsmechanismen, die bestimmte Aktionen und Daten für Benutzer eingrenzen.

### <a name="database-access"></a>Datenbankzugriff

Datenschutz beginnt mit der Steuerung des Zugriffs auf die Daten. Das Datencenter, in dem Ihre Daten gehostet werden, kümmert sich um den physischen Zugriff. Sie selbst können eine Firewall konfigurieren, um die Sicherheit auf der Netzwerkebene zu verwalten. Darüber hinaus steuern Sie den Zugriff durch Konfigurieren von Anmeldungen für die Authentifizierung sowie durch Definieren von Berechtigungen für Server- und Datenbankrollen.

In diesem Abschnitt behandeln wir Folgendes:

-   Firewall und Firewallregeln
-   Authentifizierung
-   Autorisierung

#### <a name="firewall-and-firewall-rules"></a>Firewall und Firewallregeln

Die Microsoft Azure SQL-Datenbank bietet einen relationalen Datenbankdienst für Azure und andere internetbasierte Anwendungen. Zum Schutz Ihrer Daten verhindern Firewalls jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung. Weitere Informationen finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

Der Dienst [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) ist nur über den TCP-Port 1433 verfügbar. Wenn Sie von Ihrem Computer auf eine SQL-Datenbank zugreifen möchten, sollten Sie sicherstellen, dass die Firewall Ihres Clientcomputers die ausgehende TCP-Kommunikation über TCP-Port 1433 zulässt. Sofern nicht für andere Anwendungen benötigt, blockieren Sie eingehende Verbindungen über TCP-Port 1433.

#### <a name="authentication"></a>Authentifizierung

Die SQL-Datenbank-Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

-   **SQL-Authentifizierung:** Bei der Erstellung einer logischen SQL-Instanz wird ein einzelnes Anmeldekonto erstellt, das als Azure SQL-Datenbank-Abonnentenkonto bezeichnet wird. Dieses Konto stellt die Verbindung unter Verwendung der [SQL Server-Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (Benutzername und Kennwort) her. Dieses Konto ist ein Administrator auf der logischen Serverinstanz und in allen Benutzerdatenbanken, die an diese Instanz angefügt sind. Die Berechtigungen des Abonnentenkontos können nicht eingeschränkt werden. Nur eines dieser Konten kann vorhanden sein.
-   **Azure Active Directory-Authentifizierung:** Die [Azure Active Directory-Authentifizierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) ist ein Mechanismus zur Herstellung einer Verbindung mit Microsoft Azure SQL-Datenbank und SQL Data Warehouse unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Dies ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern.

![Authentifizierung](./media/azure-databse-security-overview/azure-database-fig2.png)

 Die Azure Active Directory-Authentifizierung hat unter anderem folgende Vorteile:
  - Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
  - Des Weiteren dämmt sie die datenbankserverübergreifende Verbreitung von Benutzeridentitäten ein und ermöglicht Kennwortrotationen an einem zentralen Ort.
  - Sie können Datenbankberechtigungen mithilfe externer (Azure Active Directory-)Gruppen verwalten.
  - Durch das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen wird das Speichern von Kennwörtern überflüssig.

#### <a name="authorization"></a>Autorisierung
[Autorisierung](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) bezieht sich darauf, welche Aufgaben ein Benutzer innerhalb einer Azure SQL-Datenbank ausführen kann. Dies wird durch datenbankbezogene [Rollenmitgliedschaften](https://msdn.microsoft.com/library/ms189121) und [Objektebenenberechtigungen](https://msdn.microsoft.com/library/ms191291.aspx) Ihres Benutzerkontos gesteuert. Im Zuge der Autorisierung wird bestimmt, auf welche sicherungsfähigen Ressourcen ein Prinzipal zugreifen kann und welche Vorgänge für diese Ressourcen zulässig sind.

### <a name="application-access"></a>Anwendungszugriff

In diesem Abschnitt behandeln wir Folgendes:

-   Dynamische Datenmaskierung
-   Sicherheit auf Zeilenebene

#### <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung
Ein Servicemitarbeiter in einem Callcenter kann Anrufer anhand mehrerer Ziffern ihrer US-Sozialversicherungsnummer oder Kreditkartennummer identifizieren, diese Datenelemente sollten dem Servicemitarbeiter jedoch nicht vollständig angezeigt werden.

Es kann eine Maskierungsregel definiert werden, mit der die US-Sozialversicherungsnummer oder Kreditkartennummer im Resultset einer Abfrage bis auf die letzten vier Ziffern ausgeblendet wird.

![Dynamische Datenmaskierung](./media/azure-databse-security-overview/azure-database-fig3.png)

In einem weiteren Beispiel kann eine entsprechende Datenmaske zum Schutz personenbezogener Daten definiert werden, damit ein Entwickler Produktionsumgebungen zu Problembehandlungszwecken abfragen kann, ohne gegen Vorschriften zu verstoßen.

Die [dynamische Datenmaskierung in SQL-Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) schränkt die Offenlegung sensibler Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Die dynamische Datenmaskierung wird für die Version V12 von Azure SQL-Datenbank unterstützt.

Mit der [dynamischen Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) können Sie mit minimalen Auswirkungen auf die Anwendungsschicht festlegen, wie viel von den sensiblen Daten offengelegt werden soll. Dies trägt zur Verhinderung des unbefugten Zugriffs auf sensible Daten bei. Es handelt sich um eine richtlinienbasierte Sicherheitsfunktion, die die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern ausblendet, ohne dass die Daten in der Datenbank geändert werden.


> [!Note]
> Die dynamische Datenmaskierung kann von den Rollen "Azure-Datenbankadministrator", "Serveradministrator" oder "Sicherheitsbeauftragter" konfiguriert werden.

#### <a name="row-level-security"></a>Sicherheit auf Zeilenebene
Eine weitere gängige Sicherheitsanforderung für mehrinstanzenfähige Datenbanken ist die [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131.aspx). Mit diesem Feature können Sie den Zugriff auf Zeilen in einer Datenbanktabelle auf der Grundlage von Merkmalen des Benutzers steuern, der eine Abfrage ausführt (also etwa auf der Grundlage einer Gruppenmitgliedschaft oder eines Ausführungskontexts).

![Sicherheit auf Zeilenebene](./media/azure-databse-security-overview/azure-database-fig4.png)

Die Zugriffsbeschränkungslogik befindet sich auf der Datenbankebene und nicht auf einer anderen, von den Daten getrennten Anwendungsebene. Das Datenbanksystem wendet die Zugriffsbeschränkungen unabhängig von der Ebene bei jedem Datenzugriffsversuch an. Dadurch bietet Ihr Sicherheitssystem eine geringere Angriffsfläche und ist zuverlässiger und robuster.

Die Sicherheit auf Zeilenebene führt die prädikatbasierte Zugriffssteuerung ein. Diese zeichnet sich durch eine flexible, zentrale und prädikatbasierte Auswertung unter Einbeziehung von Metadaten oder anderen Kriterien aus, die der Administrator für geeignet hält. Das Prädikat wird als Kriterium verwendet, um anhand von Benutzerattributen zu bestimmen, ob der Benutzer über die erforderlichen Datenzugriffsberechtigungen verfügt. Mithilfe der prädikatbasierten Zugriffssteuerung kann eine bezeichnerbasierte Zugriffssteuerung implementiert werden.

## <a name="proactive-monitoring"></a>Proaktive Überwachung
Zum Schutz Ihrer Daten stellt SQL-Datenbank Funktionen für die **Überwachung** und **Bedrohungserkennung** bereit.

### <a name="auditing"></a>Überwachung
Mit der SQL-Datenbanküberwachung können Sie sich besser über Ereignisse und Änderungen in der Datenbank informieren (beispielsweise über Datenaktualisierungen und -abfragen).

Die [SQL-Datenbanküberwachung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) in Azure verfolgt Datenbankereignisse nach und schreibt sie in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto. Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können. Die Überwachung ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht.

Die Überwachung von SQL-Datenbank bietet folgende Möglichkeiten:

-   **Beibehalten** eines Überwachungspfads von ausgewählten Ereignissen. Sie können Kategorien für zu protokollierende Datenbankaktionen und -ereignisse konfigurieren.
-   **Melden** von Datenbankaktivitäten. Sie können vorkonfigurierte Berichte und ein Dashboard verwenden, um schnell mit der Berichterstattung über Aktivitäten und Ereignisse zu beginnen.
-   **Analysieren** von Berichten. Sie können nach verdächtigen Ereignissen, ungewöhnliche Aktivitäten und Trends suchen.

Es stehen zwei Überwachungsmethoden zur Verfügung:

-   **Blobüberwachung**: Protokolle werden in Azure Blob Storage geschrieben. Dies ist eine neuere Überwachungsmethode, die eine höhere Leistung bietet, eine detailliertere Überwachung auf Objektebene unterstützt und kostengünstiger ist.
-   **Tabellenüberwachung**: Protokolle werden in Azure Table Storage geschrieben.

### <a name="threat-detection"></a>Bedrohungserkennung
Die [Bedrohungserkennung von Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) erkennt verdächtige Aktivitäten, die auf potenzielle Sicherheitsbedrohungen hindeuten. Dank der Bedrohungserkennung können Sie umgehend auf verdächtige Ereignisse in der Datenbank reagieren (etwa auf die Einschleusung von SQL-Befehlen). Sie liefert Warnungen und ermöglicht die Untersuchung verdächtiger Ereignisse mithilfe der Azure SQL-Datenbanküberwachung.

![Bedrohungserkennung](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Beispielsweise ist die Einschleusung von SQL-Befehlen ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet und wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können.

Sicherheitsbeauftragte oder andere zugewiesene Administratoren können eine sofortige Benachrichtigung über verdächtige Datenbankaktivitäten erhalten, sobald diese auftreten. Jede Benachrichtigung enthält Details zur verdächtigen Aktivität und empfiehlt die Vorgehensweise zur weiteren Untersuchung und Abwendung der Bedrohung.        

## <a name="centralized-security-management"></a>Zentralisierte Sicherheitsverwaltung

Mit [Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) können Sie Bedrohungen verhindern, erkennen und bekämpfen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) bietet Einblicke in die Sicherheit Ihrer Server und Datenbanken und trägt so zum Schutz der Daten in SQL-Datenbank bei. Mit Security Center haben Sie folgende Möglichkeiten:

-   Definieren von Richtlinien für die Verschlüsselung und Überwachung in SQL-Datenbank
-   Überwachen der Sicherheit von SQL-Datenbank-Ressourcen in allen Abonnements
-   Schnelles Erkennen und Beheben von Sicherheitsproblemen
-   Integrieren von Warnungen aus der [Bedrohungserkennung von Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
-   Security Center unterstützt rollenbasierten Zugriff.

## <a name="azure-marketplace"></a>Azure Marketplace

Der Azure Marketplace ist ein Onlinemarktplatz für Anwendungen und Dienste, über den Startups und unabhängige Softwareanbieter (ISVs) ihre Lösungen Azure-Kunden auf der ganzen Welt anbieten können.
Von dieser zentralen und einheitlichen Plattform für Microsoft Azure-Partner profitieren sowohl Kunden als auch Partner. Klicken Sie [hier](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1), um einen Eindruck davon zu bekommen, welche Datenbanksicherheitsprodukte über den Azure Marketplace erhältlich sind.

## <a name="next-steps"></a>Nächste Schritte

- Ausführlichere Informationen zum Schutz von Azure SQL-Datenbank finden Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
- Ausführlichere Informationen zu Azure Security Center und Azure SQL-Datenbank finden Sie [hier](https://docs.microsoft.com/azure/security-center/security-center-sql-database).
- Ausführlichere Informationen zur Bedrohungserkennung finden Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- Ausführlichere Informationen zur Optimierung der Leistung von SQL-Datenbank finden Sie [hier](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial). 

