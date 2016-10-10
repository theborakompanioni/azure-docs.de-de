<properties
	pageTitle="Azure AD Domain Services: Vergleich zwischen Azure AD Domain Services und DIY-Domänencontrollern | Microsoft Azure"
	description="Vergleich zwischen Azure Active Directory Domain Services und DIY-Domänencontrollern"
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
	ms.date="09/23/2016"
	ms.author="maheshu"/>

# So entscheiden Sie, ob die Azure AD Domain Services für Ihren Anwendungsfall geeignet sind
Die Azure AD Domain Services ermöglichen es Ihnen, Ihre Workloads in Azure-Infrastrukturdiensten bereitzustellen, ohne sich über die Wartung Ihrer Identitätsinfrastruktur Gedanken machen zu müssen. Dieser verwaltete Dienst unterscheidet sich von einer typischen Windows Server Active Directory-Bereitstellung, die Sie selbst bereitstellen und verwalten. Der Dienst ist für eine einfache Bereitstellung, eine automatisierte Integritätsüberwachung und -bereinigung sowie eine einfache Identitätsstruktur für die Cloud konzipiert. Wir verbessern den Dienst fortlaufend, um Unterstützung für gängige Bereitstellungsszenarios hinzuzufügen.

Um zu entscheiden, ob Sie die Azure AD Domain Services verwenden oder in Azure eine eigene AD-Infrastruktur (Do-It-Yourself) einrichten und verwalten sollten, können Sie folgende Ressourcen heranziehen:

- Sehen Sie sich die Liste der [Azure AD Domain Services-Features](active-directory-ds-features.md) an.

- Informieren Sie sich über die [Bereitstellungsszenarios für die Azure AD Domain Services](active-directory-ds-scenarios.md).

- Und schließlich sollten Sie sich den [Vergleich zwischen Azure AD Domain Services und einer DIY-AD-Option](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure) ansehen.


## Vergleich zwischen Azure AD Domain Services und einer DYI-AD-Domäne in Azure
Die folgende Tabelle dient als Entscheidungshilfe bei der Auswahl zwischen der Verwendung der Azure AD Domain Services und der Verwaltung einer eigenen AD-Infrastruktur in Azure.

|Feature|Azure AD Domain Services|„Do-It-Yourself“ AD in Azure-VMs|
|---|:---:|:---:|
|[**Verwalteter Dienst**](active-directory-ds-comparison.md#managed-service)|Ja|Nein|
|[**Sichere Bereitstellungen**](active-directory-ds-comparison.md#secure-deployments)|Ja|Der Administrator muss die Bereitstellung absichern.|
|[**DNS-Server**](active-directory-ds-comparison.md#dns-server)|Ja (verwalteter Dienst)|Ja|
|[**Berechtigungen als Domänen- oder Unternehmensadministrator**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|Nein|Ja|
|[**Domänenbeitritt**](active-directory-ds-comparison.md#domain-join)|Ja|Ja|
|[**Domänenauthentifizierung mithilfe von NTLM und Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|Ja|Ja|
|[**Benutzerdefinierte OE-Struktur**](active-directory-ds-comparison.md#custom-ou-structure)|Ja|Ja|
|[**Schemaerweiterungen**](active-directory-ds-comparison.md#schema-extensions)|Nein|Ja|
|[**AD-Domänen-/Gesamtstrukturvertrauensstellungen**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|Nein|Ja|
|[**LDAP-Lesevorgänge**](active-directory-ds-comparison.md#ldap-read)|Ja|Ja|
|[**Sicheres LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|Ja|Ja|
|[**LDAP-Schreibvorgänge**](active-directory-ds-comparison.md#ldap-write)|Nein|Ja|
|[**Gruppenrichtlinie**](active-directory-ds-comparison.md#group-policy)|Einfach|Vollständig|
|[**Geografisch verteilte Bereitstellungen**](active-directory-ds-comparison.md#geo-dispersed-deployments)|Nein|Ja|

#### Verwalteter Dienst
Azure AD Domain Services-Domänen werden von Microsoft verwaltet. Sie müssen sich weder um Patches, Updates, Überwachung oder Sicherung noch um die Gewährleistung der Verfügbarkeit Ihrer Domäne kümmern. Diese Verwaltungsaufgaben werden als Dienst von Microsoft Azure für Ihre verwalteten Domänen angeboten.

#### Sichere Bereitstellungen
Die verwaltete Domäne wird basierend auf den Microsoft Best Practices für AD-Bereitstellungen zuverlässig geschützt. Diese Best Practices basieren auf der jahrzehntelangen Erfahrung des AD-Produktteams mit der Entwicklung und Unterstützung von AD-Bereitstellungen. Für DIY-Bereitstellungen (Do-It-Yourself) müssen bestimmte Bereitstellungsschritte ausgeführt werden, um Ihre Bereitstellung einzuschränken/zu schützen.

#### DNS-Server
Eine verwaltete Azure AD Domain Services-Domäne umfasst verwaltete DNS-Dienste. Mitglieder der Gruppe „AAD DC Adminstrators“ können das DNS-System in der verwalteten Domäne verwalten. Mitglieder dieser Gruppe erhalten vollständige DNS-Administratorrechte für die verwaltete Domäne. Die DNS-Verwaltung kann mithilfe der im RSAT-Paket (Remoteserver-Verwaltungstools) enthaltenen DNS-Verwaltungskonsole durchgeführt werden.

#### Berechtigungen als Domänen- oder Unternehmensadministrator
Diese erhöhten Rechte werden in einer verwalteten AAD DS-Domäne nicht bereitgestellt. Anwendungen, die diese erhöhten Rechte für eine Installation/Ausführung benötigen, können in verwalteten Domänen nicht ausgeführt werden. Ein kleinerer Teilsatz der Administratorrechte steht Mitgliedern der Gruppe der delegierten Administratoren namens „AAD DC Administrators“ zur Verfügung. Mitglieder dieser Gruppe erhalten die Berechtigung zum Konfigurieren von DNS, können Gruppenrichtlinien konfigurieren, erhalten Administratorrechte auf in die Domäne eingebundenen Computern usw.

#### Domänenbeitritt
Sie können der verwalteten Domäne in ähnlicher Weise virtuelle Computer hinzufügen wie Sie einer AD-Domäne Computer hinzufügen.

#### Domänenauthentifizierung mithilfe von NTLM und Kerberos
Mit den Azure AD Domain Services können Sie Ihre Unternehmensanmeldeinformationen zur Authentifizierung bei der verwalteten Domäne verwenden. Die Anmeldeinformationen werden mit Ihrem Azure AD-Mandanten synchronisiert. Für synchronisierte Mandanten stellt Azure AD Connect sicher, dass Änderungen, die lokal an den Anmeldeinformationen vorgenommen werden, mit Azure AD synchronisiert werden. Bei einem DIY-Domänensetup müssen Sie möglicherweise eine Domänenvertrauensstellung mit einer lokalen Kontogesamtstruktur einrichten, damit Benutzer sich mit ihren Unternehmensanmeldeinformationen authentifizieren können. Alternativ ist möglicherweise die Einrichtung einer AD-Replikation erforderlich, um sicherzustellen, dass Benutzerkennwörter mit Ihren Azure-Domänencontroller-VMs synchronisiert werden.

#### Benutzerdefinierte OE-Struktur
Mitglieder der Gruppe „AAD DC Adminstrators“ können benutzerdefinierte OEs in der verwalteten Domäne erstellen.

#### Schemaerweiterungen
Sie können das Basisschema einer verwalteten Azure AD Domain Services-Domäne nicht erweitern. Daher können Anwendungen, die von Erweiterungen des AD-Schemas abhängen (beispielsweise neue Attribute unter dem Benutzerobjekt) nicht per „Lift and Shift“ in AAD DS-Domänen verschoben werden.

#### AD-Domänen-/Gesamtstrukturvertrauensstellungen
Verwaltete Domänen können nicht zum Einrichten von Vertrauensstellungen (eingehend/ausgehend) mit anderen Domänen konfiguriert werden. Aus diesem Grund können die Azure AD Domain Services in Szenarios mit Ressourcengesamtstruktur-Bereitstellungen oder in Fällen, in denen Kennwörter nicht mit Azure AD synchronisiert werden sollen, nicht verwendet werden.

#### LDAP-Lesevorgänge
Die verwaltete Domäne unterstützt LDAP-Leseworkloads. Daher können Sie Anwendungen bereitstellen, die LDAP-Lesevorgänge in der verwalteten Domäne ausführen.

#### Sicheres LDAP (LDAPS)
Sie können die Azure AD Domain Services so konfigurieren, dass ein Zugriff über sicheres LDAP auf Ihre verwaltete Domäne möglich ist – auch über das Internet.

#### LDAP-Schreibvorgänge
Die verwaltete Domäne ist für Benutzerobjekte schreibgeschützt. Deshalb können Anwendungen, die LDAP-Schreibvorgänge für Attribute des Benutzerobjekts ausführen, nicht in einer verwalteten Domäne verwendet werden. Darüber hinaus können Benutzerkennwörter nicht innerhalb der verwalteten Domäne geändert werden. Das Ändern von Gruppenmitgliedschaften oder Gruppenattributen innerhalb der verwalteten Domäne ist ebenfalls unzulässig. Änderungen hingegen, die (über PowerShell/Azure-Portal) in Azure AD an Benutzerattributen oder -kennwörtern oder im lokalen AD-System durchgeführt werden, werden mit der verwalteten AAD DS-Domäne synchronisiert.

#### Gruppenrichtlinie
Komplexe Gruppenrichtlinienelemente werden in der verwalteten AAD DS-Domäne nicht unterstützt. Beispielsweise ist es nicht möglich, separate Gruppenrichtlinienobjekte (GPOs) für jede Organisationseinheit (OE) in der Domäne zu erstellen und bereitzustellen oder eine WMI-Filterung für Gruppenrichtlinienziele durchzuführen. Es ist jeweils ein integriertes GPO für die Container „AADDC Computers“ und „AADDC Users“ vorhanden, die zum Konfigurieren von Gruppenrichtlinien angepasst werden können.

#### Geografisch verteilte Bereitstellungen
Verwaltete Azure AD Domain Services-Domänen stehen in einem einzigen virtuellen Netzwerk in Azure zur Verfügung. Für Szenarios, die es erforderlich machen, dass Domänencontroller in mehreren Azure-Regionen weltweit zur Verfügung stehen, stellt das Einrichten von Domänencontrollern in Azure-IaaS-VMs möglicherweise die bessere Alternative dar.


## DIY-AD-Bereitstellungsoptionen
Es gibt Anwendungsfälle, bei denen möglicherweise einige der Funktionen einer Windows Server AD-Installation benötigt werden. Erwägen Sie in diesen Fällen eine der folgenden DIY-Optionen (Do-It-Yourself):

- **Eigenständige Clouddomäne:** Sie können eine eigenständige Clouddomäne mithilfe von virtuellen Azure-Computern einrichten, die als Domänencontroller konfiguriert wurden. Diese Infrastruktur kann nicht in Ihre lokale AD-Umgebung integriert werden. Diese Option erfordert einen anderen Satz mit Anmeldeinformationen, um sich bei den VMs in der Cloud anzumelden und diese zu verwalten.

- **Ressourcengesamtstruktur-Bereitstellung:** Sie können unter Verwendung von virtuellen Azure-Computern, die als Domänencontroller konfiguriert sind, eine Domäne in der Ressourcengesamtstruktur-Topologie einrichten. Als Nächstes können Sie eine AD-Vertrauensstellung mit Ihrer lokalen AD-Umgebung konfigurieren. Dann können Sie in die Domäne eingebundene Computer (Azure-VMs) zu dieser Ressourcengesamtstruktur in der Cloud hinzufügen. Die Benutzerauthentifizierung erfolgt über eine VPN- oder ExpressRoute-Verbindung mit Ihrem lokalen Verzeichnis.

- **Ausweitung Ihrer lokalen Domäne auf Azure:** Sie können ein virtuelles Azure-Netzwerk über eine VPN-/ExpressRoute-Verbindung mit Ihrem lokalen Netzwerk verbinden, sodass Sie Azure-VMs in Ihre lokale AD-Umgebung einbinden können. Eine weitere Alternative besteht darin, Replikatdomänencontroller Ihrer lokalen Domäne zu einer VM in Azure heraufzustufen. Anschließend können Sie eine Replikation über eine VPN- oder ExpressRoute-Verbindung mit Ihrem lokalen Verzeichnis einrichten. Durch diese Bereitstellung wird Ihre lokale Domäne effizient auf Azure ausgeweitet.

> [AZURE.NOTE] Sie stellen möglicherweise fest, dass für Ihre Anwendungsfälle eine DIY-Option besser geeignet ist. Wir würden uns über [Ihr Feedback](active-directory-ds-contact-us.md) freuen, damit wir besser verstehen, welche Features Sie benötigen, um sich in Zukunft für die Azure AD Domain Services zu entscheiden. Dieses Feedback hilft uns dabei, den Dienst weiter zu verbessern und an die Anforderungen Ihrer Bereitstellungen und Anwendungsfälle anzupassen.

Wir haben [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx) veröffentlicht, um DIY-Installationen zu vereinfachen.


## Verwandte Inhalte
- [Features: Azure AD Domain Services](active-directory-ds-features.md)

- [Bereitstellungsszenarios: Azure AD Domain Services](active-directory-ds-scenarios.md)

- [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx)

<!---HONumber=AcomDC_0928_2016-->