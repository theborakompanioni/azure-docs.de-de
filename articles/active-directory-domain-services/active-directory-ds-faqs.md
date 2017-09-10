---
title: "Azure Active Directory Domain Services: Häufig gestellte Fragen | Microsoft Docs"
description: "Häufig gestellte Fragen zu den Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 193e59c610d5c5b553469cf2ea2ecc150236ac27
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: häufig gestellte Fragen
Auf dieser Seite werden häufig gestellte Fragen zu Azure Active Directory Domain Services beantwortet. Die Seite wird bei Bedarf aktualisiert.

### <a name="troubleshooting-guide"></a>Handbuch zur Problembehandlung
Im [Leitfaden zur Problembehandlung](active-directory-ds-troubleshooting.md) finden Sie Lösungen für häufig auftretende Probleme beim Konfigurieren oder Verwalten der Azure Active Directory Domain Services.

### <a name="configuration"></a>Konfiguration
#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Kann ich mehrere Domänen für ein einzelnes Azure AD-Verzeichnis erstellen?
Nein. Sie können nur eine einzelne durch Azure AD Domain Services verwaltete Domäne für ein einzelnes Azure AD-Verzeichnis erstellen.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kann ich Azure AD-Domänendienste in einem virtuellen Azure Resource Manager-Netzwerk aktivieren?
Ja. Azure AD Domain Services können in einem virtuellen Azure Resource Manager-Netzwerk aktiviert werden. Diese Funktion befindet sich derzeit in der Vorschauphase.

#### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Kann ich meine vorhandene verwaltete Domäne aus einem klassischen virtuellen Netzwerk zu einem virtuellen Resource Manager-Netzwerk migrieren?
Derzeit ist dies nicht möglich. Wir werden einen Mechanismus bereitstellen, mit dem Sie Ihre vorhandene verwaltete Domäne aus einem klassischen virtuellen Netzwerk zu einem virtuellen Resource Manager-Netzwerk migrieren können. Bitte achten Sie auf Updates.

#### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-use-adfs-to-authenticate-users-for-access-to-office-365-and-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Kann ich Azure AD Domain Services in einem Azure AD-Verbundverzeichnis aktivieren? Ich verwende AD FS zum Authentifizieren von Benutzern für den Zugriff auf Office 365 und synchronisiere keine Kennworthashes mit Azure AD. Kann ich Azure AD Domain Services für dieses Verzeichnis aktivieren?
Nein. Für Azure AD Domain Services ist der Zugriff auf die Kennworthashes von Benutzerkonten erforderlich, um Benutzer über NTLM oder Kerberos zu authentifizieren. In einem Verbundverzeichnis werden Kennworthashes im Azure AD-Verzeichnis nicht gespeichert. Daher kann Azure AD Domain Services mit diesen Azure AD-Verzeichnissen nicht verwendet werden.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kann ich Azure AD Domain Services in mehreren virtuellen Netzwerken in meinem Abonnement zur Verfügung stellen?
Der Dienst selbst bietet keine direkte Unterstützung für dieses Szenario. Ihre verwaltete Domäne ist jeweils in nur einem virtuellen Netzwerk verfügbar. Sie können jedoch die Konnektivität zwischen mehreren virtuellen Netzwerken zum Bereitstellen von Azure AD Domain Services mit anderen virtuellen Netzwerken konfigurieren. Erfahren Sie, wie Sie eine [Verbindung zu virtuellen Netzwerken in Azure herstellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kann ich die Azure AD Domain Services mithilfe von PowerShell aktivieren?
Die PowerShell-/automatisierte Bereitstellung der Azure AD Domain Services ist derzeit nicht verfügbar.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Sind die Azure AD Domain Services im neuen Azure-Portal verfügbar?
Ja. Die Azure AD Domain Services können im [Azure-Portal](https://portal.azure.com) konfiguriert werden. Wir gehen davon aus, dass wir die Unterstützung des [klassischen Azure-Portals](https://manage.windowsazure.com) in der Zukunft beenden werden.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kann ich Domänencontroller zu einer verwalteten Domäne der Azure AD Domain Services hinzufügen?
Nein. Die von den Azure AD Domain Services bereitgestellte Domäne ist eine verwaltete Domäne. Die Domänencontroller für diese Domäne müssen nicht bereitgestellt, konfiguriert oder anderweitig verwaltet werden – diese Verwaltungsschritte werden als Dienst von Microsoft bereitgestellt. Daher können Sie keine zusätzlichen Domänencontroller (weder mit Lese-/Schreibzugriff noch mit reinem Lesezugriff) für die verwaltete Domäne hinzufügen.

### <a name="administration-and-operations"></a>Verwaltung und Betrieb
#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kann ich über Remotedesktop eine Verbindung zum Domänencontroller für meine verwaltete Domäne herstellen?
Nein. Sie besitzen keine Berechtigungen, um über Remotedesktop eine Verbindung mit Domänencontrollern in der verwalteten Domäne herzustellen. Mitglieder der Administratorengruppe für Azure AD-Domänencontroller können die verwaltete Domäne mithilfe von AD-Verwaltungstools wie dem Active Directory-Verwaltungscenter (Active Directory-Administration Center, ADAC) oder AD PowerShell verwalten. Diese Tools werden mithilfe der Remoteserver-Verwaltungstools (Remote Server Administration Tools, RSAT) auf einem Windows-Server installiert, der der verwalteten Domäne beigetreten ist.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ich habe die Azure AD Domain Services aktiviert. Welches Benutzerkonto verwende ich für den Domänenbeitritt von Computern zu dieser Domäne?
Mitglieder der administrativen Gruppe „AAD DC Administrators“ können Computer in die Domäne einbinden. Darüber hinaus erhalten Mitglieder dieser Gruppe Remotedesktopzugriff auf Computer, die der Domäne beigetreten sind.

#### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>Verfüge ich über Domänenadministratorrechte für die über Azure AD Domain Services bereitgestellte verwaltete Domäne?
Nein. Ihnen werden keine Administratorrechte für die verwaltete Domäne gewährt. Die Berechtigungen für „Domänenadministrator“ und „Unternehmensadministrator“ stehen innerhalb der Domäne nicht zur Verfügung. Vorhandene Domänenadministrator- oder Unternehmensadministratorgruppen innerhalb Ihres Azure AD-Verzeichnisses erhalten ebenfalls keine Domänen-/Unternehmensadministratorberechtigungen in der Domäne.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Kann ich mithilfe von LDAP oder anderen AD-Verwaltungstools Gruppenmitgliedschaften in verwalteten Domänen ändern?
Nein. Gruppenmitgliedschaften können nicht in Domänen geändert werden, die von den Azure AD Domain Services verwaltet werden. Das gleiche gilt für Benutzerattribute. Sie können jedoch in Azure AD oder in Ihrer lokalen Domäne Gruppenmitgliedschaften und Benutzerattribute ändern. Solche Änderungen werden automatisch mit den Azure AD Domain Services synchronisiert.

#### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Wie lange dauert es, bis Änderungen an meinem Azure AD-Verzeichnis in meiner verwalteten Domäne angezeigt werden?
Sowohl über die Benutzeroberfläche von Azure AD als auch über PowerShell vorgenommene Änderungen an Ihrem Azure AD-Verzeichnis werden mit Ihrer verwalteten Domäne synchronisiert. Diese Synchronisation erfolgt im Hintergrund. Nach Abschluss der einmaligen Erstsynchronisierung Ihres Verzeichnisses dauert es in der Regel ca. 20 Minuten bis Änderungen in Azure AD in Ihrer verwalteten Domäne sichtbar werden.

#### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>Kann ich das Schema der über Azure AD Domain Services bereitgestellten verwalteten Domäne erweitern?
Nein. Das Schema für die verwaltete Domäne wird von Microsoft verwaltet. Schemaerweiterungen werden von den Azure AD Domain Services nicht unterstützt.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kann ich DNS-Einträge in meiner verwalteten Domäne ändern oder hinzufügen?
Ja. Mitgliedern der Gruppe „AAD DC Administrators“ werden DNS-Administratorrechte gewährt, damit sie DNS-Einträge in der verwalteten Domäne ändern können. Sie können die DNS-Manager-Konsole auf einem der verwalteten Domäne beigetretenen Windows Server-Computer verwenden, um DNS zu verwalten. Um die DNS-Manager-Konsole zu verwenden, installieren Sie die DNS-Servertools, die zum optionalen Feature der Remoteserver-Verwaltungstools auf dem Server gehören. Weitere Informationen zu [Hilfsprogrammen zur Verwaltung, Überwachung und Problembehandlung von DNS](https://technet.microsoft.com/library/cc753579.aspx) finden Sie in TechNet.

### <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit
#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Was sind die Azure AD Domain Services?
Ja. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Gibt es eine kostenlose Testversion des Diensts?
Dieser Dienst ist in der kostenlosen Testversion von Azure enthalten. Sie können sich für eine [kostenlose einmonatige Testversion von Azure](https://azure.microsoft.com/pricing/free-trial/)registrieren.

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Kann ich die Azure AD Domain Services als Teil der Enterprise Mobility Suite (EMS) erwerben? Benötige ich Azure AD Premium, um Azure AD-Domänendienste zu verwenden?
Nein. Azure AD-Domänendienste ist ein Azure-Dienst mit nutzungsbasierter Bezahlung und nicht Teil von EMS. Azure AD Domain Services kann mit allen Editionen von Azure AD verwendet werden kann (Free, Basic und Premium). Die Abrechnung erfolgt auf Stundenbasis, je nach Verwendung.

#### <a name="what-azure-regions-is-the-service-available-in"></a>In welchen Azure-Regionen ist der Dienst verfügbar?
Eine Liste der Azure-Regionen, in denen Azure AD-Domänendienste verfügbar ist, finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services/) .

