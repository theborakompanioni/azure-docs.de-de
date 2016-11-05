---
title: 'Azure Active Directory Domain Services: häufig gestellte Fragen | Microsoft Docs'
description: Häufig gestellte Fragen zu den Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand

ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: maheshu

---
# <a name="azure-active-directory-domain-services:-frequently-asked-questions-(faqs)"></a>Azure Active Directory Domain Services: häufig gestellte Fragen
Auf dieser Seite werden häufig gestellte Fragen zu Azure Active Directory Domain Services beantwortet. Die Seite wird bei Bedarf aktualisiert.

### <a name="troubleshooting-guide"></a>Handbuch zur Problembehandlung
Im [Leitfaden zur Problembehandlung](active-directory-ds-troubleshooting.md) finden Sie Lösungen für häufig auftretende Probleme beim Konfigurieren oder Verwalten der Azure Active Directory Domain Services.

### <a name="configuration"></a>Konfiguration
#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory?"></a>Kann ich mehrere Domänen für ein einzelnes Azure AD-Verzeichnis erstellen?
Nein. Sie können nur eine einzelne durch Azure AD Domain Services verwaltete Domäne für ein einzelnes Azure AD-Verzeichnis erstellen.  

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription?"></a>Kann ich Azure AD Domain Services in mehreren virtuellen Netzwerken in meinem Abonnement zur Verfügung stellen?
Der Dienst selbst bietet keine direkte Unterstützung für dieses Szenario. Azure AD Domain Services sind jeweils in nur einem virtuellen Netzwerk verfügbar. Sie können jedoch die Konnektivität zwischen mehreren virtuellen Netzwerken zum Bereitstellen von Azure AD Domain Services mit anderen virtuellen Netzwerken konfigurieren. Dieser Artikel beschreibt, wie Sie eine [Verbindung zu virtuellen Netzwerken in Azure herstellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell?"></a>Kann ich die Azure AD Domain Services mithilfe von PowerShell aktivieren?
Die PowerShell-/automatisierte Bereitstellung der Azure AD Domain Services ist derzeit nicht verfügbar.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal?"></a>Sind die Azure AD Domain Services im neuen Azure-Portal verfügbar?
Nein. Die Azure AD Domain Services können nur im [klassischen Azure-Portal](https://manage.windowsazure.com)konfiguriert werden. Dies wird in Zukunft voraussichtlich auch im [Azure-Portal](https://portal.azure.com) unterstützt.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain?"></a>Kann ich Domänencontroller zu einer verwalteten Domäne der Azure AD Domain Services hinzufügen?
Nein. Die von den Azure AD Domain Services bereitgestellte Domäne ist eine verwaltete Domäne. Die Domänencontroller für diese Domäne müssen nicht bereitgestellt, konfiguriert oder anderweitig verwaltet werden – diese Verwaltungsschritte werden als Dienst von Microsoft bereitgestellt. Daher können Sie keine zusätzlichen Domänencontroller (weder mit Lese-/Schreibzugriff noch mit reinem Lesezugriff) für die verwaltete Domäne hinzufügen.

### <a name="administration-and-operations"></a>Verwaltung und Betrieb
#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop?"></a>Kann ich über Remotedesktop eine Verbindung zum Domänencontroller für meine verwaltete Domäne herstellen?
Nein. Sie besitzen keine Berechtigungen, um über Remotedesktop eine Verbindung mit Domänencontrollern in der verwalteten Domäne herzustellen. Mitglieder der Administratorengruppe für Azure AD-Domänencontroller können die verwaltete Domäne mithilfe von AD-Verwaltungstools wie dem Active Directory-Verwaltungscenter (Active Directory-Administration Center, ADAC) oder AD PowerShell verwalten. Diese Tools werden mithilfe der Remoteserver-Verwaltungstools (Remote Server Administration Tools, RSAT) auf einem Windows-Server installiert, der der verwalteten Domäne beigetreten ist.

#### <a name="i’ve-enabled-azure-ad-domain-services.-what-user-account-do-i-use-to-domain-join-machines-to-this-domain?"></a>Ich habe die Azure AD Domain Services aktiviert. Welches Benutzerkonto verwende ich für den Domänenbeitritt von Computern zu dieser Domäne?
Benutzer, die Sie der Administratorgruppe (z.B. der Gruppe „AAD DC Administrators“) hinzugefügt haben, können Computer in die Domäne einbinden. Darüber hinaus erhalten Benutzer in dieser Gruppe Remotedesktopzugriff auf Computer, die der Domäne beigetreten sind.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services?"></a>Kann ich Domänenadministratorrechte für die von den Azure AD Domain Services bereitgestellte Domäne erhalten?
Nein. Ihnen werden keine Administratorrechte für die verwaltete Domäne gewährt. Die Berechtigungen für „Domänenadministrator“ und „Unternehmensadministrator“ stehen innerhalb der Domäne nicht zur Verfügung. Vorhandene Domänenadministrator- oder Unternehmensadministratorgruppen innerhalb Ihres Azure AD-Verzeichnisses erhalten ebenfalls keine Domänen-/Unternehmensadministratorberechtigungen in der Domäne.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services?"></a>Kann ich mithilfe von LDAP oder anderen Active Directory-Verwaltungstools Gruppenmitgliedschaften in Azure AD Domain Services-Domänen ändern?
Nein. Gruppenmitgliedschaften können nicht in Domänen geändert werden, die von den Azure AD Domain Services verwaltet werden. Das gleiche gilt für Benutzerattribute. Sie können jedoch in Azure AD oder in Ihrer lokalen Domäne Gruppenmitgliedschaften und Benutzerattribute ändern. Solche Änderungen werden automatisch mit den Azure AD Domain Services synchronisiert.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services?"></a>Kann ich das Schema der von den Azure AD Domain Services bereitgestellten Domäne erweitern?
Nein. Das Schema für die verwaltete Domäne wird von Microsoft verwaltet. Schemaerweiterungen werden von den Azure AD Domain Services nicht unterstützt.

#### <a name="can-i-modify-dns-records-provided-by-azure-ad-domain-services?"></a>Kann ich DNS-Einträge ändern, die von den Azure AD Domain Services bereitgestellt werden?
Ja. Benutzern, die zur Administratorengruppe für Azure AD-Domänencontroller gehören, werden DNS-Administratorrechte gewährt, damit sie DNS-Einträge in der verwalteten Domäne ändern können. Diese Benutzer können die DNS-Manager-Konsole auf einem der verwalteten Domäne beigetretenen Windows Server-Computer verwenden, um DNS zu verwalten. Um die DNS-Manager-Konsole zu verwenden, installieren Sie die DNS-Servertools, die zum optionalen Feature der Remoteserver-Verwaltungstools auf dem Server gehören. Weitere Informationen zu [Hilfsprogrammen zur Verwaltung, Überwachung und Problembehandlung von DNS](https://technet.microsoft.com/library/cc753579.aspx) finden Sie in TechNet.

### <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit
#### <a name="is-azure-ad-domain-services-a-paid-service?"></a>Was sind die Azure AD Domain Services?
Ja. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service?"></a>Gibt es eine kostenlose Testversion des Diensts?
Dieser Dienst ist in der kostenlosen Testversion von Azure enthalten. Sie können sich für eine [kostenlose einmonatige Testversion von Azure](https://azure.microsoft.com/pricing/free-trial/)registrieren.

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-(ems)?"></a>Kann ich die Azure AD Domain Services als Teil der Enterprise Mobility Suite (EMS) erwerben?
Nein. Azure AD Domain Services ist ein Azure-Dienst mit nutzungsbasierter Bezahlung und nicht Teil von EMS. Die Azure AD Domain Services sind für alle SKUs von Azure AD (Free, Basic und Premium) verfügbar und werden stündlich je nach Nutzung abgerechnet.

#### <a name="what-azure-regions-is-the-service-available-in?"></a>In welchen Azure-Regionen ist der Dienst verfügbar?
Auf unserer [Seite für Regionen](active-directory-ds-regions.md) finden Sie eine Liste der Azure-Regionen, in denen die Azure AD Domain Services verfügbar sind.

<!--HONumber=Oct16_HO2-->


