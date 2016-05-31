<properties
	pageTitle="Benutzerdefinierte Installation von Azure AD Connect | Microsoft Azure"
	description="In diesem Dokument werden die Optionen für die benutzerdefinierte Installation für Azure AD Connect aufgeführt. Gehen Sie folgendermaßen vor, um Active Directory über Azure AD Connect zu installieren."
	services="active-directory"
    keywords="Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"  
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/19/2016"
	ms.author="billmath;andkjell"/>

# Benutzerdefinierte Installation von Azure AD Connect
Die folgende Dokumentation stellt Informationen zur Verwendung der benutzerdefinierten Installation für Azure AD Connect bereit. Sie können diese Option verwenden, wenn Sie zusätzliche Konfigurationsoptionen oder optionale Features benötigen, die von der Express-Installation nicht abgedeckt werden.

## Verwandte Dokumentation
Wenn Sie die Dokumentation zum [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md) nicht gelesen haben, finden Sie in der folgenden Tabelle Links zu verwandten Themen. Sie müssen die ersten drei, fett hervorgehobenen Themen gelesen haben, bevor Sie mit der Installation beginnen.

Thema |  
--------- | ---------
**Azure AD Connect herunterladen** | [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771)
**Hardware und Voraussetzungen** | [Azure AD Connect: Hardware und Voraussetzungen](active-directory-aadconnect-prerequisites.md#hardware-requirements-for-azure-ad-connect)
**Für die Installation verwendete Konten** | [Azure AD Connect-Konten und -Berechtigungen](active-directory-aadconnect-accounts-permissions.md)
Installieren mit den Express-Einstellungen | [Expressinstallation von Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Upgrade von DirSync | [Upgrade von Azure AD-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Nach der Installation | [Überprüfen der Installation und Zuweisen von Lizenzen ](active-directory-aadconnect-whats-next.md)

## Installieren der erforderlichen Komponenten
Bei der Installation der Synchronisierungsdienste können Sie den optionalen Konfigurationsabschnitt deaktiviert lassen, sodass Azure AD Connect alles automatisch einrichtet. Azure AD Connect richtet eine SQL Server 2012 Express LocalDB-Instanz ein, erstellt die entsprechenden Gruppen und weist Berechtigungen zu. Wenn Sie die Standardeinstellungen ändern möchten, können Sie sich in der folgenden Tabelle über die verfügbaren optionalen Konfigurationsoptionen informieren.

![Erforderliche Komponenten](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Optionale Konfiguration | Beschreibung
------------- | -------------
Verwenden eines vorhandenen SQL Servers | Ermöglicht Ihnen die Angabe des SQL-Servernamens und des Instanznamens. Wählen Sie diese Option aus, wenn Sie bereits über einen Datenbankserver verfügen, den Sie verwenden möchten. Geben Sie unter **Instanzname** den Instanznamen, ein Komma und die Portnummer ein, falls das Browsen für SQL Server nicht aktiviert ist.
Verwenden eines vorhandenen Dienstkontos | Azure AD Connect erstellt standardmäßig ein lokales Dienstkonto für die zu verwendenden Synchronisierungsdienste. Das Kennwort wird automatisch generiert und ist der Person, die Azure AD Connect installiert, nicht bekannt. Wenn Sie einen Remote-SQL Server oder einen Proxy mit Authentifizierungsanforderung verwenden, benötigen Sie ein Dienstkonto in der Domäne und müssen das Kennwort kennen. Geben Sie in diesen Fällen das zu verwendende Dienstkonto ein. Stellen Sie sicher, dass der die Installation ausführende Benutzer ein SA in SQL ist, damit das Dienstkonto erstellt werden kann. Siehe [Azure AD Connect-Konten und -Berechtigungen](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Angeben benutzerdefinierter Synchronisierungsgruppen | Azure AD Connect erstellt beim Installieren der Synchronisierungsdienste standardmäßig vier lokale Gruppen auf dem Server. Diese Gruppen sind: Administratorengruppe, Operatorengruppe, Durchsuchen-Gruppe und die Gruppe "Kennwort zurücksetzen". Hier können Sie Ihre eigenen Gruppen angeben. Die Gruppen müssen sich lokal auf dem Server befinden und dürfen nicht in der Domäne sein.

## Benutzeranmeldung
Nach der Installation der erforderlichen Komponenten werden Sie aufgefordert, die Methode für das einmalige Anmelden Ihrer Benutzer auszuwählen. Die folgende Tabelle enthält eine kurze Beschreibung der verfügbaren Optionen. Eine vollständige Beschreibung der Anmeldemethoden finden Sie unter [Benutzeranmeldung](active-directory-aadconnect-user-signin.md).

![Benutzeranmeldung](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Option zum einmaligen Anmelden | Beschreibung
------------- | -------------
Kennwortsynchronisierung | Benutzer können sich bei Microsoft Cloud Services wie Office 365 mit dem Kennwort anmelden, das sie auch in ihrem lokalen Netzwerk verwenden. Die Benutzerkennwörter werden über einen Kennworthash mit Azure AD synchronisiert, und die Authentifizierung erfolgt in der Cloud. Weitere Informationen finden Sie unter [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md).
Verbund mit AD FS | Benutzer können sich bei Microsoft Cloud Services wie Office 365 mit dem Kennwort anmelden, das sie auch in ihrem lokalen Netzwerk verwenden. Die Benutzer werden für die Anmeldung jeweils an ihre lokale AD FS-Instanz umgeleitet, und die Authentifizierung erfolgt lokal.
Nicht konfigurieren | Keines der Features wird installiert oder konfiguriert. Wählen Sie diese Option aus, wenn Sie bereits über einen Verbundserver eines Drittanbieters verfügen oder eine andere vorhandene Lösung eingesetzt wird.

## Herstellen einer Verbindung mit Azure AD
Geben Sie im Bildschirm "Mit Azure AD verbinden" ein Konto und ein Kennwort für den globalen Administrator ein. Wenn Sie auf der vorherigen Seite **Verbund mit AD FS** ausgewählt haben, melden Sie sich nicht mit einem Konto in einer Domäne an, die Sie für den Verbund aktivieren möchten. Wir empfehlen Ihnen die Verwendung eines Kontos in der standardmäßigen Domäne **onmicrosoft.com**, die in Ihrem Azure AD-Verzeichnis enthalten ist.

Dieses Konto dient ausschließlich der Erstellung eines Dienstkontos in Azure AD und wird nach Abschluss des Assistenten nicht mehr verwendet. ![Benutzeranmeldung](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Wenn MFA für Ihr globales Administratorkonto aktiviert ist, müssen Sie das Kennwort im Anmelde-Popupfenster erneut eingeben und die MFA-Abfrage ausfüllen. Bei der Abfrage kann es sich etwa um die Eingabe eines Überprüfungscodes oder um einen Anruf handeln. ![MFA-Benutzeranmeldung](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Für das globale Administratorkonto kann auch [Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) aktiviert sein.

Falls ein Fehler auftritt und Sie Probleme mit der Konnektivität haben, können Sie unter [Problembehebung bei Konnektivitätsproblemen](active-directory-aadconnect-troubleshoot-connectivity.md) nach einer Lösung suchen.

## Seiten im Abschnitt "Synchronisierung"

### Verzeichnisse verbinden
Zum Verbinden mit Ihrem Active Directory-Domänendienst benötigt Connect von Azure AD die Anmeldeinformationen für ein Konto, das über ausreichende Berechtigungen verfügt. Sie können den Domänenteil entweder im NetBIOS- oder FQDN-Format eingeben, also „FABRIKAM\\syncuser“ oder „fabrikam.com\\syncuser“. Dieses Konto kann ein normales Benutzerkonto sein, da nur die standardmäßigen Leseberechtigungen benötigt werden. Abhängig von Ihrem Szenario benötigen Sie jedoch möglicherweise weitere Berechtigungen. Weitere Informationen finden Sie unter [Azure AD Connect-Konten und -Berechtigungen](active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account).

![Verzeichnis verbinden](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### Azure AD sign-in configuration (Konfiguration der Azure AD-Anmeldung)
Auf dieser Seite können Sie die UPN-Domänen anzeigen, die in der lokalen AD DS-Instanz vorhanden sind und in Azure AD überprüft wurden. Darüber hinaus können Sie auf dieser Seite das Attribut für „userPrincipalName“ konfigurieren.

![Nicht überprüfte Domänen](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png) Überprüfen Sie alle Domänen, die als **Nicht hinzugefügt** und **Nicht überprüft** markiert sind. Stellen Sie sicher, dass die verwendeten Domänen in Azure AD überprüft wurden. Klicken Sie auf das Symbol zum Aktualisieren, wenn Sie Ihre Domänen überprüft haben. Weitere Informationen finden Sie im Artikel mit Informationen zum [Hinzufügen und Überprüfen der Domäne](active-directory-add-domain.md).

**userPrincipalName**: Das userPrincipalName-Attribut wird von Benutzern verwendet, wenn sie sich bei Azure AD und Office 365 anmelden. Die verwendeten Domänen, auch als UPN-Suffix bezeichnet, sollte in Azure AD überprüft werden, bevor die Benutzer synchronisiert werden. Microsoft empfiehlt, das Standardattribut „userPrincipalName“ beizubehalten. Wenn dieses Attribut nicht routingfähig ist und nicht überprüft werden kann, können Sie ein anderes Attribut auswählen. So können Sie beispielsweise „email“ als Attribut mit der Anmelde-ID verwenden. Wenn Sie ein anderes Attribut als „userPrincipalName“ verwenden, wird dieses als **alternative ID** bezeichnet. Der Attributwert der alternativen ID muss dem RFC822-Standard entsprechen. Eine alternative ID kann sowohl für die Kennwortsynchronisierung als auch in einem Verbund verwendet werden.

>[AZURE.WARNING]
Eine alternative ID ist nicht mit allen Office 365-Workloads kompatibel. Weitere Informationen finden Sie unter [Konfigurieren der alternativen Anmelde-ID](https://technet.microsoft.com/library/dn659436.aspx).

### Filterung von Domänen und Organisationseinheiten
Standardmäßig werden alle Domänen und Organisationseinheiten synchronisiert. Wenn Sie Domänen oder Organisationseinheiten nicht in Azure AD synchronisieren möchten, können Sie diese Domänen und Organisationseinheiten deaktivieren. ![Domänen und Organisationseinheiten filtern](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) Diese Seite des Assistenten dient zum Konfigurieren der domänenbasierten Filterung. Weitere Informationen finden Sie unter [Domänenbasierte Filterung](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

Möglicherweise sind bestimmte Domänen aufgrund von Beschränkungen der Firewall nicht erreichbar. Diese Domänen sind standardmäßig nicht ausgewählt und mit einer Warnung versehen. ![Nicht erreichbare Domänen](./media/active-directory-aadconnect-get-started-custom/unreachable.png) Sollte diese Warnung angezeigt werden, vergewissern Sie sich, dass die entsprechenden Domänen tatsächlich nicht erreichbar sind und die Warnung zu erwarten ist.

### Eindeutige Identifizierung der Benutzer
Mit dem Feature zum Abgleich über Gesamtstrukturen können Sie definieren, wie Benutzer Ihrer AD DS-Gesamtstrukturen in Azure AD repräsentiert werden. Ein Benutzer kann entweder nur einmal in allen Gesamtstrukturen vorhanden sein oder über eine Kombination aus aktivierten und deaktivierten Konten verfügen. In bestimmten Gesamtstrukturen wird der Benutzer unter Umständen auch als Kontakt dargestellt.

![Eindeutig](./media/active-directory-aadconnect-get-started-custom/unique.png)

Einstellung | Beschreibung
------------- | -------------
[Benutzer sind nur einmal in allen Gesamtstrukturen vorhanden.](active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Alle Benutzer werden in Azure AD jeweils als einzelne Objekte erstellt. Die Objekte werden nicht im Metaverse verknüpft.
[E-Mail-Attribut](active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Diese Option verknüpft Benutzer und Kontakte, wenn dieses Attribut in verschiedenen Gesamtstrukturen denselben Wert aufweist. Verwenden Sie diese Option, wenn Ihre Kontakte mit GALSync erstellt wurden.
[ObjectSID und msExchangeMasterAccountSID/ msRTCSIP-OriginatorSid](active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Mit dieser Option wird ein aktivierter Benutzer in einer Kontogesamtstruktur mit einem deaktivierten Benutzer in einer Ressourcengesamtstruktur verknüpft. In Exchange wird diese Konfiguration als verknüpftes Postfach bezeichnet. Diese Option kann auch verwendet werden, wenn Sie nur Lync verwenden und Exchange in der Ressourcengesamtstruktur nicht vorhanden ist.
sAMAccountName und MailNickName | Mit dieser Option werden Attribute mit der Stelle verknüpft, an der sich erwartungsgemäß die Anmelde-ID für den Benutzer befindet.
Ein bestimmtes Attribut | Mit dieser Option können Sie Ihr eigenes Attribut auswählen. **Einschränkung**: Wählen Sie unbedingt ein Attribut aus, das bereits im Metaverse vorhanden ist. Wenn Sie ein benutzerdefiniertes (nicht im Metaverse vorhandenes) Attribut auswählen, kann der Assistent nicht abgeschlossen werden.

**Quellanker**: Das sourceAnchor-Attribut ist während der Lebensdauer eines Benutzerobjekts unveränderlich. Das Attribut ist der Primärschlüssel, der den lokalen Benutzer mit dem Benutzer in Azure AD verknüpft. Da das Attribut nicht geändert werden kann, müssen Sie sorgfältig planen, welches Attribut Sie verwenden möchten. Hier empfiehlt sich "objectGUID". Dieses Attribut wird nicht geändert, es sei denn, das Benutzerkonto wird zwischen Gesamtstrukturen/Domänen verschoben. In einer Umgebung mit mehreren Gesamtstrukturen, in der Sie Konten zwischen Gesamtstrukturen verschieben, muss ein anderes Attribut verwendet werden, z. B. ein Attribut mit der Mitarbeiter-ID. Vermeiden Sie die Verwendung von Attributen, die sich ändern, wenn eine Person heiratet oder den Aufgabenbereich wechselt. Sie können keine Attribute mit einem @-Zeichen verwenden, daher sind E-Mail-Adressen und Benutzerprinzipalnamen ungeeignet. Bei dem Attribut wird die Groß-/Kleinschreibung beachtet. Beim Verschieben von Objekten zwischen Gesamtstrukturen muss daher die Groß-/Kleinschreibung beibehalten werden. Binäre Attribute werden base64-codiert, andere Attributtypen bleiben dagegen unverschlüsselt. In Verbundszenarien und bei einigen Azure AD-Schnittstellen wird dieses Attribut auch als immutableID-Attribut bezeichnet. Weitere Informationen über den Quellanker finden Sie unter [Entwurfskonzepte](active-directory-aadconnect-design-concepts.md#sourceAnchor).

### Synchronisierungsfilterung anhand von Gruppen
Mit der Filterung nach Gruppen haben Sie die Möglichkeit, nur eine kleine Teilmenge von Objekten für einen Piloten zu synchronisieren. Erstellen Sie dazu in Ihrem lokalen Active Directory eine Gruppe für diesen Zweck. Fügen Sie anschließend Benutzer und Gruppen hinzu, die als direkte Mitglieder mit Azure AD synchronisiert werden sollen. Später können Sie Benutzer hinzufügen und entfernen, um die Liste mit den Objekten zu verwalten, die in Azure AD vorhanden sein sollen. Alle Objekte, die Sie synchronisieren möchten, müssen direkte Mitglieder der Gruppe sein. Benutzer, Gruppen, Kontakte und Computer/Geräte müssen jeweils direkte Mitglieder sein. Geschachtelte Gruppenmitgliedschaften werden nicht aufgelöst. Wenn Sie eine Gruppe als Mitglied hinzufügen, wird nur die Gruppe hinzugefügt, nicht aber ihre Mitglieder.

![Synchronisierungsfilterung](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Dieses Feature dient nur zur Unterstützung von Pilotbereitstellungen. Verwenden Sie es nicht in einer Produktionsbereitstellung.

In einer Produktionsbereitstellung wird es schwer, eine einzelne Gruppe mit allen zu synchronisierenden Objekten zu verwalten. Verwenden Sie stattdessen eine der unter [Konfigurieren der Filterung](active-directory-aadconnectsync-configure-filtering.md) beschriebenen Methoden.

### Optionale Features
Über diesen Bildschirm können Sie die optionalen Features für Ihre spezifischen Szenarios auswählen.

![Optionale Features](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Wenn derzeit DirSync oder Azure AD Sync aktiv ist, aktivieren Sie keine der Features zum Rückschreiben in Azure AD Connect.

Optionale Features | Beschreibung
------------------- | -------------
Exchange-Hybridbereitstellung | Das Exchange-Hybridbereitstellungsfeature ermöglicht die Koexistenz lokaler und Office 365-basierter Exchange-Postfächer. Azure AD Connect synchronisiert eine bestimmte Gruppe von [Attributen](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) aus Azure AD mit Ihrem lokalen Verzeichnis.
Azure AD-App- und Attributfilterung | Mithilfe der App- und Attributfilterung von Azure AD kann die Gruppe synchronisierter Attribute individuell konfiguriert werden. Durch diese Option wird der Assistent um zwei weitere Konfigurationsseiten erweitert. Weitere Informationen finden Sie unter [Azure AD-App- und Attributfilterung](#azure-ad-app-and-attribute-filtering).
Kennwortsynchronisierung | Diese Option ist verfügbar, wenn Sie als Anmeldelösung die Verbundoption ausgewählt haben. Die Synchronisierung von Kennwörtern kann dann als Sicherungsoption verwendet werden. Weitere Informationen finden Sie unter [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md).
Rückschreiben von Kennwörtern | Durch Aktivieren des Kennwortrückschreibens werden Kennwortänderungen aus Azure AD in Ihr lokales Verzeichnis zurückgeschrieben. Weitere Informationen finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).
Gruppenrückschreiben | Bei Verwendung des Features **Office 365-Gruppen** können diese Gruppen in Ihrem lokalen Active Directory dargestellt werden. Diese Option ist nur verfügbar, wenn Exchange in Ihrem lokalen Active Directory vorhanden ist. Weitere Informationen finden Sie unter [Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback).
Geräterückschreiben | Ermöglicht für bedingte Szenarios das Rückschreiben von Geräteobjekten in Azure AD auf Ihr lokales Active Directory. Weitere Informationen finden Sie unter [Aktivieren des Geräterückschreibens in Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md).
Verzeichniserweiterungen-Attributsynchronisierung | Durch Aktivieren der Attributsynchronisierung für Verzeichniserweiterungen werden die angegebenen Attribute mit Azure AD synchronisiert. Weitere Informationen finden Sie unter [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md).

### Azure AD-App- und Attributfilterung
Wenn Sie einschränken möchten, welche Attribute mit Azure AD synchronisiert werden, wählen Sie zunächst die verwendeten Dienste aus. Falls Sie auf dieser Seite Konfigurationsänderungen vornehmen, muss durch erneutes Ausführen des Installations-Assistenten explizit ein neuer Dienst ausgewählt werden.

![Optionale Features – Apps](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Auf der Grundlage der im vorherigen Schritt ausgewählten Dienste werden auf dieser Seite alle synchronisierten Attribute angezeigt. Diese Liste ist eine Kombination aller Objekttypen, die synchronisiert werden. Falls bestimmte Attribute nicht synchronisiert werden sollen, können Sie deren Auswahl aufheben.

![Optionale Features – Attribute](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Das Entfernen von Attributen kann sich negativ auf die Funktionalität auswirken. Bewährte Methoden und Empfehlungen finden Sie unter [Synchronisierte Attribute](active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### Verzeichniserweiterungen-Attributsynchronisierung
Das Schema in Azure AD kann durch von Ihrer Organisation hinzugefügte benutzerdefinierte Attribute oder durch andere Attribute in Active Directory erweitert werden. Wählen Sie auf der Seite **Optionale Features** die Option **Verzeichniserweiterungen-Attributsynchronisierung** aus, um dieses Feature zu verwenden. Auf dieser Seite können weitere zu synchronisierende Attribute ausgewählt werden.

![Verzeichniserweiterungen](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Weitere Informationen finden Sie unter [Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md).

## Konfigurieren des Verbunds mit AD FS
Das Konfigurieren von AD FS mit Azure AD Connect ist mit nur wenigen Mausklicks erledigt. Für die Konfiguration wird Folgendes benötigt:

- Ein Windows Server 2012 R2-Server für den Verbundserver mit aktivierter Remoteverwaltung
- Ein Windows Server 2012 R2-Server für den Webanwendungsproxy-Server mit aktivierter Remoteverwaltung
- Ein SSL-Zertifikat für den Verbunddienstnamen, den Sie verwenden möchten (z. B. „sts.contoso.com“)

### Voraussetzungen für die AD FS-Konfiguration
Vergewissern Sie sich, dass WinRM auf den Remoteservern aktiviert ist, damit Sie Ihre AD FS-Farm mithilfe von Azure AD Connect konfigurieren können. Machen Sie sich außerdem in [Tabelle 3: Azure AD Connect und Verbund-/WAP-Server](active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap) mit den Portanforderungen vertraut.

### Erstellen einer neuen AD FS-Farm oder Verwenden einer vorhandenen AD FS-Farm
Sie können eine vorhandene AD FS-Farm verwenden oder eine neue AD FS-Farm erstellen. Wenn Sie eine neue Farm erstellen, müssen Sie ein SSL-Zertifikat bereitstellen. Bei Verwendung eines kennwortgeschützten SSL-Zertifikats werden Sie zur Eingabe des Kennworts aufgefordert.

![AD FS-Farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Wenn Sie sich für die Verwendung einer bereits vorhandenen AD FS-Farm entscheiden, gelangen Sie direkt zur Konfiguration der Vertrauensstellung zwischen AD FS und Azure AD.

### Angeben der AD FS-Server
Geben Sie die Server ein, auf denen Sie AD FS installieren möchten. Sie können je nach Ihren Kapazitätsplanungsanforderungen einen oder mehrere Server hinzufügen. Verknüpfen Sie vor dieser Konfiguration alle Server mit Active Directory. Microsoft empfiehlt, einen einzelnen AD FS-Server für Test- und Pilotbereitstellungen zu installieren. Nach der Erstkonfiguration können Sie dann Azure AD Connect erneut ausführen und weitere Server hinzufügen und bereitstellen, um Ihren Skalierungsanforderungen gerecht zu werden.

>[AZURE.NOTE]
Vergewissern Sie sich vor dieser Konfiguration, dass alle Ihre Server mit einer AD-Domäne verknüpft sind.

![AD FS-Server](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### Angeben von Webanwendungsproxy-Servern
Geben Sie die Server ein, die Sie als Webanwendungsproxy-Server verwenden möchten. Der Webanwendungsproxy-Server wird in Ihrer DMZ bereitgestellt (Extranetzugriff) und unterstützt Authentifizierungsanforderungen aus dem Extranet. Sie können je nach Ihren Kapazitätsplanungsanforderungen einen oder mehrere Server hinzufügen. Microsoft empfiehlt, einen einzelnen Webanwendungsproxy-Server für Test- und Pilotbereitstellungen zu installieren. Nach der Erstkonfiguration können Sie dann Azure AD Connect erneut ausführen und weitere Server hinzufügen und bereitstellen, um Ihren Skalierungsanforderungen gerecht zu werden. Es empfiehlt sich, eine entsprechende Anzahl von Proxyservern bereitzustellen, um die Authentifizierung über das Intranet zu ermöglichen.

>[AZURE.NOTE]
<li> Wenn es sich bei dem verwendeten Konto nicht um ein lokales Administratorkonto auf den AD FS-Servern handelt, werden Sie zur Eingabe der Administratoranmeldeinformationen aufgefordert.</li>
<li> Vergewissern Sie sich vor diesem Schritt, dass zwischen dem Azure AD Connect-Server und dem Webanwendungsproxy-Server eine HTTP/HTTPS-Verbindung besteht.</li>
<li> Vergewissern Sie sich zudem, dass zwischen dem Webanwendungsserver und dem AD FS-Server eine HTTP/HTTPS-Verbindung besteht, um die Durchleitung von Authentifizierungsanforderungen zu ermöglichen.</li>

![Web App](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Sie werden zur Eingabe von Anmeldeinformationen aufgefordert, damit der Webanwendungsserver eine sichere Verbindung mit dem AD FS-Server herstellen kann. Dabei muss es sich um die Anmeldeinformationen für einen lokalen Administrator für den AD FS-Server sein.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### Angeben eines Dienstkontos für den AD FS-Dienst
Der AD FS-Dienst erfordert ein Domänendienstkonto zur Authentifizierung von Benutzern und zur Suche nach Benutzerinformationen in Active Directory. Zwei Dienstkontotypen werden unterstützt:

- **Gruppenverwaltetes Dienstkonto**: Wurde in den Active Directory-Domänendiensten mit Windows Server 2012 eingeführt. Dieser Kontotyp stellt für Dienste wie AD FS ein einzelnes Konto bereit, das keine regelmäßige Aktualisierung des Kontokennworts erfordert. Verwenden Sie diese Option, wenn Sie in der Domäne, der die AD FS-Server angehören, bereits über Windows Server 2012-Domänencontroller verfügen.
- **Domänenbenutzerkonto**: Bei diesem Kontotyp müssen Sie ein Kennwort angeben und das Kennwort regelmäßig aktualisieren, wenn sich das Kennwort ändert oder abläuft. Verwenden Sie diese Option nur, wenn Sie in der Domäne, der die AD FS-Server angehören, über keine Windows Server 2012-Domänencontroller verfügen.

Wenn Sie das gruppenverwaltete Dienstkonto ausgewählt haben und dieses Feature in Active Directory noch nie verwendet wurde, werden Sie zur Eingabe von Enterprise-Administratoranmeldeinformationen aufgefordert. Diese werden zum Initiieren des Schlüsselspeichers und zum Aktivieren des Features in Active Directory verwendet.

![AD FS-Dienstkonto](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### Auswählen der zu verbindenden Azure AD-Domäne
Diese Konfiguration wird verwendet, um die Verbundbeziehung zwischen AD FS und Azure AD einzurichten. Damit wird AD FS zur Ausstellung von Sicherheitstoken an Azure AD konfiguriert, und Azure AD wird so konfiguriert, dass es den Token dieser spezifischen Instanz von AD FS vertraut. Auf dieser Seite kann bei der Erstinstallation nur eine einzelne Domäne konfiguriert werden. Weitere Domänen können Sie später durch erneutes Ausführen von Azure AD Connect konfigurieren.

![Azure AD-Domäne](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### Überprüfen der für den Verbund ausgewählten Azure AD-Domäne
Wenn Sie die Domäne in einem Verbund verwenden möchten, stellt Azure AD Connect die erforderlichen Informationen zur Überprüfung einer nicht überprüften Domäne bereit. Informationen zur Verwendung dieser Informationen finden Sie unter [Hinzufügen und Überprüfen der Domäne](active-directory-add-domain.md).

![Azure AD-Domäne](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
AD Connect versucht in der Konfigurationsphase, die Domäne zu überprüfen. Wenn Sie die Konfiguration ohne Angabe der erforderlichen DNS-Einträge fortsetzen, kann die Konfiguration nicht abgeschlossen werden.

## Konfigurieren und Überprüfen von Seiten
Die Konfiguration wird auf dieser Seite vorgenommen.

>[AZURE.NOTE]
Wenn Sie einen Verbund konfiguriert haben, sollten Sie sich vor dem Fortsetzen der Installation vergewissern, dass Sie die [Namensauflösung für Verbundserver](active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers) konfiguriert haben.

![Bereit für Konfiguration](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### Stagingmodus
Mit dem Stagingmodus können Sie parallel einen neuen Synchronisierungsserver einrichten. Nur ein Synchronisierungsserver, der einen Export zu einem Verzeichnis in der Cloud durchführt, wird unterstützt. Wenn Sie jedoch eine Verschiebung von einem anderen Server durchführen möchten, z. B. einem Server, auf dem DirSync ausgeführt wird, kann Azure AD Connect im Stagingmodus aktiviert werden. Bei aktiviertem Stagingmodus werden Daten vom Synchronisierungsmodul wie gewohnt importiert und synchronisiert, es findet aber kein Export an Azure AD oder AD statt. Kennwortsynchronisierung und Kennwortrückschreiben sind im Stagingmodus deaktiviert.

![Stagingmodus](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Im Stagingmodus können Sie die erforderlichen Änderungen am Synchronisierungsmodul vornehmen und überprüfen, was exportiert werden soll. Wenn die Konfiguration Ihren Vorstellungen entspricht, führen Sie erneut den Installations-Assistenten aus, und deaktivieren Sie den Stagingmodus. Daraufhin werden Daten von diesem Server an Azure AD exportiert. Stellen Sie sicher, das der andere Server währenddessen deaktiviert ist, sodass nur ein Server aktiv exportieren kann.

Weitere Informationen finden Sie unter [Stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode).

### Überprüfen der Verbundkonfiguration
Wenn Sie auf die Schaltfläche „Überprüfen“ klicken, überprüft Azure AD Connect die DNS-Einstellungen.

![Abgeschlossen](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Überprüfen](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Führen Sie darüber hinaus die folgenden Überprüfungsschritte aus:

- Vergewissern Sie sich, dass Sie sich auf einem mit der Domäne verknüpften Computer über einen Browser anmelden können: Stellen Sie eine Verbindung mit https://myapps.microsoft.com her, und überprüfen Sie die Anmeldung mit Ihrem angemeldeten Konto. Das integrierte AD DS-Administratorkonto wird nicht synchronisiert und kann nicht für die Überprüfung verwendet werden.
- Vergewissern Sie sich, dass Sie sich auf einem Gerät über das Extranet anmelden können. Stellen Sie auf einem privaten Computer oder mobilen Gerät eine Verbindung mit https://myapps.microsoft.com her, und geben Sie Ihre Anmeldeinformationen an.
- Überprüfen Sie die Anmeldung per Rich Client. Stellen Sie eine Verbindung mit https://testconnectivity.microsoft.com her, wechseln Sie zur Registerkarte **Office 365**, und wählen Sie **Office 365-Test für einmaliges Anmelden**.

## Nächste Schritte
Melden Sie sich nach Abschluss der Installation von Windows ab und erneut wieder an, ehe Sie den Synchronisierungsdienst-Manager oder Synchronisierungsregel-Editor verwenden.

Nachdem Sie Azure AD Connect installiert haben, können Sie [die Installation überprüfen und Lizenzen zuweisen](active-directory-aadconnect-whats-next.md).

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0525_2016-->