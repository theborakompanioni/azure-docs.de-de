<properties
	pageTitle="Vorschau von Azure Active Directory-Domänendienste: häufig gestellte Fragen | Microsoft Azure"
	description="Häufig gestellte Fragen zu Azure Active Directory-Domänendienste"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/31/2016"
	ms.author="maheshu"/>

# Vorschau von Azure Active Directory-Domänendienste: häufig gestellte Fragen

Auf dieser Seite werden häufig gestellte Fragen zur Vorschauversion von Azure Active Directory-Domänendienste beantwortet. Die Seite wird bei Bedarf aktualisiert.

### Handbuch zur Problembehandlung
In den [Handbüchern zur Problembehandlung](active-directory-ds-troubleshooting.md) finden Sie Lösungen für häufig auftretende Probleme beim Konfigurieren oder Verwalten von Azure Active Directory-Domänendienste.


### Konfiguration

#### Kann ich mehrere Domänen für ein einzelnes Azure AD-Verzeichnis erstellen?
Nein. Sie können nur eine einzelne durch Azure AD-Domänendienste verwaltete Domäne für ein einzelnes Azure AD-Verzeichnis erstellen.

#### Kann ich Azure AD-Domänendienste in mehreren virtuellen Netzwerken in meinem Abonnement zur Verfügung stellen?
Der Dienst selbst bietet keine direkte Unterstützung für dieses Szenario. Azure AD-Domänendienste sind jeweils in nur einem virtuellen Netzwerk verfügbar. Sie können jedoch die Konnektivität zwischen mehreren virtuellen Netzwerken zum Bereitstellen von Azure AD-Domänendienste mit anderen virtuellen Netzwerken konfigurieren. Dieser Artikel beschreibt, wie Sie eine [Verbindung zu virtuellen Netzwerken in Azure herstellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### Kann ich Azure AD-Domänendienste mithilfe von PowerShell aktivieren?
Die PowerShell-/automatisierte Bereitstellung von Azure AD-Domänendienste ist derzeit nicht verfügbar.

#### Ist Azure AD-Domänendienste im neuen Azure-Portal verfügbar?
Nein. Azure AD-Domänendienste können nur im [klassischen Azure-Portal](https://manage.windowsazure.com) konfiguriert werden. Dies wird in Zukunft voraussichtlich auch im [Azure-Portal](https://portal.azure.com) unterstützt.

#### Kann ich Domänencontroller zu einer verwalteten Domäne der Azure AD-Domänendienste hinzufügen?
Nein. Die von den Azure AD-Domänendiensten bereitgestellte Domäne ist eine verwaltete Domäne. Die Domänencontroller für diese Domäne müssen nicht bereitgestellt, konfiguriert oder anderweitig verwaltet werden – diese Verwaltungsschritte werden als Dienst von Microsoft bereitgestellt. Daher können Sie keine zusätzlichen Domänencontroller (weder mit Lese-/Schreibzugriff noch mit reinem Lesezugriff) für die verwaltete Domäne hinzufügen.

### Verwaltung und Betrieb

#### Kann ich über Remotedesktop eine Verbindung zum Domänencontroller für meine verwaltete Domäne herstellen?
Nein. Da es sich um eine verwaltete Domäne handelt, können Sie über Remotedesktop keine Verbindung zu den Domänencontroller für die Domäne herstellen. Mitglieder der Administratorengruppe für Azure AD-Domänencontroller können die verwaltete Domäne mithilfe von AD-Verwaltungstools wie dem Active Directory-Verwaltungscenter (Active Directory-Administration Center, ADAC) oder AD PowerShell verwalten. Diese Tools werden mithilfe der Remoteserver-Verwaltungstools (Remote Server Administration Tools, RSAT) auf einem Windows-Server installiert, der der verwalteten Domäne beigetreten ist.

#### Ich habe Azure AD-Domänendienste aktiviert. Welches Benutzerkonto verwende ich für den Domänenbeitritt von Computern zu dieser Domäne?
Alle Benutzerkonten, die Sie der Administratorgruppe (z.B. der Gruppe „AAD DC Administrators“) hinzugefügt haben, können Computer in die Domäne einbinden. Darüber hinaus erhalten Benutzer in dieser Gruppe Remotedesktopzugriff auf Computer, die der Domäne beigetreten sind.

#### Kann ich Domänenadministratorrechte für die von Azure AD-Domänendienste bereitgestellte Domäne erhalten?
Nein. Da es sich um einen verwalteten Dienst handelt, erhalten Sie keine Administratorrechte in der Domäne. Dies bedeutet, dass die Berechtigungen für "Domänenadministrator" und "Unternehmensadministrator" innerhalb der Domäne nicht zur Verfügung stehen. Vorhandene Domänenadministrator- oder Unternehmensadministratorgruppen innerhalb Ihres Azure AD-Verzeichnisses erhalten ebenfalls keine Domänen-/Unternehmensadministratorberechtigungen in der Domäne.

#### Kann ich mithilfe von LDAP oder anderen Active Directory-Verwaltungstools Gruppenmitgliedschaften in Azure AD-Domänendienste-Domänen ändern?
Nein. Gruppenmitgliedschaften können nicht in Domänen geändert werden, die von Azure AD-Domänendienste verwaltet werden. Das gleiche gilt für Benutzerattribute. Sie können jedoch in Azure AD oder in Ihrer lokalen Domäne Gruppenmitgliedschaften und Benutzerattribute ändern. Solche Änderungen werden automatisch mit Azure AD-Domänendienste synchronisiert.

#### Kann ich das Schema der von den Azure AD-Domänendiensten bereitgestellten Domäne erweitern?
Nein. Das Schema für die verwaltete Domäne wird von Microsoft verwaltet. Schemaerweiterungen werden von Azure AD-Domänendiensten nicht unterstützt.

#### Kann ich DNS-Einträge ändern, die von Azure AD-Domänendiensten bereitgestellt werden?
Ja. Benutzern, die zur Administratorengruppe für Azure AD-Domänencontroller gehören, werden DNS-Administratorrechte gewährt, damit sie DNS-Einträge in der verwalteten Domäne ändern können. Diese Benutzer können die DNS-Manager-Konsole auf einem der verwalteten Domäne beigetretenen Windows Server-Computer verwenden, um DNS zu verwalten. Um die DNS-Manager-Konsole zu verwenden, installieren Sie die DNS-Servertools, die zum optionalen Feature der Remoteserver-Verwaltungstools auf dem Server gehören. Weitere Informationen zu [Hilfsprogrammen zur Verwaltung, Überwachung und Problembehandlung von DNS](https://technet.microsoft.com/library/cc753579.aspx) finden Sie in TechNet.

### Abrechnung und Verfügbarkeit

#### Ist dieser Dienst kostenpflichtig?
Der Dienst ist für die Dauer der öffentlichen Vorschau zu einem speziellen günstigeren Preis verfügbar. Die Abrechnung zum vollen Preis beginnt, sobald der Dienst allgemein verfügbar (GA) ist. Weitere Informationen hierzu finden Sie in der Preisübersicht.

#### Gibt es eine kostenlose Testversion des Diensts?
Dieser Dienst ist in der kostenlosen Testversion von Azure enthalten. Sie können sich für eine [kostenlose einmonatige Testversion von Azure](https://azure.microsoft.com/pricing/free-trial/) registrieren.

#### Kann ich Azure AD-Domänendienste als Teil der Enterprise Mobility Suite (EMS) erwerben?
Nein. Azure AD-Domänendienste ist ein Azure-Dienst mit nutzungsbasierter Bezahlung und nicht Teil von EMS. Die Azure AD-Domänendienste sind für alle SKUs von Azure AD (Free, Basic und Premium) verfügbar und werden stündlich je nach Nutzung abgerechnet.

#### In welchen Azure-Regionen ist der Dienst verfügbar?
Auf unserer [Seite für Regionen](active-directory-ds-regions.md) finden Sie eine Liste der Azure-Regionen, in denen Azure AD-Domänendienste verfügbar ist.

#### Wann wird Azure AD-Domänendienste allgemein verfügbar sein?
Wir können derzeit keine Zeitpläne für die allgemeine Verfügbarkeit des Diensts bekannt geben.

<!---HONumber=AcomDC_0427_2016-->